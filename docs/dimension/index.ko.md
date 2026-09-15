# DIMENSION

DIMENSION은 새로운 선박 설계가 시작되는 곳입니다. 오너 요구사항과 비교 대상이 되는 유사선(parent ship)의 제원을 입력하면, SySHIP이 신조선의 주요 제원을 산출하고, 건현을 검토하고, 저항과 추진 성능을 예측하고, 엔진을 선정하고, 최적의 제원 비율을 자동으로 탐색해줍니다.

이 탭은 대략 위에서 아래 순서로 작업하도록 되어 있는 6개의 서브탭으로 구성됩니다:

| 서브탭 | 용도 |
|---|---|
| [Requirements](requirements.md) | 오너 요구사항과 parent ship의 제원을 입력합니다 — 이후 모든 계산의 입력값입니다. |
| [Principal Dimensions](principal-dimensions.md) | 신조선의 L/B/D/T/C<sub>B</sub>와 경하중량을 산출합니다. |
| [Freeboard](freeboard.md) | 법정 최소 건현(ICLL 1966, Type A)을 설계값과 대조하여 검토합니다. |
| [Resistance & Propeller](resistance-propeller.md) | Holtrop & Mennen법으로 저항/동력을 예측하고, Wageningen B-계열로 이에 맞는 프로펠러를 설계합니다. |
| [Engine Selection](engine-selection.md) | 엔진 카탈로그를 구축하고, 설계선의 운항점을 엔진의 layout diagram과 대조합니다. |
| [Optimize](optimize.md) | 건조 비용/추진 성능 관점에서 최적의 제원 비율을 자동으로 탐색합니다. |

DIMENSION을 거치지 않고 바로 HULL 탭에서 STL 선체를 임포트해도 무방합니다. 다만 DIMENSION에서 설계선(design ship)이 산출된 상태라면, HULL의 Variation 패널과 COMPART의 Loading 탭에서 그 추천 제원과 경하중량을 한 번의 클릭으로 가져다 쓸 수 있는 버튼이 나타납니다.

!!! note
    DIMENSION의 값들은 각 라벨에 표기된 단위(m, m³, ton, kW, rpm, knots 등)로 직접 입력·표시됩니다 — HULL의 밀리미터 내부 좌표계처럼 별도의 단위 변환 과정이 있는 것은 아닙니다.
