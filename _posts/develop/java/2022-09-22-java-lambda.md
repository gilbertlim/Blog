---
title: "[Java] Lambda"
category: Java
---

# Lambda

> 메서드로 전달할 수 있는 익명함수를 단순화한 것

<aside>
⛔ 람다식 사용 시 Interface는 함수형 인터페이스여야 한다.
디폴트 메서드가 많더라도 추상 메서드가 1개면 함수형 인터페이스

</aside>

<aside>
⛔ 제네릭 파라미터에는 참조형 형식(≠ 기본형)만 사용할 수 있다.
Byte, Integer, Object, List (≠ int, double, byte, char)

기본형은 Boxing을 해서 참조형으로 변환해줘야 한다.

</aside>

<aside>
⛔ Java에서 제공하는 함수형 인터페이스는 예외를 던지는 동작을 허용하지 않는다.
예외를 던지는 람다식을 만드려면,  함수형 인터페이스를 직접 생성하거나 람다를 try/catch로 감싸야 한다.

</aside>

## How to Express

> (람다 파라미터) → 람다 바디

(파라미터) → 표현식(expression)
(파라미터) → { 구문(statement); }
> 

### 1) `(Type Parameter) -> {};`

```java
CalculatorParamAll cal4 = (int num1, int num2) -> { return num1 + num2; };
```

### 2) `(Parameter) -> {};`

> 모든 매개변수의 Type이 같으면
생략 가능
> 

```java
CalculatorParamAll cal5 = (num1, num2) -> { return num1 + num2; };
```

### 3) `() -> {};`

> Method에 매개변수가 없다면
생략 가능
> 

```java
CalculatorParamNull cal6 = () -> { System.out.println("cal6.cal() = 매개변수가 없는 경우입니다."); };

execute(() -> {});
```

### 4) `() -> ;`

> 실행할 문장이 1개이면
중괄호 생략 가능, return도 생략 필수
> 

```java
CalculatorParamAll cal7 = (num1, num2) -> num1 + num2;

return () -> "Tricky example ;-)";
```

### 5) `Paramter -> ;`

> 매개변수가 1개이고, 실행할 문장이 1개이면
괄호, 중괄호 생략 가능
> 

```java
CalculatorParamOne cal8 = num1 -> System.out.println("cal8.cal(num1) = " + num1);
```

## Anonymous class → lambda

ApplePredicate

> 시그니처 : methodName=test, type=Apple
> 

global

```java
public interface ApplePredicate {
    // predicate : true or false를 반환하는 함수

    boolean test(Apple apple);
}
```

```java
public List<Apple> filterApples(List<Apple> inventory, ApplePredicate p) { // 파라미터로 Interface가 들어간다.
        List<Apple> result = new ArrayList<>();

        for (Apple apple : inventory) {
            if ( p.test(apple)) {
                result.add(apple);
            }
        }

        return result;
    }

```

as-is

```java
List<Apple> resultAnonymousLambda = filterApples(inventory, new ApplePredicate() {
            @Override
            public boolean test(Apple apple) {
                return RED.toString().equals(apple.getColor());
            }
});
```

to-be

```java
List<Apple> result = filterApples(inventory, (Apple apple) -> RED.toString().equals(apple.getColor()));
```

# Functional Interface (함수형 인터페이스)

> 추상 메서드가 1개인 인터페이스₩
디폴트 메서드 상관 X
> 

```java
@FunctionalInterface
public interface Runnable {
    /**
     * When an object implementing interface {@code Runnable} is used
     * to create a thread, starting the thread causes the object's
     * {@code run} method to be called in that separately executing
     * thread.
     * <p>
     * The general contract of the method {@code run} is that it may
     * take any action whatsoever.
     *
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
}
```

## Java에서 제공하는 함수형 인터페이스 종류

[https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)

## @FunctionalInterface

> 함수형 인터페이스를 가리키는 어노테이션
 람다식으로 표현하기 위해 사용되는 Interface위에 해당 어노테이션을 붙이면 컴파일 타임에 에러를 잡아 준다.
추상메서드가 1개 이상이면 에러 발생
> 

```java
@FunctionalInterface
public interface CalculatorParamAll {

    // lambda를 사용하기 위해서는 메소드가 하나여야 한디.
    public int cal(int num1, int num2);

    public int cal2(); // 에러 발생
}
```

# Function Descriptor (함수 디스크립터)

> 함수형 인터페이스의 추상메서드 시그니처

람다 표현식의 시그니처를 서술하는 메서드
> 

| Interface | Function Descriptor | Abstract method |
| --- | --- | --- |
| Predicate<T> | (T) -> boolean | boolean test(T t); |
| Consumer<T> | (T) -> void | void accept(T t); |
| Function<T,R> | (T) -> R | R apply(T t); |
| Supplier<T> | () -> T | T get(); |
| UnaryOperator<T> | (T) -> T | T apply(T t); |

## 시그니처(signature)

> 메서드명(타입, 파라미터 순서, 개수)
> 

같은 시그니처를 가지는 메서드

```java
int doSomething(int y) 
String doSomething(int x)
int doSomething(int z) throws java.lang.Exception
```

다른 시그니처를 가지는 메서드

```java
doSomething(String[] y);
doSomething(String y);
```

# Execute Around Pattern

> 초기화/준비 코드 → 작업 → 정리/마무리 코드 형식의 코드
> 

```java
@Component
public class BufferedReaderMain {

    ClassPathResource resource = new ClassPathResource("static/data.txt");

    @EventListener(ApplicationStartedEvent.class)
    public void main() throws IOException {
        System.out.println("=====ExecuteAroundPattern / Lambda =====");
        String twoLines = processFile((BufferedReader br) -> br.readLine() + " " + br.readLine());

        System.out.println("twoLines = " + twoLines);

    }

    public String processFile(BufferedReaderProcessor b) throws IOException {

        try (BufferedReader br = new BufferedReader(new FileReader(resource.getFile().getAbsolutePath()))) {
            return b.process(br);
        }
    }

}
```

```java
@FunctionalInterface
public interface BufferedReaderProcessor {
    String process(BufferedReader b) throws IOException;
}
```

# 형식 검사 / 추론 / 제약

## 형식 검사