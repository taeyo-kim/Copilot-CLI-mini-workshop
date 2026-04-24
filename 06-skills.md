# Step 6. 스킬 시스템

> ⏱️ 예상 시간: 20분

## 🎯 목표

- 스킬(Agent Skills)이 무엇인지, 에이전트와 어떻게 다른지 이해
- `SKILL.md` 파일로 커스텀 스킬 작성
- 프롬프트만 자연스럽게 쓰면 스킬이 **자동으로** 적용되는 경험

---

## 🧠 핵심 차이: 에이전트 vs 스킬 vs MCP

| | 역할 | 언제 |
|---|------|------|
| **에이전트** | AI의 **사고방식**을 바꿈 (페르소나) | 여러 작업에 걸친 전문성 |
| **스킬** | **특정 작업의 절차**를 지시 | 반복되는 구체적 절차 |
| **MCP** | **외부 시스템**과 연결 | 라이브 데이터 (GitHub API 등) |

에이전트 하나가 여러 스킬을 불러 쓸 수도 있습니다. 예) code-reviewer 에이전트가 security-audit 스킬 + code-checklist 스킬을 자동 적용.

## 📂 스킬 위치

| 경로 | 범위 |
|------|------|
| `.github/skills/<name>/SKILL.md` | 프로젝트 (팀 공유) |
| `~/.copilot/skills/<name>/SKILL.md` | 개인 (모든 프로젝트) |

## 📝 SKILL.md 포맷

```markdown
---
name: security-audit
description: Use for security reviews, vulnerability scanning, SQL injection, XSS, authentication issues, OWASP Top 10 checks
---

# Security Audit

Perform a security audit checking for:

## Injection
- SQL injection (string concatenation in queries)
- Command injection (unsanitized shell commands)

## Authentication
- Hardcoded credentials
- Weak password requirements
- Missing rate limiting

## Sensitive Data
- Plaintext passwords, API keys in code
- Logging sensitive information

## Output
For each issue: file, line, type, severity (CRITICAL/HIGH/MEDIUM/LOW), recommended fix.
```

> 💡 **description이 전부**다. Copilot은 프롬프트와 description을 매칭해 스킬을 자동 로드합니다. 키워드를 자연어로 풍부하게 쓰세요.

## 🚀 자동 트리거 — 그냥 자연스럽게 물어보면 끝

```powershell
copilot
```

```text
> @samples/book-app-project/ Check this code for security vulnerabilities
```

Copilot이 "security vulnerabilities" → `security-audit` 스킬과 매칭 → OWASP 체크리스트를 자동 적용.

### 직접 호출도 가능

```text
> /security-audit Check the API endpoints
> /code-checklist Check books.py for quality issues
```

### 현재 스킬 확인

```text
> /skills list
> /skills info security-audit
> /skills reload          # SKILL.md 수정 후 다시 읽기
```

## ▶️ 실습 — 스킬 2개 만들기

### 1) Security Audit 스킬

```powershell
New-Item -ItemType Directory -Force -Path .github/skills/security-audit | Out-Null
```

`.github/skills/security-audit/SKILL.md`:

```markdown
---
name: security-audit
description: Use for security reviews, vulnerability scanning, SQL injection, XSS, OWASP Top 10, hardcoded secrets, input validation
---

# Security Audit

Check for:

## Injection
- SQL / command / LDAP injection
- Unsafe eval / exec

## Auth
- Hardcoded credentials / API keys
- Missing rate limiting

## Data
- Sensitive data in logs
- Missing encryption

## Output
For each issue: [SEVERITY] file:line — problem — recommended fix.
```

### 2) pytest 생성 스킬

```powershell
New-Item -ItemType Directory -Force -Path .github/skills/pytest-gen | Out-Null
```

`.github/skills/pytest-gen/SKILL.md`:

```markdown
---
name: pytest-gen
description: Generate comprehensive pytest tests with fixtures, edge cases, mocks, and pytest.raises for error scenarios
---

# pytest Generator

Generate pytest tests that include:

## Structure
- `test_` prefix, one assertion per test when possible
- Descriptive names explaining the expected behavior
- Use fixtures for setup/teardown

## Coverage
- Happy path, edge cases (None, "", []), boundaries, errors
- Use pytest.raises() for expected exceptions

## Output
A complete runnable test file with proper imports.
```

### 사용

```powershell
copilot
```

```text
> @samples/book-app-project/books.py Check for security issues
# → security-audit 스킬 자동 적용

> Generate tests for the BookCollection class
# → pytest-gen 스킬 자동 적용

> What skills did you use for that response?
```

## 🌐 커뮤니티 스킬 설치 (선택)

GitHub CLI v2.90+ 에서:

```powershell
gh skill install github/awesome-copilot code-checklist
```

> ⚠️ 설치 전 항상 `SKILL.md`를 읽어 보세요. 악의적 스킬은 위험한 명령을 실행시킬 수 있습니다.

## 🔑 한 줄 요약

> 자주 쓰는 프롬프트를 `SKILL.md`로 박제. **description에 트리거 키워드를 풍부하게** 쓰면 자연어 프롬프트만으로 자동 적용.

---

[⬅ 이전: 에이전트](./05-agents.md) · [다음: MCP ➡](./07-mcp.md)
