# Engine Selection

![Engine Selection 탭](../assets/screenshots/dimension-engine-selection.png)

엔진 제작사 카탈로그에 실리는 4-corner 출력/rpm **layout diagram**으로 후보 엔진 카탈로그를 구축(또는 임포트)하고, 설계선의 요구 운항점이 선택한 엔진의 layout diagram 안에 들어오는지 확인한 뒤, 보간된 연료소비율을 확인합니다.

## Engine Catalog

- `.csv`, `.xlsx`, `.xlsm`, `.json` 형식의 카탈로그 파일을 파일 선택창을 통해 임포트할 수 있습니다. 기본 제공되는 카탈로그는 없으며, 직접 준비해야 합니다.
- 카탈로그 표에는 임포트된 각 엔진의 Type, Cylinders, 네 개의 코너점 **L1–L4**(kW @ rpm)가 표시됩니다. 각 행의 작은 **×** 버튼으로 해당 엔진을 삭제할 수 있습니다.
- 또는 직접 입력할 수도 있습니다: **Engine type**, **Cylinders**, 그리고 **L1/L2/L3/L4** 각각을 kW / rpm / SFOC 세 값으로 입력한 뒤 **+ Add Engine**을 클릭합니다.

## Select Engine

1. **Select an engine...** 드롭다운에서 엔진을 선택합니다.
2. **Check Layout**을 클릭합니다(엔진 선택과 [Principal Dimensions](principal-dimensions.md)의 design ship이 모두 필요합니다).

이 계산은 선박의 운항점 — 가능하면 [Resistance & Propeller](resistance-propeller.md)의 프로펠러 설계에서 나온 MCR 점, Design Propeller를 아직 실행하지 않았다면 parent ship의 고정 NCR값을 대체값으로 사용 — 을 기준으로:

- 그 점이 엔진의 4-corner layout 다각형 **내부**에 있는지 확인합니다.
- 네 코너값으로부터 **SFOC**를 (역거리 가중) 보간합니다.

**결과:** 엔진 이름, 운항점("X kW @ Y rpm"), 보간된 SFOC, 그리고 **Yes**(layout 내부) 또는 **No — pick a different engine or cylinder count** 상태 배지.

## Engine layout chart

엔진의 4-corner layout 다각형(출력 vs. rpm)을 닫힌 사각형으로 표시하고, 그 위에 운항점 체인(DHP → BHP → NCR → MCR)을 라벨이 붙은 선으로 겹쳐 그립니다 — 운항선이 엔진의 허용 범위 안에 있는지 시각적으로 확인하는 전형적인 "layout diagram" 점검입니다.
