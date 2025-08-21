# HTTP

**HTTP**는 TCP 위에서 작동하는 텍스트 기반 client-server 프로토콜이며 주로 80번 포트를 사용한다.  
최근에는 보안 기능이 추가된 TLS 기반 HTTPS를 사용하고 HTTPS는 443 포트를 사용

HTTP는 html, text, 이미지, 음성, 영상파일, json, xml등 다양한 형태를 주로 받을 수 있음  
현재 1.1버전이 가장 많이 사용되고, UDP를 기반으로 한 HTTP/3버전이 개발중

HTTP는 **request**와 **response** 메세지로 나뉘고, 각각은 **header**와 **body**로 나뉨

## 특징

HTTP는 stateless, conntectionless 두가지 특징이 있음

### stateless

서버가 클라이언트의 상태를 보존하지 않는 특성

- 장점: 서버의 확장성이 높음
- 단점: 클라이언트에 추가 데이터 전송을 요구함 ex) 쿠키

### connectionless

서버가 클라이언트와의 연결을 유지하지 않는 특성

- 장점: 서버 자원을 효율적으로 사용할 수 있음
- 단점: 지속적인 TCP 3-way handshake 오버헤드 발생
  - 단점 해결을 위해 HTTP/1.1버전부터 연결을 끊지 않고 재사용하는 **persistent connection**이 제공됨

## URI

URI는 uniform resource identifier의 약자로 자원을 식별하는 방식이다  
URN과 URL 두가지 방식이 있다

- URN
  - 자원의 이름(name) 으로 식별하는 방식
- URL
  - 자원의 위치(location)으로 식별하는 방식

주로 URL 방식을 많이 사용한다.  

### URL

URL은 7가지 부분으로 나뉜다

```
scheme://[userinfo@]host[:port][/path][?query][#fragment]
예시)https://www.google.com:443/search?q=hello&hl=ko
```

- 프로토콜 : https
  - http, https, ftp 등의 프로토콜

- userinfo
  - URL에 사용자 정보를 포함에서 인증
  - 거의 사용하지 않음

- 호스트 이름: www.google.com
  - 도메인명 또는 ip주소 사용 가능

- 포트 번호 : 443
  - http는 80, https는 443포트가 기본값, 기본값인 경우 생략 가능
- path: search
  - 리소스의 경로
- query string: q=hello&hl=ko
  - key=value 형태
  - ?으로 시작하고 &로 추가할 수 있음
- fragment
  - html 내부 북마크 등에 사용 ex)위키피디아


## HTTP Request

### Type

요청 메세지는 일반적으로 6개의 타입을 가진다

- **GET**
  - 클라이언트가 자원을 받고싶을 때 사용
- **HEAD**
  - GET과 유사하지만 그 자원의 정보만을 가져옴
- **POST**
  - 클라이언트가 정보를 서버에게 전송할 때 사용
- **PUT**
  - 자원을 수정할 때 사용
  - 리소스가 이미 있다면 요청 정보로 대체하고, 없다면 생성함
- **PATCH**
  - 리소스를 부분 변경할 때 사용
- **DELETE**
  - 자원을 삭제할 때 사용

### Format

해당하는 자원을 요청  
GET의 경우에는 body가 없지만 POST와 같은 경우는 body가 존재함  
각각의 라인은 **\r\n**으로 구분하고 헤더와 바디는 **\r\n\r\n**으로 구분  

GET 메세지 예시

```http
GET /page1.htm HTTP/1.1
User-Agent: Mozilla/5.0
Accept-Language: en-US
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Encoding: gzip, defalte
Host: example.com
Connection: Keep-Alive  
```

| Key             | Value                                                        |
| --------------- | ------------------------------------------------------------ |
| request line    | 요청 타입, 요청 경로, HTTP 버전 명시                         |
| User-Agent      | 브라우저 정보                                                |
| Accept-Language | 언어 정보                                                    |
| Accept          | 받을 수 있는 데이터의 타입                                   |
| Accept-Encoding | 클라이언트가 이해 가능한 인코딩 타입                         |
| Host            | 접근하고자 하는 호스트 이름                                  |
| Connection      | 연결 유지 여부<br />연결을 유지할 경우에는 Keep-Alive<br />연결을 끊을 경우에는 Close |

## HTTP Response

### code

HTTP 응답 메세지에는 응답의 상태를 알려주는 상태 코드가 있음

#### 200

요청이 성공정으로 처리되었음을 의미

- 200 OK
  - 요청이 성공적이었고, 서버가 자원을 보냄
- 201 Created
  - 요청이 성공적이었고, 새로운 리소스가 생성됨
- 202 Accept
  - 요청이 접수되었지만 처리되지 않음
- 204 No content
  - 요청이 성공적이었지만, 응답 페이로드에 보낼 데이터가 없음

#### 300

요청을 완료하기 위해 추가 조치 필요  
응답 헤더의 결과에 Location 헤더가 있으면 그 위치로 자동 이동

- 영구 리다이렉션: 리소스의 URI가 영구적으로 이동
  - 301 Moved Permanently
    - 리다이렉트 요청시 요청 메소드가 GET으로 변하고 본문이 제거될 수 있음
  - 308 Permanent Redirect
    - 요청 메소드와 본문 유지
