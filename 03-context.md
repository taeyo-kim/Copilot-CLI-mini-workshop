# 3: 컨텍스트와 세션

> 🕒 실제 핸즈온 소요 시간: 약 25~35분

이번 실습의 중심은 세 가지입니다.

- `@` 문법으로 파일과 폴더를 정확히 참조하기
- 세션을 저장하고 이어서 작업하기
- 컨텍스트 한도를 관리하면서 대화를 효율적으로 유지하기

## 이 장에서 익힐 것

이 장을 마치면 다음을 할 수 있습니다.

- `@` 문법으로 파일, 폴더, 이미지를 참조하기
- `--continue`, `--resume` 으로 이전 작업 이어가기
- `/context`, `/clear`, `/compact` 같은 명령으로 컨텍스트 관리하기
- 여러 파일을 함께 읽혀서 교차 분석 요청하기
- 여러 번 이어지는 대화로 점진적으로 작업 발전시키기

## 핵심 비유: 동료에게 맥락 주기

<img src="images/colleague-context-analogy.png" alt="Context Makes the Difference - Without vs With Context" width="800"/>

Copilot CLI는 마음을 읽지 못합니다. 사람 동료에게도 "앱이 안 돼요" 보다 "`books.py`의 `find_book_by_title` 함수가 대소문자를 구분해서 문제예요" 라고 말해야 더 좋은 답을 받듯이, Copilot CLI에도 구체적인 맥락을 줘야 정확한 답을 얻습니다.

그 역할을 하는 것이 `@` 문법입니다.

## 가장 중요한 기능: `@` 문법

`@` 는 Copilot CLI에게 특정 파일이나 폴더를 보라고 지시하는 방법입니다.

기본 패턴은 아래 정도만 알면 충분합니다.

| 패턴 | 의미 | 예시 |
| ---- | ---- | ---- |
| `@file.py` | 파일 하나 참조 | `Review @samples/book-app-project/books.py` |
| `@folder/` | 폴더 전체 참조 | `Review @samples/book-app-project/` |
| `@file1.py @file2.py` | 여러 파일 동시 참조 | `Compare @samples/book-app-project/book_app.py @samples/book-app-project/books.py` |
| `@image.png` | 이미지 참조 | `@images/mockup.png Explain this UI` |

처음에는 아래처럼 시작하면 됩니다.

[한글]
```bash
copilot

> @README.md이 무엇을 하는지 설명해 주세요
> @samples/book-app-project/books.py 파일을 리뷰해 주세요
> @samples/book-app-project/book_app.py @samples/book-app-project/books.py 두 파일을 비교해 주세요
```

[영문]
```bash
copilot

> Explain what @README.md does
> Review @samples/book-app-project/books.py
> Compare @samples/book-app-project/book_app.py @samples/book-app-project/books.py
```

## 왜 컨텍스트가 중요한가

한 파일만 보면 보이지 않는 문제가 여러 파일을 함께 볼 때 드러납니다. 예를 들어 아래 같은 질문은 단일 파일 검토보다 훨씬 강력합니다.

[한글]
```bash
copilot

> @samples/book-app-project/book_app.py @samples/book-app-project/books.py
> 어떻게 이 두 파일이 함께 작동하는지, 데이터 흐름이 어떻게 되는지 설명해 주세요
```

[영문]
```bash
copilot

> @samples/book-app-project/book_app.py @samples/book-app-project/books.py
> How do these files work together? What's the data flow?
```

이런 식의 교차 분석으로 찾기 쉬운 것들은 다음과 같습니다.

- 중복 코드
- 모듈 간 데이터 흐름
- 에러 처리 방식의 불일치
- 구조적으로 유지보수성을 해치는 패턴

즉, `@` 문법은 단순히 파일을 열어 보여주는 기능이 아니라, Copilot CLI가 코드베이스를 관계 중심으로 이해하게 만드는 장치입니다.

## 빠른 실전 예시

### 1. 파일 하나 이해하기

```bash
copilot

> @samples/book-app-project/utils.py 이 파일은 무엇을 하나요?
```

### 2. 여러 파일 비교하기

```bash
copilot

> @samples/book-app-project/book_app.py @samples/book-app-project/books.py 두 파일을 비교해 주세요
```

