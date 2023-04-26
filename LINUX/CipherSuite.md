# 암호화 스위트 (Cipher Suite) 의 구조

암호화 스위트 (Cipher Suite) 는 일반적으로 다음과 같은 구조를 가지고 있습니다.

![](https://velog.velcdn.com/images/sunblock99/post/93a4bed3-5920-48a3-955a-a3205ae6d222/image.png)

## 프로토콜 (Protocol)

SSLv3, TLSv1, TLSv1.1, TLSv1.2 와 같이 암호화 통신에 사용할 프로토콜을 명시하는 부분 입니다. 일반적으로 SSLv3는 취약점으로 인하여 브라우저에서 지원을 중단하고 있는 추세 입니다.

## 키교환 (Key Exchange)

WITH 앞쪽 부분이 보통 키교환과 인증 (전자서명을 통한 인증서 검증) 을 담당하는 부분입니다. 키 교환 알고리즘은 RSA, DH, DHE, ECDH, ECDHE 을 제공하게 됩니다.

- RSA : 비대칭키를 이용한 키교환 방식
- DH : 디피 헬만 (Diffie Hellman) 방식
- DHE : Ephermeral 을 지원하는 디피 헬만 (Diffie Hellman) 방식 (PFS, - Perfect Foward Secrecy 지원)
- ECDH : Elliptic Curve Diffie Hellman, Propose Cheme 방식
- ECDHE : Elliptic Curve 및 Ephermeral 을 지원하는 디피 헬만 (Diffie Hellman) 방식 (PFS, Perfect Foward Secrecy 지원)

## 인증 (Authentication)

키를 교환할 상대방이 교부한 인증서가 정말 내가 접속하고자 하는 상대방이 맞는지 상위 인증기관 (CA)를 통하여 확인하게 되는데, 이 때 사용되는 알고리즘을 의미합니다. 보통 RSA, DSS, ECDSA, ANON 등이 있습니다. 인증서를 만들때 인증서 서명 요청 (CSR)을 작성하여 상위 인증기관 (CA)에 요청하게 되는데, 이 때 선택한 알고리즘이 선택되게 됩니다.

만약 인증서 생성시 사용한 CSR에 RSA알고리즘을 넣어서 제출했다면 Cipher Suite는 다음과 같이 선택 될 수 있습니다.

- RSA : 키교환 및 인증을 모두 RSA 알고리즘을 이용
- DH-RSA : 키교환은 디피 헬만 (Diffie Hellman), 인증은 RSA
- ECDHE-RSA : 키교환은 ECDHE, 인증은 RSA
  위의 예시와 같이 키교환과 인증을 모두 RSA를 사용하게 될 때는 키교환과 인증을 RSA 하나로 생략하여 표현하게 됩니다.

## 암호화 (Encryption)

실제 데이터를 암호화 하는 부분은 WITH 뒷쪽에 표현된 Cipher Suite 값을 이용하여 처리 합니다. 공개키 알고리즘을 이용하여 대칭키를 만들어 공유한 이후 실데이터 암호화를 진행하게 되는데 이 때 사용되는 알고리즘이 3DES, AES, AES128 같은 것들입니다.

## 블록 암호 운용 방식 (Block Cipher Operation Mode)

실데이터를 AES128 과 같은 것으로 암호화 할 때 실데이터를 일률적으로 암호화 하는 것이 아니라 블록 단위로 쪼개서 암호화 하게 됩니다. 이 때 암호화 된 암호문을 가지고 실데이터를 추측하는 것을 방지하기 위하여 특정한 블록 암호 운용방식을 선택하게 되는데 이것이 블록 암호 운용 방식 입니다.

- CBC : Chipher Block Chaining, 암호 블록 체인 모드, CTR과 CBC-MAC을 조합하여 계산, 느림
- GCM : Galois/Counter Mode, 갈르와 카운터 모드, CTR과 GHASH를 조합하여 계산, 빠름
