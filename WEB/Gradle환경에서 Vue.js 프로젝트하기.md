# Gradle 환경에서 Vue.js 프로젝트 연동하기
IntelliJ에서 웹 프로젝트에서 프론트 부분은 **Vue.js** 를 사용하기로 했다. 백앤드 서버 코드는 Java Spring Boot 프레임워크로 이미 구현중이었고, 여기에 프론트를 붙이는 작업을 수행했다. 서버를 실행했을 때 프론트 코드가 같이 빌드되면서 html 파일이 서버 부분에 저장되도록 했다.

## 들어가기 전...
기본적으로 NPM과 Node.js는 설치되어 있다고 가정한다.

- npm 버전 확인

```
$ npm -v
```

```
6.12.1
```

- Node.js 버전 확인

```
$ node -v
```

```
v12.13.1
```

## Vue 프로젝트 만들기

### 1. Vue CLI 3 설치

```
$ npm i -g @vue/cli
```

만약 권한 오류가 뜬다면 다음과 같이 명령어를 실행한다.

```
$ sudo npm i -g @vue/cli
```

### 2. Vue 프로젝트 만들기
인텔리제이 프로젝트에서 `./[프로젝트]/src` 디렉토리안에서 다음의 명령어를 실행해야 한다.

```
$ vue create frontend
```

### 3. Vue 플러그인 설치
플러그인 설치는 Vue 프로젝트 디렉토리 내부에서 실행한다.

```
$ cd frontend
```

- Vue Router 설치

```
$ vue add router
```

- Vuetify 설치

```
$ vue add vuetify
```

### 4. Vue 설정
Vue 설정은 대표적으로 해당 Vue 코드의 결과(`index.html`)을 서버 자바 코드의 resources 디렉토리 내부로 결과 파일을 만들어주기 위함이다. 그 외에도 Vue에서 사용하는 개발용 서버의 프록시를 설정해줄 수 있다.

- `vue.config.js`

```js
const path = require("path");

module.exports = {
  "transpileDependencies": [
    "vuetify"
  ],
  outputDir: path.resolve(__dirname, "../main/resources/static"),
  devServer: {
    proxy: {
      '/' : {
        target: 'http://localhost:9000',
        ws: true,
        changeOrigin: true
      },
    },
  },
};
```

fronted 디렉토리 내부에 `vue.config.js`가 있으며, 만약 없다면 만들어주면 된다.

### 5. Gradle 설정
서버 코드가 빌드될 때 Vue 프로젝트를 같이 실행하기 위해 Gradle을 설정한다.

- `build.gradle`

```js
plugins {
  // ...

  id 'com.moowork.node' version '1.3.1'
}

apply plugin: 'com.moowork.node'

node {
    version = '12.13.1'
    npmVersion = '6.12.1'
    workDir = file("./src/frontend")
    npmWorkDir = file("./src/frontend")
    nodeModulesDir = file("./src/frontend")
}

task setUp(type: NpmTask) {
    description = "Install Node.js packages"
    args = ['install']
    inputs.files file('package.json')
    outputs.files file('node_modules')
}

task buildFrontEnd(type: NpmTask, dependsOn: setUp) {
    description = "Build vue.js"
    args = ['run', 'build']
}

processResources.dependsOn 'buildFrontEnd'
```

### 6. 컨트롤러 코드 작성
서버 코드에서 URL이 루트(/)로 접속했을 때 해당 `index.html`을 보여주는 코드를 작성한다.

```java
@Controller
public class HomeController {

    @GetMapping("/")
    public String home() {
        return "index.html";
    }
}
```

### 7. 서버 실행하기
모든 세팅이 끝난 후 서버를 실행하고 `http://localhost:8080/`로 접속하여 Vue 프로젝트가 정상적으로 동작하는지 확인한다.
