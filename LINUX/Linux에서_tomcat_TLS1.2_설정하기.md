![](https://velog.velcdn.com/images/sunblock99/post/98ba8c22-0308-46fd-a546-bdc023b9a06a/image.png)

# 1. HTTPS로 설정하기

사내 테스트 서버(Linux)에 HTTPS로 접속할 수 있는 환경을 구성해야했다.

1. SSL인증서 발급받기
   HTTPS 통신을 하기 위해서는 SSL인증서가 필요하다. openssl을 통해 발급받았다.
   참고사이트1 : https://namjackson.tistory.com/25
   참고사이트2 : https://m.blog.naver.com/espeniel/221845133507

위의 사이트들을 참고해서 인증키를 발급했다.

# 2) HTTPS 설정하기

server.xml의 주석을 잘 읽어보면 HTTP와 HTTPS 모두 설정하는 부분이 있다.

server.xml

 <!-- A "Connector" represents an endpoint by which requests are received
         and responses are returned. Documentation at :
         Java HTTP Connector: /docs/config/http.html (blocking & non-blocking)
         Java AJP  Connector: /docs/config/ajp.html
         APR (HTTP/AJP) Connector: /docs/apr.html
         Define a non-SSL HTTP/1.1 Connector on port 8080
    -->

    <Connector port="9090" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="38443"
               URIEncoding="UTF-8" />

non-SSL, HTTP 설정

- port : 9090
  <!-- Define a SSL HTTP/1.1 Connector on port 8443
           This connector uses the BIO implementation that requires the JSSE
           style configuration. When using the APR/native implementation, the
           OpenSSL style configuration is required as described in the APR/native
           documentation -->

      <Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol"
                 maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
                 clientAuth="false" sslProtocol="TLS"
                  keystorePass="{비밀번호}" keystoreFile="/home/{keystore경로}/.keystore"
                  keystoreType="pkcs12"/>

  SSL, HTTPS 설정

- port : 8443
  HTTPS를 설정할 port의 방화벽을 뚫어주고, HTTPS를 설정하는 Connector 태그의 주석을 풀어준다. 해당 태그에 keystore 관련 설정을 추가해준다.

keystorePass : keystore 비밀번호
keystoreFile : keystore 파일 경로
keystoreType : keystore 포맷

## 2. TLS1.2으로만 통신되게 설정하기

HTTPS 설정 태그에 sslEnabledProtocols 설정을 추가해준다.
나는 TLS1.2만 되도록 설정해야하니깐 sslEnabledProtocols="TLSv1.2” 로 설정해줬다.

    <!-- Define a SSL HTTP/1.1 Connector on port 8443
         This connector uses the BIO implementation that requires the JSSE
         style configuration. When using the APR/native implementation, the
         OpenSSL style configuration is required as described in the APR/native
         documentation -->

    <Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol"
               maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
               clientAuth="false" sslProtocol="TLS"
               sslEnabledProtocols="TLSv1.2"
               keystorePass="tomcat" keystoreFile="/home/{keystore경로}/.keystore"
               keystoreType="pkcs12"/>

※ sslEnabledProtocols
TLS1, TLS1.1, TLS1.2 모두 설정 가능하다. 원하는 TLS 버전으로 지정해주면 된다.
',' 쉼표로 구분해주면 된다.
EX) TLS1, TLS1.1만 지정하고 싶을 때 : sslEnabledProtocols="TLSv1,TLSv1.1"

3. OPENSSL로 TLS 버전 확인하기
   TOMCAT을 기동시키고 openssl 명령어로 SSL인증서 정보를 살펴볼 수 있다.
   참고사이트 : https://m.blog.naver.com/jihye2340/220659855526

openssl s_client -connect {IP주소}:{port}

```
CONNECTED(00000003)

... 생략 ...

-----BEGIN CERTIFICATE-----
... 생략 ...
-----END CERTIFICATE-----

---
No client certificate CA names sent
Server Temp Key: ECDH, secp521r1, 521 bits
---
SSL handshake has read 1454 bytes and written 481 bytes
---
New, TLSv1/SSLv3, Cipher is ECDHE-RSA-AES128-SHA256
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES128-SHA256
    Session-ID: ... 생략 ...
    Session-ID-ctx:
    Master-Key: ... 생략 ...
    Key-Arg   : None
    Krb5 Principal: None
    PSK identity: None
    PSK identity hint: None
    Start Time: 1641189980
    Timeout   : 300 (sec)
    Verify return code: 21 (unable to verify the first certificate)
---
Secure Renegotiation IS supported : HTTPS로 접속 가능
SSL-Session 부분을 보면 Protocol이 TLSv1.2로 뜨는게 보인다.
```

TLS1.2!만! 접속이 되는지 어떻게 확인할까?
⇒ openssl 명령어 뒤에 -tls 속성을 넣어본다.

💡 ssl2, -ssl3, -tls1*2 , -tls1_1, -tls1
-> 설정한 프로토콜만 통신을 하겠다는 의미이다.
-> no* 를 저 옵션 앞에 붙이면 해당 옵션을 제외하고 통신을 하겠다는 의미이다.(ex. -no_ssl2)

TLS1로 통신이 되는지 테스트 하기
openssl s_client -connect {IP주소}:{port} -tls1_1

```
    CONNECTED(00000003)
    140356373260192:error:14094410:SSL routines:SSL3_READ_BYTES:sslv3 alert handshake failure:s3_pkt.c:1259:SSL alert number 40
    140356373260192:error:1409E0E5:SSL routines:SSL3_WRITE_BYTES:ssl handshake failure:s3_pkt.c:598:
    ---
    no peer certificate available
    ---
    No client certificate CA names sent
    ---
    SSL handshake has read 7 bytes and written 0 bytes
    ---
    New, (NONE), Cipher is (NONE)
    Secure Renegotiation IS NOT supported
    Compression: NONE
    Expansion: NONE
    SSL-Session:
        Protocol  : TLSv1
        Cipher    : 0000
        Session-ID:
        Session-ID-ctx:
        Master-Key:
        Key-Arg   : None
        Krb5 Principal: None
        PSK identity: None
        PSK identity hint: None
        Start Time: 1641196394
        Timeout   : 7200 (sec)
        Verify return code: 0 (ok)
    ---
```

=> 버전이 TLS1.2가 아니고 TLS1.0이라서 support 하지 않는다.

※참고※
TOMCAT5.5에서 위와 똑같이 설정하고 openssl s_client -connect {IP주소}:{port} -tls1_1 으로 테스트했을 때, support 한다는 결과가 나온다.... 그래서 tomcat7로 버전을 업데이트하게 되었다.

-> (TOMCAT 사이트에 가보면 5버전은 7로 올릴것으로 권장하고있다)

**중요점 : Secure Renegotiation IS NOT supported (지원 안됨)**
