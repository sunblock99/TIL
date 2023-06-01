![](https://velog.velcdn.com/images/sunblock99/post/f854e1c0-f318-4af8-9a0b-1422f54e6cda/image.png)


AWS EC2 프리티어 유형인 t2.micro는 RAM이 1GB이다. 이는 젠킨스를 돌리기에 매우 부족하다. 따라서 t2.micro에서 아무 설정을 하지 않고 젠킨스를 돌리면 서버가 먹통이 되고 만다.

 

그러나 인스턴스 유형을 업그레이드 하자니 비용이 부담된다면 SSD의 일정 부분을 swap 용량으로 할당하는 방법을 고려해보자.

 

쉽게 말해, RAM 용량이 가득 차서 더 이상 활용할 공간이 없을 때, swap 공간에 데이터를 기록해서 RAM 사양을 키운듯한 효과를 낼 수 있다. swap 공간을 사용하면 RAM 급의 속도는 아니더라도 젠킨스 서버 하나 정도는 충분히 돌릴 수 있는 속도와 용량을 제공받을 수 있다.

 

유의할 점으로 swap 공간은 무한정 설정할 수 있는 것이 아니라는 것이다.

AWS가 제공하는 권장 스왑 공간은 다음과 같다.


![](https://velog.velcdn.com/images/sunblock99/post/c68f5330-3dd4-4096-bd6f-b45a9e2e1344/image.png)


즉, 기본적으로 swap 공간은 최소 32MB는 되야 하며, t2.micro의 경우 RAM이 1GB이기 때문에 2배인 2GB까지 swap 공간을 할당하는 것을 권장한다.

# 1. dd 명령을 사용하여 루트 파일 시스템에 스왑 파일 생성

~~~
sudo dd if=/dev/zero of=/swapfile bs=128M count=16
~~~

명령에서 bs는 블록 크기이고 count는 블록 수다. 스왑 파일의 크기는 dd 명령의 블록 크기 옵션에 블록 수 옵션을 곱한 값이다.

명령에서 스왑 파일의 크기는 2GB(128MB x 16)입니다.

# 2. 스왑 파일의 읽기 및 쓰기 권한 업데이트

~~~
sudo chmod 600 /swapfile
~~~

# 3. Linux 스왑 영역 설정

~~~
sudo mkswap /swapfile
~~~

# 4. 스왑 공간에 스왑 파일을 추가하여 스왑 파일을 즉시 사용

~~~
sudo swapon /swapfile
~~~

# 5. 프로시저가 성공적인지 확인

~~~
sudo swapon -s
~~~

![](https://velog.velcdn.com/images/sunblock99/post/49428b94-1edb-41e2-a479-be5d4f5e0837/image.png)

**/swapfile에 2GB가 할당되었다.**

# 6. /etc/fstab 파일을 편집하여 부팅 시 스왑 파일 시작

~~~
sudo vi /etc/fstab
~~~

편집기에서 파일을 연다.

~~~
/swapfile swap swap defaults 0 0
~~~
파일 끝에 다음 줄을 새로 추가하고 파일을 저장한 다음 종료한다.

![](https://velog.velcdn.com/images/sunblock99/post/cbb87f3f-5e86-4fe9-aa4b-d00c342695eb/image.png)


# 7. 추가된 free 공간 확인

~~~
free -h
~~~

추가된 swap 공간을 확인한다.


![](https://velog.velcdn.com/images/sunblock99/post/083c5ffb-b3db-4702-a192-2ac3c46f582a/image.png)
