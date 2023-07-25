# 심볼릭링크 생성

심볼릭링크를 만들기위해서는 새로만들 링크의 이름과 원본 파일 혹은 디렉터리의 경로가 필요합니다.

~~~
$ ln -s [원본 경로] [링크 이름]
~~~

ex 
~~~
$ ln -s /var/lib/jenkins/workspace ./jenkins
~~~

![](https://velog.velcdn.com/images/sunblock99/post/25855413-af06-467e-aa12-dbb0e1a152b4/image.png)


# 심볼릭링크 삭제

~~~
$ rm [심볼릭링크 이름]
~~~

ex 

~~~
$ rm ./jenkins
~~~

<br><br><br>

> [TIP]
심볼릭 링크는 FTP를 통하여 삭제하지 말것. FTP 클라이언트툴의 특성인지는 몰라도 FTP로 심볼릭 링크를 삭제하니 원본 디렉토리까지 삭제하는 문제가 있었다.