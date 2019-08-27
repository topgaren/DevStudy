## 템플릿 메소드(Template Method) 패턴

### 개요
상위 클래스에서 처리의 흐름을 제어하고, 하위 클래스에서 처리의 내용을 구체화시키는 패턴. 여러 클래스에서 공통적인 사항을 상위 추상 클래스에 구현하고 각각의 상세 부분은 하위 클래스에서 구현함으로써 코드의 중복을 줄이고, 리팩토링에 유리한 패턴이다. **상속을 이용한 확장 개발 방법**으로써 전략 패턴(Strategy Pattern)과 함께 가장 많이 사용되는 패턴 중 하나.

### 템플릿 메소드 패턴 적용 예시 : 커피와 차를 만드는 알고리즘
```java
public class Coffee {

    public void makeCoffee() {
        boilWater();
        brewCoffeeGrinds();
        pourInCup();
        addSugarAndMilk();
    }

    public void boilWater() { System.out.println("물을 끓인다."); }
    public void brewCoffeeGrinds() { System.out.println("커피를 우려낸다."); }
    public void pourInCup() { System.out.println("컵에 따른다."); }
    public void addSugarAndMilk() { System.out.println("설탕과 우유를 추가한다."); }
}
```

```java
public class Tea {

    public void makeTea() {
        boilWater();
        steepTeaBag();
        pourInCup();
        addLemon();
    }

    public void boilWater() { System.out.println("물을 끓인다."); }
    public void steepTeaBag() { System.out.println("차를 우려낸다."); }
    public void pourInCup() { System.out.println("컵에 따른다."); }
    public void addLemon() { System.out.println("레몬을 추가한다."); }
}
```
커피와 차를 만드는 과정에서 공통적인 부분은 다음과 같다.
1. 물을 끓인다.
2. 컵에 따른다.
3. 대상을 우려낸다. (커피와 차에 따라 대상이 달라진다.)
4. 첨가물이 있다. (커피와 차에 따라 첨가물이 달라진다.)

공통적인 부분을 추상화 시켜 상위 클래스를 생성하고, 커피와 차에 따라 달라지는 코드는 하위 클래스에서 구체화 할 수 있도록 코드를 아래와 같이 변경할 수 있다.

![template_method_pattern](https://user-images.githubusercontent.com/45558487/63245458-34056900-c29b-11e9-8499-236600d4b7c0.png)


```java
public abstract class CaffeineBeverage {

    public void prepareRecipe() {
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }

    public abstract void brew();
    public abstract void addCondiments();
    
    public void boilWater() { System.out.println("물을 끓인다."); }
    public void pourInCup() { System.out.println("컵에 따른다."); }
}
```

```java
public class Coffee extends CaffeineBeverage {

    @Override
    public void brew() { System.out.println("커피를 우려낸다."); }

    @Override
    public void addCondiments() { System.out.println("설탕과 우유를 추가한다."); }
}
```

```java
public class Tea extends CaffeineBeverage {

    @Override
    public void brew() { System.out.println("차를 우려낸다."); }

    @Override
    public void addCondiments() { System.out.println("레몬을 추가한다."); }
}
```

> Reference
> https://jusungpark.tistory.com/24