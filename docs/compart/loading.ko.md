# Loading

경하중량/무게중심과 각 카테고리 지정 구획의 채움 정도를 조합해 적하 상태(loading condition)를 만듭니다. 여기서 산출되는 총중량과 무게중심을 [Stability](stability.md)가 그대로 사용해 평형을 계산합니다.

![Loading 탭](../assets/screenshots/compart-loading-lightship.png)

## Lightship

- **Weight (t)**과 **CG X/Y/Z (m)**.
- DIMENSION 설계선이 존재하고 무게가 아직 0이라면, **"Use DIMENSION estimate (N t)"** 버튼이 나타납니다 — 클릭 한 번으로 [DIMENSION → Principal Dimensions](../dimension/principal-dimensions.md)의 W<sub>s</sub>+W<sub>o</sub>+W<sub>m</sub> 값을 채울 수 있습니다.

## 구획 적하

카테고리가 지정된 구획([Volume](volume.md)에서 지정)만 적하할 수 있습니다 — 그렇지 않은 구획은 이 탭이 활성화된 동안 Mesh List에서 비활성화(회색)로 표시됩니다. 구획을 선택하면:

- 해당 카테고리와 비중이 읽기 전용으로 표시됩니다.
- **fill percentage** 슬라이더(0~100%, 연동된 숫자 입력란 포함)로 얼마나 채울지 설정합니다.
- 그 채움 정도에서의 **volume (m³) / weight (t)**가 실시간으로 표시됩니다.
- **Apply**를 클릭해 해당 구획의 적하를 확정합니다(항상 탱크 바닥부터 채워집니다).

## Loads 요약

하나 이상의 구획을 적하하면, 읽기 전용 **Loads** 목록에 적하된 각 구획의 이름, 카테고리, 채움 비율, 무게가 표시됩니다. 그 아래에는:

- **Loads weight** — 모든 구획 무게의 합.
- **Total weight (+ lightship)** — Loads weight + Lightship weight. 이 총중량과 그로부터 계산되는 무게중심이 바로 [Stability](stability.md)가 평형 계산 시 사용하는 선박의 무게/무게중심입니다.

적하된 구획은 3D View에서 반투명한 색상 솔리드로 표시됩니다(liquid는 파란색, solid는 갈색, void 카테고리는 회색), 현재 채움 정도에 맞는 액면까지 채워진 형태로 렌더링됩니다.
