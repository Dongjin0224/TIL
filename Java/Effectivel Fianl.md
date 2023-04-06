# Java의 Effectively final이란?
Java8에서 final이 붙지 않은 변수의 값이 변경되지 않는다면, 그 변수를 **Effectively final**이라고 한다.

```java
int num = 10;
Runnable runnable = () -> {
    System.out.println("number : " + num);
};
runnable.run(); // number : 10

// num을 변경하면?

int num = 10;
Runnable runnable = () -> {
    num++;
    System.out.println("number : " + num);
};
runnable.run(); 
```
![image](https://user-images.githubusercontent.com/82895809/230246123-26034cdb-a9b6-49c7-8de2-dcd2c81709a4.png)


컴파일 에러가 발생.

에러 내용을 보면 final을 사용하거나 effectively final을 사용하라고 한다.

num이 변경되는 순간 effectively final이 아니다.

```java
		Runnable runnable = () -> {
			System.out.println("number : " + num);
		};
		runnable.run();
		num++;
```
이렇게 람다 표현식 밖에서 값을 변경해도 동일한 컴파일 에러가 발생한다.
