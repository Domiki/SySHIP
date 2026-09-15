# Modeling

Modeling은 실제로 구획과 탱크를 만드는 곳입니다 — 형상을 만들고, 결합하고, 자르고, 변형시켜 솔리드로 다룹니다. 내부적으로 **Create**, **Operation**, **Script** 세 개의 탭으로 나뉩니다.

## Create

![Create 패널, 비어있는 상태](../assets/screenshots/compart-modeling-create-empty.png)

드롭다운에서 형상 종류를 선택하면 아래 입력 폼이 그에 맞게 바뀝니다. 모든 형상은 실제로 생성하기 전에 3D View에 **실시간 미리보기**가 표시됩니다:

| 종류 | 필드 | 비고 |
|---|---|---|
| **Box** | Min (x,y,z), Max (x,y,z), 모두 m 단위 | 축에 정렬된 박스입니다. |
| **Cylinder** | Radius (m), Point 1 (x,y,z), Point 2 (x,y,z) | 두 점을 잇는 원기둥입니다. |
| **Plane** | Axis (X/Y/Z), Position (m) | 선택한 축에 수직인 절단 평면으로, 현재 구획 그리드 크기에 맞춰집니다. |
| **Polyline** | Axis, Position (m), 2D (a, b) 점 목록 | 고정된 축 위치에 있는 이름/색상이 지정된 2D 외곽선입니다 — **Skinsurf**의 재료가 됩니다. 점은 순서 변경(▲/▼), 삽입, 삭제가 가능합니다. **Add Polyline**을 클릭하면 저장됩니다(솔리드가 아니라 Mesh List의 **Line** 섹션에 나타납니다). |
| **Skinsurf** | (직접 입력하는 필드 없음) | 기존 polyline **2개 이상**을 통과하는 곡면을 로프트합니다. Mesh List의 Line 섹션에서 로프트 순서대로 클릭하며, 선택한 polyline들은 축과 점 개수가 같아야 합니다. |
| **Load** | STL 파일 | 외부 STL 파일을 새로운 구획 형상으로 임포트합니다. |

![Box 생성 전 미리보기](../assets/screenshots/compart-modeling-create-box-preview.png)

**Create**를 클릭해 형상을 확정하면(Mesh List에 추가됩니다), 또는 **Cancel**로 취소할 수 있습니다.

![선체 + 생성된 Box 구획](../assets/screenshots/compart-modeling-box-created.png)

## Operation

![Operation 패널](../assets/screenshots/compart-modeling-operation.png)

드롭다운에서 연산을 선택한 뒤, 화면의 안내에 따라 (3D View가 아니라) **Mesh List**에서 요소를 클릭해 "슬롯"을 채웁니다:

| 연산 | 슬롯 | 효과 |
|---|---|---|
| **Union** | Mesh 1, Mesh 2 | 두 mesh를 하나로 합칩니다. 원본은 제거됩니다. |
| **Intersect** | Mesh 1, Mesh 2 | 겹치는 부피만 남깁니다. |
| **Subtract** | Target, Tool | Target에서 Tool의 부피를 제거합니다. |
| **Split (Mesh)** | Target, Cutting tool | 커팅 mesh가 지나가는 곳을 기준으로 Target을 분할하여 여러 개의 새 mesh를 만듭니다. |
| **Split (Plane)** | Target, Cutting plane | 위와 동일하지만 커터로 Plane mesh를 사용합니다. |
| **Copy** | Element | 선택한 mesh를 복제합니다. |
| **Delete** | Element | 선택한 mesh, plane, polyline을 삭제합니다. |
| **Transform** | Element | 선택한 mesh에 Translate/Rotate/Scale과, 필요시 mirror를 적용합니다(아래 참고). |

**Transform**의 필드(mesh를 선택한 이후):

- **Translate (m)** — dx/dy/dz.
- **Rotate (degrees, about the origin)** — 각 축을 기준으로 회전합니다.
- **Scale (factor, 1 = no change)** — 축별 배율입니다.
- **Also mirror** — 지정한 축의 특정 위치를 기준으로 mesh를 반사합니다(m).

연산 버튼(선택한 연산의 이름이 표시됨)을 클릭해 실행하거나, **Cancel**로 선택한 슬롯을 지우고 취소할 수 있습니다.

## Script

![Script 패널](../assets/screenshots/compart-modeling-script.png)

일괄 작업이나 매개변수화된 구획 모델링을 위해, **Load Script**는 하나의 파일(`.txt` / `.script` / `.dsl`) 안에서 형상 생성, 불리언 결합, 변형, 결과 내보내기를 모두 처리할 수 있는 작은 텍스트 기반 스크립트 언어를 실행합니다. 스크립트를 불러오면 실제로 무엇이 만들어질지 실행 전에 평이한 문장으로 단계별 **미리보기**를 보여줍니다. 내용을 확인한 뒤 **Confirm**으로 실행하거나(또는 **Cancel**로 취소).

