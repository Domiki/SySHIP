# Hydro

draft/trim/heel을 스윕하면서 배수량, 부심/부양중심, 메타센터 높이, 형상계수 등 정수력학 제원을 격자 형태로 계산합니다. 이는 전형적인 선박의 "정수력학표/곡선"이지만, HULL 탭의 Properties 패널(section line으로부터 단일 흘수 값을 계산)과 달리 실제 COMPART 선체 mesh 위에서 직접 계산됩니다.

선체 mesh와 그 AP/FP(HULL에서 **New from HULL** 실행 시 자동으로 가져옵니다)가 필요합니다. HULL에서 AP/FP를 설정하기 전에 임포트했다면, HULL에서 설정한 뒤 다시 임포트하라는 안내가 표시됩니다.

## 스윕 설정

- **Draft**, **Trim**, **Heel** — 각각 **start / step / end** 범위입니다(Draft와 Trim은 m, Heel은 도(degree)). 흘수만 신경 쓴다면 Trim과 Heel은 기본값 0/0/0(직립, 등흘수)으로 두면 됩니다.
- **Values to compute** — 계산 가능한 모든 항목의 체크박스 목록입니다: Volume(moulded/extreme), Displacement(moulded/extreme), LCB(전체/전반부/후반부), LCF, VCB, TCB, 횡/종 관성모멘트(I<sub>T</sub>/I<sub>L</sub>), BM<sub>T</sub>/KM<sub>T</sub>, BM<sub>L</sub>/KM<sub>L</sub>, MTC, TPC, WSA, A<sub>WP</sub>, A<sub>M</sub>, C<sub>B</sub>/C<sub>WP</sub>/C<sub>M</sub>/C<sub>P</sub>, Trim. 기본적으로 합리적인 항목들이 미리 체크되어 있습니다.

![스윕을 설정한 Hydro 탭](../assets/screenshots/compart-hydro-form.png)

**Calculate**를 클릭합니다.

![계산 후 Hydro 탭](../assets/screenshots/compart-hydro-calculated.png)

## 결과 보기

**Show**를 클릭하면 별도의 결과 창이 열립니다:

![정수력학 결과 창 (표 보기)](../assets/screenshots/compart-hydro-popup.png)

- 상단에 **Table / Curve** 전환 버튼이 있습니다.
- **Trim** / **Heel** 드롭다운으로 스윕 중 어느 슬라이스를 표시할지 선택합니다.
- **Table 뷰**: draft 값마다 한 행, 선택한 각 항목이 단위와 함께 한 열로 표시됩니다. **Export CSV** 버튼은 (현재 표시된 슬라이스뿐 아니라) 전체 격자를 다운로드합니다.
- **Curve 뷰**: 선택한 각 항목이 draft를 기준으로 곡선으로 그려집니다(세로축이 draft, 아래에서 위로 증가). 오른쪽 범례에서 각 계열을 켜고 끄거나, 색을 바꾸거나, 숫자 **scale**과 **offset**을 적용해 크기 차이가 큰 곡선들(예: TPC와 LCB)도 한 차트에서 함께 읽기 좋게 만들 수 있습니다. **Export Image**는 차트를 PNG로 저장합니다.
