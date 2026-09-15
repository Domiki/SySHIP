# Principal Dimensions

![Principal Dimensions 탭](../assets/screenshots/dimension-principal-dimensions.png)

**Solve**를 클릭하면 [Requirements](requirements.md)에서 입력한 Owner's Requirements와 Parent Ship을 바탕으로 신조선의 주요 제원을 계산합니다. 이 계산은 **Weight Equation**(배수량 = 경하중량 + 재화중량)과 **Volume Equation**(화물창 용적)을 동시에 만족시켜, DIMENSION의 다른 서브탭들과 HULL의 Variation 패널, COMPART의 Loading 탭이 참조하는 "Design Ship"을 산출합니다.

Parent Ship이 설정되기 전에는 버튼이 비활성화되며 "Set a parent ship in the Requirements tab first." 라는 안내가 표시됩니다.

## Solve가 하는 일

- **Carrier type**(deadweight vs. volume)을 Requirements 탭의 설정 또는 자동 판정에 따라 결정합니다.
- **Deadweight carrier**(탱커, 벌커 등): L/B와 C<sub>B</sub>를 parent의 비율로 고정한 뒤, Volume Equation이 암시하는 D와 Weight Equation이 동시에 성립하도록 L을 수치적으로 구합니다.
- **Volume carrier**(컨테이너선 등): L/B와 B/D를 parent 비율로 유지한 채 화물창 Volume Equation으로부터 L, B, D를 직접 구하고, Weight Equation으로부터 C<sub>B</sub>를 closed form으로 계산합니다.
- bulb 면적, transom 면적, 상부구조물 치수 등 모든 선형/건현 세부값을 새 L/B/D/T 비율에 맞춰 parent로부터 비례 스케일링합니다.
- 기존에 계산되어 있던 Freeboard, Resistance curve, Propeller 결과는 모두 지워집니다 — design ship에 의존하는 값들이라 더 이상 유효하지 않기 때문입니다.

## 결과

- **Carrier type / Weight method** — 실제로 어떤 방식이 사용되었는지 표시합니다.
- **Principal Particulars**(읽기 전용): LOA, LBP, B molded, D molded, T<sub>d</sub>, T<sub>s</sub>(m), T<sub>d</sub>에서의 C<sub>B</sub>, Displacement(ton).
- **Lightweight**(읽기 전용): Structural W<sub>s</sub>, Outfit W<sub>o</sub>, Machinery W<sub>m</sub>(ton), LWT 합계, 그리고 Requirements의 비용 모델로 계산한 Building cost.
- **Equation Closure**(읽기 전용): Weight Equation error(%)와 Volume Equation error(%) — 수치 solver가 두 방정식을 얼마나 정확히 닫았는지를 보여주는 자체 검증값입니다. 두 값 모두 거의 0이어야 정상입니다.

Solve가 끝나면 산출된 LBP/B/D/C<sub>B</sub>가 HULL의 Length/Breadth/Depth/C<sub>P</sub> Variation 패널에서 "DIMENSION recommended" 값으로 제공되고, 경하중량 합계(W<sub>s</sub>+W<sub>o</sub>+W<sub>m</sub>)는 COMPART Loading 탭의 원클릭 경하중량 추정값이 됩니다.
