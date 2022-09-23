---
title: "[Java] Behavior Parameterization (동작 파라미터화)"
category: Java
---

# Behavior Parameterization

> 아직 어떻게 실행할 것인지 결정하지 않은 코드 블럭을 파라미터화(변수화) 하는 것

로직이 매번 바뀌거나 정해지지 않았을 경우 Interface를 사용하여 로직을 파라미터화 한다.

요구사항에 따라 동일한 메서드를 여러 구현체에서 implements하기 때문에 파라미터화 되어있다고 볼 수 있다.

익명 함수를 쓰거나, 람다함수를 쓰는 경우가 있다.

# 선택 조건을 결정하는 인터페이스

> 로직이 변할 때마다 Interface를 구현하고, 구현체를 파라미터로 사용하여 로직을 수행한다.

filtering 함수에는 리스트를 입력으로 하여 특정 조건에 따라 필터링하는 test 메서드가 있다.

요구사항에 따라 test 메서드의 조건이 달라진다.

<br>

to-be

1. ApplePredicate Interface의 test 메서드를 선언한다.
2. 구현체 AppleRedAndHeavyPredicate에서 색상, 무게 조건으로 필터링하는 test 메서드를 구현한다. 
3. 만든 구현체를 filtering 함수의 파라미터로 넣어 사용한다.
4. filtering 함수가 동작할 때 test 메서드는 생성된 구현체의 로직에 따라 동작하게 된다.
5. 로직이 달라질 때마다 구현체를 생성하고, 생성된 구현체를 파라미터로 사용한다.

<br>

as-is

> 의미 없는 플래그를 사용하는 경우… 

```java
public void filterApples() {
		List<Apple> apples = filterApples(inventory, GREEN, 100, true);
		
		System.out.println("apples = " + apples.get(0).getColor());
}

public static List<Apple> filterApples(List<Apple> inventory, Color color, int weight, boolean flag) {
        List<Apple> result = new ArrayList<>();

        for (Apple apple : inventory) {
            if ((flag && apple.getColor().equals(color.toString())) ||
                (!flag && apple.getWeight() > weight)) {
                result.add(apple);
            }
        }

        return result;
}
```

to-be

```java
public void filterApples() {
		List<Apple> inventory = new ArrayList<>(Arrays.asList(
                new Apple(80, "GREEN"),
                new Apple(155, "GREEN"),
                new Apple(120, "RED")
    ));

		AppleRedAndHeavyPredicate appleRedAndHeavyPredicate = new AppleRedAndHeavyPredicate();
		
		List<Apple> apples = filtering(inventory, appleRedAndHeavyPredicate);
		System.out.println("apples = " + apples.get(0).getColor());
		System.out.println("apples = " + apples.get(0).getWeight());
}

public static List<Apple> filtering(List<Apple> inventory, ApplePredicate p) {
        List<Apple> result = new ArrayList<>();

        for (Apple apple : inventory) {
            if ( p.test(apple)) {
                result.add(apple);
            }
        }
        return result;
}

public static class AppleRedAndHeavyPredicate implements ApplePredicate {

        @Override
        public boolean test(Apple apple) {
            return RED.toString().equals(apple.getColor())
            && apple.getWeight() > 110;
        }
}

public interface ApplePredicate {
    // predicate : true or false를 반환하는 함수
    boolean test(Apple apple);

}
```