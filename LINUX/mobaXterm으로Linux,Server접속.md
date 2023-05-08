# mobaXterm이란?

- SSH, RDP, SFTP 등 다양한 네트워크 클라이언트와 터미널을 제공하는 툴
- 다른 네트워크 클라이언트 프로그램들에 비해 훨씬 강력하고 편리한 세션 연결을 지원하기 때문에 이 프로그램을 사용하시길 권장드립니다
- 다운받기 : https://mobaxterm.mobatek.net/download-home-edition.html
- Portable or Installer 버전 중 원하는 방식 다운
- 무료 버전은 세션을 12개까지밖에 저장 못하니까 참고하세요
  더 자세한 설명 : https://cloudest.oopy.io/posting/015

## SSH로 Linux Server 접속 (+SFTP)

![](https://velog.velcdn.com/images/sunblock99/post/bcaa8295-8f7d-4bbe-8833-bf260e42a7f4/image.png)

1.  Session 클릭
2.  SSH 클릭
3.  접속하려는 서버의 퍼블릭 IP 입력
4.  Advanced SSH settings 클릭
5.  Use private key 체크 후 EC2 Key (.pem) 업로드

![](https://velog.velcdn.com/images/sunblock99/post/eb051e6a-720e-44a2-a747-f3f5b616641e/image.png)

- 빨간 박스로 표시한 Sftp 탭에 드래그 앤 드롭으로 파일 전송이 가능합니다
