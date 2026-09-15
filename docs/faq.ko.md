# 자주 묻는 질문

## "Starting..." 화면에서 무한 로딩되거나, `syship-backend.exe` 파일이 계속 사라져요 (Windows)

`syship-backend.exe`가 아직 코드 서명이 안 되어 있어서, Windows가 이걸 "확인되지 않은 프로그램"으로 취급해서 생기는 문제입니다. 실제 바이러스가 아니라, **서로 독립적인 두 개의 Windows 보안 계층**에서 각각 막고 있는 오탐이라 두 단계 모두 처리해야 합니다.

**1단계 — Windows Defender에서 복원하고 예외 폴더로 등록하기**

1. **Windows 보안 → 바이러스 및 위협 방지 → 보호 기록**을 엽니다.
2. 격리된 `syship-backend.exe` 항목을 찾아 **복원**을 클릭합니다.
3. 다시 격리되지 않도록, **바이러스 및 위협 방지 → 설정 관리 → 제외 항목 추가 또는 제거 → 제외 항목 추가 → 폴더**에서 SySHIP 설치 폴더(기본값 `%LocalAppData%\Programs\SySHIP`)를 선택합니다.

**2단계 — Smart App Control 확인하기**

복원했는데도 실행이 안 되거나 "시스템이 지정된 프로그램을 실행할 수 없습니다" 오류가 뜬다면, **Smart App Control**이 막고 있을 가능성이 큽니다. 이건 Defender 예외 처리로는 우회되지 않는 별도의 더 엄격한 계층으로, Windows 11 신규 설치 시 기본값이 "켬"이며 신뢰되지 않은 앱은 아예 실행을 차단합니다.

1. **설정 → 개인 정보 및 보안 → Windows 보안 → 앱 및 브라우저 컨트롤 → Smart App Control**을 엽니다.
2. **끄기**로 변경합니다.
   - 참고: 한번 끄면 Windows를 초기화하거나 재설치하기 전까지는 다시 켤 수 없습니다.

두 단계를 모두 마친 뒤 SySHIP을 다시 실행해보세요.

이 문제는 저희가 앱에 정식 코드 서명을 적용하면 근본적으로 해결될 예정입니다.

## "SySHIP.app이 손상되어 열 수 없습니다" 또는 프로그램이 실행되지 않아요 (macOS)

아직 Apple 공증(notarization)을 받지 않은 상태라, 다운로드해서 설치하면 macOS Gatekeeper가 실행 권한을 막아둡니다. 아래처럼 직접 권한을 부여해야 합니다.

1. `.dmg` 파일로 SySHIP을 설치합니다 (**Applications** 폴더로 드래그).
2. **터미널(Terminal)**을 엽니다.
3. 아래 명령어를 입력합니다.
   ```
   sudo xattr -rd com.apple.quarantine /Applications/SySHIP.app
   ```
4. 비밀번호 입력을 요구하면 **자신의 컴퓨터 로그인 비밀번호**를 입력합니다 (입력해도 화면에 아무 표시가 안 뜨는 게 정상입니다).
5. SySHIP을 평소처럼 실행합니다.

이 명령어는 App Store 외부에서 받은 앱에 macOS가 붙이는 격리(quarantine) 표시를 제거하는 것으로, 설치당 한 번만 해주면 됩니다.
