# Variation

Variation은 (Fairing의 국부적인 정점/면 편집과 달리) 선체 *전체*를 새로운 목표 제원이나 계수에 맞춰 형상을 유지한 채로 다시 만들어냅니다. **Variation type** 드롭다운에서 종류를 선택하면 그 아래 패널이 그에 맞게 바뀝니다.

![Protection이 펼쳐진 Length Variation 패널](../assets/screenshots/hull-variation-protection.png)

전역 제원을 바꾸는 모든 Variation 패널은 선체에서 읽어온 **현재(current)** 값과 직접 입력하는 **목표(target)** 값을 함께 보여주며, 굵은 글씨의 **Change &lt;X&gt;** 제목이 붙습니다. DIMENSION 탭에서 설계선이 산출되어 있으면 한 번의 클릭으로 그 값을 가져오는 **"Reset to DIMENSION recommendation"** 버튼도 나타납니다.

## Change Length

- **Current LBP**(읽기 전용) vs. **Target LBP (m)**.
- **Reference (fixed point)** — 목표 길이에 맞춰 선체가 늘어나거나 줄어드는 동안 어느 지점을 고정할지 선택합니다: **A.P. (stern)**, **F.P. (bow)**, **Midship**, 또는 **Custom position (m)**.

## Change Breadth

- **Current B**(읽기 전용, 선체 Y 범위를 미러링한 값) vs. **Target B (m)**.

## Change Depth

- **Current D**(읽기 전용, 선체 Z 범위 기준) vs. **Target D (m)**.

## Change Draft

- **Current T<sub>d</sub>** vs. **Target T<sub>d</sub> (m)**. 이를 적용하면 새 흘수에서 정수력학만 다시 계산할 뿐, 선체 형상 자체는 바뀌지 않습니다.

## Change LCB/C<sub>B</sub>

![Cp Variation 패널](../assets/screenshots/hull-variation-cp.png)

Lackenby 방식의 변환을 이용해 프리즈매틱 곡선을 이동시켜, 선체가 목표 방형계수와/또는 부심종방향위치에 도달하도록 만듭니다:

- **Current C<sub>B</sub>** vs. **Target C<sub>B</sub>**(가능하면 **Reset to DIMENSION recommendation** 버튼도 표시됩니다).
- **Current LCB from midship (m)** vs. **Target LCB from midship (m)**.
- **Start X (m)** / **End X (m)** — *active zone*: 변환이 선체 형상을 바꿀 수 있도록 허용되는 X 범위(AP 기준)입니다. 이 구간 밖의 선체는 그대로 유지됩니다.
- **Smoothing range (m)** — active zone 경계에서 변환되지 않은 영역으로 급격하게 끊기지 않고 부드럽게 이어지도록 블렌딩합니다.

## Protection (Length / Breadth / Depth Variation)

Length, Breadth, Depth 패널에서는 변형되는 나머지 부분과 달리 형상을 그대로 유지할 **protection box**(보호 영역)를 정의할 수 있습니다:

- **Add protection** 체크박스를 켜면 박스 목록이 나타납니다.
- **+ Add box**로 **Min/Max X/Y/Z (m)**로 정의되는 박스를 추가합니다. 각 박스에는 개별 활성화 체크박스와 **Remove** 버튼이 있습니다.
- **Smoothing range (m)** — protection box 바깥으로 변형이 얼마나 멀리서부터 다시 원래대로 섞여 들어가는지를 결정합니다(박스 벽에서 갑자기 끊기지 않도록).
- 아래의 **falloff curve** 편집기는 이 블렌딩이 정확히 어떻게 감쇠하는지를, box 가장자리로부터의 정규화된 거리(X축)에서 완전한 변형(Y축)까지로 형상화합니다:
  - 점이나 그 접선 핸들을 드래그해 곡선 형태를 바꿉니다.
  - 곡선의 빈 공간을 더블클릭하면 새 점이 추가되고, 기존 점(끝점 제외)을 더블클릭하면 제거됩니다.
- **Rigid follow** — 체크하면 protection box 내부의 형상이 고정된 위치에 머무르는 대신 주변 변형을 따라 강체처럼 이동/회전합니다.

## FFD (Free Form Deformation)

![펼쳐진 FFD 패널](../assets/screenshots/hull-variation-ffd-expanded.png)

FFD는 변형 격자(lattice)를 이용해 국부 변형을 완전히 수동으로 제어할 수 있게 해줍니다 — 3D 공간에서 잡아당길 수 있는 8개의 코너 핸들을 가진 바운딩 박스입니다:

1. **Bounding box (m)** — **Min X/Y/Z**와 **Max X/Y/Z**를 직접 입력하거나, **Fit to hull bounds**를 클릭해 박스를 현재 선체 범위에 맞춥니다. **Reset bounding box**로 초기화할 수 있습니다.
2. 박스가 설정되면 3D View에 8개의 코너 핸들이 색이 입혀진 점으로 나타납니다. 하나를 클릭하면(어느 팔분공간에 있는지, 예: "X+ Y- Z+"로 라벨이 붙습니다) 선택되며, 이후 다음 중 하나를 할 수 있습니다:
   - 3D View에서 직접 드래그(실시간 dX/dY/dZ 툴팁이 커서를 따라다닙니다), 또는
   - 패널에서 **Displacement (dx/dy/dz, m)** 또는 절대 위치인 **Target position (x/y/z, m)**을 직접 입력.
   - **Reset corner displacement**로 해당 코너만 0으로 초기화.
3. **Smoothing range (m)**과 Protection과 동일한 falloff curve 편집기로 변형이 lattice box 바깥으로 어떻게 감쇠하는지 조절합니다.
4. **Apply**를 클릭하면 변형이 선체 표면에 반영됩니다. 적용 전, 코너를 드래그하는 동안 예상되는 형상이 3D View에 실시간 와이어프레임 미리보기로 표시됩니다.

패널 헤더의 **&gt;** 화살표로 FFD 섹션 전체를 접고 펼 수 있습니다.

## Undo / Redo와 Protection

모든 Variation "Apply"는 Fairing 편집과 동일한 공유 undo/redo 이력의 한 단계입니다 — 패널 하단(또는 하단 고정 바 아이콘)의 **Undo/Redo** 버튼으로 변형 체인을 되돌릴 수 있습니다.
