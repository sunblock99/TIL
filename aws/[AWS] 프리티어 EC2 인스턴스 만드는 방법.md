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

# 6. 네트워크 설정이다. HTTPs 와 HTTP 접근을 위해 체크해 포트를 열어주었다.

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
