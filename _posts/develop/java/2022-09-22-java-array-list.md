---
title: "[Java] ArrayList"
category: Java
---

# ArrayList

- 표준 배열보다는 느리지만 배열에서 많은 조작이 필요한 경우 유용하게 사용할 수 있음
- List Interface를 상속 받아 사용됨
- 객체가 추가되어 용량을 초과하면 자동으로 부족한 크기만큼 용량이 늘어남

## 1차원적인 방법

```java
List<Apple> inventory = new ArrayList<>();

Apple apple = new Apple(70, "RED");
inventory.add(apple);

Apple apple2 = new Apple(60, "GREEN");
inventory.add(apple2);
```

### 배열 생성 시 초기화 하기

- `.add()` 불가

```java
List<Apple> inventory = Arrays.asList(
			new Apple(80, "GREEN"),
			new Apple(155, "GREEN"),
			new Apple(120, "RED")
);

// 에러남.
Apple apple = new Apple(10, "GREEN");
inventory.add(apple);
```

- `.add()` 가능

```java
List<Apple> inventory = new ArrayList<>(Arrays.asList(
			new Apple(80, "GREEN"),
			new Apple(155, "GREEN"),
			new Apple(120, "RED")
));

Apple apple = new Apple(10, "GREEN");
inventory.add(apple);
```