# Step 4. 개발 워크플로우

> ⏱️ 예상 시간: 30분

## 🎯 목표

매일 쓰는 5가지 개발 작업을 Copilot CLI로 가속합니다.

1. 코드 리뷰
2. 리팩터링
3. 디버깅
4. 테스트 생성
5. Git 통합 (커밋 메시지 / PR)

---

## 1) 코드 리뷰

```powershell
copilot
```

```text
> @samples/book-app-project/books.py Review this file for bugs, edge cases, and maintainability issues.
> Categorize findings as CRITICAL / HIGH / MEDIUM / LOW.
```

또는 스테이징된 변경사항에 대해 내장 리뷰 에이전트 사용:

```text
> /review
```

## 2) 리팩터링 (테스트 먼저!)

```text
> @samples/book-app-project/books.py
> Generate pytest tests for find_book_by_title() before we refactor it

> Now refactor find_book_by_title() to support case-insensitive and partial matching.
> Keep the tests passing.
```

## 3) 디버깅

에러 메시지 + 관련 코드를 함께 제공하면 원인 추적이 빠릅니다.

```text
> @samples/book-app-project/books.py
> Users report: "Finding books by author name doesn't work for partial names"
> Analyze the likely cause and propose a fix.
```

`/research` 로 라이브러리/베스트 프랙티스 조사도 가능:

```text
> /research What are the best Python libraries for validating user input in CLI apps?
```

## 4) 테스트 생성

```text
> @samples/book-app-project/books.py
> Generate pytest tests for BookCollection covering:
> - Happy path (add, find, remove)
> - Edge cases (empty title, year=0, None)
> - Error scenarios with pytest.raises()
> Use fixtures for reusable setup.
```

## 5) Git 통합

### 커밋 메시지 생성

```powershell
git add .
copilot -p "Generate a conventional commit message for: $(git diff --staged)"
```

PowerShell에서는:

```powershell
git add .
$diff = git diff --staged
copilot -p "Generate a conventional commit message for: $diff"
```

### PR 작성

Interactive 세션에서:

```text
> /pr
```

내장 `/pr` 명령이 변경사항을 분석해 PR 제목/본문 초안을 만들어 줍니다.

## 🎬 End-to-End: 버그 수정 풀 워크플로우

```powershell
copilot
```

```text
# 1. 버그 분석
> Users report: 'Finding books by author name doesn't work for partial names'
> @samples/book-app-project/books.py Analyze and identify the likely cause

# 2. 수정
> Fix find_by_author() to support partial, case-insensitive matches

# 3. 테스트 추가
> Generate pytest tests for:
>   full match, partial match, case-insensitive, not found

> /exit
```

```powershell
# 4. 커밋
git add .
copilot -p "Generate a conventional commit message for: $(git diff --staged)"
git commit -m "<위 출력 붙여넣기>"
```

## 🔑 한 줄 요약

> **리뷰 → 테스트 먼저 → 리팩터링/수정 → 테스트 추가 → 커밋 메시지 생성**. 한 세션에서 다 된다.

---

[⬅ 이전: 컨텍스트](./03-context.md) · [다음: 커스텀 에이전트 ➡](./05-agents.md)
