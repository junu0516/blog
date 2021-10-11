

+++
title = "Http 메시지와 Https 통신"
tags = ["네트워크","CS스터디"]
categories = ["네트워크"]
date = "2021-10-10"

+++



> Http 메시지의 구조와, 암호화가 추가된 Https 통신의 특징에 대해 알아보도록 한다.

<br>

## 1. Http 메시지 구조

```
모든 Http 통신에서의 정보는 Http 메시지로부터 나온다!
```

- Http 프로토콜이라고 하면 보통 서버/클라이언트 모델을 따르는 네트워크 프로토콜을 일컫음
- 서버와 클라이언트는 각각 요청 & 응답(Request, Response)를 통해 서로 정보를 주고받음
- Http는 __비연결 지향(Connectionless)__ 적인 특징으로 인해, 일단 클라이언트가 보낸 요청에 대해 응답이 오면 연결을 끊음
  - 이를 통해 과도한 서버에의 부하를 줄일 수 있는 것

<br>

### 1-1. HTTP 메시의 구조

<img src="https://media.vlpt.us/post-images/codemcd/6ce81a10-e267-11e9-bbca-93d7aa4138bb/HTTP3-4.png" alt="img" style="zoom: 67%;" />

- Http 통신에서 교환되는 정보를 보통 Http 메시지라고 부름

  - 리퀘스트 메시지와 리스폰스 메시지의 총 2가지가 존재

- Http 메시지는 __`복수행`__ 의 데이터로 구성된 텍스트 문자열의 형태이며 크게 __`메시지 헤더`__ 와 __`메시지 바디`__ 로 구성되어있으며, 각각은 __`개행 문자(CR+LF)`__ 로 구분됨

  - 혹은 메시지 헤더를 시작줄과 헤더 블록(필드)로 나눌 수도 있음
  - __메시지 헤더__ : 서버와 클라이언트가 반드시 처리해야 하는 리퀘스트나 리스폰스의 내용 및 속성이 정의되어 있음
    - __HTTP 통신에 필요한 모든 부가정보가 헤더에 명시__ 되어 있는 것
    - ex) 메시지 바디의 크기, 인증, 클라이언트 정보, 캐시 관리 정보 등등
  - __메시지 바디__ : 전송되는 데이터 그 자체가 들어 있음
    - 여기에는 HTML 문서와 json 형식의 데이터에서부터 이미지 및 영상에 이르기까지 byte로 표현 가능한 모든 데이터가 전송될 수 있음

- 리퀘스트 헤더의 첫 시작 라인은 __`리퀘스트 라인(요청 라인)`__ 으로 구성됨

  - __리퀘스트 라인(요청 라인)__ : 요청에 사용될 메소드(GET, POST 등)와 요청 URI, 그리고 사용되는 HTTP 버전 정보가 명시되어 있음

    ``` 
    GET/search HTTP/1.1 
    ```

- 리스폰스 헤더의 첫 시작 라인은 __`상태 라인(응답 라인)`__ 으로 구성됨

  - __상태 라인(응답 라인)__ : 리스폰스 결과를 나타내는 상태코드와 설명, 그리고 사용된 HTTP 버전 정보가 명시되어 있음 

    ```
    HTTP/1.1 200 OK
    ```

  - __keep-alive 헤더__ : 아래와 같이 명시되며, 연결을 계속 유지하겠다는 것을 의미함(HTTP 1.1부터 기본적으로 동작)

    ```
    Connection:Keep-Alive
    Keep-Alive: timeout=5, max=1000
    ```

    - 기본적으로 Http 통신은 TCP 통신을 기반으로 동작하는데, TCP 통신은 일단 전송하면 연결이 끊어지므로 Http 역시 전송이 끝나면 연결이 끊어지며 이후의 일에 대해서는 신경쓰지 않음
    - 따라서 이러한 문제를 막고자 Keep-Alive 헤더가 사용되는 것이며, __특정 시간(timeout)__ 동안 __최대 요청(max)__ 의 수를 명시함

    

### 1-2. 자주 사용되는 HTTP 메소드 및 응답(상태) 코드

- HTTP 메소드

  - __GET__ : 서버로부터 특정 정보(혹은 문서)를 가져올 때 사용
  - __HEAD__ : GET과 비슷한 동작을 하되, 서버의 응답에는 헤더만 포함되며 본문은 존재하지 않음
    - 단순히 리소스의 존재 및 변경 여부와 같은 정보가 필요한 경우에는 본문에 리소스를 포함시키지 않고 헤더의 확인만으로도 충분하기 때문
    - 단, 서버개발을 할 경우에는 HEAD 요청으로 응답받은 헤더와 GET 요청으로 응답받은 헤더의 정확한 일치를 항상 염두에 둬야 함!
  - __POST__ : 서버가 처리해야 할 데이터를 전송하며, 보통 새로운 데이터를 서버에 입력할 경우 사용
    - 데이터는 메시지 바디에 포함되므로(ex. 요청 json), 헤당 메소드 사용시에는 본문이 존재
  - __PUT__ : 서버에 존재하는 데이터를 수정하고자 할 경우 사용(새 문서를 만들거나, 요청한 본문으로 교체)
    - 마찬가지로 본문이 존재하며, POST와 달리 __`멱등성(Idempotence)`__ 을 지님(GET, DELETE도 동일)
  - __PATCH__ : PUT과 마찬가지로 자원의 수정을 위한 메소드지만, 일부만을 수정할 경우에 사용됨
    - PUT의 경우 요청을 일부만 보낼 경우에는 나머지 부분은 default값으로 수정되는 문제가 있음
      - 자칫 보내지 않은 데이터가 null로 변경될 수 있기 때문에, 변경되지 않는 데이터도 모두 전달해야 함
    - 반면, PATCH의 경우에는 요청 본문에 포함된 부분만 수정됨
  - __DELETE__ : 서버에서 특정 정보(혹은 문서)를 제거
    - 하지만 서버가 요청을 무시할 수도 있기 때문에 클라이언트는 항상 삭제가 정상적으로 수행되는 것을 보장받지 못함