### 3. 폴더 전체 리뷰하기

```bash
copilot

> @samples/book-app-project/ 폴더 안의 모든 파일을 에러 처리 관점에서 리뷰해 주세요
```

### 4. 프로젝트를 한 번에 파악하기

```bash
copilot

> @samples/book-app-project/
> 이 앱이 무엇을 하는지, 가장 큰 품질 문제는 무엇인지 간단하게 설명해 주세요
```

이 방식은 새로운 저장소를 빠르게 이해할 때 특히 유용합니다.

## 다중 턴 대화의 힘

Copilot CLI는 한 번 묻고 끝나는 도구가 아니라, 이전 대화를 기억하며 이어가는 도구입니다.

예를 들어 아래처럼 흐름을 이어갈 수 있습니다.

```bash
copilot

> @samples/book-app-project/books.py 이 모듈은 무엇을 하나요?
> @samples/book-app-project/ 이 폴더의 코드 구조를 간단히 설명해 주세요
> 앱이 책을 저장하고 불러오는 방식은 어떻게 되나요?
```

이미 읽은 파일과 앞선 질문을 바탕으로 대답이 점점 정교해집니다. 이 점이 단순 검색과 가장 크게 다른 부분입니다.

## 세션 관리

<img src="images/session-persistence-timeline.png" alt="Timeline showing how GitHub Copilot CLI sessions persist across days - start on Monday, resume on Wednesday with full context restored" width="800"/>

Copilot CLI 세션은 자동 저장됩니다. 따라서 작업을 끝내고 나중에 다시 이어서 할 수 있습니다.

### 가장 최근 세션 이어가기

```bash
copilot --continue
```

### 특정 세션 다시 열기

```bash
copilot --resume
copilot --resume abc123
```

### 세션 이름 붙이기

```bash
copilot

> /rename book-app-review
```

세션 이름을 잘 붙여 두면 나중에 다시 찾기가 훨씬 쉽습니다.

### 세션 안에서 전환하기

```text
> /resume
```

현재 세션 안에서도 다른 세션으로 이동할 수 있습니다.

### 실습: 세션 닫기, 이어가기, resume 하기

아래 순서대로 따라 하면 `세션 종료 -> 가장 최근 세션 이어가기 -> 특정 세션 다시 열기` 흐름을 한 번에 연습할 수 있습니다.

#### 1. 새 세션 시작

터미널에서 Copilot CLI를 실행합니다.

```bash
copilot
```

#### 2. 세션 이름 붙이기

나중에 찾기 쉽도록 먼저 이름을 붙입니다.

```text
> /rename practice-session
```

#### 3. 컨텍스트를 하나 넣고 대화하기

아무 파일이나 하나 읽혀서 세션에 맥락을 남깁니다.

```text
> @samples/book-app-project/books.py 이 파일이 하는 일을 세 문장으로 요약해 주세요
> 여기서 가장 먼저 개선하면 좋을 점 하나만 말해 주세요
```

#### 4. 세션 종료하기

대화를 끝내고 세션을 닫습니다.

```text
> /exit
```

이 단계가 끝나면 지금까지의 대화가 자동으로 저장됩니다.

#### 5. 가장 최근 세션 이어가기

이제 터미널에서 가장 마지막 세션을 다시 엽니다.

```bash
copilot --continue
```

세션이 열리면, 방금 했던 대화를 기억하는지 확인합니다.

```text
> 방금 전에 우리가 어떤 파일을 보고 있었는지 알려 주세요
```

#### 6. 다시 종료하기

이어서 한 번 더 세션을 닫습니다.

```text
> /exit
```

#### 7. 세션 목록에서 직접 resume 하기

이번에는 가장 최근 세션 자동 이어가기 대신, 저장된 세션 목록에서 직접 선택합니다.

```bash
copilot --resume="practice-session"
```

목록이 뜨면 조금 전에 이름 붙인 `practice-session` 을 선택합니다.

#### 8. 특정 세션을 다시 연 뒤 확인 질문하기

세션이 다시 열리면 아래처럼 물어봅니다.

```text
> 이 세션에서 이전에 논의한 개선 포인트를 다시 요약해 주세요
```

#### 9. 선택 사항: 세션 ID로 바로 열기

