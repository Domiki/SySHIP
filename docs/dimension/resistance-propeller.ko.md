# Resistance & Propeller

![Resistance & Propeller 탭](../assets/screenshots/dimension-resistance-propeller.png)

**Holtrop & Mennen(1984)** 법으로 정수 중 저항과 유효마력을 예측하고, **Wageningen B-계열** 단독성능 K<sub>T</sub>/K<sub>Q</sub> 회귀식을 이용해 이에 맞는 프로펠러를 설계합니다.

두 섹션 모두 [Principal Dimensions](principal-dimensions.md)에서 design ship이 먼저 산출되어 있어야 합니다(그렇지 않으면 안내 문구가 표시됩니다).

## Resistance (Holtrop & Mennen)

**Speed min / max / step (kn)**을 설정합니다(기본값 10 / 20 / 0.5). 입력을 멈춘 뒤 약 300ms 후 저항 곡선이 자동으로 다시 계산되며, 별도의 계산 버튼은 없습니다.

- **차트: Total resistance (kN) vs. speed**
- **차트: Effective power EHP (kW) vs. speed** (EHP = 저항 × 속도)

## Propeller Design (Wageningen B-series)

**Design Propeller**를 클릭하면 3단계 절차가 실행됩니다:

1. **Stage 1** — 프로펠러 지름 D<sub>p</sub>를 Requirements의 값으로 고정한 채 pitch ratio를 스윕하고, thrust identity로부터 전진계수 J를 구해 단독효율 η가 최대가 되는 pitch를 선택합니다.
2. **Stage 2** — D<sub>p</sub>도 자유변수로 두고, torque identity를 목표 동력/rpm(가능하면 [Engine Selection](engine-selection.md)에서 선정한 엔진, 아직 없다면 Stage 1의 DHP→BHP→NCR 체인에 sea margin을 적용한 값)에 맞춰 풉니다.
3. **Stage 3** — 최종 D<sub>p</sub>/pitch에서 10~20kn 구간을 스윕하여 BHP/RPM 운항 곡선을 만듭니다.

**결과:** Stage 1 η와 pitch ratio; Stage 2(선정)의 D<sub>p</sub>(m)와 pitch(m, = pitch ratio × D<sub>p</sub>); Stage 2 η.

- **차트: BHP (PS) vs. speed**
- **차트: Engine RPM vs. speed**

이렇게 산출된 운항점 체인(DHP → BHP → NCR → MCR)이 [Engine Selection](engine-selection.md)에서 엔진의 layout diagram과 대조하는 데 사용됩니다.
