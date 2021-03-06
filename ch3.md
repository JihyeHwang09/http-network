# 3장 HTTP 정보는 HTTP 메시지에 있다
HTTP 통신에는 클라이언트에서 서버로 보내는 리퀘스트와
서버에서 클라이언트로 보내는 리스폰스가 있다.
이 리퀘스트와 리스폰스가 어떻게 동작하는지 살펴보자.

## 3.1. HTTP 메시지

## 3.2. 리퀘스트 메시지와 리스폰스 메시지의 구조
- 리퀘스트 라인: 리퀘스트에 사용하는 메소드, 리퀘스트 URI, HTTP 버전
- 상태 라인: 리스폰스 결과(상태 코드, 설명), HTTP 버전
- 헤더 필드: 일반, 리퀘스트, 리스폰스, 엔티티 헤더 필드 포함
- 그 외: HTTP의 RFC에는 없는 헤더 필드(쿠키 등)가 포함되는 경우가 있다. 

## 3.3. 인코딩으로 전송 효율을 높이다
### 3.3.1. 메시지 바디와 엔티티 바디의 차이
- 메시지(message)
: **HTTP 통신의 기본 단위**로 옥텟 시퀀스(Octet sequence, octet은 8비트)
- 엔티티(entity)
    - 리퀘스트와 리스폰스의 페이로드(payload, 부가물)로 전송되는 정보
    - 엔티티 헤더 필드와 엔티티 바디로 구성 
    
HTTP 메시지 바디의 역할: 리퀘스트와 리스폰스에 관한 엔티티 바디를 운반하는 일

#### 전송 코딩이 적용된 경우
기본적으로 `메시지 바디`와 `엔티티 바디`는 같다.
But, **전송 코딩이 적용된 경우, `엔티티 바디`의 내용이 변화한다.**
-> **`메시지 바디`와 `엔티티 바디`는 달라진다.**

### 3.3.2. 압축해서 보내는 콘텐츠 코딩
`콘텐츠 코딩`
- 엔티티에 적용하는 인코딩
- 서버에서 **엔티티 정보를 유지한채로 압축**한다.
- 콘텐츠 코딩된 엔티티는 수신한 클라이언트 측에서 디코딩한다.
- 주요 콘텐츠 압축
    - gzip(GNU zip)
    - compress(UNIX 표준 압축)
    - deflate(zlib)
    - identity(인코딩 없음)
    
### 3.3.3. 분해해서 보내는 청크 전송 코딩
HTTP 통신에서는 리퀘스트했었던 리소스 전부에서 엔티티 바디의 전송이 완료되지 않으면,
브라우저에 표시되지 않는다.
사이즈가 큰 데이터를 전송하는 경우에 데이터를 분할해서 조금씩 표시할 수 있다.
- `청크 전송 코딩(Chunked transfer Coding)`: 엔티티 바디를 분할하는 기능
    서버에서 엔티티 바디를 작게 쪼개고 나서 이 분해한 청크(덩어리)를 송신한다.
    수신한 클라이언트 측에서는 원래의 엔티티 바디로 디코딩한다.

## 3.4. 여러 데이터를 보내는 멀티 파트
HTTP도 멀티파트에 대응하고 있어 하나의 메시지 바디 내부에 엔티티를 여러 개 포함시켜 보낼 수 있다.
주로 이미지, 텍스트 파일 등을 업로드할 때 사용한다.
### 멀티 파트의 종류
- `multipart/form-data`: Web 폼으로부터 파일 업로드에 사용한다.
- `multipart/byteranges`: 상태 코드 206(Partial Content) 리스폰스 메시지가 **복수 범위의 내용을 포함하는 때에 사용**된다.


HTTP 메시지로 멀티파트를 사용할 때는 `Content-type` 헤더 필드를 사용한다.
멀티파트 각각의 엔티티를 구분하기 위해 "boundary" 문자열을 사용한다.
**각 엔티티의 선두**에는 "boundary" 문자열 앞에 "`--`"를 삽입한다. 
    ex) "--AaB03x", "--THIS_STRING_SEPARATES"
    
