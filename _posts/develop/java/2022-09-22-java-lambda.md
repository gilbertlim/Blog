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

> Lambda가 사용되는 Context를 이용해서 람다의 Type을 추론할 수 있다.
> 

형식 확인 과정

1. filter 메서드 선언 확인
    
    ```java
    List<Apple> heaviorThan150g = filter(inventory, (Apple apple) -> apple.getWeight() > 150);
    ```
    
2. filter 메서드는 두번 째 파라미터로 Predicate(Apple) **형식을 기대한다. (대상 형식, target type)**
    
    ```java
    public List<Apple> filter(List<Apple> inventory,  Predicate<Apple> p) {
    
    }
    ```
    
3. Predicate(Apple)은  test라는 한 개의 추상 메서드를 정의하는 함수형 인터페이스이다.
    
    ```java
    public interface Predicate<T> {
    	boolean test(Apple apple)
    }
    ```
    
4. test 메서드는 Apple을 받아 boolean을 반환하는 함수 디스크립터를 묘사한다.
    
    ```java
    Apple -> boolean
    ```
    
5. filter 메서드로 전달된 인수는 이와 같은 요구사항을 만족해야 한다.
    
    ```java
    (동일)
    
    Functional Descripter : Apple -> boolean
    Lambda Signature : Apple -> boolean
    ```
    

## 같은 람다, 다른 함수형 인터페이스

같은 람다 표현식이더라도 호환되는 추상메서드를 가진 다른 함수형 인터페이스로 사용될 수 있다.

```java
Comparator<Apple>                 c1 = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
ToINtBiFunction<Apple, Apple>     c2 = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
BiFunction<Apple, Apple, Integer> c3 = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```

## 형식 추론

람다 표현식이 사용된 콘텍스트(대상 형식)을 사용하여 람다 표현식과 관련된 함수형 인터페이스를 추론한다.

상황에따라 사용하거나 말거나~

**람다식 (람다 시그니처 : 타입, 파라미터 순서, 개수)**

```java
List<Apple> greenApples = filter(inventory, apple -> GREEN.equals(apple.getColor())); // 대상 형식을 이용해서 함수 디스크립터를 알 수 있으므로 컴파일러가 람다의 시그니처를 추론할 수 있다.
```

**메서드 (대상형식)**

```java
public List<Apple> filter(List<Apple> inventory,  Predicate<Apple> p) { // 대상 형식 : Predicate<Apple>

}
```

**함수형 인터페이스**

```java
public interface Predicate<T> {
	boolean test(Apple apple)
}
```

## 지역 변수 사용 (Capturing Lambda)

파라미터로 넘겨진 변수 이외에 자유 변수를 활용할 수 있음

- final로 선언된 변수 만 가능(한 번만 할당 할 수 있음)

```java
int portNumber = 1337;

Runnerble r = () -> System.out.prinln(portNumber);
```

## 메서드 참조 (Method Reference)

특정 메서드만을 호출하는 람다의 축약형

기존 메서드 정의를 재활용해서 람다처럼 전달 가능

as-is

```java
inventory.sort((Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
```

Method Referernce

```java
// Apple 클래스에 정의된 getWeight의 메서드 참조
inventory.sort(comparing(Apple::getWeight));
```

### Lambda & Method Reference

| Lambda | Method Reference |
| --- | --- |
| (Apple apple) -> apple.getWeight() | Apple::getWeight |
| Thread.currentThread().dumpStack() | Thread.currentTread()::dumpStack |
| (str, i) → str.substring(i) | String::substring |
| (String s) → System.out.println(s) | System.out::println |
| (String s) → this.isValidName(s) | this::isValidName |

### 생성 방법

1. 정적 메서드 참조 : `Integer::parseInt`
2. 다양한 형식의 인스턴스 메서드 참조 : `String::length`
3. 기존 객체의 인스턴스 메서드 참조 : `expensiveTransaction::getValue`

```java
@Component
public class MethodReference {

    @EventListener(ApplicationStartedEvent.class)
    public void main() {
        System.out.println("===== Method Reference =====");

        List<String> str = Arrays.asList("a", "b", "A", "D");
        str.sort(String::compareToIgnoreCase);

        System.out.println("str = " + str);

        ToIntFunction<String> stringToInt = Integer::parseInt;

        int number = stringToInt.applyAsInt("0") + 1;
        System.out.println("number + 1 = " + number);

        BiPredicate<List<String>, String> contains = List::contains;
        boolean existA = contains.test(str, "a");
        System.out.println("existA = " + existA);

        Predicate<String> startsWithNumber = this::startsWithNumber;
        boolean isStartedNumber = startsWithNumber.test("1sdasd");
        System.out.println("isStartedNumber = " + isStartedNumber);

    }

    public Boolean startsWithNumber(String s) {
        return Character.isDigit(s.charAt(0));
    }
}
```

