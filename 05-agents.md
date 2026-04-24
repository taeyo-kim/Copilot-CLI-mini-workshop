# Step 5. 커스텀 에이전트

> ⏱️ 예상 시간: 20분

## 🎯 목표

- 내장 에이전트(`/plan`, `/review`) 이해
- `.agent.md` 파일로 나만의 전문가 페르소나 만들기
- 작업별로 에이전트 전환하기

---

## 🧩 비유: 전문가 고용

범용 AI 하나보다, 보안 전문가 / 테스트 전문가 / 문서 작성자를 나눠 쓰는 게 더 깔끔하고 일관됩니다.

## 1) 내장 에이전트

| 에이전트 | 호출 | 용도 |
|---------|------|------|
| Plan | `/plan` 또는 Shift+Tab | 구현 전 단계별 계획 |
| Code-review | `/review` | 변경사항 리뷰 |
| Init | `/init` | 프로젝트 구성 파일 자동 생성 |
| Explore | 자동 | 코드베이스 탐색 |
| Task | 자동 | 테스트/빌드/린트 실행 |

## 2) 커스텀 에이전트 만들기

### 파일 위치

| 경로 | 범위 |
|------|------|
| `.github/agents/*.agent.md` | 프로젝트 전용 (팀 공유) |
| `~/.copilot/agents/*.agent.md` | 전역 (모든 프로젝트) |

### 최소 형식

```markdown
---
name: python-reviewer
description: Senior Python code reviewer focused on security, type hints, and PEP 8
---

# Python Reviewer

You are a senior Python code reviewer.

**Review priorities:**
1. Security vulnerabilities (SQL injection, hardcoded secrets, unsafe eval)
2. Missing type hints on public functions
3. PEP 8 style violations
4. Error handling gaps

**Output format:**
Numbered list with severity tags: [CRITICAL] / [HIGH] / [MEDIUM] / [LOW].
For each finding, include: file, line number, issue, recommended fix.
```

> 💡 `description`이 핵심입니다. 자연어로 "언제 이 에이전트를 써야 하는지"를 키워드 포함해 쓰세요.

## 3) 두 가지 사용 방법

### Interactive — 세션 안에서 전환

```powershell
copilot
```

```text
> /agent
# 목록에서 python-reviewer 선택

> Review @samples/book-app-project/books.py
```

### Launch 시 지정

```powershell
copilot --agent python-reviewer
```

기본 상태로 돌아가려면 `/agent` 후 "no agent" 선택.

## ▶️ 실습 — 에이전트 팀 만들기

원본 저장소 루트에서:

```powershell
New-Item -ItemType Directory -Force -Path .github/agents | Out-Null
```

### reviewer.agent.md

`.github/agents/reviewer.agent.md`:

```markdown
---
name: reviewer
description: Senior code reviewer focused on security and best practices
---

# Reviewer

You are a senior code reviewer focused on code quality.

**Review priorities:**
1. Security vulnerabilities
2. Performance issues
3. Maintainability
4. Best practice violations

**Output:** Numbered list with [CRITICAL] / [HIGH] / [MEDIUM] / [LOW] tags.
```

### documentor.agent.md

`.github/agents/documentor.agent.md`:

```markdown
---
name: documentor
description: Technical writer for clear and complete code documentation
---

# Documentor

You are a technical writer who creates clear documentation.

**Standards:**
- Start with a one-sentence summary
- Include usage examples
- Document parameters and return values
- Note gotchas or limitations
```

### 사용

```powershell
copilot --agent reviewer
```

```text
> Review @samples/book-app-project/books.py
```

```powershell
copilot
```

```text
> /agent
# documentor 선택
> Document @samples/book-app-project/books.py
```

## 🔑 한 줄 요약

> `.github/agents/<name>.agent.md` 에 페르소나를 정의 → `/agent` 로 전환. **description이 곧 검색어**.

---

[⬅ 이전: 워크플로우](./04-workflows.md) · [다음: 스킬 ➡](./06-skills.md)
