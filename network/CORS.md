# CORS란?

Cross-Origin Resouce Sharing의 약어로, 브라우저의 SOP를 우회하기 위한 방법이다.  
CORS는 추가 HTTP 헤더(Origin)를 사용해서 한 '출처'에서 실행중인 웹 어플리케이션이  
다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 '브라우저'에 알려주는 체제이다.

## Origin(출처)

Origin이란 Protocol, Host, Port를 합친 것이다.

```
https://www.naver.com/user?id=1@sort=asc

Protocol = 서로 다른 컴퓨터간에 통신을 하기 위한 규약으로 HTTP,FTP,Telnet 등이 있다. (http://)
Host = 네트워크에 연결된 장치 또는 서버들에 부여되는 고유한 이름 (www.naver.com)
Port = 네트워크 또는 인터넷을 사용하여 통신하는 각 응용 프로그램 또는 프로세스의 논리주소 (HTTP:80, HTTPS:443, FTP:21)
Directory = 해당 자원이 서버의 어디에 있는지를 나타내는 경로 (/user)
Query string = 웹 서버에 제공하는 추가 파라미터들도 & 기호로 구분된 키/값의 짝 (id = 1, sort = asc)
```

이 셋 중 하나라도 다르면 다른 출처로 판단한다.

## SOP

### API서버 요청의 경우

SOP란 Same-Origin policy로 '동일한 출처에서만 리소스를 공유할 수 있다'는 정책이다.  
예를 들자면 3000번 포트에서 돌아가는 리액트 서버에 접속할 때 React 서버에 있는 리소스는 자유롭게 가져올 수 있지만,  
다른 출처 서버에 있는 5000번 포트의 Nest 서버의 리소스는 상호작용이 불가능하다.

![CORS1](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/CORS1.png?raw=true)

React 서버에서 받아온 Document에 Nest로 가는 요청이 있다면  
React 서버의 Origin인 3000번 포트와 Nest서버의 Origin인 5000번 포트가 비교되어 CORS오류를 낸다.

### 악의적 요청의 경우

![CORS2](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/CORS2.png?raw=true)

유저가 해커의 주소로 접속해 iframe을 통해 서버에 있는 메일에 관련된 document를 로드할 경우  
브라우저는 메일 서버의 Origin과 해커의 서버의 Origin을 비교하여 CORS 오류 낸다.  
스크립트 실행 과정에서 http/hacker.com의 document에서 server의 document를 참조할때  
CORS오류를 내기 때문에 server의 document는 null값을 내보내고 해커는 정보를 얻을 수 없게 된다.

# CORS의 동작

## Preflight Request

본 요청을 보내기 전에 브라우저 스스로 요청이 안전한지 확인하기 위해 보내는 예비 요청을 Preflight라고 한다.  
이 예비 요청에는 HTTP 메서드 중 OPTIONS 메서드가 사용된다.

![CORS_Preflight](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/CORS_Preflight.png?raw=true)

예비요청에서 브라우저는 Access-Control-Request-Method를 사용해서 본 요청에서 사용할 메서드를 알려주고,  
Access-Control-Request-Headers를 사용해서 본 요청에서 사용할 헤더를 알려준다.  
서버는 헤더에 포함된 Access-Control-Allow-Origin으로 접근 가능한 출처를 알려주며  
요청이 CORS 정책을 위반한다면 브라우저는 CORS 오류를 내보낸다.

Preflight Request는 CORS를 인식하지 못하는 서버가 잘못된 출처로 정보를 보내지 않도록 설계된 메커니즘이다.

## Simple Request

![CORS_Simple](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/CORS_Simple.png?raw=true)

단순 요청은 예비 요청을 보내지 않고 바로 본 서버에 요청을 보낸다.  
서버가 응답 헤더에 Access-Control-Allow-Origin을 보내면 브라우저에서 CORS 정책 위반 여부를 판별한다.

## Credential Reqeust

인증 관련 헤더를 담을 수 있게 해주는 옵션이 credentials이다.  
credentials의 기본값은 same-origin(같은 출처 간 요청에만 인증 정보를 담을 수 있음)이며,  
쿠키나 토큰을 클라이언트에서 자동으로 담아서 보내고 싶을때 credentials를 include하면 서버까지 전달된다.  
이때 서버에서도 Access-Control-Allow-Credentials를 true로 설정하고, Access-Control-Allow-Origin에 허용할 출처를 적어야 한다(\*은 안된다).

# CORS의 해결 방안

## 프런트 서버에서 Proxy 사용하기

![CORS_Proxy](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/CORS_Proxy.png?raw=true)

프런트엔드에서 Proxy api요청이 있다면 5000번 포트로 요청이 가도록 설정 해준다면 브라우저에서는 같은 Origin으로 요청을 보낸 것으로 처리 되기 때문에 CORS 문제를 해결할 수 있다.

## Access-Control-Allow-Origin 세팅하기

서버에서 Access-Control-Allow-Origin 헤더에 알맞는 출처를 명시 해준다면  
CORS 정책을 통과하게 되어 문제가 해결된다.

---

Reference:  
[CORS는 왜 이렇게 우리를 힘들게 하는걸까?](https://evan-moon.github.io/2020/05/21/about-cors/)  
[10분 테코톡 나봄의 CORS](https://www.youtube.com/watch?v=-2TgkKYmJt4)  
[CORS MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)  
[SOP를 모르고 Web을 논하지 마라](https://www.youtube.com/watch?v=6QV_JpabO7g)  
[URL주소란 무엇일까?](http://www.codns.com/b/B05-195)