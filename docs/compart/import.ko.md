# Import

![Import 탭, 프로젝트 시작 전](../assets/screenshots/compart-import.png)

**HULL** 탭에서 이미 임포트한 선체를 외피(outer shell)로 삼아 새 COMPART 프로젝트를 시작합니다. 프로젝트가 존재하기 전에는 이 서브탭만 사용할 수 있습니다.

**New from HULL**을 클릭합니다(선체가 아직 임포트되지 않았다면 비활성화되며 안내 문구가 표시됩니다). HULL에 이름이 붙은 형상이 여러 개 있다면 선택 버튼 목록이 나타나고, 하나뿐이라면 바로 진행됩니다.

이 동작은:

- 선택한 선체를 **"Hull"**이라는 이름의 mesh로 임포트합니다(원본 그대로의 사본도 별도로 보관해 [Export](export.md)의 "Export Hull STL"과 [Stability](stability.md)의 손상 범위 참고 표에 사용합니다).
- 선체의 현재 station/waterline/buttock line과 AP/FP 위치, 그리고 주요 제원을 COMPART 프로젝트에 스냅샷으로 저장합니다 — line은 Mesh List의 **Lines** 탭에서 읽기 전용으로 확인할 수 있고, 두 스냅샷 모두 [Modeling](modeling.md) 스크립트가 `$LOA`/`$LBP`/.../`$ST5`/`$WL7` 등으로 참조하는 값입니다.

이미 COMPART 프로젝트가 존재하는 상태에서 **New from HULL**을 클릭하면 먼저 확인을 요청합니다 — 현재 프로젝트의 모든 내용이 삭제되기 때문입니다.

![선체 임포트 후](../assets/screenshots/compart-modeling-box-created.png)
