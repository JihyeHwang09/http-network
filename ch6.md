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

지정한 디렉티브에는 파라미터가 있는 것과 없는 것도 있으며
여러 개의 디렉티브를 지정하는 경우에는 콤마`,`로 구분한다.
Cache-Control 헤더 필드의 디렉티브는 리퀘스트 및 리스폰스 할 때 사용할 수 있다.
```http request
Cache-Control: private, max-age=0, no-cache
```

사용 가능한 디렉티브를 리퀘스트와 리스폰스로 나눠서 나타낸다.

---

#### 캐시가 가능한지 여부를 나타내는 디렉티브
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

3) no-cache 디렉티브
- 클라이언트: 캐시했던 건 필요없으니까 오리진 서버에서 받아오라는 의미
- 캐시 서버
    - **중간 캐시 서버가 오리진 서버까지 리소스를 전송해야 한다.**
    - 캐시 서버는 리소스를 저장할 수 없다.
    
- 오리진 서버: **캐시해도 좋지만, 사용할 때 매번 오리진 서버에 확인**받으라는 의미


#### **클라이언트의 리퀘스트**로 no-cache 디렉티브가 사용된 경우
캐시로부터 오래된 리소스가 반환되는 것을 막기 위해 사용된다.
```http request
Cache-control: no-cache
```
#### **서버의 리스폰스**로 no-cache 디렉티브가 사용된 경우
헤더 필드 명이 지정된 경우, 지정된 헤더 필드만 캐시할 수 없다. 
-> 지정된 헤더 필드 이외에는 캐시하는 것이 가능하다.
이 파라미터는 리스폰스 디렉티브만 사용 가능하다.
```http request
Cache-control: no-cache=Location
```
---
#### 캐시가 보존 가능한 것을 제어하는 디렉티브
1) no-store 디렉티브
```http request
Cache-control: no-store 
```
리퀘스트(그와 대응되는 리스폰스) 혹은 리스폰스에 **기밀 정보가 포함되어 있을 때 사용한다.**
캐시는 리퀘스트, 리스폰스의 일부분을 로컬 스토리지에 보존되지 않도록 지정한다. 
---
#### 캐시 기한이나 검증을 지정하는 디렉티브
1) s-maxage 디렉티브
```http request
Cache-control: s-maxage=604800 (단위: 초)
```
max-age 디렉티브와 기능은 동일하다.
다른 점은 **여러 유저가 이용할 수 있는 공유 캐시 서버에만 적용된다**는 점이다.
-> 같은 유저에 반복해서 리스폰스를 반환하는 캐시 서버는 무효한 디렉티브이다.
s-maxage 디렉티브가 사용되는 경우, **Expires 헤더 필드와 max-age 디렉티브는 무시된다.**

2) max-age 디렉티브
```http request
Cache-control: max-age=604800 (단위: 초)
```
##### 클라이언트의 리퀘스트에서 max-age 디렉티브가 사용될 경우
 지정되었던 값보다 새로운 경우에는 캐시되었던 리소스를 받아들일 수 있다.
```http request
Cache-control: max-age=0 (단위: 초)
```
지정한값이 0이면, 캐시 서버는 리퀘스트를 항상 오리진 서버에 넘길 필요가 있다.

##### 서버의 리스폰스에서 max-age 디렉티브가 사용될 경우
**캐시 서버가 유효성의 재확인을 하지 않고 리소스를 캐시에 보존해 두는 최대 시간**을 나타낸다.

