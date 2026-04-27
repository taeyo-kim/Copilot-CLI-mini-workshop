# GitHub Copilot CLI 미니 워크샵

> 원본:  [github/copilot-cli-for-beginners](https://github.com/github/copilot-cli-for-beginners)
>
> 이 랩은 원본 코스의 방대한 내용 중 **반드시 알아야 할 핵심**만 추려 재구성한 단축 버전입니다. 2–3시간 안에 끝낼 수 있도록 설계되었습니다.

---

## 🎯 이 랩에서 배우는 것

- GitHub Copilot CLI 설치 및 인증
- 3가지 상호작용 모드 (Interactive / Plan / Programmatic)
- `@` 문법으로 파일/폴더 컨텍스트 제공
- 세션 관리 (`--continue`, `--resume`)
- 실전 개발 워크플로우 (리뷰 / 리팩터링 / 디버깅 / 테스트 / Git)
- 커스텀 에이전트(`.agent.md`)와 스킬(`SKILL.md`)
- MCP 서버로 외부 서비스 연결

## ✅ 사전 요구사항

- GitHub 계정 + Copilot 구독 ([구독 옵션](https://github.com/features/copilot/plans) · 학생은 [GitHub Education](https://education.github.com/pack)으로 무료)
- 터미널 기본 사용 경험 (`cd`, `ls`)
- Node.js 또는 Homebrew/WinGet (설치 도구 중 하나)

---

## 📚 랩 구성 (총 7개 Step)

| # | 제목 | 예상 시간 |
|---|------|-----------|
| 1 | [설치 및 인증](./01-setup.md) | 10분 |
| 2 | [3가지 모드와 기본 명령](./02-modes.md) | 20분 |
| 3 | [컨텍스트와 세션](./03-context.md) | 30분 |
| 4 | [개발 워크플로우](./04-workflows.md) | 30분 |
| 5 | [커스텀 에이전트](./05-agents.md) | 20분 |
| 6 | [스킬 시스템](./06-skills.md) | 20분 |
| 7 | [MCP 서버 연결](./07-mcp.md) | 20분 |

> 💡 각 Step은 **실습 코드 → 핵심 포인트 → 한 줄 요약** 순서로 구성되어 있습니다.

## 🗂️ 이 랩에서 사용하는 샘플 앱

원본 저장소의 [`samples/book-app-project`](https://github.com/github/copilot-cli-for-beginners/tree/main/samples/book-app-project) (Python 기반 책 관리 앱)를 사용합니다. 시작 전 복제해 두세요:

```powershell
git clone https://github.com/github/copilot-cli-for-beginners
cd copilot-cli-for-beginners
```

혹은 현재의 리포를 복제해서 사용하셔도 됩니다. 원본 리포에서 Sample 폴더를 그대로 복사해 두었기에 현재 리포를 복제하면 동일하게 `samples/book-app-project` 폴더를 사용할 수 있습니다.

```powershell
https://github.com/taeyo-kim/Copilot-CLI-mini-workshop
cd Copilot-CLI-mini-workshop
```
## 권장 사항

이 실습은 주로 `samples/book-app-project` 폴더 안의 Python 파일들을 대상으로 Copilot CLI를 사용하여 점진적으로 코드 개선을 진행하는 방식으로 진행됩니다. 매우 간단한 샘플 파이선 파일들이지만 사전에 샘플 프로젝트 구조와 각 파일의 역할을 이해한다면 실습 시나리오를 조금 더 직관적으로 체감할 수 있습니다. 그렇기에 **[book-app-project 구조 및 Python 파일 분석](./desc.md)** 파일을 참고하여 각 파일의 역할과 프로젝트 구조를 먼저 숙지하는 것을 권장합니다. 

## 🔑 꼭 기억할 5가지

1. **대화형으로 시작** — `copilot` 명령으로 진입 후 자연어로 질문
2. **`@`로 파일 지정** — `@path/to/file.py` 형식으로 Copilot이 파일 내용을 읽음
3. **세션은 자동 저장** — `copilot --continue`로 마지막 세션 이어가기
4. **복잡한 건 Plan 모드** — `/plan` 으로 구현 전 계획부터
5. **자동화는 `-p`** — `copilot -p "프롬프트"` 로 일회성 실행

## 📖 공식 참고

- [Copilot CLI 공식 문서](https://docs.github.com/en/copilot/how-tos/copilot-cli)
- [명령어 레퍼런스](https://docs.github.com/en/copilot/reference/cli-command-reference)
- [원본 풀 코스 (영문)](https://github.com/github/copilot-cli-for-beginners)
