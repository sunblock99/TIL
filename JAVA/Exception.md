예외 처리

1. try, catch
   예외가 발생했을때 우리는 try ... catch ... finally 라는 키워드로 예외를 처리할 수 있거나 메소드를 호출한 곳으로 던질 수 있습니다. 한 가지 중요한 점은 자바에서 모든 예외는 Exception이라는 클래스를 상속받습니다. Exception의 상속 트리를 아래에 간략하게 나타내었습니다.

![](https://velog.velcdn.com/images/sunblock99/post/d9f0b4ab-bb1d-4c1c-b024-884e2c11e1f8/image.png)

예외 처리하는 방식은 이렇습니다.

try{
//예외가 발생될만한 코드
}catch(FileNotFoundException e){ //FileNotFoundException이 발생했다면

}catch(IOException e){ //IOException이 발생했다면

}catch(Exception e){ //Exception이 발생했다면

}finally{
///어떤 예외가 발생하던 말던 무조건 실행
}

try 블록 : 이 블록에서 예외가 발생할만한 코드가 쓰여집니다.

catch (예외 종류) 블록 : 이 부분에서 예외가 발생되었을때 처리하는 동작을 명시합니다. catch블록은 여러 개가 있을 수 있습니다. 맨 처음 catch 블록에서 잡히지 않는 예외라면 다음 catch의 예외를 검사합니다. 이때 상속관계에 있는 예외 중 부모가 위의 catch, 그리고 그 자식 예외 클래스가 아래의 catch로 놓일 순 없습니다. 예를 들어 아래와 같이말이죠.

```
try{
	//.. 중략 ..//
} catch (Exception e){
	//컴파일 오류 발생
} catch (IOException e){

}

```

Exception 클래스는 모든 예외의 부모이기 때문에 Exception을 IOException보다 위에서 처리할 수는 없다는 뜻입니다. 왜냐면 IOException의 catch블록은 도달할 수 없는 코드이기 때문이죠.

finally 블록 : 여기서는 예외가 발생하건 발생하지 않건 공통으로 수행되어야할 코드가 쓰여집니다. 임시 파일의 삭제 등 뒷정리 코드가 쓰입니다.

이것을 이용해서 우리는 위의 코드를 예외처리할 수 있습니다.

```
public static void main(String[] ar){
	int a,b;
	a=10;
	b=0;
	try {
		int c=a/b;
		System.out.println(c);	//예외발생으로 실행 불가한 코드
	}catch(ArithmeticException e) {
		System.out.println("ArithmeticException 발생");
		System.out.println("0으로 나눌 수는 없습니다");
		e.printStackTrace();
	}finally {
		System.out.println("finally 실행");
	}
}
```

2. throws
   아까전에 예외를 그냥 던질 수 있다고 했죠? 그 의미가 어떤 의미냐면 예외를 여기서 처리하지 않을테니 나를 불러다가 쓰는 녀석에게 에러 처리를 전가하겠다는 의미이며 코드를 짜는 사람이 이 선언부를 보고 어떤 예외가 발생할 수 있는지도 알게 해줍니다. 어떤 뜻인지 모르겠다구요? 아래의 코드를 통해서 알아보도록 합니다.

```
public static void divide(int a,int b) throws ArithmeticException {
	if(b==0) throw new ArithmeticException("0으로 나눌 수는 없다니까?");
	int c=a/b;
	System.out.println(c);
}
public static void main(String[] ar){
	int a=10;
	int b=0;

	divide(a,b);
}
```

divide()메소드는 a와 b를 나눈 후에 출력하는 역할을 하는데, 이 나누기 부분에서 우리는 예외가 발생할 수 있음을 알았습니다. 그래서 try, catch로 예외 처리를 해야하지만, divide()를 호출하는 부분에서 처리하기를 원합니다. 왜냐면 divide()를 호출한 곳에서 예외가 발생한 다음의 처리를 divide() 메소드가 정하지 않기 때문입니다. 예를 들어 main메소드에서는 예외가 발생하면 다시 divide()를 호출하거나, 프로그램을 끝내거나, b의 값을 다시 입력받거나 해야하기 때문이고, divide()메소드가 그 결정을 할 수 없다는 의미입니다. 그래서 throws ArithmeticException을 divide를 호출한 main에다가 던지는 것(throw)입니다. 여기서 예외를 던지는 방법은 아래와 같습니다.

(아, 참고로 Exception 생성자 중에서 메시지를 받는 생성자가 있는데, 메시지를 보려면 getMessage()메소드를 이용할 수 있습니다. 아래에서 그 메소드를 사용합니다.)

```
throw 예외객체
ex) throw new Exception("예외 발생!")
```