- HTTP/**1.1** 캐시 서버: 동시에 Expires 헤더 필드가 달린 경우, **max-age 디렉티브 지정을 우선하고, Expires 헤더를 무시한다.** 
- HTTP/**1.0** 캐시 서버: 반대로 max-age 디렉티브가 무시된다. 


3) min-fresh 디렉티브
```http request
Cache-control: min-fresh=60 (단위: 초)
```
캐시된 리소스가 적어도 지정된 시간은 최신 상태의 것을 반환하도록 캐시 서버에 요구한다.
ex) 60초로 지정되어 있는 경우, 60초 이내에 유효 기한이 끝나는 리소스를 리스폰스로 반환하면 안된다. 
(리소스의 유효기간은 min-fresh에 지정한 시간 이상(같거나 길어야)이어야 한다.)
---

1) max-stale 디렉티브
```http request
Cache-control: max-stale=3600 (단위: 초)  
```
캐시된 리소스의 유효 기한이 끝났더라도 받아들일 수 있음을 나타낸다.
디렉티브의 값이 지정되어 있지 않은 경우: 클라이언트는 아무리 시간이 경과했더라도 리스폰스를 받아 들인다. 

2) only-if-cached 디렉티브 
```http request
Cache-control: only-if-cached
```
- 클라이언트: 캐시 서버에 대해서 목적한 리소스가 **로컬 캐시에 있는 경우만 리스폰스를 반환하도록 요구**한다.
    -> 캐시 서버에서 리스폰스의 리로드와 유효성을 재확인하지 않도록 요구한다.
- 캐시 서버가 로컬 캐시로부터 응답할 수 없는 경우, `504 Gateway Timeout`상태를 반환한다.

3) must-revalidate 디렉티브
```http request
Cache-control: must-revalidate 
```

- **리스폰스의 캐시가 현재도 유효한지 아닌지의 여부를 오리진 서버에 조회를 요구**한다.
- 리퀘스트에서 **max-stale 디렉티브를 사용하고 있더라도 무시한다.(효과를 없앤다.)** 

- 프록시가 오리진 서버에 도달할 수 없고, 리소스를 다시 요구할 수 없는 경우, 
    캐시는 클라이언트에 `504 Gateway Timeout`를 반환한다.


4) proxy-revalidate 디렉티브
```http request
Cache-control: proxy-revalidate
```
모든 캐시 서버에 대해서 이후의 리퀘스트로 해당 리스폰스를 반환할 때는 **반드시 유효성 재확인을 하도록 요구**한다.

5) no-transform 디렉티브
```http request
Cache-control: no-transform
```
리퀘스트와 리스폰스의 어느 쪽에 있어도 **캐시가 엔티티 바디의 미디어 타입을 변경하지 않도록 지정**한다.
-> 캐시 서버 등에 의해서 이미지가 압축되는 것을 방지한다.

---
#### Cache-Control 확장
1) cache-extension 토큰
```http request
Cache-control: private, community="UCI"
```
cache-extension 토큰을 사용하여 **디렉티브를 확장할 수 있다.**
community라는 디렉티브는 헤더 필드에는 없지만, extension tokens에 의해서 추가할 수 있다.
캐시 서버가 새로운 디렉티브 `community`를 이해하지 못할 경우, 무시된다.
(`community`는 이해할 수 있는 캐시 서버에 대해서만 의미가 있다.) 
---

### 6.3.2. Connection
#### Connection의 역할
1) 프록시에 더 이상 전송하지 않는 헤더 필드를 지정
```http request
Connection: 더 이상 전송하지 않는 헤더 필드명
```
클라이언트의 리퀘스트 혹은 서버의 리스폰스에서 Connection 헤더 필드를 사용하며,
프록시 서버에 더 이상 전송하지 않는 헤더 필드(hop-by-hop 헤더)를 지정할 수 있다. 

ex) 클라이언트 -> 프록시 서버 -> 오리진 서버

- 클라이언트 -> 프록시 서버
```http request
GET/HTTP/1.1
Upgrade: HTTP/1.1
Connection: Upgrade
```

- 프록시 서버 -> 오리진 서버
```http request
GET/HTTP/1.1
```


2) 지속적 접속 관리
```http request
Connection: Close
```
HTTP/1.1에서는 **지속적 접속**이 디폴트
-> 리퀘스트를 송신했던 클라이언트는 접속이 계속 유지되면서 추가 리퀘스트를 송신하도록 한다.
서버 측에서 명시적으로 접속을 끊고 싶을 때, Connection 헤더 필드에 Close라고 지정한다. 
ex)
클라이언트 -> 서버
```http request
GET/HTTP/1.1
Connection: Keep-Alive
```

서버 -> 클라이언트 
```http request
HTTP/1.1 200 OK
#...
Keep-Alive: timeout=10, max=500
Connection: Keep-Alive
#...
```
HTTP/1.1 이전의 버전의 HTTP에서는 지속적인 접속이 디폴트가 아니었다.
-> 오래된 버전의 HTTP에서 지속적 접속을 하고 싶은 경우,
Connection 헤더 필드에 Keep-Alive 헤더 필드와 Connection 헤더 필드를 붙여서 리스폰스한다.
```http request
Connection: Keep-Alive
```
---
### 6.3.3. Date
Date 헤더 필드: HTTP 메시지를 생성한 날짜를 나타낸다. 

### 6.3.4. Pragma
```http request
Pragma: no-cache
```
- `HTTP/1.1`보다 오래된 버전의 흔적으로, `HTTP/1.0`과의 후방 호환성만을 위해서 정의되어 있는 헤더 필드
- 일반 헤더 필드이지만, **클라이언트의 리퀘스트에서만 사용**된다.(서버의 리스폰스에서는 사용 X!!)
클라이언트는 캐시된 리소스의 리스폰스를 원하지 않음을 모든 중간 서버에 알리기 위해 사용된다. 
- 중간 서버의 HTTP 버전을 모두 파악한 후에 리퀘스트를 보내는 일은 현실적으로 없다.
    -> 양쪽을 보내는 경우도 있다.
```http request
Cache-Control: no-cache
Pragma: no-cache
```
### 6.3.5. Trailer
메시지 바디의 뒤에 기술되어 있는 헤더 필드를 **미리 전달 가능**
`HTTP/1.1`에 구현되어 있는 **청크 전송 인코딩**을 사용하고 있는 경우에 **사용 가능**
ex) 
클라이언트 -> 중간 서버
```http request
HTTP/1.1 200 OK
Date: Tue, 03, Jul 2012 04:40:56 GMT
Content-Type: text/html
#...
Transfer-Encoding: chunked
# Trailer 헤더 필드에 Expires를 지정한다.
Trailer: Expires

#(메시지 바디)
# 메시지 바디의 뒤(청크의 길이가 0의 뒤)에 Expires 헤더 필드를 나타내고 있다. 
0
Expires: Tue, 28 Sep 2004 23:59:59 GMT
```

### 6.3.6. Transfer-Encoding
메시지 바디의 전송 코딩 형식을 지정하는 경우에 사용
```http request
# 청크 전송 코딩이 유효한 상태
Transfer-Encoding: chunked
```

### 6.3.7. Upgrade
HTTP 및 다른 프로토콜의 새로운 버전이 통신에 이용되는 경우 사용
지정하는 대상이 전혀 다른 통신 프로토콜이라고 하더라도 문제 없다. 

```http request

```

### 6.3.8. Via
클라이언트와 서버 간의 리퀘스트 혹은 리스폰스 메시지의 경로를 알기 위해 사용
프록시나 게이트웨이는 자신의 서버 정보를 `Via 헤더 필드`에 추가한 뒤에 메시지를 전송한다.

`Via 헤더 필드`는 전송된 메시지의 추적과 리퀘스트 루프의 회피 등에 사용되기 때문에
**프록시를 경유하는 경우에는 반드시 부가해야 한다.** 

### 6.3.9. Warning
HTTP/1.0 리스폰스 헤더(Retry-After)가 HTTP/1.1에서 변경된 것
리스폰스에 관한 추가 정보(캐시에 관한 문제의 경고)를 유저에게 전달
```http request
#Warning: [경고 코드][경고한 호스트:포트 번호]"[경고문]" ([날짜])
Warning: 113 gw hackr.jp:8080 "Heuristic expiration"Tue, 03 Jul =>
2012 05:09::44 GMT
```

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

