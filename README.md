
> 공식 사이트: https://www.thymeleaf.org/
> 
> 공식 메뉴얼 - 기본 기능: https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html 
> 
> 공식 메뉴얼 - 스프링 통합: https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html

## 특징
*** 
1. 서버 사이드 HTML 렌더링 (SSR)
   * 백엔드 서버에서 HTML을 동적으로 렌더링하는 용도로 사용한다.
2. 네츄럴 템플릿
   * 순수 HTML을 유지하면서도 뷰 템플릿도 사용 가능하다.
3. 스프링 통합 지원

## 기본 기능
***
### 텍스트 - text, utext
1. HTML 콘텐츠에 데이터 출력
```html
<!--디폴트로 escape 제공-->
<span th:text="${data}"></span>

<span th:utext="${data}"></span>
```
2. HTML 콘텐츠 영역내에 직접 데이터를 출력
```html
<!--디폴트로 escape 제공-->
[[${data}]]

[(${data})]
```

### 변수 - SpringEL
1. 타임리프에서 변수를 사용할 때는 변수 표현식을 사용한다.
```html
${...}
```
2. Object
```html
user.username
user['username']
user.getUsername()
```
3. List
```html
users[0].username
users[0]['username']
users[0].getUsername()
```
4. Map
```html
userMap['userA'].username
userMap['userA']['username']
userMap['userA'].getUsername()
```

### 지역 변수 선언
1. 선언한 태그 내에서만 스코프가 선언된다.
```html
<div th:with="first=${users[0]}">
    <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
</div>
```

### 기본 객체
1. ${#reqeust}
2. ${#response}
3. ${#session}
4. ${#serveletContext}
5. ${#locale}
6. 기본 객체를 편리하게 접근
   1. HTTP 요청 파라미터 접근
   ```java
   // param.파라미터명
   ${param.paramData}
   ```
   2. HTTP 세션 접근
   ```java
   // session.속성
   ${session.sessionData}
   ```
   3. 스프링 빈 접근
   ```java
   //@스프링빈명
   ${@helloBean.hello('Spring!')}
   ```

### 유틸리티
1. #message : 메시지, 국제화 처리
2. #uris : URI 이스케이프 지원
3. #dates : java.util.Date 서식 지원 
4. #calendars : java.util.Calendar 서식 지원
5. #temporals : 자바8 날짜 서식 지원
6. #numbers : 숫자 서식 지원
7. #strings : 문자 관련 편의 기능
8. #objects : 객체 관련 기능 제공
9. #bools : boolean 관련 기능 제공
10. #arrays : 배열 관련 기능 제공
11. #lists , #sets , #maps : 컬렉션 관련 기능 제공 #ids : 아이디 처리 관련 기능 제공

### URL 링크
1. 타임리프에서 URL 생성시 아래와 같은 문법 사용
```html
<!--기본 URL-->
<li><a href="" th:href="@{/hello}">basic url</a></li>
<!--요청 파라미터 URL-->
<li><a href="" th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query param</a></li>
<!--PathVariable URL-->
<li><a href="" th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">path variable</a></li>
<!--요청 파라미터 + PathVariable-->
<li><a href="" th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}">path variable + query parameter</a></li>
```
2. 상대경로와 절대경로
   1. /hello - 절대경로
   2. hello - 상대경로

### 리터럴
1. 문자 : '' (문자 리터럴은 반드시 ''로 감싸야한다.)
   1. 주의
   ```html
   <!--오류-->
   <!--org.thymeleaf.exceptions.TemplateProcessingException: Could not parse as expression: "hello world!"-->
   <span th:text="hello world!"></span>
   <!--정상-->
   <span th:text="'hello world!'"></span>
   ```
2. 숫자 : 10
3. boolean : true, false
4. null : null

### 연산
1. Elvis 연산자
```html
<!--${nullData}이 참이면 ${nullData}를 출력하고 거짓이면 '데이터가 없습니다.' 출력-->
<li>${nullData}?: '데이터가 없습니다.' = <span th:text="${nullData}?: '데이터가 없습니다.'"></span></li>
```
2. No-Operation
```html
<!--${nullData}이 참이면 ${nullData}를 출력하고 거짓이면 타임리프가 실행되지 않는것 처럼 작동하여 HTML 컨텐츠의 기본데이터가 출력된다.-->
<li>${nullData}?: _ = <span th:text="${nullData}?: _">데이터가 없습니다.</span></li>
```

### 속성값 설정
1. 속성 설정
```html
<!--th:* 속성을 지정하면 타임리프는 기존 속성을 th:* 로 지정한 속성으로 대체-->
<input type="text" name="mock" th:name="userA" />
```
2. 속성 추가
```html
<!--속성 값의 뒤에 추가-->
<input type="text" class="text" th:attrappend="class=' large'" />
<!--속성 값의 앞에 추가-->
<input type="text" class="text" th:attrprepend="class='large '" />
<!--class 속성을 추가-->
<input type="text" class="text" th:classappend="large" />
```
3. checked 처리
```html
<!--HTML에서는 checked속성이 존재하면 참/여부에 관련없이 무조건 체크가 된다.
타임리프에서는 이를 편리하게 사용가능하다록 th:checked="false" checked속성을 삭제한다.--> 
<input type="checkbox" name="active" th:checked="true" /><br/>
<input type="checkbox" name="active" th:checked="false" /><br/>
```