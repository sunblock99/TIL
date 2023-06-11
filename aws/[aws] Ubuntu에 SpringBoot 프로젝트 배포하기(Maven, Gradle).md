![](https://velog.velcdn.com/images/sunblock99/post/32fab447-dae3-408f-818b-5a90550ddeb4/image.png)

> 개발환경

- java 11

# 1. git clone [repository]

# 2. 프로젝트 빌드 파일이 있는 위치로 이동

# 3. 빌드 및 실행

## Maven

```
$ sudo ./mvnw clean package -DskipTests
$ cd target/
$ sudo nohup java -jar [빌드된 jar 파일 이름] &
```

> ❗️error
> ./mvnw clean package permission denied
> 해당 명령어의 수행 권한이 없어서 발생하는 에러이므로 sudo chmod +x mvnw 로 권한을 주면 됩니다.

## Gradle

```
$ sudo ./gradlew clean build -x test
$ cd build/libs/
$ sudo nohup java -jar [빌드된 jar 파일 이름] &
```

nohup 커맨드를 추가해 daemon으로 실행시킬 수 있습니다.
tail nohup.out으로 로그 확인이 가능합니다.

> ❗️error
> ./gradlew clean package permission denied
> 해당 명령어의 수행 권한이 없어서 발생하는 에러이므로 sudo chmod +x gradlew 로 권한을 주면 됩니다.
