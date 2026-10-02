# Stability

현재 적하 상태에서 배가 어떤 자세로 뜨는지 구하고, 복원정(**GZ**) 곡선을 복원성 기준으로 판정합니다. [Damage](damage.md)에서 침수 구획이 없으면 **비손상(intact)** 케이스, 있으면 **손상(damage)** 케이스입니다. 계산 방식은 같고 기본 판정 기준만 다릅니다.

Stability는 [Loading](loading.md) 탭의 총중량과 결합 무게중심을 그대로 사용합니다. 부분 적재된 액체 탱크는 실제 자유수면으로 계산합니다. heel과 trim이 바뀔 때마다 액면을 다시 맞추므로, 자유수면 효과가 GZ 값에 이미 반영되어 있습니다.

탭 맨 위의 **Seawater density (t/m³)**(기본값 1.025)는 아래 두 섹션에 모두 적용됩니다.

## 1. Equilibrium

**Find Equilibrium**을 누르면 부력과 무게가 같고 부력중심과 무게중심이 일직선이 되도록 heel, trim, 흘수를 함께 풉니다.

결과로 다음 값을 보여줍니다.

- **Heel** - 기울기 크기와 방향(port 또는 starboard), 똑바로 서 있으면 0°
- **Trim** - 각도, 그리고 선체의 AP/FP를 알고 있으면 LBP 기준 trim 길이
- **Waterline z** - 모델 좌표에서 수면의 높이(용골에서 잰 흘수가 아님)
- **Displacement**, **LCB**, **TCB**, **VCB**
- **Deck edge freeboard** - 평형 상태에서 갑판 가장자리의 최소 건현

**Show Result / Hide Result**로 3D View를 똑바로 선 모델과 평형 자세(평형 수면 위치의 불투명한 해수면 포함) 사이에서 전환합니다.

## 2. GZ Curve

**Heel step (deg)**(기본값 5°, 0.25°~90°)을 정하고 **Run GZ Curve**를 누르면, trim을 자유롭게 두고 heel을 -90°(port)부터 90°(starboard)까지 계산합니다. 간격이 작을수록 곡선이 정확해지지만 시간이 더 걸립니다.

**View Details**를 누르면 **Stability criteria** 창이 열립니다.

## Stability criteria 창

![Stability criteria 창](../assets/screenshots/compart-stability-criteria.png)

### GZ curve (왼쪽)

- **Curve fit** - 계산한 점들을 잇는 방식입니다. 선택한 방식은 그림뿐 아니라 판정에 쓰는 값 읽기, 교점, 정점, 면적 계산에 모두 적용됩니다.
    - **Cubic spline**(기본값) - 매끄럽고, heel 간격이 넓어도 정확합니다. 곡선이 급하게 꺾이는 곳(예: 갑판 가장자리가 잠기는 각도)에서는 약간 넘칠 수 있습니다.
    - **PCHIP** - 넘침 없이 매끄럽지만, 두 점 사이에 있는 정점은 깎입니다.
    - **Linear** - 점 사이를 직선으로 잇습니다.
- **Download CSV** - 계산한 모든 점(heel, 흘수, trim, LCB, TCB, VCB, GZ, 수렴 여부)을 내려받습니다.
- **Heel / GZ** - 각 축의 시작, 끝, 그리드 간격입니다.

곡선은 배가 기울어지는 쪽을 + heel로 그립니다. 점선은 평형 heel 위치입니다.

### Conditions (오른쪽)

한 줄이 조건 하나입니다. 체크하면 조건이 포함되고, 결과가 바로 **Satisfied**(만족), **Not satisfied**(불만족), **N/A**(필요한 값이 정의되지 않음, 예: 90° 안에 소실각이 없음), **Error**(스크립트 오류)로 표시됩니다. 그룹 머리의 체크박스로 그룹 전체를 켜고 끌 수 있습니다.

기본 제공 조건은 다음과 같습니다.

