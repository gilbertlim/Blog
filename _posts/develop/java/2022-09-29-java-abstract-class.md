---
title: "[Java] Abstract Class (추상클래스)"
category: Java
---

# Abstract Class

인터페이스의 역할도 하면서 클래스의 기능도 가지고 있는 자바의 돌연변이 같은 클래스

자바8 버전부터 인터페이스에 default 메서드가 추가되어 추상 클래스와의 차이점이 살짝 모호해졌다.

하지만 추상 클래스는 인터페이스와는 달리 일반 클래스처럼 객체변수, 생성자, private 메서드 등을 가질 수 있다.

<br>

## Class

```java
public class Animal {

    @Setter
    String name;

    void printName() {
        System.out.println("name");
    }
}
```

<br>

## Interface

```java
public interface Barkable {

    void bark();

    // 인터페이스의 메서드는 몸통을 가질 수 없지만 디폴트 메서드를 사용하면 구현된 형태의 메서드를 가질 수 있음.
    default void dm() {
        System.out.println();
    }
}
```

<br>

## Abstract Class

```java
public abstract class Predator extends Animal implements Barkable {

    // 인터페이스의 메소드와 같은 역할을 하는 메소드는 abstract를 붙여야 함.
    // 상속받는 클래스에서 해당 abstract 메소드를 구현해야 함.
    abstract String getFood();

    void printFood() {
        System.out.println();
    }

    // 추상 클래스에서 상수는 static을 붙여야 함.
    static int LEG_COUNT = 4;

    static int speed() {
        return LEG_COUNT * 30;
    }

}
```

<br>

## Child

```java
public class Lion extends Predator {

		// 추상 클래스를 상속받으면 abstract 메소드를 구현해야 함.
    @Override
    String getFood() {
        return null;
    }

    @Override
    public void bark() {

    }
}
```