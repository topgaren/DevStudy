## 옵저버(Observer) 패턴

### 개요
옵저버 패턴은 한 객체의 상태 변화에 따라 다른 객체의 상태도 연동되도록 일대다 객체 의존 관계를 구성 하는 패턴을 말한다. 데이터의 변경이 발생했을 경우 상대 클래스나 객체에 의존하지 않으면서 데이터 변경을 통보하고자 할 때 유용하다.

### 옵저버 패턴의 클래스 다이어그램
![observer_pattern](https://user-images.githubusercontent.com/45558487/63321110-bbfe7800-c35a-11e9-8428-1842638cf198.png)

* Observer : 데이터의 변경을 통보 받는 인터페이스
* Publisher : ConcreteObserver 객체를 관리한다. 이 때 Observer 인터페이스를 참조하기 때문에 ConcreteObserver 객체 변화에 독립적일 수 있다.
* ConcretePublisher : 변경 관리 대상이 되는 데이터가 있는 클래스(=통보하는 클래스)
* ConcreteObserver : ConcretePublisher의 변경을 통보 받는 클래스. 변경된 데이터에 접근할 수 있다.

### 옵저버 패턴 구현 예시 : 뉴스 구독
뉴스를 구독하는 NewsSubscriber 클래스와 뉴스를 전달하는 NewsPublisher 클래스를 설계한다. 뉴스 데이터는 제목(title)과 내용(content)으로 구성되어 있다. 
```java
public interface Publisher {

    public void add(Observer observer);
    public void delete(Observer observer);
    public void notifyObservers();
}
```
```java
public interface Observer {

    public void update(String title, String content);
}
```
```java
public class NewsPublisher implements Publisher {

    private List<Observer> observerList;
    private String title; 
    private String content;
    
    public NewsPublisher() {
        observerList = new ArrayList<>();
    }
    
    @Override
    public void add(Observer observer) {
        observerList.add(observer);
    }
    
    @Override
    public void delete(Observer observer) {
        int delIndex = observerList.indexOf(observer);
        observerList.remove(delIndex);
    }
    
    @Override
    public void notifyObservers() {
        for(Observer observer : observerList) {
            observer.update(title, content);
        }
    }
    
    public void setNewsInfo(String title, String content) {
        this.title = title;
        this.content = content;
        notifyObservers();
    }
}
```
```java
public class NewsSubscriber implements Observer {

    private String newsString;
    private Publisher publisher; 
    
    public NewsSubscriber(Publisher publisher) {
        this.publisher = publisher;
        publisher.add(this);
    }
    
    @Override
    public void update(String title, String content) {
        newsString = new StringBuilder("").
                append("title : ").append(title).append("\ncontent : ").append(content).toString();
        
        displayNews();
    }
    
    public void displayNews() {
        System.out.println("=== News ===\n" + newsString);
    }
    
    public void withdraw() {
        publisher.delete(this);
    }
}
```
다음은 옵저버 패턴을 적용하여 뉴스를 구독하고 탈퇴하는 것을 코드로 구현한 것이다.
```java
public class NewsSubscribe {

    public static void newsSubscribe() {
        
        NewsPublisher sbc = new NewsPublisher();
        
        NewsSubscriber subscriber1 = new NewsSubscriber(sbc);
        NewsSubscriber subscriber2 = new NewsSubscriber(sbc);
        
        sbc.setNewsInfo("Greeting", "Hello World");
        
        subscriber1.withdraw(); 
        
        // 탈퇴한 subscriber1은 해당 뉴스를 받지 못한다.
        sbc.setNewsInfo("Farewell", "Goodbye");
    }
}
```
```
=== News ===
title : Greeting
content : Hello World
=== News ===
title : Greeting
content : Hello World
=== News ===
title : Farewell
content : Goodbye
```

> Reference 
> https://gmlwjd9405.github.io/2018/07/08/observer-pattern.html
> https://flowarc.tistory.com/entry/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%98%B5%EC%A0%80%EB%B2%84-%ED%8C%A8%ED%84%B4Observer-Pattern