| 그룹 | 조건 |
|---|---|
| IMO IS Code 2008 (MSC.267(85), A.749(18)과 같은 기준값) | 0~30° 면적 ≥ 0.055 m·rad, 0~40° 면적 ≥ 0.09 m·rad, 30~40° 면적 ≥ 0.03 m·rad, 30° 이상에서의 GZ ≥ 0.2 m, 최대 GZ 각도 ≥ 25°, 초기 GM ≥ 0.15 m |
| MARPOL Annex I Reg.28 | 평형 heel ≤ 25°(갑판 가장자리가 잠기지 않으면 30°), 평형 이후 양의 GZ 범위 ≥ 20°, 최대 잔류 GZ ≥ 0.1 m, 평형 이후 면적 ≥ 0.0175 m·rad |
| ICLL Reg.27 (Type A) | MARPOL과 같고 heel 한계만 15°(17°) |
| Constructions | 0°에서의 접선으로 구한 GM, 소실각(판정 없음, 학습용) |

- 비손상 케이스는 IMO 조건이, 손상 케이스는 MARPOL과 ICLL 조건이 켜진 상태로 시작하고, 라이브러리에서 `@default`로 표시한 규정도 함께 켜집니다. **Reset to default**로 이 상태로 돌아갑니다.
- 40° 기준은 40°와 소실각 중 작은 값을 씁니다. 개구부(침수점)는 모델링하지 않습니다.
- **Initial GM**은 똑바로 선 상태에서 GZ의 기울기이므로, 자유수면과 침수 구획의 영향이 이미 들어 있습니다.

### Details

조건의 **Details**를 누르면 목록 아래에 그 조건이 열립니다. 만족 여부, 각 판정의 값과 기준값, 그리고 조건 스크립트를 줄마다 계산된 값과 함께 보여줍니다. 이때 GZ 차트에는 그 조건의 작도(선, 점, 면적)가 그려집니다. 스크립트 줄을 누르면 그 줄과 관련된 작도가 강조됩니다.

**Edit condition**을 누르면 같은 자리에서 이름과 스크립트를 바로 고칠 수 있습니다. 입력하는 대로 작도와 결과가 갱신됩니다. **Save to conditions**로 저장하면 규정 라이브러리에 들어가서, 이 컴퓨터의 다른 프로젝트에서도 다시 쓸 수 있습니다.

- 라이브러리에 있는 규정과 같은 이름으로 저장하면 그 규정을 덮어씁니다. 기본 규정은 원본이 따로 보존되고, 목록에 **edited ↺** 표시가 붙습니다. 이 표시를 누르면 원래 스크립트로 돌아갑니다.
- 새 이름으로 저장하면 **User** 그룹에 추가됩니다.

**+ Make new condition**은 빈 조건을 편집 상태로 만듭니다.

### 규정 라이브러리와 .rule 파일

켤 수 있는 조건은 규정 라이브러리에서 옵니다. SySHIP에 들어 있는 기본 규정과, 직접 만들거나 가져온 규정입니다. 규정은 `.rule` 텍스트 파일로 저장됩니다.

- **Import...**: 하나 이상의 `.rule` 파일에 있는 규정을 추가합니다. 예를 들어 동료가 내보낸 규정 묶음을 가져올 수 있습니다. 같은 그룹에 같은 이름의 규정이 이미 있으면 가져오지 않으니, 파일에서 이름을 바꾼 뒤 다시 가져오세요.
- 그룹의 **Export**: 그 그룹의 규정을 `.rule` 파일 하나로 저장해 공유할 수 있습니다. 규정 하나만 내보내려면 그 줄에 마우스를 올리고 **⤓**를 누르세요.
- **Restore built-in rules**: 고친 기본 규정을 모두 원본으로 되돌립니다. 원본은 바뀌지 않으므로 언제든 되돌릴 수 있습니다.
- 직접 만든 규정의 **×**(꺼져 있을 때 보임): 그 규정을 지웁니다.

