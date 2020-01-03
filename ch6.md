# 6장 HTTP 헤더
HTTP 헤더의 구조와 각 헤더 필드의 역할에 대해 알아보자.

## 6.1. HTTP 메시지 헤더
`메시지 헤더`: 클라이언트와 서버 처리에 필요한 주요 정보가 거의 다 여기에 있다.
`개행 문자(CR+LF)`
`메시지 바디`: 사용자와 리소스를 필요로 하는 정보가 있다. 

HTTP 프로토콜의 리퀘스트와 리스폰스에는 반드시 메시지 헤더가 포함되어 있는데,
메시지 헤더에는 클라이언트나 서버가 리퀘스트나 리스폰스를 처리하기 위한 정보가 들어 있다. 

### 리퀘스트의 HTTP 메시지
메소드, URI, HTTP 버전, HTTP 헤더 필드 등으로 구성되어 있다. 

### 리스폰스의 HTTP 메시지
HTTP 메시지와 HTTP 버전, 상태 코드(코드와 설명), HTTP 헤더 필드 등으로 구성되어 있다.

헤더 필드는 HTTP 버전과 확장 사양에 따라서 지원하는 내용이 달라진다. 

---

## 6.2. HTTP 헤더 필드
### 6.2.1. HTTP 헤더 필드는 중요한 정보를 전달한다
`HTTP 헤더 필드`는 HTTP 메시지를 구성하는 요소의 하나이다.
헤더 필드는 HTTP 프로토콜 중에서 클라이언트와 서버간의 통신에서 리퀘스트에도 리스폰스에도 사용되고 있고,
부가적으로 중요한 정보를 전달하는 역할을 담당한다.

메시지 바디의 크기나 사용하고 있는 언어, 인증 정보 등을 브라우저나 서버에 제공하기 위해 사용되고 있다.
> 헤더 필드에서는 부가적인 정보를 다루는 일이 많다

### 6.2.2. HTTP 헤더 필드의 구조
HTTP 헤더 필드는 헤더 필드 명과 필드 값으로 구성되어 있고, 콜론`:`으로 나뉘어져 있다.
```http request
헤더 필드 명: 필드 값
```
메시지 바디의 오브젝트의 타입을 가리키는 `Content-Type`이라는 HTTP 헤더 필드가 포함되어 있다.
```http request
Content-Type: text/html
```
- Content-Type: 필드명
- "text/html": 필드값

하나의 HTTP 헤더 필드가 여러 개의 필드값을 가질 수 있다.
```http request
Keep-Alive:timeout=15, max=100
```

cf) HTTP 헤더 필드가 중복된 경우
사양으로 명확하게 정해져 있지 않기 때문에 브라우저마다 다른 동작을 하게 된다. 
어떤 브라우저는 최초의 헤더 필드를 우선적으로 처리하고,
 어떤 브라우저는 마지막 헤더 필드를 우선적으로 처리한다.
---

### 6.2.3. 4종류의 HTTP 헤더 필드
- `일반적 헤더 필드(General Header Fields)`:
 리퀘스트 메시지와 리스폰스 메시지 둘 다 사용되는 헤더
    
- `리퀘스트 헤더 필드(Request Header Fields)`
클라이언트 측에서 서버 측으로 송신된 리퀘스트 메시지에 사용되는 헤더
리퀘스트의 부가적 정보와 클라이언트의 정보, 리스폰스의 콘텐츠에 관한 우선 순위 등을 부가한다.

- `리스폰스 헤더 필드(ResponseHeader Fields)`
서버 측에서 클라이언트 측으로 송신한 리스폰스 메시지에 사용되는 헤더
리스폰스의 정보와 서버의 정보, 클라이언트의 추가 정보 요구 등을 부가한다.

- `엔티티 헤더 필드(Entity Header Fields)`
리퀘스트 메시지와 리스폰스 메시지에 포함된 엔티티에 사용되는 헤더
콘텐츠 갱신 시간 등의 엔티티에 관한 정보를 부가한다. 

### 6.2.4. HTTP/1.1 헤더 필드 일람

### 6.2.5. HTTP/1.1 이외의 헤더 필드
### 6.2.6. End-to-end 헤더와 Hop-by-hop 헤더

#### `End-to-end 헤더`
이 카테고리에 분류된 헤더는 **리퀘스트나 리스폰스의 최종 수신자에게 전송된다.**
캐시에서 구축된 리스폰스 중 보존되어야 하고, 다시 전송되지 않으면 안되도록 되어 있다.

