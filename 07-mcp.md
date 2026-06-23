# 🌐 6: MCP Servers 핵심 실습

> 🕒 실제 핸즈온 소요 시간: 약 25~35분

이번 실습의 핵심은 Copilot을 로컬 파일, GitHub, 최신 문서 검색과 같은 외부 정보원과 연결하는 방법을 익히는 것입니다. MCP는 Copilot이 사용자가 직접 붙여 넣은 내용만 보는 상태에서 벗어나, 실제 개발 환경과 실시간 데이터에 접근하게 해 주는 연결 계층입니다.

## 이 장에서 바로 익힐 것

- MCP가 왜 필요한지 이해하기
- GitHub MCP가 기본 내장이라는 점 확인하기
- /mcp show, /mcp search 같은 핵심 명령 익히기
- filesystem, Context7 서버를 가장 실용적인 예제로 설정해 보기
- 여러 MCP를 한 세션에서 조합하는 흐름 체험하기

## 🧩핵심 개념 

![MCP Servers are like Browser Extensions](images/browser-extensions-analogy.png)

MCP는 Copilot을 외부 서비스에 연결하는 방식입니다. 브라우저 확장 프로그램이 브라우저에 새 기능을 붙여 주는 것처럼, MCP 서버는 Copilot에 GitHub 조회, 파일 시스템 탐색, 공식 문서 검색 같은 능력을 추가합니다.

예를 들면 이렇게 나눠 볼 수 있습니다.

| MCP 서버 | 연결 대상 | 대표 활용 |
| ---- | ---- | ---- |
| GitHub MCP | 저장소, 이슈, PR | 커밋 조회, 브랜치 확인, 이슈/PR 작업 |
| Filesystem MCP | 로컬 프로젝트 파일 | 디렉터리 탐색, 파일 내용 분석 |
| Context7 MCP | 최신 라이브러리 문서 | 공식 사용법, 최신 패턴 조회 |

핵심은 단순합니다. MCP가 없으면 Copilot은 직접 넘겨준 파일과 대화 내용에 주로 의존합니다. MCP가 있으면 필요한 정보를 스스로 가져와 더 실제적인 답을 할 수 있습니다.

## 가장 빠른 확인 방법 ⚡

![Quick Start MCP](images/quick-start-mcp.png)

GitHub MCP는 기본 내장입니다. 별도 설치 없이 바로 아래처럼 확인할 수 있습니다.

```bash
copilot

> List the recent commits in this repository
> 현재 리포지토리에 있는 최근의 커밋을 나열해줘
```

실제 커밋 정보가 응답으로 오면 MCP가 이미 동작한 것입니다. 이어서 현재 연결 상태를 확인합니다.

```bash
copilot

> /mcp show
```

보통 처음에는 GitHub 서버만 보여도 정상입니다. 추가 서버는 직접 설치하거나 설정 파일에 넣으면 됩니다.

![MCP Status Demo](images/mcp-status-demo.gif)

## 꼭 알아둘 핵심 명령

| 명령 | 용도 |
| ---- | ---- |
| /mcp show | 현재 설정된 MCP 서버와 활성화 상태 확인 |
| /mcp search | 레지스트리에서 MCP 서버 검색 및 설치 |
| /login | GitHub MCP 인증 재설정 |

설정 관점에서는 두 가지만 기억하면 됩니다.

- 사용자 전체 설정: ~/.copilot/mcp-config.json
- 현재 프로젝트 전용 설정: 프로젝트 루트의 .mcp.json

처음 시작할 때는 JSON을 직접 쓰기보다 /mcp search로 설치하는 방식이 가장 쉽습니다.

## 설정 핵심만 보기 ⚙️

![Configuring MCP Servers](images/configuring-mcp-servers.png)

가장 실용적인 조합은 filesystem과 Context7입니다. 하나는 내 파일을 읽고, 다른 하나는 최신 문서를 찾습니다.

```json
{
  "mcpServers": {
    "filesystem": {
      "type": "local",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."],
      "tools": ["*"]
    },
    "context7": {
      "type": "local",
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"],
      "tools": ["*"]
    }
  }
}
```

이 설정은 ~/.copilot/mcp-config.json 또는 프로젝트 루트의 .mcp.json에 넣으면 됩니다.

설정에서 기억할 점은 아래 정도면 충분합니다.

- GitHub MCP는 기본 내장이라 보통 config에 직접 추가하지 않습니다.
- filesystem의 . 경로는 Copilot을 실행한 현재 디렉터리를 뜻합니다.
- Context7은 최신 문서를 찾아 주므로 오래된 학습 데이터 한계를 보완할 때 유용합니다.
- JSON 문법이 틀리면 서버가 로드되지 않으니 /mcp show로 바로 확인합니다.