직접 만든 규정은 SySHIP 사용자 데이터 폴더의 `rules` 폴더에 저장됩니다(Windows는 `%APPDATA%\SySHIP\rules`, macOS는 `~/Library/Application Support/SySHIP/rules`). 그룹마다 폴더가 있고, 규정 하나가 `.rule` 파일 하나입니다(예: `rules/User/My GZ check.rule`, `rules/Owner spec/GM 0.5.rule`). 여러 규정이 든 파일을 가져오면 규정별 파일로 나뉘어 저장됩니다. 기본 규정을 고친 내용은 `rules/_edited-builtins`에 규정마다 따로 저장됩니다. 규정에 마우스를 올리면 해당 파일 위치가 보입니다.

`.rule` 파일은 일반 텍스트입니다. `@`로 시작하는 줄은 규정의 정보이고, 나머지 줄은 바로 위 규정의 스크립트입니다.

```
# SySHIP stability rules
@group Owner spec
@edition Owner specification rev. 2

@rule GM 0.5
@description Initial GM of at least 0.5 m.
@default intact
gm_min = 0.5  # m
check("GM 0.5", GM0 >= gm_min)
```

| 줄 | 의미 |
|---|---|
| `@group` | 목록에 보이는 그룹. 첫 `@rule`보다 앞에 쓰면 파일 전체에 적용됩니다. |
| `@edition` | 툴팁에 보이는 판(edition)이나 출처. |
| `@rule` | 규정의 시작. 줄의 나머지가 규정 이름입니다. |
| `@description` | 툴팁 설명. |
| `@default intact` / `@default damage` | **Reset to default** 때 비손상 또는 손상 케이스에서 켜집니다. |
| `@id` | 내보낸 기본 규정에만 있습니다. 이런 파일을 가져오면 해당 기본 규정의 스크립트를 바꾸며, 언제든 원본으로 되돌릴 수 있습니다. |

### 조건 작성하기

조건 하나는 짧은 스크립트 하나입니다. 예:

```
area_min = 0.055
a = area(GZ, hline(0), vline(0), vline(30))
check("Area 0-30°", a >= area_min)
```

- `GZ`는 기울어지는 쪽을 +로 둔 GZ 곡선(heel은 도, GZ는 m)입니다. `GZ_SIGNED`는 원본 곡선(+ heel = starboard 쪽으로 기울어짐)입니다.
- 계산 결과로 주어지는 값: `EQ_HEEL`(도), `GM0`(m), `KG`(m), `DISP`(t), `DRAFT`(수면 높이, m), `TRIM`(도), `DECK_EDGE_IMMERSED`, `DECK_EDGE_ANGLE`(도), `FLOODED`, `LIST_SIDE`. 편집 화면에 현재 값과 함께 표시됩니다.
- 작도와 측정: `vline`, `hline`, `point`, `seg`, `line`, `tangent`, `at`, `slope`, `intersect`(교점이 정확히 1개여야 함), `root`(처음 0 아래로 내려가는 각), `peak`, `area`(m·rad), `x`, `y`
- 보조 함수: `min`, `max`, `abs`, `deg2rad`, `rad2deg`, `ifna(값, 대체값)`, `ifelse(조건, 참, 거짓)`, 비교 연산과 `and` / `or` / `not`
- `check("이름", 비교식)`은 실제 값과 기준값을 함께 판정 결과로 기록합니다.
- 각도는 도 단위로 씁니다. 면적은 m·rad, 기울기는 m/rad로 나옵니다.

해수 밀도, Heel step, 조건, Curve fit 설정은 프로젝트에 함께 저장됩니다. [명령줄](../cli.md)에서도 바꿀 수 있습니다.

!!! warning "지나치게 가벼운 적하 상태는 결과를 검산하세요"
    선체의 정상적인 배수량 범위를 크게 벗어나는 적하 상태(예: 화물이나 밸러스트 없이 경하중량만 있는 상태)에서는 solver가 물리적으로 비현실적인 결과를 내거나 수렴하지 못할 수 있습니다. 흘수가 지나치게 깊거나 3D 미리보기가 이상하게 기울어 보이면, 결과를 믿기 전에 [Loading](loading.md)의 총중량이 그 선체에 현실적인 값인지 먼저 확인하세요.
