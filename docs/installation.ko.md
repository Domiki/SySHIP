# 설치

## Windows

1. [Releases](https://github.com/Domiki/SySHIP/releases) 페이지에서 `SySHIP-Setup-<version>.exe`를 다운로드합니다.
2. 설치 파일을 실행합니다. 원클릭 설치가 아니라 설치 경로를 확인(또는 변경)하도록 되어 있으며, 바탕화면 바로가기가 자동으로 생성됩니다.
3. 바탕화면 바로가기나 시작 메뉴에서 SySHIP을 실행합니다.

첫 실행 시 Windows가 프로그램을 차단하거나 파일을 격리 조치한다면, 아직 코드 서명이 되어 있지 않아 발생하는 Windows Defender 및/또는 Smart App Control의 오탐입니다 — 두 단계로 해결하는 방법은 [FAQ](faq.md)를 참고하세요.

## macOS

1. [Releases](https://github.com/Domiki/SySHIP/releases) 페이지에서 `SySHIP-<version>.dmg`를 다운로드합니다.
2. `.dmg` 파일을 열고 **SySHIP**을 옆에 표시된 **Applications** 폴더로 드래그합니다.
3. Applications(또는 Launchpad/Spotlight)에서 SySHIP을 실행합니다.

아직 Apple 공증(notarization)이 되어 있지 않아 첫 실행 시 "손상되어 열 수 없음" Gatekeeper 경고가 표시됩니다 — 한 번만 조치하면 되는 해결 방법은 [FAQ](faq.md)를 참고하세요.

## 첫 실행

SySHIP은 몇 가지 샘플 선체 파일(`kvlcc.stl`, `kvlcc2.stl`, `kcs.stl`)을 함께 제공합니다. HULL → Import 탭의 파일 탐색기는 기본적으로 이 샘플 폴더를 가리키므로, 직접 준비한 형상이 없어도 바로 프로그램을 체험해볼 수 있습니다 — 자세한 내용은 [선체 임포트](hull/import.md)를 참고하세요.