## 자주 쓰는 서버별 활용 🛠️

![Using MCP Servers](images/using-mcp-servers.png)

### GitHub MCP

별도 설정 없이 저장소 상태를 조회할 수 있습니다.

```bash
copilot

> List the last 5 commits in this repository
> What branches exist in this repository?
> Are there any open pull requests?
```

### Filesystem MCP

프로젝트 구조와 파일 내용을 탐색할 때 유용합니다.

```bash
copilot

> How many Python files are in samples/book-app-project/?
> Find functions without type hints in samples/book-app-project/
> Read samples/book-app-project/data.json and summarize any data issues
```

### Context7 MCP

공식 문서 기반으로 최신 사용법을 물을 수 있습니다.

```bash
copilot

> What are the best practices for using pytest fixtures?
> How should I apply that to the book app tests?
```

## MCP의 진짜 장점: 조합 워크플로 🔄

![Issue to PR Workflow using MCP](images/issue-to-pr-workflow.png)

MCP의 가치가 커지는 지점은 여러 서버를 한 번에 쓰는 순간입니다. 예를 들면 이런 흐름이 가능합니다.

1. Filesystem MCP로 book app 코드와 테스트 파일을 읽습니다.
1. GitHub MCP로 최근 커밋, 이슈, PR 상태를 확인합니다.
1. Context7 MCP로 관련 라이브러리의 최신 권장 패턴을 조회합니다.
1. Copilot이 세 정보를 합쳐 개선안이나 구현 계획을 정리합니다.

예시 프롬프트는 아래처럼 구성할 수 있습니다.

```bash
copilot

> Read samples/book-app-project/tests/test_books.py and compare it with samples/book-app-project/books.py. Then check recent commits touching samples/book-app-project/ and summarize what test coverage is missing.

> samples/book-app-project/tests/test_books.py 파일을 읽어서 samples/book-app-project/books.py와 비교합니다. 그런 다음, samples/book-app-project/에 대한 최근 커밋을 확인하고 누락된 테스트 커버리지를 요약합니다.

```

이 한 번의 요청 안에 파일 읽기, GitHub 기록 확인, 분석이 모두 들어갑니다. 이게 MCP의 핵심 체감 포인트입니다.

## 가장 짧은 실습 루트 ✅

아래 순서대로만 해도 이 장의 핵심은 거의 익힐 수 있습니다.

1. GitHub MCP가 동작하는지 확인합니다.

```bash
copilot

> /mcp show
> List the last 5 commits in this repository
```
만일, GitHub MCP가 올바로 동작하지 않는다면, 다음 명령으로 Copilot을 시작합니다.

```bash
copilot --enable-all-github-mcp-tools
```

2. filesystem MCP를 설정합니다.

```bash
copilot

> /mcp search
```

또는 설정 파일에 filesystem 서버를 직접 추가합니다.

3. book app 파일을 읽어 보게 합니다.

```bash
> How many Python files are in samples/book-app-project/?
> What functions are defined in each file?
```

4. GitHub MCP와 filesystem MCP를 같이 써 봅니다.

```bash
> Read samples/book-app-project/data.json and tell me what books are in the collection. Then check the recent commits to see when this file was last modified.
```

5. 가능하면 Context7까지 붙여 개선안까지 받아 봅니다.

```bash
> What are pytest fixture best practices, and how should they apply to samples/book-app-project/tests/test_books.py?
```

## 자주 막히는 부분

| 문제 | 먼저 볼 것 |
| ---- | ---- |
| /mcp show에 서버가 안 보임 | 설정 파일 위치와 JSON 문법 확인 |
| GitHub 조회가 안 됨 | /login 또는 gh auth status 확인 |
| 서버를 추가했는데 반영이 안 됨 | Copilot 재시작 또는 설정 재확인 |
| filesystem이 파일을 못 읽음 | 실행 위치와 경로 설정 확인 |
| 문서 결과가 기대와 다름 | Context7 설치 여부와 질문 표현 점검 |

짧게 정리하면, 먼저 /mcp show로 상태를 확인하고, 그다음 인증과 설정 파일을 보는 순서가 가장 빠릅니다.

## 이 장의 핵심만 다시 정리 📌

- MCP는 Copilot을 외부 정보원과 연결해 주는 구조입니다.
- GitHub MCP는 기본 내장이라 가장 먼저 체험하기 쉽습니다.
- 실무에서는 filesystem과 Context7을 같이 붙였을 때 효율이 크게 올라갑니다.
- MCP의 진짜 장점은 한 세션 안에서 여러 서버를 조합해 분석까지 이어지는 점입니다.
- 처음에는 /mcp search, /mcp show, GitHub 조회 예제만 익혀도 충분합니다.

**[다음으로 이동하기 →](./08-putting-it-together.md)**