스크립트는 현재 선체의 주요 제원을 내장 변수로 참조할 수 있습니다 — `$LOA`, `$LBP`, `$LWL`, `$BEAM`, `$BWL`, `$DEPTH`, `$DRAFT` — 이를 활용하면 한 선체를 위해 작성한 스크립트를 크기가 다른 선체에도 재사용할 수 있습니다.

**사용 가능한 구문** (한 줄에 하나씩; `//`는 주석):

| 구문 | 문법 | 용도 |
|---|---|---|
| 변수 | `var name = expr` | 이후 표현식에서 재사용할 수 있는 이름 붙은 숫자를 정의합니다. |
| Split | `xsplit name = expr` (또는 `ysplit`/`zsplit`) | 어떤 축을 따라 하나의 절단 위치에 이름을 붙입니다. 여러 줄을 추가해 여러 개의 이름 붙은 station을 만듭니다. |
| Grid | `grid min (x,y,z) max (x,y,z)` | split/cell이 적용될 전체 바운딩 그리드를 설정합니다. |
| Box | `box name = min (x,y,z) max (x,y,z)` | Create 패널의 Box와 동일합니다. |
| Cylinder | `cylinder name = radius (r) p1 (x,y,z) p2 (x,y,z)` | Create 패널의 Cylinder와 동일합니다. |
| Plane | `plane name = origin (x,y,z) normal (x,y,z) size (s)` | 절단/기준 평면입니다. |
| Skinsurf | `skinsurf name = direction (x) profile(depth(d), points[(a,b), ...]) profile(...)` (2개 이상) | 이름 붙은 profile들을 통과하는 곡면을 로프트합니다. |
| Union | `union name = a + b [+ c ...]` | 2개 이상의 이름 붙은 형상을 결합합니다. |
| Intersect | `intersect name = a & b` | 두 형상의 겹치는 부분만 남깁니다. |
| Subtract | `subtract name = target - tool` | `target`에서 `tool`의 부피를 제거합니다. |
| Split by mesh | `split (inside, outside) = target by tool` | `target`을 다른 mesh로 분할하고, 결과 두 조각에 이름을 붙입니다. |
| Split by plane | `split (inside, outside) = target by plane origin (x,y,z) normal (x,y,z) size (s)` | 위와 동일하지만 이름 붙은 mesh 대신 인라인 평면을 사용합니다. |
| Transform | `translate name (dx,dy,dz)` / `rotate name (rx,ry,rz)` / `scale name (sx,sy,sz)` | 이름 붙은 형상을 그 자리에서 변형합니다. |
| Mirror | `mirror name axis = y at = expr` | 이름 붙은 형상을 특정 축의 위치 기준으로 반사합니다. |
| Copy | `copy name = source` | 이름 붙은 형상을 복제합니다. |
| Delete | `delete name` | 이름 붙은 형상을 제거합니다. |
| Cell | `cell "name" = between(x: lo..hi, y: lo..hi, z: lo..hi)` | split 그리드의 한 칸에 해당하는 박스를 만듭니다 — 각 축은 선택 사항이며(생략하면 그리드 전체 범위), `lo`/`hi`는 split 이름(또는 리터럴 `min`/`max`)입니다. |
| Cells | `cells "name" = chain axis = x [s0, s1, s2, ...] between(y: ..., z: ...)` | 한 축을 따라 연속된 split 쌍 사이마다 인접한 셀들의 체인을 만듭니다. |
| Clip | `clip [name, ...] to hullName` | 나열한 형상들을 선체 mesh 내부로 잘라냅니다. |
| Export | `export "filename.stl" = [name, ...]` | 나열한 형상들을 하나의 STL로 병합하여, 스크립트 실행 시 다운로드합니다. |

검증된 짧은 예시 — 선체 자체 길이의 일부 크기로 만든 박스 구획 하나를 내보내는 스크립트:

```text
// midship을 중심으로 LBP의 1/3 길이인 탱크 하나.
var tankLength = $LBP / 3
var midX = $LBP / 2

box tank = min (midX - tankLength / 2, -$BEAM / 2, 0) max (midX + tankLength / 2, $BEAM / 2, $DEPTH)

export "tank.stl" = [tank]
```

스크립트가 파싱되지 않거나 실행에 실패하면, 오류 메시지에 문제가 발생한 줄 번호와 내용이 함께 표시됩니다.
