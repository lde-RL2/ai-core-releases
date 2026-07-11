# AI-Core Notion 연결 매뉴얼

AI-Core는 로컬 라이브러리를 기본 저장소로 사용하면서, 사용자가 선택한 Notion 페이지 아래에
`AI-Core Papers` 데이터베이스를 만들고 PDF·논문 정보·태그·메모·주석·읽기 상태를
미러링합니다.

이 매뉴얼은 개인 또는 지인끼리 사용하는 현재 AI-Core 배포판의 **내부 연결(Internal
connection)** 방식에 맞춰 작성되었습니다.

## 1. 어떤 인증 방법을 선택해야 하나요?

Notion 연결 생성 화면에서 **Access token**과 **OAuth** 중 하나를 묻는다면
**Access token**을 선택합니다.

- **Access token / 내부 연결**: 한 Notion 워크스페이스에서 개인 자동화로 사용합니다.
  AI-Core의 현재 방식입니다.
- **OAuth / 공개 연결**: 여러 사용자가 웹 승인 화면을 통해 앱을 설치하는 서비스용입니다.
  현재 AI-Core에서는 사용하지 않습니다.

내부 연결은 워크스페이스마다 별도로 만들어야 합니다. 지인이 자기 Notion을 사용하려면
지인도 자신의 워크스페이스에서 연결과 토큰을 만들어야 하며, 다른 사람에게 본인의 토큰을
공유하면 안 됩니다.

Notion 공식 문서상 내부 연결 생성에는 해당 워크스페이스의 **Workspace Owner** 권한이
필요합니다. 회사·학교 워크스페이스에서 생성 메뉴가 보이지 않으면 워크스페이스 관리자에게
권한을 요청하거나 본인의 개인 워크스페이스를 사용하세요.

## 2. 저장용 상위 페이지 만들기

먼저 Notion 앱이나 웹에서 AI-Core 전용 일반 페이지를 하나 만듭니다.

권장 예시:

```text
AI-Core Storage
```

다음 조건을 지켜주세요.

- 연결을 만들 워크스페이스와 같은 워크스페이스에 생성합니다.
- 기존 데이터베이스가 아니라 **일반 페이지**로 만듭니다.
- 페이지 내용은 비어 있어도 됩니다.
- `AI-Core Papers` 데이터베이스는 직접 만들지 않습니다. AI-Core가 필요한 속성과 함께
  자동으로 생성합니다.
- 다른 컴퓨터에서도 같은 라이브러리를 쓰려면 모든 컴퓨터에 이 페이지의 동일한 URL을
  입력합니다.

페이지를 연 상태에서 브라우저 주소를 복사하거나 우측 상단 **Share → Copy link**를
눌러 페이지 URL을 보관합니다. AI-Core에는 전체 URL 또는 32자리 페이지 ID를 입력할 수
있습니다.

## 3. 내부 연결과 액세스 토큰 만들기

