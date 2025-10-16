# LF와 CRLF

LF, CRLF는 줄 바꿈 문자입니다. <br>

**줄 바꿈 문자란** <br>
텍스트 파일은 눈에 보이는 '줄'이 아니라, 문자들의 연속된 데이터입니다. <br>
우리가 엔터를 칠 때, 실제로는 눈에 보이지 않는 '제어 문자'가 삽입되어 줄이 끝났음을 표시합니다. <br>
이걸 EOL(End Of Line) 또는 줄 바꿈 문자(Line Ending) 라고 합니다.

이런 줄 바꿈이 운영체제마다 문자가 다릅니다.<br>
그래서 다른 운영체제의 줄 바꿈 문자로 작성하면 Git에서 LF/CRLF 경고가 발생하게 됩니다. <br>
프로젝트에서는 일관된 EOL 규칙을 정하고 .gitattributes + 에디터 설정 + Git 설정으로 강제하는 것이 가git장 안전합니다.

줄 바꿈 문자로는 LF/CRLF가 있습니다.

## LF
LF는 Line Feed에 약자로 Unix, Linux, MaxOS에서 쓰는 줄 바꿈 문자로 \n을 사용합니다.

### LF 권장 이유
Git이나 대부분의 개발도구는 LF를 기본이자 권장 형식으로 사용 합니다.

그 이유는 크게 세 가지 입니다.

1. Unix 기반 환경 표준
Git, Node.js, Docker, Python 등 요즘 사용하는 대부분의 개발 도구와 서버 환경은 Unix/Linux 기반 입니다.
이 환경에서는 LF(\n)가 줄바꿈의 기본 값입니다.

즉 Mac이나 서버 환경에서 문제없이 동작하려면 LF가 더 안전한 선택이라고 할 수 있습니다.

2. 협업과 버전 관리에 유리
CRLF와 LF가 섞이면 Git에서 diff가 전부 바뀐 것처럼 보일 수 있습니다.
이는 코드 리뷰를 어렵게 만들고 충돌도 늘어납니다.

그래서 대부분의 협업 환경은 LF로 일관된 줄바꿈을 유지하는 걸 권장합니다.

3. 자동 변환 기능이 LF 중심으로 동작
Git의 core.autocrlf 설정도 대부분 LF 중심입니다.
Windows에서는 CRLF를 써도 커밋 시 LF로 변환되고. 다른 환경에서 가져올 때도 LF 형태로 유지됩니다.

즉 Git 내부 저장소에서는 항상 LF 형태로 관리됩니다. <br>
그래서 .gitattributes에서도 아래처럼 eol=lf로 지정하는 경우가 많습니다.

## CRLF
CRLF는 Carriage Return + Line Feed에 약자로 Windos에서 쓰는 줄 바꿈 문자로 \r\n 을 사용합니다.

## Git이 경고를 띄우는 이유
Git은 파일의 줄바꿈 문자까지 포함해서 파일내용을 추적합니다. <br>
그런데 협업 중 팀원이 서로 다른 운영체데를 쓴다면 파일의 줄바꿈이 뒤섞이게 됩니다.

예를 들어 <br>
Mac을 사용하는 팀원이 LF로 커밋한 파일을 Windows를 사용하는 팀원이 수정하고 커밋하면 Git은 파일의 줄바꿈 방시이 바뀌었다고 인식합니다.

그러면 이런 경고가 뜨게 됩니다.

warning: LF will be replaced by CRLF the next time Git touches it
오류는 아니지만 Git이 자동으로 줄바꿈을 변환할 예정이란 의미입니다. <br>
하지만 줄바꿈이 뒤섞이면 앞서 말한 것 처럼 diff가 엉망으로 표시되어 코드 리뷰나 병합 시 불편함이 생길 수 있습니다.

### 해결 방법
줄바꿈 문제는 크게 두 가지 방법으로 해결할 수 있습니다.

1. Git 설정 변경
Git에는 줄바꿈을 자동으로 변환해주는 설정이 있습니다.

|환경 | 명령어 | 동작 |
|---|---|---|
|Windows|git config --global core.autocrlf true | 커밋할 때 CRLF -> LF로 변환|
|MacOS / Linux | git config --global core.autocrlf input | LF 그대로 유지 |

Windows -> true <br>
macOS/Linux -> input
으로 맞추면 됩니다.

이 설정은 로컬에서 Git이 자동으로 줄바꿈을 관리하도록 돕는 역할을 합니다.

2. 프로젝트 내부에서 통일
팀 프로젝트에서는 각자 OS 설정이 다를 수 있으므로 저장소 단위로 일관된 규칙을 강제하는 게 가장 안전합니다.

루트 폴더에 .gitattributes 파일을 만들고 다음을 작성합니다.
```
# 모든 텍스트 파일은 LF로 저장
* text=auto eol=lf

# 바이너리 파일은 변환하지 않음
*.png binary
*.jpg binary
*.ttf binary
```
이후 저장하고 커밋한 후 실행하면 기존 파일의 줄바꿈도 정리됩니다.
이렇게 되면 앞으로는 모든 팀원이 어떤 운영체제를 쓰든 자동으로 LF 형식으로 통일됩니다.

3. 에디터 확인 가능
VSCODE 같은 에디터에서는 오른쪽 하단에 CRLF,LF 표시가 있습니다.
이걸 클릭해서 현재 파일의 줄바꿈 형식을 바로 바꿀 수 있습니다.
