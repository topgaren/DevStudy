## 팩토리 메소드(Factory Method) 패턴

### 개요
팩토리 메소드 패턴은 객체를 생성하는 부분을 서브 클래스에 위임하는 패턴이다. 객체를 직접 생성하지 않고 팩토리 메소드 클래스를 통해 객체를 대신 생성시키고 그 객체를 반환 받아 사용하기 때문에 결합도를 낮추고, 효율적인 코드 제어를 할 수 있어 유지보수가 용이하다는 장점이 있다.

### 팩토리 메소드 구현의 예

![factory_method_pattern](https://user-images.githubusercontent.com/45558487/63327259-d0497180-c368-11e9-912b-b2d7de900506.png)

**Coffee.java**
```java
public abstract class Coffee {

    public abstract String getName();
}
```

**VanilaCoffee.java**
```java
public class VanilaCoffee extends Coffee {

    @Override
    public String getName() {
        return "Vanila Coffee";
    }
}
```

**MokaCoffee.java**
```java
public class MokaCoffee extends Coffee {

    @Override
    public String getName() {
        return "Moka Coffee";
    }
}
```

**Factory.java**
```java
public abstract class Factory {

    public abstract Coffee makeCoffee(String coffeeName);
}
```

**CoffeeFactory.java**
```java
public class CoffeeFactory extends Factory {

    @Override
    public Coffee makeCoffee(String coffeeName) {

        switch(coffeeName) {
            case "Vanila": return new VanilaCoffee();
            case "Moka":   return new MokaCoffee();
        }
        return null;
    }
}
```