JAVA에서 문자열을 비교할때는 주로 equals 메서드를 사용한다.

하지만 equals.() 를 부르는 인스턴스가 null인 경우 NullPointerException이 발생하게 된다.

예를들면 아래와 같은 상황

```
public class MyClass {
    public static void main(String args[]) {
        String strA = null;
        System.out.println(" Result : " + strA.equals("test"));
    }
}

결과값 :
Exception in thread "main" java.lang.NullPointerException
    at MyClass.main(MyClass.java:4)
Command exited with non-zero status 1
```

이 경우 strA에는 null 값이 들어있는데, equals를 호출하는 상황이 생겨 Null 포인터 익셉션이 발생하게 된다.

이를 위해서는

1.strA의 null 여부를 체크한 후 equals 호출

```
public class MyClass {
    public static void main(String args[]) {
        String strA = null;
        if(strA!=null)
          System.out.println(" Result : " + strA.equals("test"));
    }
}

결과값:
```

2.비교할 문자열이 equals메서드를 호출

```
public class MyClass {
    public static void main(String args[]) {
        String strA = null;
        System.out.println(" Result : " + "test".equals(strA));
    }
}


결과값 : false
```

위와 같이 비교할 문자열이 equals 메서드를 호출할 경우에는 NullPointerException이 발생하지 않는다.

이때는 null값과 문자열을 비교하는데, 결과값은 false로 출력이 된다.
