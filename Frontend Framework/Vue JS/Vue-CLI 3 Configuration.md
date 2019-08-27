## Vue-CLI Installation
vue-cli version : 3.11.0 (Installation date: 2019.08.26)
```bash
$ npm install -g @vue/cli
  ... (installed) ...
$ vue --version
3.11.0
```

## Vue.js Configuration (with Spring Framework)
vue.js는 `npm run serve` 명령으로 node.js 서버 상에서 실행 시킬 수 있다. 본 문서에서는 vue.js를 스프링 프레임워크와 연동하고 톰캣(Tomcat)을 통해 실행하는 방법을 설명한다.

### Backend Project Structure
STS(Spring Tool Suite)를 통해 Spring MVC 프로젝트를 생성한다. 생성된 프로젝트의 폴더 구조는 다음과 같다.
```
[Spring Project Root]
├── pom.xml
├── src
│   ├── main
│   │   ├── resources
│   │   └── webapp
│   │       ├── WEB-INF
│   │       │   ├── classes
│   │       │   ├── spring
│   │       │   │   ├── appServlet
│   │       │   │   │   └── servlet-context.xml
│   │       │   │   └── root-context.xml
│   │       │   ├── views
│   │       │   │   └── home.jsp
│   │       │   └── web.xml
│   │       └── resources
│   └── test
└── target
```

### Frontend Project Structure
`vue create [project-name]` 명령을 통해 vue 프로젝트를 생성할 수 있다. 본 문서에서는 `[SpringProjectRoot]/src/` 경로에서 `frontend`라는 이름의 vue 프로젝트를 생성한다.
```bash
$ ls
main    test
$ vue create frontend
```

`vue create [project-name]` 명령을 입력하면 아래와 같이 preset을 선택할 수 있다. `default(babel, eslint)`를 선택할 경우 디폴트 설정으로 프로젝트가 생성된다. `Manually select features`를 선택할 경우 사용자가 직접 프로젝트 설정 과정을 진행할 수 있다.
```bash
Vue CLI v3.11.0
? Please pick a preset: (Use arrow keys)
❯ default (babel, eslint) 
  Manually select features 
```

vue 프로젝트 생성이 완료되면 아래와 같은 프로젝트 폴더 구조를 갖는다.
```
[Spring Project Root]/src/frontend
├── README.md
├── babel.config.js
├── node_modules
├── package-lock.json
├── package.json
├── postcss.config.js
├── public
│   ├── favicon.ico
│   └── index.html
└── src
    ├── App.vue
    ├── assets
    │   └── logo.png
    ├── components
    │   └── HelloWorld.vue
    ├── main.js
    ├── router.js
    ├── store.js
    └── views
        ├── About.vue
        └── Home.vue
```

### webpack 설정
webpack 설정을 위해 vue 프로젝트 루트 경로(`[Spring Project Root]/src/frontend`)에 `vue.config.js` 파일을 생성하고, 다음의 내용을 추가한다.
```javascript
/* 상대 주소는 Frontend 루트 경로를 기준으로 한다. */
module.exports = {
  outputDir: '../main/webapp/resources',
  assetsDir: '../resources/static'
  /* 주의! : assetsDir을 'static'으로 설정하면 index.html에서 정적 파일들에 접근을 못한다. */
}
```
* `outputDir` : 빌드 결과로 생성되는 파일들의 기본 경로
* `assetsDir` : `outputDir` 기준의 상대 경로. 정적 파일(`js`, `css`, `fonts`, `img`) 생성 경로
* `indexPath` : `outputDir` 기준의 상대 경로. 지정하지 않으면 디폴트 값인 `index.html`이 설정된다.

### 빌드(Build)
빌드는 vue 프로젝트 루트 경로 상에서 `npm run build` 명령으로 수행한다. 빌드 결과로 생성된 파일들은 `vue.config.js` 에서 설정한 경로에 생성된다. 위의 설정에 따른 빌드 결과로 생성된 파일들은 다음과 같다.
```
[Spring Project Root]/src/main/webapp/resources
├── favicon.ico
├── index.html
└── static
    ├── css
    │   └── app.3c0b035c.css
    ├── img
    │   └── logo.82b9c7a5.png
    └── js
        ├── about.df00c587.js
        ├── about.df00c587.js.map
        ├── app.0fa21d74.js
        ├── app.0fa21d74.js.map
        ├── chunk-vendors.099df166.js
        └── chunk-vendors.099df166.js.map
```

### Spring Framework 설정
Spring이 `vue.config.js`에서 설정한 경로에서 `index.html` 파일을 찾을 수 있도록 `servlet-context.xml` 파일을 수정한다.
```xml
...
<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <beans:property name="prefix" value="/resources/" />
    <beans:property name="suffix" value=".html" />
</beans:bean>
...
```
Request URL `/` 경로를 `index.html`에 매핑 시키기 위해 `HomeController.java`를 수정한다.
```java
@Controller
public class HomeController {
    ....
    @RequestMapping(value = "/", method = RequestMethod.GET)
    public String home(Locale locale, Model model) {
        ....
        return "index";
    }
}
```

### Frontend & Backend 설정 후 최종 프로젝트 구조
```
[Spring Project Root]
├── pom.xml
├── src
│   ├── frontend
│   │   ├── README.md
│   │   ├── babel.config.js
│   │   ├── node_modules
│   │   ├── package-lock.json
│   │   ├── package.json
│   │   ├── postcss.config.js
│   │   ├── public
│   │   │   ├── favicon.ico
│   │   │   └── index.html
│   │   ├── src
│   │   │   ├── App.vue
│   │   │   ├── assets
│   │   │   │   └── logo.png
│   │   │   ├── components
│   │   │   │   └── HelloWorld.vue
│   │   │   ├── main.js
│   │   │   ├── router.js
│   │   │   ├── store.js
│   │   │   └── views
│   │   │       ├── About.vue
│   │   │       └── Home.vue
│   │   └── vue.config.js
│   ├── main
│   │   ├── resources
│   │   └── webapp
│   │       ├── WEB-INF
│   │       │   ├── classes
│   │       │   ├── spring
│   │       │   │   ├── appServlet
│   │       │   │   │   └── servlet-context.xml
│   │       │   │   └── root-context.xml
│   │       │   ├── views
│   │       │   │   └── home.jsp
│   │       │   └── web.xml
│   │       └── resources
│   │           ├── favicon.ico
│   │           ├── index.html
│   │           └── static
│   │               ├── css
│   │               │   └── app.3c0b035c.css
│   │               ├── img
│   │               │   └── logo.82b9c7a5.png
│   │               └── js
│   │                   ├── about.df00c587.js
│   │                   ├── about.df00c587.js.map
│   │                   ├── app.0fa21d74.js
│   │                   ├── app.0fa21d74.js.map
│   │                   ├── chunk-vendors.099df166.js
│   │                   └── chunk-vendors.099df166.js.map
│   └── test
└── target
```

### 실행
톰캣을 실행하고 `http://localhost:8080`으로 접속하면 다음과 같은 화면을 볼 수 있다.
![image](https://user-images.githubusercontent.com/45558487/63674070-aee0fd80-c820-11e9-9ecd-4f99229a0f20.png)