# Step 2. 3가지 모드로 첫 사용

> ⏱️ 예상 시간: 20분

## 🎯 목표

Copilot CLI의 3가지 상호작용 모드를 상황에 맞게 구분해서 사용합니다.

| 모드 | 비유 | 언제 쓰나 |
|------|------|-----------|
| **Interactive** | 웨이터와 대화 | 탐색, 반복, 문맥을 이어가며 작업 |
| **Plan** | GPS로 경로 확인 | 복잡한 작업 — 코딩 전 계획 먼저 |
| **Programmatic** | 드라이브 스루 | 일회성 자동화, 스크립트, CI/CD |

---

## 🧪 사전 준비

샘플 저장소가 없다면 복제합니다.

```powershell
git clone https://github.com/github/copilot-cli-for-beginners
cd copilot-cli-for-beginners
```

## 권장 사항

이 실습은 `samples/book-app-project` 폴더 안의 Python 파일들을 대상으로 Copilot CLI를 사용하여 점진적으로 코드 개선을 진행하는 방식으로 진행됩니다. 매우 간단한 샘플 파이선 파일들이지만 사전에 샘플 프로젝트 구조와 각 파일의 역할을 이해한다면 실습 시나리오를 조금 더 직관적으로 체감할 수 있습니다. 그렇기에 **[book-app-project 구조 및 Python 파일 분석](./desc.md)** 파일을 참고하여 각 파일의 역할과 프로젝트 구조를 먼저 숙지하는 것을 권장합니다. 

## 1) Interactive 모드 (여기서부터 시작)

```powershell
copilot
```

프롬프트(`>`)에서 자연어로 질문합니다.

[영문]
```text
> Explain what @samples/book-app-project/books.py does in simple terms

> Review @samples/book-app-project/book_app.py for code quality issues

> Add type hints to all functions in the same file

> /exit
```
[한글]
```text 한글
> @samples/book-app-project/books.py 파일이 하는 일을 간단하게 설명해주세요

> @samples/book-app-project/book_app.py 파일의 코드 품질 문제를 검토해주세요

> 동일한 파일의 모든 함수에 타입 힌트를 추가해주세요

> /exit
```

**포인트**: 이전 질문의 문맥이 다음 질문으로 이어집니다. 사람과 대화하듯 점진적으로 개선.

### 꼭 알아야 할 슬래시 명령

| 명령 | 용도 |
|------|------|
| `/help` | 전체 명령 목록 |
| `/ask` | 대화 히스토리에 영향 없는 일회성 질문 |
| `/clear` | 문맥 초기화 (세션 기록 X) |
| `/new` | 세션 저장 후 새 세션 시작 |
| `/plan` | Plan 모드로 전환 |
| `/model` | AI 모델 변경 |
| `/rewind` | 대화 특정 시점으로 되돌리기 |
| `/exit` | 종료 |

## 2) Plan 모드

복잡한 기능은 계획부터 세웁니다. Interactive 세션 안에서:

```text
> /plan Add a "mark as read" command to the book app
```

Copilot이 단계별 구현 계획을 먼저 보여주고 `Proceed? [Y/n]`으로 승인을 받은 뒤 코드를 작성합니다.

또는 시작부터 Plan 모드로:

```powershell
copilot --plan
```

> 💡 **Shift+Tab** 으로 Interactive ↔ Plan ↔ Autopilot 모드를 순환합니다.

## 3) Programmatic 모드

스크립트/자동화에서 한 번 호출하고 끝낼 때:

```powershell
copilot -p "Write a Python function that checks if a number is prime"
```

여러 파일에 반복 적용 (PowerShell 예):

```powershell
Get-ChildItem samples/book-app-project/*.py | ForEach-Object {
  $p = "samples/book-app-project/$($_.Name)"
  copilot --allow-all -p "Quick code quality review of @$p - critical issues only"
}
```

> ⚠️ `--allow-all`은 모든 도구 실행을 자동 승인합니다. 신뢰할 수 있는 프롬프트에만 사용.

## ▶️ 실습 과제

원본 코스의 **`utils.py` 개선 과제**를 간단 버전으로 진행합니다.

```powershell
copilot
```

```text
> @samples/book-app-project/utils.py What does each function do?

> Add input validation to get_user_choice() so it handles empty input and non-numeric entries

> Add a comprehensive docstring to get_book_details()

> /exit
```

문맥이 이어지면서 한 파일을 점진적으로 개선하는 경험을 체감해 보세요.

## 🔑 한 줄 요약

> **Interactive**로 대화, **Plan**으로 계획, **Programmatic**으로 자동화. 시작은 무조건 Interactive.

---

[⬅ 이전: 설치](./01-setup.md) · [다음: 컨텍스트와 세션 ➡](./03-context.md)
