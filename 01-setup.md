# 1: 설치 및 인증

> ⏱️ 예상 시간: 10분
> 🕒 실제 핸즈온 소요 시간: 약 10~15분

## 🎯 목표

- GitHub Copilot CLI를 로컬에 설치
- GitHub 계정으로 인증
- 동작 확인

---

## 1) 설치

플랫폼에 맞는 방법 하나를 선택하세요.

### npm (모든 플랫폼, Node.js 필요)

```powershell
npm install -g @github/copilot
```

### Windows (WinGet)

```powershell
winget install GitHub.Copilot
```

### macOS/Linux (Homebrew)

```bash
brew install copilot-cli
```

### macOS/Linux (스크립트)

```bash
curl -fsSL https://gh.io/copilot-install | bash
```

> 💡 **Codespaces를 쓰면 설치 불필요** — 원본 저장소를 Codespace로 열면 CLI가 이미 설치되어 있습니다.

## 2) 실행 및 인증

터미널에서 다음 명령을 실행합니다.

```powershell
copilot
```

- 처음이라면 현재 폴더를 신뢰할지 묻습니다 → **Trust** 선택
- 프롬프트가 뜨면 로그인:

```text
> /login
```

브라우저가 열리고 터미널에 표시된 일회용 코드(`ABCD-1234` 형식)를 입력 → **Authorize** → 터미널로 돌아오면 로그인 완료.

> ✅ 로그인은 한 번만 하면 됩니다. 토큰이 만료되면 다시 `/login`.

## 3) 동작 확인

Copilot 세션 안에서 간단한 프롬프트를 보내 봅니다.

```text
> Say hello and tell me what you can help with
```

응답이 오면 성공. 종료:

```text
> /exit
```

## 🔧 문제 해결

| 증상 | 해결 |
|------|------|
| `copilot: command not found` | 다른 설치 방법 시도 (npm 권장) |
| "You don't have access to GitHub Copilot" | [github.com/settings/copilot](https://github.com/settings/copilot)에서 구독 확인 |
| 브라우저가 안 열림 | [github.com/login/device](https://github.com/login/device)에 수동으로 코드 입력 |
| 토큰 만료 | `copilot` → `/login` 다시 실행 |

## 🔑 한 줄 요약

> `copilot` 실행 → `/login` 으로 인증 → `/exit` 로 종료. 한 번 로그인하면 유지됩니다.

---

**[다음으로 이동하기 →](./02-modes.md)**