- 일시적 리다이렉션: 리소스의 URI가 일시적으로 변경
  - 302 Found
    - 리다이렉트 요청시 요청 메소드가 GET으로 변하고 본문이 제거될 수 있음
  - 307 Temporary Redirect
    - 요청 메소드와 본문 유지
  - 303 See other
    - 요청 메서드가 GET으로 변함

##### PRG 패턴

일시적인 리다이렉션의 예시로 **PRG 패턴**이 있음  
PRG 패턴은 POST 이후에 새로고침으로 인한 중복을 방지하기 위해 사용  
POST 이후 Redirect-GET으로 결과 화면으로 이동

#### 400

클라이언트의 요청이 잘못되어 서버가 처리할 수 없는 경우

- 400 Bad Request
  - 서버가 클라이언트의 요청을 이해하지 못했거나 지원하지 않음
- 401 Unauthorized
  - 클라이언트가 인증되지 않음
- 403 Forbidden
  - 클라이언트가 요청한 자원에 접근하는 것이 금지됨
- 404 Not Found
  - 클라이언트가 요청한 자원이 존재하지 않음

#### 500

- 500 Internal Server Error
  - 서버 에러

### Format

요청 메세지와 비슷하게 header와 body가 존재함  
첫 줄에 HTTP 버전과 상태 코드를 명시함

예시

```http
HTTP/1.1 200 OK
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Fri, 14 Dec 2018 16:46:09 GMT
Etag: "1541025663+gzip"
Expires: Fri, 21 Dec 2018 16:46:09 GMT
Last-modified: Fri, 09 Aug 2013 23:54:35 GMT
Server: ECS (ord/5730)
Vary: Accept-Encoding
X-Cache: HIT
Content-Length: 1270

<!doctype html>
<html>
<head>
	<title>Example Domain</title>
.......
```

| Key            | Value                         |
| -------------- | ----------------------------- |
| status line    | HTTP 버전, 상태 코드          |
| Content-Type   | body가 담고있는 콘텐츠의 타입 |
| Date           | 데이터가 만들어진 날짜        |
| Expires        | 만료 날짜                     |
| Content-Length | body의 길이                   |

Date, Etag, Expires, Last-modified는 클라이언트가 정보를 캐시할 때 사용하는 정보들임  
서버가 데이터를 보내기 시작할 때 body의 길이를 모른다면 **Transfer-Encoding**필드를 **chunked**로 설정해서 보냄  
이 경우에는 body의 앞에 데이터의 길이가 나오고 다음 줄에 데이터 본문이 나옴.

## 버전 별 차이

### HTTP 1.0

1.0 버전은 한 연결당 하나의 하나의 요청만을 처리하도록 설계되었다. 하나의 파일을 가져올 때마다 TCP 연결을 새로 맺어야 하니 RTT의 증가가 필연적이었다.

> RTT: 패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸린 시간. 즉, 패킷 왕복 시간을 뜻함

RTT의 증가를 막기 위해서 이미지 스플리팅, 코드 압축, base64 인코딩 등의 방법을 사용하였다.

이미지 스플리팅은 여러 이미지를 합쳐서 하나의 이미지로 받은 후 나누는 방식이다. 코드 압축은 개행 문자나 공백 등을 없애 코드의 크기를 줄이는 방식이다. 이미지를 base64로 인코딩해서 이미지에 대한 요청을 하지 않는 방법도 있었다.

### HTTP/1.1

1.1버전에서는 한 연결에 하나의 요청만 처리하는 것이 아닌 keep-alive 옵션을 사용해서 여러개의 요청을 처리할 수 있도록 변경되었다. 한번의 TCP 3-way handshake 이후에 다시 연결을 맺지 않고 여러 리소스를 한 연결 안에서 받아올 수 있다.

문제점으로는 HOL 블로킹이 발생할 수 있다. HOL 블로킹은 패킷을 전송할 때 큐에서 대기하게 되는데, 첫번째 패킷을 보내는 속도가 느려지면 뒤의 패킷들이 줄줄이 느려지는 현상이다. 또한 1.1 버전의 헤더에는 메타데이터가 많이 들어있어 무겁다는 단점이 있다.

### HTTP/2

2.0 버전에서는 지연 시간을 줄이고 응답 시간을 더 빠르게 하도록 변경되었다.

멀티 플렉싱은 여러개의 스트림을 사용해서 송수신한다는 뜻이다. 특정 스트림의 패킷이 손실된다 해도 다른 스트림은 문제없이 동작한다. 병렬 처리를 이용해서 HOL 블로킹을 해결한다.

1.1 버전의 헤더 크기 문제는 헤더 압축을 통해 해결했다. 하프만 코딩 압축 알고리즘을 사용해서 헤더를 압축해 크기를 줄였다.

2.0 버전에서는 서버 푸시도 가능하게 되었다. 1.1에서는 클라이언트가 요청을 해야만 파일을 다운받을 수 있었지만 2.0부터는 클라이언트 요청이 없어도 서버에서 먼저 푸시할 수 있다.

### HTTP/3

HTTP 3 버전은 TCP가 아닌 UDP를 기반으로 한 QUIC 프로토콜을 사용한다. TCP를 사용하지 않기 때문에 3way-handshake를 수행하지 않아 첫 연결 설정에서 걸리는 시간이 감소하였다.