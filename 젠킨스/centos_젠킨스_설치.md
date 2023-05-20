출처 : https://da2uns2.tistory.com/entry/Jenkins-CentOS%EC%97%90-Jenkins-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0

# 1. jdk 설치 및 환경변수 설정

jenkins는 java로 작성된 프로그램으로 jdk8 또는 jdk11을 이용하여 동작한다.

따라서 jdk를 설치해야 한다.

jdk11 64bit용으로 설치할 것이다.

$ yum install -y java-11-openjdk-devel.x86_64
다음과 같이 입력해준다.

중간중간 y를 입력하며 설치를 진행한다. Complete!가 뜨면 설치가 정상적으로 완료된 것이다.

설치가 다되었다면

해당 명령어를 입력해 설치가 정상적으로 되었는지 확인한다.

```
javac -version
```

```
rpm -qa java*jdk-devel
```

![](https://velog.velcdn.com/images/sunblock99/post/e33be087-3051-41dd-8c8b-64d44417f18d/image.png)

_다음으로 CentOS7 jdk 환경변수를 설정할 것이다._

```
echo $JAVA_HOME
```

해당 명령어를 입력했을때 아무것도 뜨지 않으면 설정이 안되어 있는 것이다.

![](https://velog.velcdn.com/images/sunblock99/post/62079e20-12f8-418d-98b5-46bb7e3792d1/image.png)

따라서 환경변수를 설정해줄 것이다

먼저 *which javac*를 입력해 javac 명령어의 위치를 찾는다.

![](https://velog.velcdn.com/images/sunblock99/post/7483c718-b048-43f7-aadf-bb9d95d18f5f/image.png)

이후 readlink -f [javac 명령어 위치]를 입력해 JAVA_HOME 경로를 얻어낸다.

![](https://velog.velcdn.com/images/sunblock99/post/4d483e54-c48c-4114-b68a-9cbe1dbe6569/image.png)

이 경로를 이용해 JAVA_HOME 환경변수로 등록할 것이다.

```
$ vi /etc/profile
```

맨아래에 해당 경로를 작성한다

```
export JAVA_HOME=[자바 경로]
```

![](https://velog.velcdn.com/images/sunblock99/post/004f1226-0041-4ae8-9d7b-4f95d60ce274/image.png)

```
해당 명령어를 입력해 적용한다

$ source /etc/profile
```

![](https://velog.velcdn.com/images/sunblock99/post/61e05759-d62c-4afe-90c3-32a0bb0a0bce/image.png)

그 이후 자바홈이 잘 설정되었는지 확인한다

![](https://velog.velcdn.com/images/sunblock99/post/8e0bac73-87f0-4f3e-bdcb-9fff87371687/image.png)

# 2. 빌드 도구 설치(Gradle, Maven ... )

### Gradle 설치

#### 1. wget을 이용한 gradle 설치

```
$ yum install wget
```

wget을 처음 쓴다면 yum 명령어를 이용해 설치해준다.

중간에 y 입력해주고, Complete!이 뜬다면 설치가 완료된 것이다.

```
$ wget https://services.gradle.org/distributions/gradle-7.0.2-bin.zip
```

이후 wget을 이용해 gradle.zip을 가져온다.

[Gradle 다른 버전 다운로드 링크](https://services.gradle.org/distributions)

```
$ mkdir /opt/gradle

$ yum install unzip //unzip 처음 쓰는 경우 설치
$ unzip -d /opt/gradle gradle-7.0.2-bin.zip
```

먼저 mkdir /opt/gradle을 입력해 디렉토리를 생성한다.

이후 위처럼 다운받은 zip파일을 gradle 디렉토리에 압축 해제한다.

다음 경로로 이동해 설치된 파일을 확인할 것이다.

```
$ ls -al 명령어 입력
```

![](https://velog.velcdn.com/images/sunblock99/post/4ee432c6-7a6c-4c31-9f1c-1e3d628accde/image.png)

#### 2. 환경변수 설정

gradle.sh를 vi 입력기를 통해 연다.

```
$ vi /etc/profile.d/gradle.sh
```

```
export GRADLE_HOME=/opt/gradle/gradle-7.0.2
export PATH=${GRADLE_HOME}/bin:${PATH}
```

위와 같은 내용을 입력하고 저장한다.

```
$ chmod +x /etc/profile.d/gradle.sh
```

파일을 빠져나와 다음과 같이 입력해 스크립트 실행 가능하도록 설정한다.

```
$ source /etc/profile.d/gradle.sh
```

source 명령어를 이용하여 변경 사항을 적용한다.

#### 3. 설치 확인

```
gradle -v를 입력했을 때, 다음과 같이 출력이 된다면 설치가 완료된 것이다.
```

![](https://velog.velcdn.com/images/sunblock99/post/4b829e2f-9710-4cf0-bffc-e42595971869/image.png)

# 3. 젠킨스 설치

```
$ sudo yum install -y ca-certificates 명령어 실행
```

```
$ wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

먼저 yum 레포지터리에 젠킨스 레드햇 안정화 버전 레포지터리를 추가한다.

```
$ sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

이후 rpm에 젠킨스를 추가한다.

```
$ yum install epel-release
$ yum install jenkins
```

다음 두 명령어를 통해 젠킨스를 설치한다.

$ rpm -qa | grep jenkins

이후 rpm -qa | grep jenkins 를 입력해 잘 설치되었는지 확인한다.

![](https://velog.velcdn.com/images/sunblock99/post/592b9986-1e73-4a44-a21e-1252115f6f06/image.png)

```
 $ vi /etc/sysconfig/jenkins
```

포트를 변경하기 위해 vi 편집기를 이용한다.

![](https://velog.velcdn.com/images/sunblock99/post/9b4d6b37-2eb4-42cb-9840-b290c38ade05/image.png)

포트 부분을 찾아서 JENKINS_PORT="8080"을 JENKINS_PORT="9090"으로 변경한다.

(8080에서 다른 포트로 변경. 꼭 9090이 아니어도 됨.)

![](https://velog.velcdn.com/images/sunblock99/post/f7a8ef6f-d4dc-4e3c-a4e9-a63ca9275756/image.png)

```
$ firewall-cmd --permanent --zone=public --add-port=9090/tcp
$ firewall-cmd --reload
```

9090 포트에 방화벽을 설정하면 젠킨스 설치가 끝난다.
![](https://velog.velcdn.com/images/sunblock99/post/de1b3bc5-2e08-4393-9221-83f6bad4bbe1/image.png)

# 4. 젠킨스 사용

```
$ service jenkins start
```

나는 여기서 에러가 나왔다...
에러내용은 jdk8로 설치하여서 ㅋㅋ
jdk11로 사용하자

```
$ ip addr
```

해당 명령어를 입력하면 ip에 대한 정보가 나온다

이후, 호스트에서 브라우저를 열고 http://IP주소:포트번호를 입력해 접속한다.
나의 경우 http://192.168.10.18:9090이었다.

![](https://velog.velcdn.com/images/sunblock99/post/8d0a035d-cd1a-42d3-a7c0-585dab5f9d3d/image.png)

https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbBI5AD%2FbtrkS8KRf3N%2FKGQ5lWXbwwQqiuwoPjDFEK%2Fimg.png

기다려달라는 창이 뜬 뒤, 다음과 같은 화면이 뜬다.

```
cat /var/lib/jenkins/secrets/initialAdminPassword
```
