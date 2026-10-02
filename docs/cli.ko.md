# 명령줄

`syship` 명령으로 SySHIP을 명령줄에서 조작할 수 있습니다. 반복 작업을 자동화하거나(예: 여러 프로젝트에 같은 적하와 복원성 검토를 실행), 파이썬 스크립트 같은 직접 만든 프로그램에서 SySHIP을 제어하거나, AI 에이전트가 SySHIP을 다루게 할 때 씁니다.

모든 명령은 실행 중인 SySHIP에 대해 동작합니다.

- **창이 열려 있으면** 명령이 실행되는 대로 창에 반영됩니다. 입력칸 값이 바뀌고, 3D 뷰가 다시 그려지고, 오래 걸리는 계산은 진행률이 표시됩니다. 지켜보다가 언제든 직접 조작하거나 되돌릴 수 있습니다.
- **창 없이**(headless) 실행하면 SySHIP이 백그라운드에서 계산만 합니다. 많은 케이스를 처리할 때 가장 빠릅니다.

## 위치

| 시스템 | 경로 |
|---|---|
| Windows | `<설치 폴더>\resources\cli\syship.exe`, 기본값은 `%LOCALAPPDATA%\Programs\SySHIP\resources\cli\syship.exe` |
| macOS | `/Applications/SySHIP.app/Contents/Resources/cli/syship` |

Windows에서는 설치 프로그램이 이 폴더를 사용자 `PATH`에 추가하고, 제거할 때 지웁니다. 설치 후 새로 연 터미널에서 바로 `syship`을 쓸 수 있습니다. macOS에서는 **Help → Install 'syship' Command in PATH**를 한 번 실행하면 `/usr/local/bin`에 명령이 연결됩니다(암호를 묻습니다). **Help → Uninstall 'syship' Command**로 연결을 지웁니다.

## 시작하기

```sh
syship status                 # 연결된 SySHIP 확인
syship list                   # 모든 명령
syship describe compart.loading.set   # 명령 하나의 파라미터
```

SySHIP 창이 열려 있지 않으면 먼저 백그라운드 인스턴스를 띄우고, 다 쓰면 멈춥니다.

```sh
syship serve --headless
syship app.shutdown
```

아무 명령에나 `--start`를 붙이면, 실행 중인 SySHIP이 없을 때 백그라운드 인스턴스를 자동으로 띄웁니다.

## 명령 쓰는 법

- 명령 이름은 `<단계>.<대상>.<동작>` 형식입니다. 예: `compart.stability.gz`.
- 파라미터는 `--이름 값`으로 줍니다. 목록은 플래그를 반복하거나(`--meshes A --meshes B`), 숫자는 쉼표로 이어 씁니다(`--translate 0,0,25`).
- 값은 창과 같은 단위를 씁니다. 길이 m, 무게 t, 각도 도, 밀도 t/m³.
- 구획은 Mesh List의 이름으로 지정합니다. `*`, `?` 와일드카드를 쓸 수 있습니다. 예: `--meshes "W.B. TK*"`.
- 파일을 덮어쓰거나 작업을 버리는 명령(다른 프로젝트 열기, 메시 삭제)은 `--yes`가 필요합니다.
- 대부분의 변경은 `syship compart.history.undo`나 창의 **Undo**로 되돌릴 수 있습니다.

## 예: 선체 준비

HULL의 Import, Variation 탭과 같은 단계입니다.

```sh
syship hull.import --path kvlcc.stl
syship hull.transform --scale 1,1,1
syship hull.stations.create --ap 0 --fp 320
syship hull.hydrostatics --draft 20.8
syship hull.lines.create --type waterline --positions "2:28:2"
syship hull.lines.create --type buttock --positions "4, 8, 12, 16, 20"
syship hull.variation.length --targetLbp 330 --reference mid
syship hull.variation.cp --targetCb 0.82 --targetLcb 10
syship hull.project.save --path hull_v2.zip
syship compart.project.new --yes
```

위치는 창과 같은 형식으로 씁니다. `1, 2, 2.5`처럼 나열하거나 `2:28:2`처럼 `시작:끝:간격`으로 씁니다. `hull.stations.create` 이후에는 A.P.가 x = 0이 되도록 선체가 옮겨지고, 이후 명령의 x 위치는 A.P.에서 잰 값입니다. `compart.project.new`는 **New from HULL**처럼 완성된 선체로 COMPART 프로젝트를 시작합니다.

## 예: 구획 분할

Create와 Operation 패널의 기능마다 같은 명령이 있으므로, 모델링 스크립트 없이 한 단계씩 구획 모델을 만들 수 있습니다.

