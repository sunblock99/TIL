젠킨스에서  Publish over SSH 를 사용하기 위해서는 원격서버에 key등록을 해놔야한다.

# 젠킨스 서버에서 키 발급
~~~
 $ su jenkins
~~~
해당 명령어로 젠킨스 계정으로 접속을해야한다.

하지만 명령어를 입력했을때 접속이 안되는 경우가 있다.

그럴땐 아래 사이트를 참고하자..

[su jenkins 안될시](https://stackoverflow.com/questions/18068358/cant-su-to-user-jenkins-after-installing-jenkins)

다 되었다면

~~~
$ ssh-keygen -t rsa
~~~

명령어를 입력하면

![](https://velog.velcdn.com/images/sunblock99/post/e6f3e838-357a-4f0c-b582-6eb2f81c2d0c/image.png)

이런식으로 뜨는데

비밀번호를 설정하려면 Enter passphrase에 비밀번호를 입력한다.


![](https://velog.velcdn.com/images/sunblock99/post/ab1d9598-dbe3-4b20-ab09-72340354dfbb/image.png)

이런식으로 만들어진다

# 젠킨스서버에서 키 확인

~~~
$ cd /var/lib/jenkins/.ssh/
~~~

~~~
$ ls -al
~~~

명령어를 입력해보면 


![](https://velog.velcdn.com/images/sunblock99/post/5ca6f9e5-d9c4-4767-83aa-6fd86cfe3c72/image.png)

해당 파일이 생겼을것이다.

이중에서 

~~~
$ cat id_rsa.pub
~~~

를 하게되면

![](https://velog.velcdn.com/images/sunblock99/post/6eb1f8d9-15df-4cbf-866b-7c46c3ca0176/image.png)

이런식으로 나오게 되고

이것을 이제 리모트서버(=배포서버)에 등록을해야한다.

리모트서버의 계정 홈디렉토리로 이동한다

~~~
$ cd /home/배포할계정
~~~

~~~
$ mkdir .ssh
~~~

~~~
$ cd .ssh
~~~

~~~
$ vi authorized_keys
~~~

파일편집기를 열어서 위에서

$ cat id_rsa.pub

해서 나왔던 내용을 복붙해준다

저장하고 나온뒤 이제 젠킨스 서버에 등록을 해줘야한다.

# 젠킨스에 등록하기


젠킨스 화면에서 좌측 메뉴에서 **Jenkins관리**에 들어간다.

![](https://velog.velcdn.com/images/sunblock99/post/b9a2461c-98e6-47de-9ed0-9ba3a63461ae/image.png)


여기서 System Configuration - System에 들어간다

![](https://velog.velcdn.com/images/sunblock99/post/b11332a6-4dcb-4b52-8461-c1405188ca5a/image.png)


쭉내리다보면

Publish over SSH 설정하는곳이 보일것이다


![](https://velog.velcdn.com/images/sunblock99/post/18fbd00d-c74e-42c3-b87d-f0451e0237f1/image.png)

Passphrase : 비밀번호

Key : 젠킨스서버에서 발급한 키 조회하여 넣으면된다 (EC2면 EC2 프라이빗키)

명령어로는 
~~~
cat  /var/lib/jenkins/.ssh/id_rsa
~~~ 
내용을 붙혀넣기 하면 된다.

그다음 밑에 **추가** 버튼을 누른다

![](https://velog.velcdn.com/images/sunblock99/post/376014d0-b36b-45ea-9e31-b529487b072a/image.png)

그러면 그 밑에 있는 SSH Server에 Name은 원하는 이름을 적고 Hostname으로 IP혹은 도메인을 적어준다.

 
Username에는 접속하길 원하는 User 이름을 적고 Remote Directory에는 작업할 원격 서버의 디렉토리를 적어준다.

이렇게 까지하면 ssh 원격배포 설정은 끝났다.