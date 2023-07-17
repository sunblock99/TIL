젠킨스 기능중 git 롤백하지않고 젠킨스 자체에서 이전 빌드로 배포 하는 방법을 찾아보았는데

특정브런치를 기준으로는 추출하기 어려웠고

찾아낸것은 Git Tag기능으로 배포를 할 수가 있다 !

# 1. Git Parameter 설치

![](https://velog.velcdn.com/images/sunblock99/post/3a3be967-7c53-41e1-ab07-63107f408da7/image.png)

![](https://velog.velcdn.com/images/sunblock99/post/d629adcb-5594-4197-b33e-171a036fbc36/image.png)

![](https://velog.velcdn.com/images/sunblock99/post/959f2b99-fa2e-43cf-809d-009fa3d27dfc/image.png)


# 2. Jenkins 프로젝트 구성

Git Parameter 가 설치 되어 있다면, 이제 프로젝트 구성시에 해당 기능을 이용할 수 있는데요.

매개변수를 이용할 경우, 해당 체크박스를 선택하면 이용할 수 있습니다. 

![](https://velog.velcdn.com/images/sunblock99/post/99001465-3e6e-46b3-a5cd-bee6eb0aac4e/image.png)

![](https://velog.velcdn.com/images/sunblock99/post/f4df9fd3-a666-4ccf-b808-5cceee32b235/image.png)


```
Git Parameter 설정시, 변수 이름을 지정 할 수 있는데, 저의 경우는 TAG_NAME 이라는 변수명을 이용했습니다. 

여기서 중요한것은 Parameter Type 인데요. 

Branch, Tag, Revision 등 원하는 내용을 선택해서 사용 할 수 있습니다. 

저는 배포 목적으로 구성된 프로젝트여서, Tag 로 선택을 했습니다. 
```

# 3. Tag 를 이용한 빌드 진행

![](https://velog.velcdn.com/images/sunblock99/post/711e5507-5793-4c2b-a196-01bb16099cb5/image.png)

![](https://velog.velcdn.com/images/sunblock99/post/bacd2ed5-37ae-4266-97c6-5130565b6976/image.png)

여기서 중요한 부분은 Branches to Build 라는 영역인데요. 

위에서 Git Parameter 에서 선택한 Name 을 ${변수명} 형태로 기입해주면 됩니다. 

Branch 의 경우도 동일하게 셋팅하시면되요.

우측에 있는 (?) 버튼을 누르면, 자세한 사항이 보이니 참고하세요.

# 4. 매개변수를 이용한 빌드

이제 프로젝트의 빌드 버튼을 선택할 경우, 아래와 같이 Tag 이름이 보여지게 되며, 해당 Tag를 선택 후 빌드버튼을 누를경우, Tag로 빌드가 됩니다.

![](https://velog.velcdn.com/images/sunblock99/post/877a4896-e2af-4a81-9b0b-a4bd25b57889/image.png)