#### `Hop-by-hop 헤더`
이 카테고리에 분류된 헤더는 **한 번 전송에 대해서만 유효하고 캐시와 프록시에 의해서 전송되지 않는 것도 있다.**
HTTP/1.1과 그 이후에서 사용되는 Hop-by-hop 헤더는 Connection 헤더 필드에 열거해야 한다.
아래의 8개 헤더 필드 이외에는 모두 End-by-end 헤더에 분류된다.
- `Connection`
- `Keep-Alive`
- `proxy-Authenticate`
- `Proxy-Authorization`
- `Trailer`
- `TE`
- `Transfer-Encoding`
- `Upgrade`

---

## 6.3. HTTP/1.1 일반 헤더 필드
일반 헤더 필드는 리퀘스트 메시지와 리스폰스 메시지 양쪽에서 사용되는 헤더

### 6.3.1. Cache-Control
디렉티브로 불리는 명령을 사용하여 캐싱 동작을 지정한다.
> Cache-Control 헤더 필드는 캐시의 동작을 지정한다.

### 6.3.2. Connection
지정한 디렉티브에는 파라미터가 있는 것과 없는 것도 있으며
여러 개의 디렉티브를 지정하는 경우에는 콤마`,`로 구분한다.
Cache-Control 헤더 필드의 디렉티브는 리퀘스트 및 리스폰스 할 때 사용할 수 있다.
```http request
Cache-Control: private, max-age=0, no-cache
```
#### Cache-Control 디렉티브 일람
사용 가능한 디렉티브를 리퀘스트와 리스폰스로 나눠서 나타낸다.

캐시가 가능한지 여부를 나타내는 디렉티브
1) public 디렉티브
```http request
Cache-control: public
``` 
Public 디렉티브가 사용되는 경우, **다른 유저에게도 돌려줄 수 있는 캐시를 해도 좋다**는 걸 명시적으로 나타냄

2) private 디렉티브
```http request
Cache-control: private
```
리스폰스는 특정 유저만을 대상으로 함
public 디렉티브와 기능이 반대
캐시 서버는 특정 유저를 위해서 리소스를 캐시할 수 있지만,
다른 유저로부터 같은 리퀘스트가 온다고 하더라도 그 캐시를 반환하지 않도록 한다.

### 6.3.3. Date
### 6.3.4. Pragma
### 6.3.5. Trailer
### 6.3.6. Transfer-Encoding
### 6.3.7. Upgrade
### 6.3.8. Via
### 6.3.9. Warning
---
## 6.4. 리퀘스트 헤더 필드
### 6.4.1. Accept
### 6.4.2. Accept-Charset
### 6.4.3. Accept-Encoding
### 6.4.4. Accept-Language
### 6.4.5. Authorizaiton
### 6.4.6. Expect
### 6.4.7. Form
### 6.4.8. Host
### 6.4.9. If-Match
### 6.4.10. If-Modified-Since
### 6.4.11. If-None-Match
### 6.4.12. If-Range
### 6.4.13. If-Unmodified-Since
### 6.4.14. Max-Forwards
### 6.4.15. Proxy-Authorizaiton
### 6.4.16. Range
### 6.4.17. Referer
### 6.4.18. TE
### 6.4.19. User-Agent
---
## 6.5. 리스폰스 헤더 필드
### 6.5.1. Accept-Ranges
### 6.5.2. Age
### 6.5.3. ETag
### 6.5.4. Location
### 6.5.5. Proxy-Authenticate
### 6.5.6. Retry-After
### 6.5.7. Server
### 6.5.8. Vary
### 6.5.9. WWW-Authenticate
---
## 6.6. 엔티티 헤더 필드
### 6.6.1. Allow
### 6.6.2. Content-Encoding
### 6.6.3. Content-Language
### 6.6.4. Content-Length
### 6.6.5. Content-Location
### 6.6.6. Content-MD5
### 6.6.7. Content-Range
### 6.6.8. Content-Type
### 6.6.9. Expires
### 6.6.10. Last-Modified

---
## 6.7. 쿠키를 위한 헤더 필드
### 6.7.1. Set-Cookie
### 6.7.2. Cookie
---
## 6.8. 그 이외의 헤더 필드
### 6.8.1. X-frame-Option
### 6.8.2. X-XSS-Protection
### 6.8.3. DNT
### 6.8.4. P3P

