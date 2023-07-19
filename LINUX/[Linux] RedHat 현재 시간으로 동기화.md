![](https://velog.velcdn.com/images/sunblock99/post/ba6eddc6-6299-4840-903f-75f83a627214/image.png)




# 시스템 시간대 확인

터미널 또는 콘솔 창을 열고 다음 명령을 입력하여 현재의 시스템 시간대를 확인합니다

~~~
$ timedatectl
~~~

# 시간대 변경하기

다음 명령을 사용하여 시스템의 시간대를 변경합니다. 예를 들어, 한국 시간대인 Asia/Seoul로 변경하려면

~~~
$ sudo timedatectl set-timezone Asia/Seoul
~~~

# 시간 동기화

시스템의 시간을 인터넷에서 동기화하려면 NTP(Network Time Protocol)를 사용합니다. NTP를 설치하고 활성화하는 방법은 다음과 같습니다:

## NTP 패키지 설치

~~~
$ sudo yum install ntp
~~~

## NTP 서비스 시작

~~~
sudo systemctl start ntpd
~~~

## 부팅 시 자동 시작 설정

~~~
$ sudo systemctl enable ntpd
~~~


# 변경된 시간대 확인하기

~~~
$ timedatectl
~~~


