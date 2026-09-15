# COMPART

COMPART는 임포트한 선체 내부에 구획과 탱크를 만들고, 그 결과에 대해 정수력학, 탱크 용적, 적하 상태(loading condition), 손상/침수 케이스, 복원성(GZ 곡선) 계산을 수행합니다.

![COMPART Import 탭, 빈 프로젝트](../assets/screenshots/compart-import.png)

## 서브탭

| 서브탭 | 용도 |
|---|---|
| [Import](import.md) | HULL 탭에서 임포트한 선체로 새 COMPART 프로젝트를 시작합니다. |
| [Modeling](modeling.md) | 구획/탱크 솔리드를 만듭니다(박스, 실린더, 평면, 곡면 로프트, 불리언 연산, 또는 일괄 스크립트). |
| [Hydro](hydro.md) | draft/trim/heel 스윕에 걸친 정수력학 제원을 계산합니다. |
| [Volume](volume.md) | 구획에 화물 카테고리를 지정하고, sounding height별 용적표를 계산합니다. |
| [Loading](loading.md) | 경하중량/무게중심을 설정하고 구획을 채워 적하 상태를 만듭니다. |
| [Damage](damage.md) | 손상 복원성 케이스를 위해 침수될 구획을 지정합니다. |
| [Stability](stability.md) | 평형 heel/trim과 복원정(GZ) 곡선을 계산합니다. |
| [Export](export.md) | 모델링한 구획을 하나로 병합된 STL로 내보냅니다. |

Import를 제외한 모든 서브탭은 프로젝트가 존재해야(즉, "New from HULL"을 한 번 실행해야) 활성화됩니다.

## 공통 UI

다음 요소들은 모든 COMPART 서브탭에서 공통으로 표시됩니다:

- **View toolbar**(3D 뷰포트 위): 카메라 프리셋 **Top / Front / Right / Iso**, **Reset View**(⟲) 버튼, **Persp/Ortho** 투영 전환, **Edge / Mesh / Both** 렌더 모드 버튼.
- **Mesh List**(오른쪽 패널, **Lines** 탭과 함께 표시): 프로젝트 안의 모든 mesh, plane, polyline을 나열하며 각각 이름 편집, 색상 스와치, 표시/숨김 버튼이 있습니다. 행을 클릭하면 선택되는데, 이것이 Modeling, Volume, Loading, Damage가 *어느* 구획에 대해 동작할지를 지정하는 방법입니다. **Lines** 탭은 (임포트 시점의) 선체 station/waterline/buttock line을 읽기 전용으로 보여주며, 하나를 클릭하면 절단 평면처럼 미리보기가 표시되어 모델링 시 가이드로 활용할 수 있습니다.
- **하단 고정 바** — Save / Load / Save As / Undo / Redo로, HULL의 프로젝트 액션 바와 동일한 방식입니다. COMPART 프로젝트는 HULL 프로젝트 파일과 별개로 `.zip` 파일로 저장됩니다.

COMPART 내부적으로 모든 위치는 밀리미터로 저장되고 화면에는 미터로 표시됩니다. 용적은 m³, 면적은 m², 무게는 톤, 밀도는 t/m³ 단위입니다.
