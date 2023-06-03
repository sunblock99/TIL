![](https://velog.velcdn.com/images/sunblock99/post/5d6a68e7-c9a1-4593-a0bc-5df4d0162035/image.png)

# 1 AWS Management Console을 사용하는 방법:

AWS Management Console에 로그인합니다.
EC2 서비스로 이동합니다.
"인스턴스" 섹션에서 재부팅할 인스턴스를 선택합니다.
상단 메뉴에서 "인스턴스 작업"을 클릭하고, "재부팅"을 선택합니다.
재부팅을 확인하는 팝업 창이 나타나면 "예, 재부팅"을 클릭합니다.

# 2.AWS CLI(Command Line Interface)를 사용하는 방법:

AWS CLI를 설치하고 구성합니다. 자세한 내용은 AWS CLI 설치 가이드를 참조하세요.
터미널 또는 명령 프롬프트에서 다음 명령을 실행합니다:

```
aws ec2 reboot-instances --instance-ids <instance_id>
```

여기서 <instance_id>는 재부팅할 인스턴스의 식별자입니다.
