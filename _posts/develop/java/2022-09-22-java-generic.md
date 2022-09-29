---
title: "[Java] Generic (제네릭)"
category: Java
---

# Generic

**제네릭(Generic)은 클래스 내부에서 지정하는 것이 아닌 외부에서 사용자에 의해 지정되는 것을 의미함.**

특정(Specific) 타입을 미리 지정해주는 것이 아닌 필요에 의해 지정할 수 있도록 하는 일반(Generic) 타입.

타입은 인스턴스 생성 시점에 확정지어 사용함.

# 관례적 명명

```java
<T>	Type
<E>	Element
<K>	Key
<V>	Value
<N>	Number
```

# Generic Class

```java
public class MadPlay<T> {
    private T val;

    public T getVal() {
        return val;
    }

    public void setVal(T val) {
        this.val = val;
    }

    public MadPlay(T val) {
        this.val = val;
    }
}
```

```java
public class GenericUse {

    public static void use() {

        // class
        GenericClass<String> stringObj = new GenericClass<>("abc");
        System.out.println("stringMadPlay.getVal() = " + stringObj.getVal());
        GenericClass<Integer> integerObj = new GenericClass<>(1);
        System.out.println("stringMadPlay.getVal() = " + integerObj.getVal());
    }
}
```

# Generic Interface

```java
public interface GenericInterface<T> {
    void addElement(T t, int index);

    T getElement(int index);
}
```

```java
public class GenericImpl<T> implements GenericInterface<T> {

    private T[] array;

    public GenericImpl() {
        array = (T[]) new Object[10];
    }

    @Override
    public void addElement(T element, int index) {
        array[index] = element;
    }

    @Override
    public T getElement(int index) {
        return array[index];
    }
}
```

```java
public class GenericUse {

    public static void use() {

        // interface
        GenericImpl<String> genericImpl= new GenericImpl<>();
        genericImpl.addElement("abc", 0);
        System.out.println("genericImpl.getElement(0) = " + genericImpl.getElement(0));;
    }
}
```

# Generic Method

```java
public class GenericUse {

    public static void use() {

        // method
        String[] array = new String[] {"a", "b", "c"};
        Stack<String> stack = new Stack<>();
        arrayToStack(array, stack);
        System.out.println("stack.get(0) = " + stack.get(0));
    }

    public static <T> void arrayToStack(T[] arr, Stack<T> stack) {
        stack.addAll(Arrays.asList(arr));
    }
}
```