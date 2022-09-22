---
title: "[Java] Behavior Parameterization (동작 파라미터화)"
category: Java
---

# BehaviorParameterization

> 아직 어떻게 실행할 것인지 결정하지 않은 코드 블럭을 파라미터화(변수화) 한다.
> 

<br>

filter 로직이 매번 바뀌거나 정해지지 않았을 경우 interface를 사용하여 파라미터화 한다.

동일한 메서드를 여러 구현체에서 implements하기 때문에 파라미터화 되어있다고 볼 수 있다.

익명 함수를 쓰거나, 람다함수를 쓰는 경우가 있다.

<br>

as-is

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
		AppleRedAndHeavyPredicate appleRedAndHeavyPredicate = new AppleRedAndHeavyPredicate();
		
		List<Apple> apples = filterApples(inventory, appleRedAndHeavyPredicate);
		System.out.println("apples = " + apples.get(0).getColor());
		System.out.println("apples = " + apples.get(0).getWeight());
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