# 타임리프 - 기본 기능
## 타임리프 소개
### 타임리프 특징
* 서버 사이드 HTML 렌더링(SSR)
* 네츄럴 템플릿
* 스프링 통합 지원

#### 네츄럴 템플릿
타임리프는 순수 HTML을 최대한 유지하는 특징이 있다.
타임리프로 작성한 파일은 HTML을 유지하기 때문에 웹 브라우저에서 파일을 직접 열어도 내용을 확인할 수 있고,
서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다.
JSP를 포함한 다른 뷰 템플릿들은 오로지 서버를 통해서 JSP가 렌더링 되고 HTML 응답 결과를 받아야 확인할 수 있다.
반면 타임리프로 작성된 작성된 파일은 해당 파일을 그대로 브라우저에서 열어도 정상적인 HTML 결과를 확인할 수 있다.
이렇게 순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 네츄럴 템플릿이라 한다.

#### 스프링 통합지원
타임리프는 스프링과 자연스럽게 통합되고, 스르핑의 다양한 기능을 편리하게 사용할 수 있게 지원한다.

## 텍스트 - text, utext
타임리프는 기본적으로 HTML 태그의 속성에 기능을 정의해서 동작한다.
HTML의 콘텐츠에 데이터를 출력할 때는 다음과 같이 `th:text`를 사용하면 된다.
`<span th:text="${data}">`

#### Escape
#### HTML 엔티티
웹 브라우저는 `<`를 HTML 태그의 시작으로 인식한다.
따라서 `<`를 태그의 시작이 아니라 문자로 표현할 수 있는 방법이 필요한데,
이것을 HTML 엔티티라고 한다.
그리고 이렇게 HTML에서 사용하는 특수 문자를 HTML 엔티티로 변경하는 것을 escape라 한다.
그리고 타임리프가 제공하는 `th:text`, `[[...]]`는 기본적으로 escape를 제공한다.

#### Unescape
이스케이프 기능을 사용하지 않으려면 어떻게 해야 할까?
타임리프는 다음 두 기능을 제공한다.
* `th:text` -> `th:untext`
* `[[...]]` -> `[(...)]`

## 변수 - SpringEL
타임리프에서는 변수를 사용할 때는 변수 표현식을 사용한다.
#### 변수 표현식: `${...}`

#### Object
* `user.name`: user의 username을 프로퍼티 접근 -> `user.getUsername()`
* `user['username']`: 위와 같음 -> `user.getUsername()`
* `user.getUsername()`: user의 `getUsername()`을 직접 호출

#### List
* `user[0].username`: List의 첫 번째 요소를 찾고 username 프로퍼티 접근 -> `list.get(0).getUsername()`
* `users[0]['username']`: 위와 같음
* `users[0].getUsername()`: List의 첫 번째 요소를 찾고 메서드 직접 호출

#### Map
* `userMap['userA'].username`: Map에서 key로 value를 찾고, username 프로퍼티 접근 -> `map.get("userA").getUsername()`
* `userMap['userA']['username']`: 위와 같음
* `userMap['userA'].getUsername()`: Map에서 userA를 찾고 메서드 직접 호출

### 지역 변수 선언
`th:with`를 사용하면 지역 변수를 선언해서 사용할 수 있다.
지연 변수는 선언한 태그 안에서만 사용할 수 있다.

```html
<h1>지역 변수 - (th:with)</h1>
<div th:with="first=${users[0]}">
    <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
</div>
```

## 기본 객체들
타임리프는 기본 객체들을 제공한다.
* `${#request}` - 스프링 부트 3.0 제공하지 않는다.
* `${#response}` - 스프링 부트 3.0 제공하지 않는다.
* `${#session}` - 스프링 부트 3.0 제공하지 않는다.
* `${#servletContext}` - 스프링 부트 3.0 제공하지 않는다.
* `${#locale}`

## 유틸리티 객체와 날짜
타임리프는 문자, 숫자, 날짜, URI등을 편리하게 다루는 다양한 유틸리티 객체들을 제공한다.

