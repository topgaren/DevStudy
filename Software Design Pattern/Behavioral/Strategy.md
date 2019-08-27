## 전략(Strategy) 패턴

### 개요
전략 패턴은 **행위를 클래스로 캡슐화해** 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴이다. 여기서 특정 행위를 전략(Strategy)이라고 하며 문제를 해결하는 알고리즘 및 비즈니스 로직을 담고 있다. 특히 게임 프로그래밍에서 게임 캐릭터가 자신이 처한 상황에 따라 공격이나 행동 방식을 변경하고 싶을 때 전략 패턴이 유용하게 사용된다.

### 템플릿 메소드 패턴과의 비교
템플릿 메소드 패턴은 같은 문제의 해결책으로 상속을 이용한 확장 방법을 이용한다. 반면 전략 패턴은 각각의 행위를 클래스로 정의하고 객체 주입을 통한 행위 변경 방법을 이용한다.

### 전략 패턴의 클래스 다이어그램
![strategy_pattern](https://user-images.githubusercontent.com/45558487/63312348-3e2b7400-c33c-11e9-980d-b51595801314.png)

* Strategy : 인터페이스 또는 추상 클래스로 외부에서 동일한 방법으로 알고리즘을 호출할 수 있게 한다.
* ConcreteStrategy : 전략 패턴에서 명시한 알고리즘을 실제로 구현한 클래스.
* Context : 전략 패턴을 이용하는 역할. 필요에 따라 동적으로 전략을 바꿀 수 있도록 `setter` 메소드를 제공.

### 전략 패턴 구현 예시 : 소총과 수류탄을 사용하는 군인 구현
소총과 수류탄을 사용하는 행위를 전략으로 구현한다.
```java
public interface Strategy {

    public void runStrategy();
}
```
```java
public class RiffleStrategy implements Strategy {

    @Override
    public void runStrategy() {
        System.out.println("Todadadadadadadadadadadada!!!");
    }
}
```
```java
public class GrenadeStrategy implements Strategy {

    @Override
    public void runStrategy() {
        System.out.println("Fire in the hole!!!");
    }
}
```
소총 및 수류탄 전략을 사용하는 군인 클래스를 정의한다. 또한 전략을 동적으로 교체할 수 있도록 `setter` 메소드를 구현한다.
```java
public class Soldier {

    private Strategy strategy;
    
    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }
    
    public void runContext() {
        System.out.println("Mission Start");
        strategy.runStrategy();
        System.out.println("Mission Complete\n");
    }
}
```
위에서 정의한 군인 클래스는 전략 패턴에서 Context에 해당한다. 전략을 교체하며 Context를 사용하는 Client 객체를 다음과 같이 구현한다.
```java
public class CommandOfficer {

    public static void giveAnOrder() {
        Soldier soldier = new Soldier();
        
        // 소총 및 수류탄 전략 생성
        Strategy riffleStrategy = new RiffleStrategy();
        Strategy grenadeStrategy = new GrenadeStrategy();
        
        // 소총 전략 사용
        soldier.setStrategy(riffleStrategy);
        soldier.runContext();
        
        // 수류탄 전략 사용
        soldier.setStrategy(grenadeStrategy);
        soldier.runContext();
    }
}
```
```
Mission Start
Todadadadadadadadadadadada!!!
Mission Complete

Mission Start
Fire in the hole!!!
Mission Complete
```

> Reference
> https://limkydev.tistory.com/84
> https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html