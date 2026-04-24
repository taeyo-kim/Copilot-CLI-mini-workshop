# Step 7. MCP 서버 연결

> ⏱️ 예상 시간: 20분

## 🎯 목표

- MCP(Model Context Protocol)가 무엇이고 왜 중요한지
- 내장 GitHub MCP 서버로 바로 시작
- Filesystem / Context7 서버 추가 구성
- 여러 서버를 조합한 워크플로우

---

## 🧩 비유: 브라우저 확장 프로그램

브라우저 자체는 페이지만 보여주지만, 확장 프로그램은 비밀번호 관리자·문법 검사기·파일 관리자 같은 서비스를 연결해 줍니다. **MCP 서버가 Copilot에게 하는 역할이 바로 그것** — GitHub Issue, 로컬 파일 시스템, 최신 라이브러리 문서 같은 외부 데이터원을 실시간으로 붙여 줍니다.

| 없이 | 있을 때 |
|------|---------|
| `@`로 공유한 파일만 본다 | 스스로 GitHub 리포, 파일 시스템, 문서를 탐색한다 |

## 1) 30초 시작 — 내장 GitHub MCP

```powershell
copilot
```

```text
> List the recent commits in this repository
```

실제 커밋이 나오면 MCP가 동작하는 중입니다. 서버 목록 확인:

```text
> /mcp show
```

## 2) 서버 추가 — 두 가지 방법

### A. 내장 레지스트리 (쉬움)

```text
> /mcp search
```

대화형 선택기에서 서버를 고르면 CLI가 설정까지 자동으로 해 줍니다.

### B. 설정 파일 직접 편집 (유연함)

**파일 위치**

| 경로 | 범위 |
|------|------|
| `~/.copilot/mcp-config.json` | 사용자 전역 |
| `.mcp.json` (프로젝트 루트) | 프로젝트 전용 |

> ⚠️ `.vscode/mcp.json`은 더 이상 지원되지 않습니다. Copilot CLI 는 `.mcp.json`을 사용합니다.

**예시 — Filesystem + Context7**

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

> 💡 GitHub MCP는 내장이라 적을 필요 없습니다. 설정 후 Copilot 재시작.

## 3) 서버별 용도

| 서버 | 용도 |
|------|------|
| **GitHub** (내장) | 레포, 이슈, PR, 커밋 조회/생성 |
| **Filesystem** | 로컬 프로젝트 파일 자율 탐색 |
| **Context7** | 최신 라이브러리 문서 검색 |
| **Microsoft Learn** | Microsoft 공식 문서 검색 (`https://learn.microsoft.com/api/mcp`) |

## ▶️ 실습 — 멀티 서버 조합

### 1. 상태 확인

```text
> /mcp show
```

### 2. Filesystem MCP로 코드 탐색

```text
> How many Python files are in samples/book-app-project/?
> What functions are defined in each file?
```

### 3. GitHub MCP로 히스토리 조회

```text
> List the last 5 commits in this repository
> What branches exist?
```

### 4. 조합 — Issue-to-Fix 한 세션

```text
> Check the open issues in this repository.
> Pick the top priority bug and find the related code in samples/book-app-project/.
> Propose a fix with tests.
```

Copilot이 **GitHub MCP로 이슈 조회 → Filesystem MCP로 코드 탐색 → 수정안 제시**를 한 번에 수행합니다.

## 🔧 `/mcp` 명령 요약

| 명령 | 설명 |
|------|------|
| `/mcp show` | 구성된 서버 목록 |
| `/mcp search` | 레지스트리에서 서버 검색/설치 |
| `/mcp auth <server>` | 인증이 필요한 서버 로그인 |

## 🔑 한 줄 요약

> GitHub MCP는 내장, 나머지는 `~/.copilot/mcp-config.json` 또는 `.mcp.json`으로 추가. 여러 서버를 **한 세션에서 조합**하는 게 핵심 가치.

---

## 🎉 완주 축하합니다

이제 Copilot CLI의 핵심을 전부 익혔습니다:

- 3가지 모드 · `@` 컨텍스트 · 세션 재개
- 5가지 개발 워크플로우
- 커스텀 에이전트 · 스킬 · MCP

### 더 배우려면

- 원본 풀 코스: [github/copilot-cli-for-beginners](https://github.com/github/copilot-cli-for-beginners) (Chapter 07 "Putting It All Together" 포함)
- [공식 문서](https://docs.github.com/en/copilot/how-tos/copilot-cli)
- [명령어 레퍼런스](https://docs.github.com/en/copilot/reference/cli-command-reference)
- [Awesome Copilot](https://github.com/github/awesome-copilot) — 커뮤니티 에이전트/스킬/MCP 모음

---

[⬅ 이전: 스킬](./06-skills.md) · [🏠 홈](./README.md)
