# Step 3. 컨텍스트와 세션

> ⏱️ 예상 시간: 20분

## 🎯 목표

- `@` 문법으로 파일/폴더를 프롬프트에 주입
- 여러 파일을 동시에 분석 (cross-file intelligence)
- 세션을 저장·재개하여 하루 넘게 작업 이어가기
- 컨텍스트 윈도우 관리

---

## 1) `@` 문법 — Copilot에게 "이 파일 봐!"

| 패턴 | 의미 |
|------|------|
| `@file.py` | 단일 파일 참조 |
| `@folder/` | 디렉터리 전체 참조 |
| `@file1.py @file2.py` | 여러 파일 동시 참조 |
| `@screenshot.png` | 이미지 분석 (UI 버그 디버깅 등) |

```powershell
copilot
```

```text
> Explain what @samples/book-app-project/utils.py does

> Review all files in @samples/book-app-project/ for error handling
```

## 2) 크로스 파일 분석 — 진짜 위력

여러 파일을 함께 주면 단일 파일 리뷰로는 못 찾는 버그가 드러납니다.

```text
> @samples/book-app-project/book_app.py @samples/book-app-project/books.py
> How do these files work together? What's the data flow?
```

Copilot은 다음을 찾아냅니다:
- 중복된 함수 (`show_books()` vs `print_books()`)
- 파일 간 일관성 없는 에러 처리
- 데이터 흐름 (UI → 비즈니스 로직 → 스토리지)

**60초 코드베이스 요약** — 새 프로젝트를 이해할 때:

```text
> @samples/book-app-project/
> In one paragraph, what does this app do and its biggest quality issues?
```

## 3) 세션 관리 — 하루 지나서도 이어가기

### 자동 저장

모든 세션은 종료 시 자동으로 저장됩니다. 그냥 `/exit` 하세요.

### 이어가기

```powershell
# 가장 최근 세션 계속
copilot --continue

# 목록에서 선택
copilot --resume
```

### 세션 이름 붙이기

```text
> /rename book-app-review
```

### 시나리오: 월요일 시작 → 수요일에 이어서

```text
# Monday
> /rename book-app-review
> @samples/book-app-project/books.py Review and number all code quality issues
> Fix issue #1 (duplicate functions)
> /exit

# Wednesday
copilot --continue
> What issues remain unfixed from our book app review?
```

Copilot이 월요일에 매긴 번호 목록과 이미 고친 항목까지 기억합니다. 다시 설명할 필요 없음.

## 4) 컨텍스트 윈도우 관리

```text
> /context      # 현재 토큰 사용량 확인 (예: 62k/200k)
> /clear        # 세션 기록 버리고 초기화
> /new          # 현재 세션 저장 후 새 세션 시작
> /compact      # 기존 대화를 압축해 토큰 절약
> /rewind       # 이전 시점으로 롤백 (Esc 두 번도 가능)
```

> 💡 주제를 바꿀 때는 `/new`를 쓰세요. 오래된 문맥이 답변을 오염시킬 수 있습니다.

## ▶️ 실습 과제 — 데이터 흐름 추적

```powershell
copilot
```

```text
> /rename data-flow-analysis

> @samples/book-app-project/books.py @samples/book-app-project/book_app.py
> Trace how a book goes from user input to being saved in data.json.
> What functions are involved at each step?

> @samples/book-app-project/data.json What happens if this file is missing or corrupted?

> /context
> /exit
```

며칠 뒤 `copilot --resume`으로 이 세션에 복귀해 보세요.

## 🔑 한 줄 요약

> `@`로 파일을 주고, `--continue`로 이어가고, `/context`로 용량을 확인한다.

---

[⬅ 이전: 3가지 모드](./02-modes.md) · [다음: 개발 워크플로우 ➡](./04-workflows.md)