```sh
syship compart.mesh.plane --axis x --position 51 --name er_bhd
syship compart.mesh.skinsurf --name inner_bhd --direction x --input inner_bhd.json
syship compart.mesh.box --name box1 --min 45.9,6.37,9.624 --max 51,35,35
syship compart.mesh.split --targets Hull --cutters er_bhd
syship compart.mesh.update --meshes "Hull (1)" --name Stern
syship compart.mesh.union --meshes Hull --meshes Cap --name Hull
syship compart.mesh.delete --meshes er_bhd --yes
syship compart.layer.create --name "Cargo Tank"
syship compart.layer.move --meshes "*C.O. TK*" --layer "Cargo Tank"
syship compart.layer.list
```

`compart.mesh.split`은 나뉜 조각마다 중심과 범위(m)를 보여 주므로, 이름을 바꾸기 전에 어느 조각인지 확인할 수 있습니다. 조각 이름은 `<이름> (1)`, `<이름> (2)`, ... 이고 절단면은 남습니다. `compart.mesh.subtract`는 절단면을 지우고, `compart.mesh.union`과 `compart.mesh.intersect`는 입력 메시를 결과로 바꿉니다. skinsurf의 단면은 `{"profiles": [{"position": 51, "points": [[6.37, 35], [6.37, 3]]}, ...]}` 형식으로 주며, 점은 각 단면 평면 위의 좌표(m)입니다.

## 예: 적하와 복원성 검토

```sh
syship compart.project.load --path ship.zip --yes
syship compart.category.assign --meshes "*C.O. TK*" --category "Crude oil"
syship compart.loading.set --meshes "*C.O. TK*" --fillPercent 98
syship compart.loading.set --meshes "W.B. TK*" --weight 12000
syship compart.lightship.set --weight 40000 --lcg 150 --tcg 0 --vcg 17
syship compart.stability.gz --heelStepDeg 1
syship compart.stability.evaluate
syship compart.project.save --path ship_loaded.zip
```

`compart.stability.evaluate`는 [Stability criteria](compart/stability.md)의 각 조건과 그 결과, 값을 출력합니다.

## 명령 목록 실행

단계를 JSON 파일에 저장하고 `syship run steps.json`으로 순서대로 실행합니다. 실패한 단계에서 멈추고 몇 번째 단계인지 알려줍니다.

```json
[
  {"command": "compart.project.load", "args": {"path": "ship.zip"}, "yes": true},
  {"command": "compart.loading.set", "args": {"meshes": ["*C.O. TK*"], "fillPercent": 98}},
  {"command": "compart.stability.gz", "args": {"heelStepDeg": 2}},
  {"command": "compart.stability.evaluate"}
]
```

## 파이썬에서 쓰기

`--json`을 붙이면 프로그램이 읽을 수 있는 JSON 한 줄을 출력합니다.

```python
import json, subprocess

out = subprocess.run(["syship", "compart.stability.gz", "--includePoints", "false", "--json"],
                     capture_output=True, text=True, encoding="utf-8")
result = json.loads(out.stdout)
if result["ok"]:
    print(result["data"]["variables"]["GM0"])
else:
    print(result["error"]["message"], result["error"].get("hint"))
```

종료 코드는 성공이면 `0`, 명령이 실패하면 `1`, 명령 이름이나 파라미터가 틀렸거나 `--yes`가 필요하면 `2`, 실행 중인 SySHIP이 없으면 `3`입니다.

## 창 전용 명령

다음 명령은 SySHIP 창이 있어야 합니다: `app.screenshot --path view.png`, `app.stage.open`, `hull.tab.open`, `hull.view.layers`, `hull.cp-curve.open`, `compart.tab.open`, `compart.view.camera`, `compart.view.select`, `compart.view.equilibrium`, `compart.stability.criteria.open`.

## AI 에이전트와 함께 쓰기

셸 명령을 실행할 수 있는 AI 에이전트는 SySHIP을 스스로 쓸 수 있습니다. `syship list`와 `syship describe`로 명령을 찾고, 실행한 뒤, 조회 명령(`compart.project.summary`, `compart.loading.get`, `compart.stability.last`)이나 `app.screenshot`으로 결과를 확인합니다. 창이 열려 있으면 에이전트가 하는 모든 단계를 볼 수 있습니다.

!!! note "지원 범위"
    현재 명령줄은 HULL(가져오기, 변형, 내보내기, 프로젝트)과 COMPART(프로젝트, 모델링 명령과 스크립트, 레이어, 카테고리와 구획 계수, 적하, 손상, 복원성)를 지원합니다. COMPART Hydro와 DIMENSION 명령은 이후에 추가됩니다.