**멀티파트의 마지막**에는 그 문자열의 마지막 부분에 "`--`"를 삽입해서 마무리한다.
    ex) "--AaB03x--", "--THIS_STRING_SEPARATES--"

멀티파트는 파트마다 헤더 필드가 포함된다.
파트의 중간에 멀티파트를 만드는 것과 같이 내부에 포함될 수 있다. 

## 3.5. 일부분만 받는 레인지 리퀘스트
`리줌(resume)`: 이전에 다운로드를 한 곳에서부터 다운로드를 재개할 수 있는 기능
이 기능을 실현하기 위해서는 엔티티의 범위를 지정해서 다운로드를 할 필요가 있다.

`레인지 리퀘스트(Range Request)`: 범위를 지정해서 리퀘스트하는 것 

레인지 리퀘스트를 사용할 경우, 전체 10,000 바이트 정도 크기의 리소스에서 
5,001 ~ 10,000 바이트의 범위(바이트 레인지)만을 리퀘스트할 수 있다. (한 이미지의 반절만 다운로드해 둔 후, 다음에 이어서 다운로드 가능)

레인지 리퀘스트를 할 때는 `Range 헤더 필드`를 사용해서 리소스의 바이트 레인지를 지정한다.
### 바이트 레인지의 형식
- 5,001 ~ 10,000 바이트
```http request
Range: bytes = 5001-10000
```
- 5,001 바이트 이상
```http request
Range: bytes=5001-
```
- 처음부터 3,000 바이트까지, 그리고 5,000 ~ 7,000 바이트까지의 복수 범위
```http request
Range: bytes=3000, 5000-7000
```
레인지 리퀘스트에 대한 리스폰스는 **상태 코드 206 Partial Content**라는 리스폰스 메시지가 되돌아온다.
복수 범위의 레인지 리퀘스트에 대한 리스폰스는 `multipart/byteranges`로 리스폰스가 되돌아온다.
서버가 레인지 리퀘스트에 지원하지 않는 경우, **상태 코드 200 OK**라는 리스폰스 메시지로 **완전한 엔티티가 되돌아온다.**

## 3.6. 최적의 콘텐츠를 돌려주는 콘텐츠 네고시에이션
`콘텐츠 네고시에이션(Content Negotiation)`
클라이언트와 서버가 제공하는 리소스의 내용에 대해서 교섭하는 것이다.
클라이언트에 더욱 적합한 리소스를 제공하기 위한 구조이다.
콘텐츠 네고시에이션은 제공하는 리소스를 언어와 문자 세트, 인코딩 방식을 기준으로 판단하고 있다.

### 콘텐츠 네고시에이션의 종류
- `서버 구동형 네고시에이션(Server-driven Negotiation)`
    - 서버 측에서 네고시에이션하는 방식
    - 서버 측에서 리퀘스트 헤더 필드의 정보를 참고해서 자동적으로 처리한다. 
    - 브라우저가 보내는 정보를 근거로 하기 때문에 유저에게 정말로 적절한 것이 선택되었다고 보기 어렵다. 
- `에이전트 구동형 네고시에이션(Agent-driven Negotiation)`
    - 클라이언트 측에서 네고시에이션하는 방식
    - 브라우저에 표시된 선택지 중에서 유저가 수동으로 선택한다.
    ex) JavaScript 등을 사용해서 웹 페이지에서 자동적으로 이것을 정하는 것도 있다. 
    OS의 종류나 브라우저의 종류 등에 의해서 PC용과 스마크폰용 웹 페이지를 자동으로 전환하는 것
- `트랜스페어런트 네고시에이션(Transparent Negotiation)`
    - 서버 구동형과 에이전트 구동형을 혼합한 것
    - 서버와 클라이언트가 각각 콘텐츠 네고시에이션을 하는 방식

