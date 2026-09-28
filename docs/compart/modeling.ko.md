# Modeling

Modeling은 실제로 구획과 탱크를 만드는 곳입니다 — 형상을 만들고, 결합하고, 자르고, 변형시켜 솔리드로 다룹니다. 내부적으로 **Create**, **Operation**, **Script** 세 개의 탭으로 나뉩니다.

## Create

![Create 패널, 비어있는 상태](../assets/screenshots/compart-modeling-create-empty.png)

드롭다운에서 형상 종류를 선택하면 아래 입력 폼이 그에 맞게 바뀝니다. Box, Cylinder, Plane은 실제로 생성하기 전에 3D View에 **실시간 미리보기**를 보여줍니다:

| 종류 | 필드 | 비고 |
|---|---|---|
| **Box** | Min (x,y,z), Max (x,y,z), 모두 m 단위 | 축에 정렬된 박스입니다. |
| **Cylinder** | Radius (m), Point 1 (x,y,z), Point 2 (x,y,z) | 두 점을 잇는 원기둥입니다. |
| **Plane** | Axis (X/Y/Z), Position (m) | 선택한 축에 수직인 절단 평면으로, 현재 선체의 범위에 맞춰집니다. |
| **Skinsurf** | Num points, Axis, 하나 이상의 **폴리라인** | 2개 이상의 폴리라인을 통과하는 곡면을 로프트합니다. 폴리라인은 이 패널 안에서 직접 입력합니다(아래 참고). |
| **Load** | STL 파일 | 외부 STL 파일을 새로운 구획 형상으로 임포트합니다. |

**Skinsurf**는 더 이상 별도의 polyline mesh가 필요하지 않습니다: **Num points**(단면마다 몇 개의 2D 점을 가질지)와 단면들이 쌓이는 **Axis**를 설정한 뒤, **+ Add polyline**으로 단면을 추가합니다 — 각 단면은 해당 축 위의 **Position (m)**과 (a, b) 점들의 행을 갖습니다. 어떤 폴리라인의 박스를 클릭하면 그 폴리라인이 활성화되어 실시간 미리보기에 표시됩니다. 프로필들은 위치 순서대로 자동 정렬되어 로프트됩니다. 모든 프로필은 점 개수가 같아야 하며, 각각 서로 다른 위치를 가져야 합니다. 직접 입력하는 대신 Mesh List의 **Lines** 탭에 있는 line 값을 이 숫자 필드(Position, 또는 점의 좌표) 위로 드래그해서 채울 수도 있습니다.

![Box 생성 전 미리보기](../assets/screenshots/compart-modeling-create-box-preview.png)

**Create**를 클릭해 형상을 확정하면(Mesh List에 추가됩니다), 또는 **Cancel**로 취소할 수 있습니다.

![선체 + 생성된 Box 구획](../assets/screenshots/compart-modeling-box-created.png)

## Operation

![Operation 패널](../assets/screenshots/compart-modeling-operation.png)

드롭다운에서 연산을 선택하면 나타나는 드롭 박스로 **Mesh List에서 mesh를 드래그해** 채웁니다(여러 개를 선택해 한 번에 드래그할 수도 있습니다):

| 연산 | 드롭 박스 | 효과 |
|---|---|---|
| **Union** | Meshes (2개 이상) | 드롭한 모든 mesh를 하나로 합칩니다. 원본은 제거됩니다. |
| **Intersect** | Meshes (2개 이상) | 드롭한 모든 mesh에 공통된 부피만 남깁니다. |
| **Subtract** | Targets, Cutters | 모든 Target에서 모든 Cutter의 부피를 제거합니다. 두 박스 모두 **watertight**(닫힌) mesh만 받습니다. |
| **Split** | Targets, Cutters | 각 Cutter(닫힌 mesh 또는 Plane)가 지나가는 곳을 기준으로 각 Target을 분할하여 새 조각을 만듭니다. cutter가 완전히 관통하지 않는 target은 분할되지 않습니다. Target은 watertight여야 하며, cutter는 (Plane처럼) 열려 있어도 됩니다. |
| **Transform** | Meshes | 드롭한 모든 mesh에 Translate/Rotate/Scale과, 필요시 Mirror를 적용합니다(아래 참고). watertight가 아닌 mesh도 여기서는 허용됩니다. |

닫힌 mesh가 필요한 연산에 watertight가 아닌 mesh를 드롭하면, 어떤 mesh가 실패했는지 알려주는 오류가 표시되며 거부됩니다.

**Transform**의 필드(mesh를 하나 이상 드롭한 이후):

- **Translate (m)** — dx/dy/dz.
- **Rotate (degrees, about the origin)** — 각 축을 기준으로 회전합니다.
- **Scale (factor, 1 = no change)** — 축별 배율입니다.
- **Mirror** — 지정한 축의 특정 위치를 기준으로 mesh를 반사합니다(m).

연산 버튼(선택한 연산의 이름이 표시됨)을 클릭해 실행하거나, **Cancel**로 드롭 박스를 비우고 취소할 수 있습니다.

개별 mesh의 이름 변경·복제·삭제는 여기서 하지 않습니다 — **Mesh List**에서 하나 이상의 행을 선택한 뒤 우클릭하면 나오는 **Rename / Copy / Delete** 컨텍스트 메뉴를 사용하세요.

## Script

![Script 패널](../assets/screenshots/compart-modeling-script.png)

