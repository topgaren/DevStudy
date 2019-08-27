## 컴포지트(Composite) 패턴

### 개요
컴포지트 패턴이란 여러 개의 객체들로 구성된 복합 객체와 단일 객체를 클라이언트에서 구별 없이 다루게 해주는 방법이다.  **전체-부분** 관계를 갖는 객체를 정의하기에 적합하다. 클라이언트는 전체와 부분을 구분하지 않고 동일한 인터페이스 를 사용할 수 있다는 특징이 있다.

### 컴포지트 패턴의 클래스 다이어그램
![composite_pattern](https://user-images.githubusercontent.com/45558487/63255345-856c2300-c2b0-11e9-8f57-96c6e9c240de.png)

* Component : Leaf와 Composite의 공통 인터페이스를 정의
* Composite : 전체를 나타내는 클래스. 복수 개의 Leaf 또는 복수 개의 Composite를 부분으로 가질 수 있다.
* Leaf : 부분을 나타내는 클래스. Composite의 부품으로 설정

### 컴포지트 패턴 적용 예시 : 파일 시스템 구현
파일 시스템을 구현하기 위해 다음과 같이 파일에 해당하는 클래스와 디렉토리에 해당하는 클래스를 각각 정의하였다.
```java
public class File {

    private String name;
}
```
```java
public class Directory {

    private String name;
    private List<File> fileList;

    public void addFile(File file) {
        //...
    }
}
```
위에서 정의한 디렉토리는 `addFile()` 메소드를 통해 디렉토리 내에 파일을 추가할 수 있다. 그러나 실제로 디렉토리는 파일 뿐만 아니라 다른 디렉토리도 저장할 수 있다. 위의 디렉토리 구조로는 이 기능을 만족시키지 못한다. 물론 멤버 변수로 `Directory` 클래스의 리스트를 선언하고 `addDirectory()` 메소드를 추가하는 방법으로 구현할 수 있다. 그러나 추가/삭제/조회 연산 시 동일한 기능을 파일과 디렉토리에 대해 각각 2번씩 수행해야 하므로 중복되는 코드가 생기고 유지 보수가 어려워지게 된다.

컴포지트 패턴을 적용하기 위해 파일과 디렉토리의 공통 인터페이스를 다음과 같이 정의한다.
```java
public interface Node {

    public String getName();
}
```

그리고 파일과 디렉토리를 `Node`를 구현한 형태로 다음과 같이 정의한다.
```java
publc class File implements Node {

    private String name;

    @Override
    public String getName() { return name; }
}
```
```java
public class Directory implements Node {

    private String name;
    private List<Node> children;

    @Override
    public String getName() { return name; }

    public void addChild(Node node) {
        children.add(node);
    }
}
```
디렉토리와 파일의 관계는 전체-부분 관계로 해석될 수 있으며, 이 때 컴포지트 패턴을 적용할 수 있다. 디렉토리와 파일을 일관성 있게 다루기 위해 추상화 시켰다. 이를 통해 디렉토리는 파일 뿐만 아니라 자기 자신을 자식으로 가질 수 있도록 구현할 수 있다.
```java
public class FileSystem {

    public static void testFileSystem() {

        Directory root = new Directory();
        root.addChild(new File());
        root.addChild(new Directory());

        Directory bin = new Directory();
        root.addChild(bin);
    }
}
```

> Reference
> https://jdm.kr/blog/228
> https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html