세션 목록에서 ID를 확인했다면, 다음처럼 특정 세션을 바로 다시 열 수도 있습니다.

```bash
copilot --resume="practice-session"
```

여기서 `practice-session` 은 실제 세션 ID로 바꿔야 합니다.

#### 실습이 끝나면 확인할 것

- `/exit` 로 종료한 세션이 실제로 저장되는지
- `copilot --continue` 가 가장 최근 세션을 이어주는지
- `copilot --resume` 이 세션 목록을 보여주는지
- 이름을 붙인 세션이 나중에 다시 찾기 쉬운지

## 컨텍스트 관리 명령

대화가 길어지고 파일을 많이 참조하면 컨텍스트 창이 차오릅니다. 이때 아래 명령들이 중요합니다.

| 명령 | 역할 |
| ---- | ---- |
| `/context` | 현재 컨텍스트 사용량 확인 |
| `/clear` | 현재 세션을 버리고 새 대화 시작 |
| `/new` | 현재 세션은 저장하고 새 대화 시작 |
| `/rewind` | 대화의 이전 시점으로 되돌아가기 |
| `/compact` | 이전 대화를 요약해서 컨텍스트 공간 확보 |

예시:

```bash
copilot

> /context
> /compact
> /new
```

실무 감각으로 정리하면 아래처럼 쓰면 됩니다.

- 주제가 완전히 바뀌면 `/new` 또는 `/clear`
- 잘못된 방향으로 갔으면 `/rewind`
- 대화는 유지하고 공간만 줄이고 싶으면 `/compact`
- 지금 얼마나 찼는지 보고 싶으면 `/context`

## 컨텍스트를 효율적으로 쓰는 방법

컨텍스트는 무한하지 않습니다. 그래서 처음부터 폴더 전체를 무조건 던지기보다, 점진적으로 좁혀 가는 방식이 효율적입니다.

좋은 흐름은 보통 이렇습니다.

1. 구조 파악: `@folder/`
2. 핵심 파일 좁히기: `@file.py`
3. 필요한 관련 파일만 추가하기
4. 컨텍스트가 커지면 `/compact` 또는 `/new`

예시:

```bash
copilot

> @package.json 이 프로젝트는 어떤 프레임워크를 사용하나요?
> @samples/book-app-project/ 이 프로젝트의 구조를 설명해 주세요
> @samples/book-app-project/books.py  BookCollection 클래스를 리뷰해 주세요
> @samples/book-app-project/book_app.py @samples/book-app-project/books.py CLI가 BookCollection을 어떻게 사용하는지 설명해 주세요

```

### 실습: /compact 와 /rewind 직접 써보기

아래 순서대로 따라 하면 두 명령의 차이를 바로 체감할 수 있습니다.

#### 1. 컨텍스트를 먼저 쌓기

하나의 세션에서 질문을 몇 번 이어 가며 맥락을 늘립니다. 

```bash
copilot

> @samples/book-app-project/ 이 프로젝트 구조를 간단히 설명해 주세요
> @samples/book-app-project/books.py BookCollection 클래스의 책임을 정리해 주세요
> @samples/book-app-project/book_app.py CLI 흐름을 단계별로 설명해 주세요
> /context
```

여기서는 /context 로 현재 컨텍스트가 어느 정도 쌓였는지 확인합니다.

#### 2. /compact 실행하기

이제 대화는 유지하되 컨텍스트 공간을 줄입니다.

```text
> /compact
> 지금까지 논의한 핵심만 세 줄로 다시 요약해 주세요
```

이 단계에서 확인할 점은, 긴 대화 전체는 압축되더라도 핵심 결정과 맥락은 남아 있어야 한다는 것입니다.

#### 3. 일부러 다른 방향으로 가 보기

이번에는 원래 흐름과 다른 질문을 던져 봅니다.

```text
> 이제 이 프로젝트를 웹 앱으로 바꾸는 방법을 설계해 주세요
```

이 질문이 원래 흐름과 맞지 않거나 잘못된 방향이라고 느껴지면 /rewind 를 써볼 수 있습니다.

#### 4. /rewind 실행하기

```text
> /rewind
```

타임라인이 열리면, 방금 웹 앱 설계 질문을 하기 전 시점으로 돌아갑니다.