#### 타임리프 유틸리티 객체들
* `#message`: 메시지, 국제화 처리
* `#uris`: URI 이스케이프 지원
* `#dates`: `java.util.Date` 서식 지원
* `#calendars`: `java.util.Calendar` 서식 지원
* `#temporals`: 자바 8 날짜 서식지원
* `#numbers`: 숫자 서식 지원
* `#strings`: 문자 관련 편의 기능
* `#objects`: 객체 관련 기능 제공
* `#bools`: boolean 관련 기능 제공
* `#arrays`: 배열 관련 기능 제공
* `#lists`, `#sets`, `#maps`: 컬렉션 고나련 기능 제공
* `#ids`: 아이디 처리 관련 기능 제공

#### 자바 8 날짜
타임리프에서는 자바 8 날짜인 `LocalDate`, `LocalDateTime`, `Instant`를 사용하려면 추가 라이브러리가 필요하다.
스프링 부트 타임리프를 사용하면 해당 라이브러리가 자동으로 추가되고 통합된다.

#### 타임리프 자바 8 날짜 지원 라이브러리
`thymeleaf-extras-java8time`
참고: 스프링 부트 3.2 이상을 사용한다면, 타임리프 자바8 날짜 지원 라이브러리가 이미 포함되어 있다. 따라서 별도로 포함하지 않아도 된다.

## URL 링크
타임리프에서 URL을 생성할 때는 `@{...}` 문법을 사용하면 된다.

#### 단순한 URL
* `@{/hello}` -> /hello

#### 쿼리 파라미터
* `@{/hello(param1=${param1}, param2=${param2})}` -> /hello?param1=data1&parm2=data2

#### 경로 변수
* `@{/hello/{param1}/{param2}(param1=${param1}, parma2=${param2})}` -> /hello/data1/data2

#### 경로 변수 + 쿼리 파라미터
* `@{/hello/{param1}(param1=${param1}, param2=${param2})}` -> /hello/data1?param2=data2

상대 경로, 절대 경로, 프로토콜 기준을 표현할 수도 있다.
* `/hello`: 절대 경로
* `hello`: 상대 경로

## 리터럴
리터럴은 소스 코드상에 고정된 값을 말하는 용어이다.

타임리프는 다음과 같은 리터럴이 있다.
* 문자
* 숫자
* 불린
* null

타임리프에서 문자 리터럴은 항상 `'`(작은 따옴표)로 감싸야 한다.   
`<span th:text="'hello'">`

그런데 문자를 항상 `'`로 감싸는 것은 너무 귀찮은 일이다.    
공백 없이 쭉 이어진다면 하나의 의미있는 토큰으로 인지해서 다음과 같이 작은 따옴표를 생략할 수 있다.   
`<span th:text="hello">`

`<span th:text="hello world!"></span>`은 중간에 공백이 있기 때문에 오류가 발생한다.
`<span th:text="'hello world!'"></span>` 이렇게 `'`로 감싸면 정상 동작한다.

리터럴 대체(Literal substitutions)
`<span th:text="|hello ${data}|">`
마지막의 리터럴 대체 문법을 사용하면 마치 템플릿을 사용하는 것 처럼 편리하다.

## 연산
타임리프 연산은 자바와 크게 다르지 않다. HTML안에서 사용하기 때문에 HTML 엔티티를 사용하는 부분만 주의하자.

* 비교연산: HTML 엔티티를 사용해야 하는 부분을 주의
  *  `>`(gt), `<`(lt), `>=`(ge), `<=`(le), `!`(not), `==`(eq), `!=`(neq, ne)
* 조건식: 자바의 조건식고 ㅏ유사하다.
* Elvis 연산자: 조건식의 편의 버전
* No-Operation: `_`인 경우 마치 타임리프가 실행되지 않은 것 처럼 동작한다. 이것을 잘 사용하면 HTML의 내용 그대로 활용할 수 있다.

## 속성 값 설정
## 반복
## 조건부 평가
## 주석
## 블록
## 자바스크립트 인라인
## 템플릿 조각
## 템플릿 레이아웃
