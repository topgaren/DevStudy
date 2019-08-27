## 데코레이터(Decorator) 패턴

### 개요
데코레이터 패턴이란 객체를 동적(dynamic)으로 서브 클래스를 이용하여 기능을 확장하는 방법이다.
> 계방-폐쇄 원칙(OCP; Open-Closed Principle)
> 클래스는 확장에 대해서는 열려 있어야 하지만, 코드 변경에 대해서는 닫혀 있어야 한다.

### 데코레이터 패턴 클래스 다이어그램

![decorator_pattern](https://user-images.githubusercontent.com/45558487/63254228-53f25800-c2ae-11e9-9f7a-3f502ca93835.png)


* 각 Decorator 안에는 Component의 인스턴스를 참조할 수 있는 참조 변수가 있다.
* Decorator는 자신이 장식할 Component와 같은 인터페이스 또는 추상 클래스를 구현한다.
* ConcreateDecoratorA, ConcreateDecoratorB 에는 그 장식 대상이 되는 Component 인스턴스를 참조할 수 있는 참조 변수가 있다. 이를 통해 Component를 확장할 수 있다.
* 새로운 메소드 추가도 가능하지만 일반적으로 Component의 원래 메소드를 호출하기 전후에 별도의 작업을 처리하는 방식으로 새로운 기능을 추가한다.

### 데코레이터 패턴 적용 예시 : 치즈 피자에 토핑 얹기
피자 추상 클래스와 이를 구현한 치즈 피자를 정의
```java
public abstract class Pizza {

    public abstract String getName();
}
```

```java
public class CheesePizza extends Pizza {

    @Override
    public String getName() { return "치즈 피자"; }
}
```

토핑 추상 클래스와 이를 구현한 페퍼로니 토핑, 베이컨 토핑 클래스를 정의
```java
public abstract class Topping extends Pizza {

    protected Pizza pizza;

    public Topping(Pizza pizza) {
        this.pizza = pizza;
    }

    @Override
    public abstract String getName();
}
```

```java
public class PepperoniTopping extends Topping {

    public PepperoniTopping(Pizza pizza) {
        super(pizza);
    }

    @Override
    public String getName() {
        return "페퍼로니 " + pizza.getName(); 
    }
}
```

```java
public class BaconTopping extends Topping {

    public BaconTopping(Pizza pizza) {
        super(pizza);
    }

    @Override
    public String getName() {
        return "베이컨 " + pizza.getName();
    }
}
```

아래 코드는 데코레이터 패턴을 사용하여 치즈 피자에 토핑을 얹는 것을 구현한 것이다.
```java
public static void main(String[] args) {
        
    // 기본적인 치즈 피자
    Pizza cheesePizza = new CheesePizza();
    System.out.println(cheesePizza.getName());
    
    // 치즈 피자 + 페퍼로니 토핑
    Pizza cheesePizzaWithPepperoni = new PepperoniTopping(cheesePizza);
    System.out.println(cheesePizzaWithPepperoni.getName());
    
    // 치즈 피자 + 베이컨 토핑
    Pizza cheesePizzaWithBacon = new BaconTopping(cheesePizza);
    System.out.println(cheesePizzaWithBacon.getName());
    
    // 치즈 피자 + 베이컨 토핑 + 페퍼로니 토핑
    Pizza cheesePizzaWithBaconAndPepperoni 
        = new BaconTopping(new PepperoniTopping(cheesePizza));
    System.out.println(cheesePizzaWithBaconAndPepperoni.getName());
}
```
```
치즈 피자
페퍼로니 치즈 피자
베이컨 치즈 피자
베이컨 페퍼로니 치즈 피자
```

> Reference
> https://jusungpark.tistory.com/9?category=630296
> https://jdm.kr/blog/78