#### 5. 원래 흐름으로 다시 질문하기

되돌아간 뒤에는 원래 주제에 맞는 질문을 이어갑니다.

```text
> books.py 와 book_app.py 사이에서 가장 먼저 고쳐야 할 구조 문제 하나만 다시 말해 주세요
```

#### 실습 후 정리

- /compact 는 대화의 핵심을 남긴 채 컨텍스트 공간을 줄일 때 사용합니다
- /rewind 는 잘못된 질문이나 방향 전환 이전 시점으로 되돌아갈 때 사용합니다
- 두 명령 모두 대화를 다시 살리는 도구이지만, /compact 는 요약이고 /rewind 는 되돌리기라는 점이 다릅니다

## 이미지도 컨텍스트가 될 수 있다

Copilot CLI는 코드뿐 아니라 이미지도 읽을 수 있습니다.

```bash
copilot

> @images/screenshot.png 이 이미지에서 무슨 일이 일어나고 있나요?
> @images/mockup.png 이 디자인과 일치하도록 HTML과 CSS를 작성해 주세요

```

스크린샷 분석, UI 구현, 에러 화면 확인 같은 작업에 유용합니다.

## 권한 관련 기본 명령

다른 디렉터리를 참조해야 할 때는 접근 권한을 추가해야 할 수 있습니다.

```bash
copilot --add-dir /path/to/directory
```

세션 안에서는 이렇게 할 수 있습니다.

```text
> /add-dir /path/to/directory
```

이 기능은 여러 프로젝트를 오가며 작업할 때 중요합니다.

## 가장 빠른 실습 루트

이 장에서 꼭 해볼 만한 세 가지만 고르면 아래 순서가 좋습니다.

1. `@samples/book-app-project/` 로 프로젝트 전체 리뷰 요청하기
2. `/rename` 후 `/exit`, `copilot --continue` 로 세션 이어가기
3. `/context` 와 `/compact` 를 써서 컨텍스트 사용량 변화를 확인하기

이 세 가지를 해보면 이 장의 핵심을 거의 다 체험하게 됩니다.

## 자주 하는 실수

- 파일명 앞에 `@` 를 빼먹기
- 매번 새 `copilot` 세션을 열고 이전 맥락이 남아 있길 기대하기
- 다른 주제로 넘어가면서 `/clear` 나 `/new` 를 쓰지 않기
- 처음부터 폴더 전체를 너무 많이 참조해 컨텍스트를 낭비하기
- 현재 작업 디렉터리 밖의 파일을 참조하면서 권한 설정을 하지 않기

## 문제 해결 팁

- 파일을 못 찾으면 현재 디렉터리와 상대 경로를 확인합니다
- 권한 오류가 나면 `--add-dir` 또는 `/add-dir` 를 사용합니다
- 응답이 엉키면 `/clear` 또는 `/new` 로 주제를 분리합니다
- 컨텍스트가 너무 빨리 차면 파일 참조 범위를 더 좁히고 `/compact` 를 사용합니다
- 원격 접속 링크가 안 열리면 세션이 아직 살아 있는지, 같은 GitHub 계정인지, `/remote` 정보가 최신인지 확인합니다

## 핵심 정리

이 장의 본질은 Copilot CLI에 "무엇을 보고", "어떤 맥락을 유지하며", "어떻게 이어서 일할지"를 알려주는 방법을 배우는 것입니다.

기억해야 할 핵심은 아래 여섯 가지입니다.

1. `@` 문법이 컨텍스트의 출발점입니다
2. 여러 파일을 함께 참조하면 더 큰 구조와 문제를 볼 수 있습니다
3. 세션은 자동 저장되므로 `--continue`, `--resume` 으로 이어서 작업할 수 있습니다
4. `/context`, `/clear`, `/compact`, `/new`, `/rewind` 로 컨텍스트를 통제할 수 있습니다
5. 많이 넣는 것보다 정확하게 넣는 것이 더 중요합니다. 컨텍스트는 유한하므로 필요한 정보만 점진적으로 추가하고, 불필요하게 큰 범위를 한 번에 넣지 않도록 합니다 
다음 장에서는 이렇게 쌓은 컨텍스트를 실제 개발 워크플로에 연결하는 방법을 다루게 됩니다.
