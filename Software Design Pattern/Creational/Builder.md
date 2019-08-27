## 빌더(Builder) 패턴

### 이펙티브 자바(Effective-Java)
> 규칙 2. 생성자 인자가 많을 때는 빌더(Builder) 패턴 적용을 고려하라

### 객체 생성
* 점층적 생성자 패턴(Telescoping Constructor Pattern)
* 자바빈 패턴(Java Bean Pattern)
* 빌더 패턴(Builder Pattern)

### 점층적 생성자 패턴
다음의 객체를 대상으로 생성자를 생성한다고 가정한다.
```java
public class NutritionFacts {
    // Required parameters(필수 인자)
    private final int servingSize;
    private final int servings;

    // Optional parameters(선택 인자)
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;
}
```
* 점층적 생성자 패턴은 다음과 같은 방법으로 생성자를 정의한다.
  1) 필수 인자를 받는 필수 생성자를 하나 만든다.
  2) 1 개의 선택적 인자를 받는 생성자를 추가한다.
  3) 2 개의 선택적 인자를 받는 생성자를 추가한다.
  4) …반복
  5) 모든 선택적 인자를 다 받는 생성자를 추가한다.

* 점층적 생성자의 특징
  + 모든 인자 조합에 대한 생성자 호출이 가능하다.
  + 인자 수정 시 코드를 수정하기가 어렵다.(특히 인자가 많을수록)
  + 코드 가독성이 떨어진다 - 인자가 많을 때 호출 코드 만으로는 각 인자의 의미를 알기 어렵다.
    ```java
    // 호출 코드만으로는 각 인자의 의미를 알기 어렵다.
    NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 3, 35, 27);
    NutritionFacts pepsiCola = new NutritionFacts(220, 10, 110, 4, 30);
    NutritionFacts mountainDew = new NutritionFacts(230, 10);
    ```

### 자바빈 패턴
자바빈 패턴은 `setter`를 사용하여 생성코드의 가독성을 높인다.
```java
NutritionFacts cocaCola = new NutritionFacts();
cocaCola.setServingSize(240);
cocaCola.setServings(8);
cocaCola.setCalories(100);
cocaCola.setSodium(35);
cocaCola.setCarbohdydrate(27);
```

* 자바빈 패턴의 특징
  + 각 인자의 의미 파악이 용이해졌다.
  + 생성자를 여러개 만들지 않아도 된다.
  + 객체 일관성(consistency)이 깨진다 - 1회의 호출로 객체 생성이 끝나지 않았다.
  + `setter` 메서드가 있으므로 immutable 클래스를 만들 수가 없다.


### 빌더 패턴
```java
// Effective Java의 Builder Pattern
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static class Builder {
        // Required parameters(필수 인자)
        private final int servingSize;
        private final int servings;

        // Optional parameters - initialized to default values(선택적 인자는 기본값으로 초기화)
        private int calories      = 0;
        private int fat           = 0;
        private int carbohydrate  = 0;
        private int sodium        = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings    = servings;
        }

        public Builder calories(int val) {
            calories = val;
            return this;
        }
        public Builder fat(int val) {
            fat = val;
            return this;
        }
        public Builder carbohydrate(int val) {
            carbohydrate = val;
            return this;
        }
        public Builder sodium(int val) {
            sodium = val;
            return this;
        }
        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder) {
        servingSize  = builder.servingSize;
        servings     = builder.servings;
        calories     = builder.calories;
        fat          = builder.fat;
        sodium       = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }
}
```
빌더 패턴으로 다음과 같이 객체 생성이 가능하다.
```java
NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
    .calories(100).sodium(35).carbohydrate(27).build();
```

* 빌더 패턴의 특징
  + 각 인자가 어떤 의미인지 알기 쉽다.
  + `setter` 메소드가 없으므로 immutable 객체를 생성할 수 있다.
  + 한 번에 객체를 생성하므로 객체 일관성이 깨지지 않는다.
  + `build()` 메소드에서 인자 유효성 검증이 가능하다.
  + Lombok의 `@Builder` 어노테이션을 사용하면 위와 비슷한 빌더 패턴 코드가 자동으로 생성된다.

> Reference
> Effective Java 3rd Edition, Joshua Bloch, 2018
> https://johngrib.github.io/wiki/builder-pattern/