일괄 작업이나 매개변수화된 구획 모델링을 위해, **Load Script**는 하나의 파일(`.txt` / `.script` / `.dsl`) 안에서 형상 생성, 불리언 결합, 변형, 이름 변경/색상 지정/레이어 정리, 삭제를 모두 처리할 수 있는 파이썬(Python)과 비슷한 문법의 작은 스크립트 언어를 실행합니다. 스크립트를 불러오면 실제로 무엇이 만들어질지 실행 전에 평이한 문장으로 단계별 **미리보기**를 보여줍니다. 내용을 확인한 뒤 **Confirm**으로 실행하거나 **Cancel**로 취소합니다.

각 줄은 함수 호출 같은 일반 표현식이거나 대입문입니다:

- `name = expr`는 결과를 스크립트 안에서만 쓰는 변수에 저장해, 이후 같은 스크립트에서 재사용할 수 있게 합니다.
- `"Some Name" = expr` — **따옴표로 감싼** 대상은 변수를 만드는 대신, 결과 mesh를 프로젝트에서 그 이름 그대로 바꿉니다.
- `[a, b] = split(target, cutter)`처럼 목록을 반환하는 함수(`split()` 등)를 여러 이름/따옴표 이름으로 한 번에 구조 분해할 수 있습니다.

`#`는 그 줄 끝까지 이어지는 주석을 시작합니다.

스크립트는 (**New from HULL**을 실행할 때 밀리미터 단위로 저장된) 현재 선체의 주요 제원을 시스템 변수로 참조할 수 있습니다: `$LOA`, `$LBP`, `$LWL`, `$B`, `$BWL`, `$D`, `$T`. 또한 선체의 station/waterline/buttock line을 이름으로도 참조할 수 있습니다 — `$ST5`, `$WL7`, `$BL3.5` — 해당 line이 저장한 위치 값을 사용합니다. HULL에서 임포트되지 않은 line을 참조하면 어떤 line이 없는지 알려주는 오류가 발생합니다. 사칙연산(`+ - * /`, 괄호)은 모든 숫자 표현식에 사용할 수 있습니다.

**사용 가능한 함수:**

| 함수 | 용도 |
|---|---|
| `point2d(a, b)` / `point3d(x, y, z)` | 2D 또는 3D 좌표를 만듭니다. |
| `box(min, max)` | Create 패널의 Box와 동일하며, 두 개의 `point3d`를 받습니다. |
| `cylinder(radius, p1, p2)` | Create 패널의 Cylinder와 동일합니다. |
| `plane_x(pos)` / `plane_y(pos)` / `plane_z(pos)` | 해당 축에 수직인 절단/기준 평면입니다. |
| `polyline_x(pos, [point2d, ...])` / `polyline_y(...)` / `polyline_z(...)` | 해당 축 위의 한 위치에 단면을 정의합니다. `skinsurf()`에서 사용합니다. |
| `skinsurf([polyline, ...])` | 2개 이상의 폴리라인(같은 축, 같은 점 개수)을 통과하는 곡면을 로프트합니다. |
| `load("path.stl")` | 외부 STL을 임포트합니다. |
| `copy(mesh)` / `copy([mesh, ...])` | mesh 하나 또는 목록을 복제합니다. |
| `union(a, b, ...)` | 2개 이상의 mesh를 결합합니다. |
| `intersect(a, b, ...)` | 모든 mesh에 공통된 부피만 남깁니다. |
| `subtract(targets, cutters)` | targets에서 cutters의 부피를 제거합니다(각 인자는 mesh 하나 또는 목록일 수 있습니다). |
| `split(targets, cutters)` | cutters를 기준으로 targets를 분할하여, target별 결과 조각들을 반환합니다. |
| `translate(meshes, offset)` / `rotate(meshes, angles)` / `scale(meshes, factors)` | `point3d`를 받아 그 자리에서 변형합니다. |
| `mirror_x(meshes, pos)` / `mirror_y(...)` / `mirror_z(...)` | 해당 축의 위치를 기준으로 반사합니다. |
| `rename(mesh(es), name(s))` | mesh 하나 또는 대응하는 목록의 이름을 바꿉니다. |
| `delete(mesh(es))` | mesh 하나 또는 목록을 제거합니다. |
| `set_color(meshes, (r, g, b))` | Mesh List 색상을 설정합니다(채널 값 0–255). |
| `layer("name")` | 레이어를 만들거나(이미 있으면 그 레이어를 반환), `move_layer()`에서 사용합니다. |
| `move_layer(meshes, layer)` / `rename_layer(layer, name)` / `delete_layer(layer)` | Mesh List의 레이어로 mesh를 정리합니다. |

검증된 짧은 예시 — 선체 자체 길이의 일부 크기로 만들어 midship을 중심에 놓고, 생성과 동시에 이름을 붙이는 박스 구획 하나:

```text
# midship을 중심으로 LBP의 1/3 길이인 탱크 하나.
tankLength = $LBP / 3
midX = $LBP / 2

"Tank 1" = box(point3d(midX - tankLength / 2, -$B / 2, 0), point3d(midX + tankLength / 2, $B / 2, $D))
```

스크립트가 파싱되지 않거나 실행에 실패하면, 오류 메시지에 문제가 발생한 줄 번호와 내용이 함께 표시됩니다.

!!! note "내보내기 함수는 없습니다"
    스크립트는 mesh를 만들고 정리하는 역할만 합니다 — 완성된 구획 모델을 내보내려면 스크립트 실행 후 [Export](export.md) 탭을 사용하세요.