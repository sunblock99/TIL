오늘은 업무를 보다가 특정 페이지가 열리지 않는 에러가 나와서

구글링을 해본결과

org.springframework.web.util.NestedServletException

아무리 찾아봐도 어떤 이유때문인지 알 수 없어서, 저 SingletonSupplier에 대해 찾아보니 springframework 5버전에 존재하는 클래스 같았다.

pom.xml 설정에서

![](https://velog.velcdn.com/images/sunblock99/post/ad866b07-3d0b-43e0-9b2a-d88ce35f358c/image.png)

스프링 버전을 바꿔줘여ㅑ한다 !!
