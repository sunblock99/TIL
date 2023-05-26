


# 1) AWS 계정 생성
개인정보와 결제 카드 등을 등록해서 가입하면 됩니다.
인당 1계정 제한이 없으니 프리티어 기간이 끝났다면 다른 이메일로 가입하면 됩니다.

# 2) 인스턴스 시작
메인 화면에서 EC2 서비스를 클릭하거나, 상단 검색 메뉴에서 EC2를 검색하면 다음과 같은 대시보드를 볼 수 있습니다.



![](https://velog.velcdn.com/images/sunblock99/post/174756a7-667e-4245-b99b-1e6090b35942/image.png)

인스턴스 시작 버튼을 누르는 것이 클라우드 서버 구축의 시작입니다.

# 3) AMI 선택 (어떤 종류의 서버 컴퓨터를 돌릴 것인지)



프리티어 사용 가능이라고 적혀있는것을 확인하고 OS의 종류, 버전 등을 비교후 원하는 제품을 선택한다.
나는 우분투 기반의 AMI를 선택했다.


![](https://velog.velcdn.com/images/sunblock99/post/fc4e8cd3-6fda-4e67-bf44-605f572e3f02/image.png)



# 4. 인스턴스 유형 선택

![](https://velog.velcdn.com/images/sunblock99/post/569d7a31-c309-4cd9-a9cb-e4a068b92f61/image.png)


t2.micro를 선택하고 다음버튼을 클릭한다. (파란버튼아님)


# 5. 키페어 생성 및 선택



- 만들 EC2 서버로 접속하기 위한 인증서 이다

- 절대로 잃어버리면 안된다!! 꼭 백업해놓자.

## 5-1) 새 키 페어 생성

![](https://velog.velcdn.com/images/sunblock99/post/db6718fe-897a-440e-bfff-1009d2fbfc3f/image.png)


## 5-2) 키페어 이름을 짓고 생성해주자.

![](https://velog.velcdn.com/images/sunblock99/post/7fa0f696-6b2d-48b5-a21b-8532a4fc8412/image.png)


# 6.  네트워크 설정이다. HTTPs 와 HTTP 접근을 위해 체크해 포트를 열어주었다.

![](https://velog.velcdn.com/images/sunblock99/post/85c68018-c29c-467c-9a13-1ca04d6ef3c9/image.png)

# 7. 스토리지를 프리 티어 최대 용량인 30GB로 설정해주자. (기본값 8GB)

![](https://velog.velcdn.com/images/sunblock99/post/b7bca38d-7751-4a9c-afc8-05bdd69f3755/image.png)


# 8. 고급 세부 정보 IAM 프로파일 생성

- 보안 강화를 위해 IAM (Identify and Access Management) 계정이 필요하다.

- 지금 우리는 Root 계정으로 로그인 되어있다. 만약 Root 권한을 탈취당해서 과금이 발생하면 막을 방법이 없어진다. 따라서 부계정과 비슷한 의미의 IAM 계정을 나눠놓는것이 좋다.

## 8-1) 새 IAM 프로파일 생성


![](https://velog.velcdn.com/images/sunblock99/post/c92739d3-9433-45b5-a72a-9906022407a5/image.png)


## 8-2) 사용자 그룹 생성

![](https://velog.velcdn.com/images/sunblock99/post/b9987453-9763-4ddc-80e4-4aa148bc0920/image.png)


그룹 이름 지정


## 8-3) 사용자 그룹이름 입력 및 권한 정책 연결

- EC2 전체 권한을 주기 위해 ec2full 검색 후 아래 뜨는 정책을 체크해준 후 그룹 생성

![](https://velog.velcdn.com/images/sunblock99/post/4f908958-72e8-4fb8-b4cb-d6fbb159dcdc/image.png)


## 8-4) 사용자 탭에서 사용자 추가 (파란색 사용자 추가 버튼 클릭)

![](https://velog.velcdn.com/images/sunblock99/post/ac4e3c18-25cf-4412-a573-afba8a3897bc/image.png)

![](https://velog.velcdn.com/images/sunblock99/post/c3787a64-b1fa-4021-986a-a47b6761a2a8/image.png)


- 권한설정 : 아까 생성한 그룹에 속하도록 체크해주자.

![](https://velog.velcdn.com/images/sunblock99/post/20e4f1a8-bd54-43df-85d9-9c52a088530d/image.png)


- 이 후 태그는 일단 넘어가고 사용자를 생성해주자.

![](https://velog.velcdn.com/images/sunblock99/post/d2277a46-0285-4f83-b10a-f39d2d094cb9/image.png)

- 사용자 추가 성공 !

## 8-5) IAM 역할 설정 - 오른쪽 역할 만들기 클릭


![](https://velog.velcdn.com/images/sunblock99/post/c468829b-fac1-4e5f-a941-b8672e7bebfb/image.png)


- 엔터티 선택 : EC2 선택후 다음으로 넘어가주자.

![](https://velog.velcdn.com/images/sunblock99/post/24748c2b-5ae9-4839-a07f-5abb92c10f6b/image.png)


- 권한 추가 : AmazonS3 저장소 사용을 위해 S3Full 입력후 선택해준다.

![](https://velog.velcdn.com/images/sunblock99/post/11ed50c5-1801-4fe7-9311-8d046cc7a0a7/image.png)
![](https://velog.velcdn.com/images/sunblock99/post/3a0766ce-d123-41c4-b592-769229388364/image.png)
![](https://velog.velcdn.com/images/sunblock99/post/c3834493-fcbb-46e9-a8c6-ab1272b0be7d/image.png)



- 이후 역할을 생성해준다.

# 9. 다시 돌아와서 생성한 IAM 프로파일을 선택해준다.

![](https://velog.velcdn.com/images/sunblock99/post/a4a7e068-5455-4569-82d8-2548a121bade/image.png)

# 10. 탄력적 IP 등록

- AWS EC2 인스턴스는 ON/OFF 또는 시간이 지나면 IP가 유동적으로 바뀐다.

- IP가 바뀐다는 것은 웹서비스를 고정적으로 운영할 수 없다는 뜻이고 고정 IP를 등록해줘야한다.

## 10-1) 탄력적IP 탭에서 탄력적 IP주소 할당을 클릭후 할당해준다.

![](https://velog.velcdn.com/images/sunblock99/post/16b2a151-0605-4a31-b48e-3e00c3879fbd/image.png)


## 10-2) 해당 IPv4 주소를 선택하고 작업-탄력적IP주소 연결을 선택해준다.

![](https://velog.velcdn.com/images/sunblock99/post/1c0a5570-a007-44d7-bc21-c149b9f7953a/image.png)

## 10-3) 이전에 생성했던 인스턴스를 선택해서 연결시켜준다.

![](https://velog.velcdn.com/images/sunblock99/post/e4ca7649-11bc-4990-85b1-685c46315c8c/image.png)



- 이후 시간이 좀 지나면 인스턴스 탭에서 탄력적 IP 주소가 할당됐음을 확인할 수 있다.


# 11. PuTTY 통해 원격접속하기 

- putty는 원격으로 장치에 접근할 수 있는 터미널 역할을 해준다. (아마존의 데이터센터에 주소를 갖고 접근한다)

- 아래에서 다운로드후 설치해준다.

https://www.softonic.kr/download/putty/windows/post-download

## 11-1) 이전 포스팅에서 생성했던 키페어 파일(.pem)을 준비한다.

## 11-2) PC에서 puttygen 을 실행시켜주고 import key를 눌러 키페어 파일을 가져온다.

![](https://velog.velcdn.com/images/sunblock99/post/32ef347a-b120-4ba2-8d22-f1be5ee62e0b/image.png)


## 11-3) 파일을 가져온 후 Save private key를 눌러 ppk 파일을 생성해준다. (준비끝)

 

## 11-4) 인스턴스 탭에서 해당 인스턴스를 체크 후 퍼블릭 IPv4 DNS를 복사해준다.

![](https://velog.velcdn.com/images/sunblock99/post/6e656f56-7e28-46f7-a73a-ae07cf84b95d/image.png)

## 11-5) PuTTY 실행 후 Connection-SSH-Auth-Credentials 탭에서 Private key file for authentication: Browse를 눌러 ppk 파일을 가져온다.

![](https://velog.velcdn.com/images/sunblock99/post/fefeb1c1-0124-419b-ad9a-f25eff8fd03c/image.png)



## 11-6) Session 탭에서 복사한 ip 주소를 붙여넣기 하고 Open 해준다.

> 주의 ! 이제 putty에 Host Name란에 해당 값을 입력할 건데 앞에 ubuntu@를 붙여야 오류가 나지 않는다. 만약 퍼블릭 IPv4주소가 1.23.567.89라면 ubuntu@1.23.567.89와 같이 입력을 해주어야 한다.



## 11-7) 예 를 눌러서 open

