## HTTP (Hypertext Transfer Protocol) 

`L7 응용 계층`

Hypertext인 HTML을 전송하기 위한 통신 규약. 그러나, 암호화되지 않은 방법으로 데이터를 전송하기 때문에 데이터의 변조나 탈취, 감청 등이 가능할 수 있다.

## HTTPS (Hypertext Transfer Protocol Secure)

`L7 응용 계층`

웹 브라우저 간의 안전한 통신을 제공하는 프로토콜. HTTP의 전송 계층에 인증과 암호화를 적용한 보안 버전이며, TLS나 SSL 인증서를 사용한다. 다음과 같은 순서로 동작한다.

- 브라우저의 연결
1. 웹서버와 3way-handshaking을 통해 TCP 연결을 시도한다. 
2. 이 과정에서 세션 아이디나 cipher suite(어떤 프로토콜 tls/ssl 사용) 등을 담은 패킷을 전송한다.
- 서버의 응답
1. 신원 확인 및 키 생성(비대칭키)
2. CSR(Certificate Signing Request, 인증서 서명 요청)을 작성. 여기에는 서버의 정보와 공개키가 첨부되어 있으며, 비밀키(개인키)로 서명된다.
3. CSR을 CA에 전송 후, CA의 비밀키로 서명된 서버의 공개키와 정보가 포함된 디지털 인증서를 발급 받음. 
- CA의 공개키는 이미 클라이언트가 가지고 있는 경우가 많다. 리스트에 없다면 외부 인터넷을 통해 인증기관의 정보와 공개키를 갖고 온다. 
4. 서버에 인증서를 설치함. 이후 브라우저가 서버에 접속하면, 인증서를 제공해 신원을 증명한다. 
5. 브라우저는 CA의 공개키로 서명을 확인하고, 일치한다면 서버의 공개키를 신뢰하고 사용한다. 

> 🤔 **그렇다면 서버가 클라이언트를 검증하는 과정은 없는 걸까?**<br>
→ 그렇다! 대부분의 웹사이트나 서비스에서는 **서버 인증만 수행한다.** 
그러나 요청할 수는 있다. 만약 서버가 클라이언트의 신원 확인을 요구하는 경우, 서버는 핸드셰이크 도중 Client Certificate Request 메시지를 전송한다. 이 경우 클라이언트는 자신의 인증서를 서버에 전송해야한다.

- 대칭키의 교환
1. 서버의 인증서에 포함된 공개키를 이용해 **Pre-Master Secret**이라는 임의의 랜덤 값을 생성. 이것을 서버의 공개키로 암호화하여 전송한다.
2. 서버와 클라이언트는 **Master Secret**을 생성한다. 이것을 기반으로 Session 키(대칭 암호화 키)와 MAC키(무결성 키)를 생성한다. 
3. ChangeCipherSpec(이제부터 이 키 사용할게!) 와 Finished(대칭키로 암호화한 메세지) 를 전송해 대칭키가 정상 교환되었음을 확인한다. 
4. 이후 대칭키를 사용해 통신한다.


### 참고자료
- [**HTTPS란 무엇입니까?**](https://www.cloudflare.com/ko-kr/learning/ssl/what-is-https/)
- [**[네트워크] HTTP와 HTTPS**]([https://velog.io/@averycode/네트워크-HTTP와-HTTPS](https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-HTTP%EC%99%80-HTTPS))
- [**[네트워크] HTTP와 HTTPS 동작 과정**]([https://velog.io/@averycode/네트워크-HTTP와-HTTPS-동작-과정](https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-HTTP%EC%99%80-HTTPS-%EB%8F%99%EC%9E%91-%EA%B3%BC%EC%A0%95))
- [**HTTPS 원리 이해하기**](https://brunch.co.kr/@growthminder/79)