## 생성자 참조 (Constructer)

ClassName::new 와 같이 new 키워드를 통해 기존 생성자의 참조를 만들 수 있다.

```java
@Component
public class ConstructorReference {

    @EventListener(ApplicationStartedEvent.class)
    public void main() {
        System.out.println("===== Constructor Reference =====");

        
        // Supplier<BlueApple> b1 = () -> new BlueApple();
        Supplier<BlueApple> ba = BlueApple::new;
        BlueApple ba1 = ba.get();
        System.out.println("ba1.getClass() = " + ba1.getClass());

        
        // Function<Integer, GreenApple> a2 = (weight) -> new GreenApple(weight);
        Function<Integer, GreenApple> ga = GreenApple::new;
        GreenApple ga1 = ga.apply(100);
        System.out.println("ga1.getWeight() = " + ga1.getWeight());

        
        List<Integer> weights = Arrays.asList(7, 3, 4, 10);
        // List<GreenApple> greenApples = map(weights, weight -> new GreenApple(weight));
        List<GreenApple> greenApples = map(weights, GreenApple::new);
        for (GreenApple gas: greenApples) {
            System.out.println("weight = " + gas.getWeight());
        }
        
        
        // BiFunction<Integer, String, Apple> a1 = (weight, color) -> new Apple(weight, color);
        BiFunction<Integer, String, Apple> a = Apple::new;
        Apple a1 = a.apply(200, "GREEN");
        System.out.println("a1.getWeight() = " + a1.getWeight());
    }

    public List<GreenApple> map(List<Integer> list, Function<Integer, GreenApple> f) {
        List<GreenApple> result = new ArrayList<>();

        for (Integer i : list) {
            result.add(f.apply(i));
        }

        return result;
    }
}
```

```java
@NoArgsConstructor
public class BlueApple {
}
```

```java
@AllArgsConstructor
@Getter
public class GreenApple {
    public Integer weight;
}
```

## 람다, 메서드 참조 활용 예제

as-is

```java
public class AppleComparator implements Comparator<Apple> {
	public int compare(Apple a1, Apple a2) {
		return a1.getWeight().compareTo(a2.getWeight());
	}
}

// void sort(Comparator<? super E> c)
inventory.sort(new AppleComparator());
```

anonymous class

```java
inventory.sort(new Comparator<Apple>() {
	public int compare(Apple a1, Apple a2) {
		return a1.getWeight().compareTo(a2.getWeight());
	}
});
```

lambda

```java
inventory.sort((Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());

inventory.sort((a1, a2) -> a1.getWeight().compareTo(a2.getWeight());
```

static method

```java
import static java.util.CoAAmparator.comparing;
inventory.sort(comparing(apple -> apple.getWeight());
```

## 람다 표현식 조합을 위한 메서드

Utility Method : Comparator, Function, Predicate 가 있었다.

조합을 위해 추가적으로 메서드를 제공한다.

**Default Method : 추상 메서드가 아니므로 함수형 인터페이스의 정의를 벗어나지 않음**

### Comparator

reversed()

```java
inventory.sort(comparing(Apple::getWeight).reversed());
```

thenComparing()

```java
inventory.sort(comparing(Apple::getWeight)
	.reversed()
	.tehnComparing(Apple::getCountry));
```

### Predicate

netgate()

```java
Predicate<Apple> notRedApple = redApple.netgate();
```

and()

```java
Predicate<Apple> redAndHeavyApple = redApple.and(apple -> apple.getWeight() > 150);
```

or()

```java
Predicate<Apple> redAndHeavyAppleOrGreen = redApple.and(apple -> apple.getWeight() > 150).or(apple -> GREEN.equals(a.getColor()));
```

### Function

andThen()

```java
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
Function<Integer, Integer> h = f.andThen(g); //g(f(x))
int result = h.apply(1) // 4
```

compose()

```java
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
Function<Integer, Integer> h = f.compose(g); //f(g(x))
int result = h.apply(1) // 3
```