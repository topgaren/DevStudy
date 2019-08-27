## 싱글톤(Singleton) 패턴

### 개요
싱글톤 패턴은 딱 한 개의 인스턴스만 만들어 사용하기 위한 패턴이다. DBCP(Database Connection Pool), Thread Pool, 디바이스 설정 객체 등의 경우 여러 개의 인스턴스를 생성하여 사용하면 자원을 낭비하게 되거나 버그를 발생시킬 수 있다. 따라서 오직 하나만 생성하고 해당 인스턴스를 사용하도록 하는 것이 이 패턴의 목적이다.

### 싱글톤 패턴 사용 시 주의할 점
싱글톤 인스턴스가 너무 많은 일을 하거나, 많은 양의 데이터를 공유할 경우 다른 인스턴스들과의 결합도가 높아져 "개방-폐쇄 원칙"을 위배하게 된다. 즉 객체 지향 설계 원칙에 어긋나게 되고, 수정 및 테스트가 어려워진다.

### 싱글톤 패턴의 구현
1. 가장 간단한 형태
```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if(instance == null) {  // (1) Null Check Point
            instance = new Singleton(); // (2) Create Instance Point
        }
        return instance;
    }
}
```

2. 멀티 쓰레드 환경에서 안전하도록 변경한 형태
```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if(instance == null) {  // (1) Null Check Point
            instance = new Singleton(); // (2) Create Instance Point
        }
        return instance;
    }
}
```

3. 중첩 클래스를 사용한 형태
```java
public class Singleton {
    
    private Singleton() {}

    public static class SingletonHolder {
        public static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```