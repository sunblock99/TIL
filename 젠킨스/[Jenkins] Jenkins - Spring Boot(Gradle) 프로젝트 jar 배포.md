# 젠킨스 프로젝트 구성
새로운 Item > 프로젝트명
Freestyle project로 해놓았다.

소스코드관리 > Git
Repositroy URL -> 배포할 프로젝트 URL 이름을 입력한다.


![](https://velog.velcdn.com/images/sunblock99/post/6da94380-340a-452e-b707-c767231a2c7e/image.png)

그다음 소스 코드 관리에서 Git을 지정한다

![](https://velog.velcdn.com/images/sunblock99/post/833c1322-baf0-46ae-95bb-a8669c6974c2/image.png)

여기서 처음엔 Credentials 설정을 추가해줘야하는데


![](https://velog.velcdn.com/images/sunblock99/post/e647f4f9-12b5-44ab-a6cf-aef18e2e0f2a/image.png)

Add - Jenkins 를 눌러

![](https://velog.velcdn.com/images/sunblock99/post/e855991f-6b3b-4393-80c4-c6affce12ed4/image.png)

Kind - Secret text 를  선택하고

Secret - 깃허브 토큰

ID - 나를 구분할수있는 자유로운 아이디 적으면 된다

그후 Add를 눌러 선택을 해주고

![](https://velog.velcdn.com/images/sunblock99/post/60a2d468-1486-4027-b178-7683d2ff1488/image.png)


빌트 유발은 GitHub hook trigger for GITScm polling 체크

그다음 Add build step을 눌러 Invoke Gradle script를 선택.

Use Gradle Wrapper를 선택한다.

Invoke Gradle을 선택할 경우 Jenkins에 Global Toll Configuration에서 Gradle 패키지를 설치해야 사용할 수 있습니다.

Use Gradle Wrapper의 경우 Gradle을 설치할 필요가 없다.

![](https://velog.velcdn.com/images/sunblock99/post/e9b5fe80-a1e6-4583-88fd-cf0d434cf8f0/image.png)

Make gradlew executable도 체크해준다. 체크하면 권한 문제로 실행이 안 되는 상황을 방지할 수 있다.

Tasks에 clean build를 입력하면 Build 정의는 한차례 완료된 것이 된다.

그이후 빌드 후 조치 추가를 눌러서

플러그인으로 설치되었던 *Post build task* 누른 후

![](https://velog.velcdn.com/images/sunblock99/post/bd86e832-652a-456e-bfd9-6c35469f0fd7/image.png)

만약 빌드가 BUILD SUCCESS 될경우 스크립트를 실행 해줄것이다.

~~~
echo "Start Spring Boot Application!"
CURRENT_PID=$(ps -ef | grep java | grep melon | awk '{print $2}')
echo "$CURRENT_PID"

 if [ -z $CURRENT_PID ]; then
echo ">현재 구동중인 어플리케이션이 없으므로 종료하지 않습니다."

else
echo "> kill -9 $CURRENT_PID"
kill -9 $CURRENT_PID
sleep 10
fi
 echo ">어플리케이션 배포 진행!"
nohup java -jar /var/lib/jenkins/workspace/melon/build/libs/demo-0.0.1-SNAPSHOT.jar &

echo "배포까지 성공 !!"
~~~
> 위 스크립트 부분은 한마디로 먼저 프로젝트가 돌아가고있으면 프로세스를 죽인다음에 다시 실행한다는 의미다.ㅉ



까지 하고 Run script only if all previous steps were successful 체크 까지해주고 Apply - 저장 누르고

![](https://velog.velcdn.com/images/sunblock99/post/87370747-9a55-46bf-8829-d8aaa3b4b9c2/image.png)


지금 빌드를 누른다.

![](https://velog.velcdn.com/images/sunblock99/post/5d22ddbc-1c24-4363-9161-907ed9619d31/image.png)


Console Output 누르게되면 콘솔까지 출력을 해주는데 기다리다보면 결과가 나온다.




![](https://velog.velcdn.com/images/sunblock99/post/4352e38d-c328-49c9-a354-82f2f6a504fb/image.png)


성공하면 

Finished: SUCCESS 가 뜬다 !!

