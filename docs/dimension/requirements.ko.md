# Requirements

![Requirements 탭](../assets/screenshots/dimension-requirements.png)

Requirements에서는 오너 요구사항과, DIMENSION 전체의 경험적 스케일링·계수 산정 기준이 되는 **parent ship**(비교 대상이 되는 기존 유사선)의 상세 제원을 입력합니다. 여기서 parent ship이 설정되기 전까지는 DIMENSION의 나머지 기능이 동작하지 않습니다.

## Load from JSON file

`ownerRequirements`와/또는 `parentShip` 키를 담은 JSON 파일로 이 페이지의 일부 또는 전체를 한 번에 불러올 수 있습니다(값을 일일이 입력할 필요 없이). 불러온 값은 즉시 반영되며, 이후에도 각 필드를 자유롭게 수정할 수 있습니다.

## Owner's Requirements

| 필드 | 단위 | 설명 |
|---|---|---|
| T<sub>d</sub> – design draft | m | |
| T<sub>s</sub> – scantling draft | m | |
| Required DWT at T<sub>d</sub> | ton | |
| Required DWT at T<sub>s</sub> | ton | |
| Required cargo hold capacity | m³ | |
| Service speed at NCR | knots | |
| Cruising range | N/M | |
| Engine margin | 비율 | NCR/MCR 비율. 예: 0.9는 NCR과 MCR 사이 10% 마진을 의미. |
| Sea margin | % | |
| T<sub>max</sub> – draft limit | m | 선택 입력. 제한이 없으면 비워두거나 0으로 둡니다. |

**Carrier type** — Auto-detect from cargo density, Deadweight Carrier, Volume Carrier 중 선택합니다. 라벨 옆에 계산된 화물 밀도(DWT@T<sub>d</sub> ÷ cargo hold capacity)와 자동 판정 결과가 실시간으로 표시됩니다(기준값 0.77 ton/m³ — 탱커·벌커 등은 이보다 높은 deadweight carrier, 컨테이너선·여객선 등은 이보다 낮은 volume carrier).

**Weight estimation method** — parent ship으로부터 경하중량(LWT)을 어떤 방식으로 스케일링할지 선택합니다: Component(W<sub>s</sub> + W<sub>o</sub> + W<sub>m</sub>, parent 비율 기준), DWT-proportional, Volume-proportional(L×B×D).

**Building Cost Model** — Structure, Outfit, Machinery 각각의 $/ton 단가로, Principal Dimensions와 Optimize에서 건조 비용 추정에 사용됩니다.

## Parent Ship – Principal Particulars

LOA, LBP, B molded, D molded, T<sub>d</sub>, T<sub>s</sub>(모두 m), T<sub>d</sub>에서의 C<sub>B</sub>, 그리고 parent ship 자체의 DWT at T<sub>d</sub>/T<sub>s</sub>와 cargo hold capacity.

## Main Engine and Speed

M/E type(텍스트), Nominal max power(kW)/speed(rpm), MCR power/speed, NCR power/speed, SFOC(g/bhp-h).

## Lightweight

parent ship 자체의 실제 중량 내역입니다: Structural weight W<sub>s</sub>, Outfit weight W<sub>o</sub>, Machinery weight W<sub>m</sub>(모두 ton) — Principal Dimensions에서 사용하는 경험적 중량계수 스케일링의 기준값이 됩니다.

## Resistance / Hull-form Detail

L<sub>WL</sub>, C<sub>M</sub>, C<sub>WP</sub>, C<sub>P</sub>, bulb area A<sub>BT</sub>(m²), lcb(%L<sub>WL</sub>, 선수 방향 +), rudder area(m²), bilge keel area(m²), bulb centroid height h<sub>B</sub>(m), immersed transom area(m²), 그리고 **Stern shape** 드롭다운(Pram / V-shaped / Normal / U-shaped). 이 값들은 [Resistance & Propeller](resistance-propeller.md)의 Holtrop & Mennen 저항 예측에 사용됩니다.

## Propulsion Factors

Transmission efficiency, relative rotative efficiency(η<sub>R</sub>), wake fraction(w), thrust deduction(t) — 프로펠러 설계에 사용되는 선체-프로펠러 상호작용 계수입니다.

## Propeller

Diameter D<sub>p</sub>(m), pitch ratio(P/D<sub>p</sub>), number of blades, expanded area ratio(A<sub>E</sub>/A<sub>O</sub>), shaft center height(m).

## Freeboard

Deck type, 상부구조물 길이/높이, forecastle 높이, AP/FP에서의 sheer(mm), freeboard deck thickness(m), L<sub>f</sub>/2 전방 waterplane area(m²), 그리고 세 개의 체크박스 — **Forecastle**, **Poop**, **Trunk** — 이 값들은 [Freeboard](freeboard.md)에서 계산되는 법정 건현 공제값에 영향을 줍니다.

## 저장

**Save Requirements & Parent Ship**을 클릭하면 이 페이지의 모든 내용이 저장됩니다. 필드 단위의 별도 유효성 검사는 없으며, 잘못된 값은 이후 Solve나 다른 계산이 실패할 때 비로소 오류 메시지로 드러납니다.
