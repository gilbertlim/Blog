---
title: "[Java] Anonymous Class (익명 클래스)"
category: Java
---

# Anonymous Class

- 이름이 없는 클래스
- 일시적으로 한번만 사용되어야 하는 객체
- 재사용성이 없고 확장성을 활용하는 것이 유지보수에서 더 불리할 때
- 예 : UI 이벤트 처리, Thread 객체 등.

<br>

ApplePredicate 객체를 익명 클래스로 선언한다. (한번 사용되고 버려지는 객체)

```java
List<Apple> applesAnonymous = filterApples(inventory, new ApplePredicate() {
            @Override
            public boolean test(Apple apple) {
                return RED.toString().equals(apple.getColor())
                        && apple.getWeight() > 110;
            }
        });
```