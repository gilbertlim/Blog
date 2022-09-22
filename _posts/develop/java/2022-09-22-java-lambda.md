---
title: "[Java] Lambda"
category: Java
---

# 주의사항

<br>

<aside>
⛔ 람다식 사용 시 Interface 내 Method는 1개여야 한다.
</aside>

# Expressions

## 1) `(Type Parameter) -> {};`

```java
CalculatorParamAll cal4 = (int num1, int num2) -> { return num1 + num2; };
```

## 2) `(Parameter) -> {};`

> 모든 매개변수의 Type이 같으면
생략 가능
> 

```java
CalculatorParamAll cal5 = (num1, num2) -> { return num1 + num2; };
```

## 3) `() -> {};`

> Method에 매개변수가 없다면
생략 가능
> 

```java
CalculatorParamNull cal6 = () -> { System.out.println("cal6.cal() = 매개변수가 없는 경우입니다."); };
```

## 4) `() -> ;`

> 실행할 문장이 1개이면
중괄호 생략 가능, return도 생략 필수
> 

```java
CalculatorParamAll cal7 = (num1, num2) -> num1 + num2;
```

## 5) `Paramter -> ;`

> 매개변수가 1개이고, 실행할 문장이 1개이면
괄호, 중괄호 생략 가능
> 

```java
CalculatorParamOne cal8 = num1 -> System.out.println("cal8.cal(num1) = " + num1);
```

# @FunctionalInterface

> 람다식으로 표현하기 위해 사용되는 Interface위에 해당 어노테이션을 붙이면 컴파일 타임에 에러를 잡아 준다.
> 

```java
@FunctionalInterface
public interface CalculatorParamAll {

    // lambda를 사용하기 위해서는 메소드가 하나여야 한디.
    public int cal(int num1, int num2);

    public int cal2(); // 에러 발생
}
```

# Anonymous class → lambda

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