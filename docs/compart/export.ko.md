# Export

![Export 탭](../assets/screenshots/compart-export.png)

서로 독립적인 두 가지 다운로드가 있습니다.

## Export Hull STL

HULL에서 임포트된 그대로의 선체 — 구획이나 Modeling 편집이 전혀 적용되지 않은 원본 watertight 외피 — 를 `<선체 이름>.stl`로 내려받습니다. 내부에 무엇을 만들었는지와 무관하게 외피 표면만 따로 필요할 때 유용합니다.

## Export STL

모델링한 모든 구획을 **하나로 병합된 STL**(`compart_merged.stl`)로 내보냅니다 — SySHIP 밖에서 전체 구획 모델을 열어볼 때(3D 프린팅, CAD, CFD 등) 유용합니다. 인접한 구획끼리 공유하는 벽은 두 개의 겹친 면이 아니라 하나의 판으로 용접·병합되며, 성공 메시지에 몇 개의 공유 벽이 병합되었는지 표시됩니다.

이 기능은 항상 모든 구획을 함께 병합합니다 — 아직은 구획 하나만 따로 내보내는 버튼은 없습니다.