- 상태 코드

  - 100-199 : 정보
  - 200-299 : 성공(200 OK)
  - 300-399 : 리디렉션
  - 400-499 : 클라이언트 에러(404 Not Found)
  - 500-599 : 서버 에러(500 Internal Server Error)

<br>

## 2. Https 통신

<img src="https://kyun2da.dev/static/c146b5a07e442c48d900034b2610ae3e/37523/https_process.png" alt="https에 대해서 | Kyun2da.dev" style="zoom:85%;" />





### 2-1. Http 통신의 한계와 Https

- **http 통신의 한계**

  - 데이터를 암호화하지 않고, 사용자가 누군지를 따로 검증하지 않기 때문에 보안에 취약함
  - 감청 혹은 데이터 변조의 가능성이 있음

- **https(Hypertext Transfer Protocol Over Secure Socket Layer)**

  - 보안이 강화된 http 통신으로 http(80번 포트)와 달리 443번 포트 사용

  - 모든 http 요청과 응답 데이터가 네트워크 계층을 거치기 전에 암호화됨

  - https는 http 하부에 SSL과 같은 보안 계층을 따로 제공하여 동작함

  - 응용계층과 전송계층 사이에 독립적인 보안 계층이 위치

  - 다시 말해, https는 http에 보안계층이 조합된 프로토콜인 것!

<br>

### 2-2. SSL

- 네트워크에 통신 보안을 제공하기 위한 계층(혹은 규약)
- https 통신에서는 보안 계층을 거치면서 모든 요청 및 응답 데이터가 네트워크로 보내지기 전에 암호화됨
- 암호화 방식은 크게 대칭키 암호화, 공개키 암호화 방식이 존재
  - **대칭키 암호화 방식**
    - 암호화, 복호화에 동일한 키(대칭키)가 사용됨
    - 수신자, 송신자가 동일한 키를 공유해야 하며, 대칭키가 통신 도중 유출되면 제 3자가 암호를 복호화할 수 있기 때문에 위험

  - **공개키 암호화 방식**
    - 위에서 언급한 대칭키 암호화 방식의 단점을 극복한 방식
    - 암호화, 복호화에 서로 다른 키가 사용됨
    - A키로 암호화하면 B키로 복호화해야 하고, 반대로 B키로 암호화하면 A키로 복호화하는 방식
    - 보통 암호화에 사용되는 키는 공개되어 있음(public key)
    - 복호화에 사용되는 키(secret key)는 호스트만 알 수 있으며 이를 통해 메시지를 안전하게 주고받을 수 있는 것
    - 대신 두 개의 키를 사용하므로 계산이 대칭키 암호화에 비해 상대적으로 느림

  - (SSL) **디지털 인증서 & 디지털 서명**
    - 클라이언트와 서버간의 통신을 공인된 제 3자(혹은 기업이나 업체)가 보증해주는 문서
    - 디지털 서명을 통해 누가 메시지를 보낸 것이며, 메시지가 위조되지 않았음을 증명하는 것

    - 데이터를 안전하게 전달할 뿐만 아니라, 데이터 제공자의 신원을 보장할 수 있음
      - 송신자는 비공개 키(secret key)로 정보를 암호화하여, 공개 키와 함께 암호화된 데이터를 전송
      - 수신자는 공개 키로 암호화된 정보를 복호화 하여 읽어들임
      - 여기서 데이터를 공개 키로 복호화했다는 것은, 해당 데이터가 공개 키와 한 쌍을 이루는 비공개 키에 의해 암호화되었음을 의미하는 것
      - 다시 말해, 제대로 전달된 공개 키가 송신자의 신원을 보장해주는 것(디지털 서명)

  - 암호화된 http 메시지를 교환하기 전 클라이언트와 서버는 **SSL 핸드쉐이크** 과정을 거쳐야 함
    - 서버와 클라이언트가 서로 연락하여 클라이언트가 여러 암호화 방식을 제시하면 서버가 여기서 하나를 선택하여 공개 키와 함께 응답
    - 클라이언트는 이후 암호화 통신에 사용할 키를 생성하여 서버에 전달하며, 전달된 키는 서버가 전달한 공개 키에 의해 암호화되어 보내짐
    - 위의 과정이 마무리되면 클라이언트와 서버 모두 finished 메시지를 주고 받으며, 이후부터는 클라이언트가 생성한 키를 이용하여 데이터가 암호화되는 것 