1. [Notion My integrations / Creator dashboard](https://www.notion.so/profile/integrations)에
   로그인합니다.
2. 왼쪽 **Build → Internal connections**로 이동합니다.
3. **Create a new connection**을 누릅니다.
4. 연결 이름을 예를 들어 `AI-Core` 또는 `AI-Core Sync`로 입력합니다.
5. 2단계에서 페이지를 만든 워크스페이스를 선택합니다.
6. 인증 방법을 묻는다면 **Access token**을 선택합니다.
7. 연결을 생성하고 **Configuration** 탭을 엽니다.

### 필요한 Capabilities

Configuration의 Content capabilities에서 다음 세 항목을 켭니다.

- **Read content**
- **Insert content**
- **Update content**

AI-Core는 기존 라이브러리를 읽고, 논문 페이지와 데이터베이스를 만들고, 이후 내용을
업데이트해야 하므로 세 권한이 모두 필요합니다.

다음 권한은 현재 필요하지 않습니다.

- Read comments / Insert comments
- 사용자 이메일 정보

권한을 저장한 뒤 Configuration의 **Installation access token**을 표시하고 복사합니다.
토큰은 일반적으로 `ntn_`으로 시작하며, 오래된 연결은 `secret_` 형식일 수 있습니다.

토큰은 비밀번호와 같습니다. 채팅, GitHub 소스, 문서에 붙여 넣지 마세요. 실수로 공개했다면
연결의 Configuration에서 토큰을 새로 발급하고 AI-Core에도 새 값을 입력합니다.

## 4. 상위 페이지에 연결 권한 주기

토큰만 만들면 아직 어떤 페이지도 읽을 수 없습니다. 2단계에서 만든 `AI-Core Storage`
페이지를 연결에 명시적으로 공유해야 합니다.

### 방법 A: Notion 페이지에서 추가

1. `AI-Core Storage` 페이지를 엽니다.
2. 우측 상단 `•••` 메뉴를 누릅니다.
3. **Connections** 또는 **연결**을 선택합니다.
4. **Add connection / 연결 추가**를 누릅니다.
5. 앞에서 만든 `AI-Core` 연결을 검색해 선택합니다.
6. 이 페이지와 하위 페이지 접근 확인 창을 승인합니다.

### 방법 B: Creator dashboard에서 추가

1. [My integrations](https://www.notion.so/profile/integrations)에서 `AI-Core` 연결을 엽니다.
2. **Content access** 탭을 선택합니다.
3. **Edit access**를 누릅니다.
4. `AI-Core Storage` 페이지를 선택하고 저장합니다.

상위 페이지에 권한을 주면 그 아래에 AI-Core가 만드는 데이터베이스와 논문 페이지에도
권한이 상속됩니다. 페이지를 다른 워크스페이스로 옮기거나 연결을 제거하면 동기화가
실패합니다.

## 5. AI-Core에 입력하기

1. AI-Core 우측 상단 톱니바퀴를 눌러 **Settings**를 엽니다.
2. **Cloud Sync Destination**에서 다음 중 하나를 선택합니다.
   - `Notion`: Notion만 사용
   - `Drive + Notion`: Google Drive와 Notion 모두 사용
3. **NOTION SYNC**의 첫 번째 칸에 Installation access token을 붙여 넣습니다.
4. 두 번째 칸에 `AI-Core Storage` 페이지의 전체 URL 또는 페이지 ID를 붙여 넣습니다.
5. **Connect and create library**를 누릅니다.

정상 연결되면 워크스페이스 이름과 파일당 업로드 한도가 표시되고, 상위 페이지 아래에
`AI-Core Papers` 데이터베이스가 생성됩니다.

앱에는 다음과 같이 입력합니다.

```text
Notion token:
ntn_********************************

Notion parent page:
https://www.notion.so/.../AI-Core-Storage-페이지ID
```

토큰은 가능한 운영체제에서 Electron `safeStorage`를 사용해 암호화한 뒤 이 컴퓨터의
AI-Core 사용자 설정에만 저장합니다. Notion이나 Google Drive 동기화 파일에는 토큰을
포함하지 않습니다.

## 6. 연결 후 Notion에 생성되는 항목

상위 페이지 아래에 `AI-Core Papers` 데이터베이스 하나가 생성됩니다. 주요 속성은 다음과
같습니다.

| 속성 | 용도 |
| --- | --- |
| Name | 논문 제목 |
| AI-Core ID | 앱 내부 논문 식별자 |
| Authors | 저자 |
| Year | 연도 |
| DOI | DOI |
| Tags | AI-Core 태그 |
| Added | 추가 날짜 |
| Updated | 충돌 판단용 수정 시각 |
| PDF | 원본 PDF 또는 분할 조각 |
| AI-Core Data | 메모·주석·읽기 상태 등의 JSON 스냅샷 |

폴더·태그·논문 배치 정보는 데이터베이스 안의 `AI-Core Library State` 항목에
`library.json`으로 저장됩니다.

다음 항목은 이름을 바꾸거나 삭제하지 않는 것을 권장합니다.

- `AI-Core Papers` 데이터베이스
- `AI-Core ID`, `Updated`, `PDF`, `AI-Core Data` 속성
- `AI-Core Library State` 항목
- PDF의 `.manifest.json`, `.partNNN.zip` 첨부파일

Notion에서 제목이나 속성을 직접 수정한 값을 AI-Core로 병합하는 편집기는 아직 제공하지
않습니다. 논문 정보·폴더·태그·메모는 AI-Core에서 수정하세요.

## 7. 5MB를 넘는 PDF 처리

Notion 워크스페이스에 파일당 업로드 한도가 있으면 AI-Core가 연결 시 실제 한도를 조회합니다.
PDF가 한도를 넘으면 원본 바이트를 안전한 크기로 나누고 각 조각을 무압축 ZIP으로
포장합니다.

```text
paper.generation.manifest.json
paper.generation.part001.zip
paper.generation.part002.zip
paper.generation.part003.zip
```

manifest에는 원본과 각 조각의 크기·SHA-256이 들어갑니다. 다른 컴퓨터에서는 모든 조각을
받아 해시를 검증하고 원본과 정확히 같은 PDF로 결합한 뒤에만 엽니다. 조각 하나가 없거나
손상되면 불완전한 PDF를 열지 않고 동기화 오류를 표시합니다.

API 요청의 배열 한도를 고려해 PDF 하나당 최대 99개 조각과 manifest 하나를 지원합니다.
이보다 많은 조각이 필요한 대용량 PDF는 Google Drive 또는 `Drive + Notion`을 사용하세요.

- [Notion 파일 도움말](https://www.notion.com/help/images-files-and-media)
- [Notion API 대용량 업로드](https://developers.notion.com/guides/data-apis/sending-larger-files)

## 8. 다른 컴퓨터에서 같은 Notion 라이브러리 연결하기

각 컴퓨터에 같은 토큰과 같은 상위 페이지 URL을 입력하고 **Connect and create library**를
누릅니다. 이미 `AI-Core Papers`가 있으면 새 데이터베이스를 만들지 않고 기존 데이터베이스를
찾아 내려받습니다.

안전한 최초 연결 순서:

1. 기존 컴퓨터에서 우측 상단 동기화 버튼을 누릅니다.
2. 동기화 완료 후 기존 컴퓨터의 AI-Core를 종료합니다.
3. 새 컴퓨터에서 같은 Notion 연결을 설정합니다.
4. 폴더·논문·메모가 모두 내려올 때까지 기다립니다.
5. 복원이 확인된 뒤 기존 컴퓨터를 다시 실행합니다.

현재 폴더·태그 배치는 `library.json` 전체 스냅샷의 최신 수정 시각을 기준으로 적용됩니다.
두 컴퓨터에서 동시에 폴더 이동·이름 변경·태그 편집을 하면 나중 스냅샷이 먼저 한 변경을
덮을 수 있습니다. 폴더 구조를 크게 정리할 때는 한 컴퓨터에서만 작업하고 동기화 완료 후
다른 컴퓨터를 여세요.

중요한 정리 작업 전에는 앱을 종료한 상태에서 로컬 DB를 복사해 두는 것을 권장합니다.

- Windows: `%APPDATA%\paper-manager\library.db`
- macOS: `~/Library/Application Support/paper-manager/library.db`
- Linux: `~/.config/paper-manager/library.db`

`library.db-wal`이 존재할 수 있으므로 앱을 완전히 종료한 뒤 백업하세요. `files` 폴더도 함께
백업하면 로컬 PDF까지 보존할 수 있습니다.

## 9. 연결 해제·토큰 교체

- **Disconnect**: AI-Core에서 토큰과 저장된 Notion 데이터베이스 ID를 제거합니다. Notion의
  데이터베이스나 논문 페이지 자체는 삭제하지 않습니다.
- 토큰 교체: Notion Configuration에서 새 토큰을 발급한 뒤 Settings의 토큰 칸에 붙여 넣고
  다시 연결합니다.
- 상위 페이지 변경: 새 페이지에 연결 권한을 부여하고 새 URL을 입력합니다. 서로 다른
  `AI-Core Papers`가 생성될 수 있으므로 기존 라이브러리를 옮길 목적이라면 먼저 백업하세요.

## 10. 오류 해결

### `Failed to finish connecting app` 또는 403/404

- 상위 페이지 `••• → Connections`에 `AI-Core` 연결이 추가됐는지 확인합니다.
- 연결의 Content access 탭에 올바른 페이지가 선택됐는지 확인합니다.
- 토큰의 워크스페이스와 페이지의 워크스페이스가 같은지 확인합니다.
- 입력한 것이 데이터베이스 URL이 아니라 일반 상위 페이지 URL인지 확인합니다.

### 401 또는 `unauthorized`

- 토큰 앞뒤 공백을 제거하고 Configuration에서 다시 복사합니다.
- 토큰을 재발급했다면 AI-Core에도 새 토큰을 입력합니다.
- 다른 워크스페이스의 토큰을 사용하지 않았는지 확인합니다.

### 데이터베이스를 만들 수 없음

- Read content, Insert content, Update content 권한 세 개를 모두 켭니다.
- 본인 계정이 상위 페이지를 편집할 수 있는지 확인합니다. 연결 권한은 연결을 추가한 사용자의
  페이지 권한을 넘어설 수 없습니다.

### 동기화가 바로 보이지 않음

폴더·태그·메모 변경은 연속 작업을 묶기 위해 약 5초 후 전송됩니다. 즉시 동기화하려면
AI-Core 우측 상단의 동기화 버튼을 누릅니다. 대용량 PDF는 분할·업로드·검증 때문에 더 오래
걸릴 수 있습니다.

### 폴더나 태그 배치가 이전 상태로 돌아감

두 컴퓨터의 전체 라이브러리 스냅샷이 충돌했을 가능성이 있습니다. 두 앱의 동기화를 잠시
끄고, 올바른 데이터가 남은 컴퓨터의 `library.db`를 먼저 백업한 뒤 복구해야 합니다. 복구가
끝날 때까지 새 폴더 편집을 계속하지 마세요.

## 공식 참고 자료

- [Notion 내부 연결 만들기](https://developers.notion.com/guides/get-started/internal-connections)
- [Notion 연결 권한 Capabilities](https://developers.notion.com/reference/capabilities)
- [Notion Developer quickstart](https://developers.notion.com/guides/get-started/quick-start)
- [Notion 연결 추가 및 관리](https://www.notion.com/help/add-and-manage-connections-with-the-api)
