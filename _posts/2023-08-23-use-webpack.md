---
title: CRA없이 React 프로젝트 구축하기
author: song
date: 2023-08-23 00:15:00 +0800
categories: [frontend]
tags: [webpack, React, Typescript]
---

<img width="1299" alt="Group 20" src="https://github.com/shyunju7/shyunju7/assets/38373150/0d93c378-4d04-46cc-9eba-5608f32a7bce">

그 동안 React(리액트) 프로젝트를 진행하면서 CRA(Create-React-App)를 이용해서 개발 환경을 구축해왔다. 최근에 개인적으로 공부하면서 CRA없이 프로젝트를 구축해봤는데, 좀 더 정리하면서 공부해야겠다는 생각이 들었다. 오늘은 Typescript기반의 React 프로젝트 환경을 구축해보려고 한다.

## webpack(웹팩)

### webpack의 역할

`webpack`은 정적 모듈 번들러로서 여러 개의 파일을 하나로 묶어주는 도구이다. 대규모 웹 애플리케이션에서는 많은 파일들이 사용되는데, 이를 개별적으로 관리하는 것은 복잡한 작업이 될 수 있다. 또한, 브라우저에서 여러 개의 파일을 로딩하는 것은 성능상 문제가 될 수 있다. 이러한 문제들을 해결하기 위해 `webpack` 애플리케이션을 구성하는 여러 모듈들을 하나의 번들로 묶어줌으로써 파일 관리와 성능을 개선해준다.

---

### webpack(웹팩)의 핵심 요소

webpack의 핵심요소들을 간단하게만 정리했다.

<table>
    <tr>
        <td>Entry(진입점)</td>
        <td>webpack이 애플리케이션을 번들링하기 위한 시작점을 의미한다. Entry에서 Dependency Graph(의존성 그래프)를 생성하며, 필요한 모듈들을 찾아내고 묶어주는 역할을 한다.
    </td>
    </tr>
     <tr>
        <td>Output(출력)</td>
        <td>웹팩이 번들링한 결과물을 지정된 위치(dir)에 출력하는 방법과 파일 이름을 설정하기 위해 사용한다.</td>
    </tr>
     <tr>
        <td>Loader(로더)</td>
        <td>Loader(로더)는 Javascript외에 css, image 파일등을 처리할 수 있게 도와주는 역할을 한다. 각 로더는 특정 파일 유형을 해석하고 변환하여 의존성 그래프에 추가한다.</td>
    </tr>
     <tr>
        <td>Plugins(플러그인)</td>
        <td>Plugins(플러그인)은 번들링 프로세스를 개선하거나 최적화를 위해 사용한다. 번들 파일을 최소화하거나 환경 변수를 주입하는 등 작업을 수행할 수 있다.
    </td>
    </tr>
     <tr>
        <td>Mode(모드)</td>
        <td>webpack은 development, production, none 세가지 빌드 모드를 제공하여 각 모드에 따른 최적화 환경을 수행할 수 있도록 한다.
production은 로드 시간을 줄이기 위해 번들 최소화하거나 애셋 최적화 등을 중심으로 설정한다. development는 주로 개발 단계에서 사용되는 모드로 디버깅을 용이하게 하기 위해 많은 정보를 제공한다.
    </td>
    </tr>
       <tr>
        <td style="width: 140px">Dependency Graph <br/>
        (의존성 그래프)</td>
        <td>webpack은 애플리케이션의 모듈 간 의존성을 그래프로 만들어낸다. 만들어진 그래프를 Dependency Graph (의존성 그래프)라고 하며, 이를 기반으로 필요한 모듈을 로딩하고 번들링하는 작업을 진행한다.
    </td>
    </tr>
</table>

---

## CRA없이 프로젝트 환경을 구축

시작하기 전에 프로젝트 폴더를 먼저 생성한다. 나는 `my-app`이라는 프로젝트 폴더를 만들었다. 폴더 초기 구조는 아래와 같다.

```
.
├── public
│   └── index.html
└── src
    ├── App.tsx
    └── index.tsx

```

### 1. package.json 생성

아래 npm을 통해 package.json을 생성한다.

```zsh
    npm init -y
```

그럼 아래와 같이 package.json이 생성된 걸 볼 수 있다.

<img width="682" alt="package.json화면 이미지" src="https://github.com/shyunju7/shyunju7/assets/38373150/384bcf2a-a217-4391-a993-6d1ecc4361f0">

### 2. 라이브러리 설치

`React`, `Typescript`와 관련된 라이브러리를 설치한다. 설치 후에는 `Typescript`설정 파일을 생성하고 아래와 같이 내용을 수정해준다.

```zsh
    npm i react react-dom
    npm i -D typescript @types/react @types/react-dom
    node_modules/.bin/tsc --init

    npm i -D webpack webpack-cli webpack-dev-server
    npm i -D html-webpack-plugin ts-loader
```

### 3. webpack.config.js 파일 생성

`webpack.config.js`는 webpack의 설정 파일로, 웹팩이 애플리케이션을 모듈을 번들링 할 떄, 어떤 로더, 플러그인, 입출력 경로 등을 사용해야 하는지 정의한다. 나는 총 3가지의 설정 파일을 만들었다. `webpack.config.js`에는 공통 설정을 정의하고 mode에 따라 동작할 `webpack.dev.js`, `webpack.prod.js`을 추가하였다.

```js
// webpack.config.js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.tsx",
  output: {
    filename: "[hash].js",
    path: path.resolve(__dirname, "dist"),
    publicPath: "/",
    clean: true
  },
  resolve: {
    extensions: [".ts", ".tsx", ".js", ".jsx"]
  },
  module: {
    rules: [
      {
        test: /\.(js|ts|tsx)$/i,
        exclude: /node_modules/,
        use: {
          loader: "ts-loader"
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html"
    })
  ]
};
```

```js
// webpack.dev.js
const { merge } = require("webpack-merge");
const config = require("./webpack.config.js");

module.exports = merge(config, {
  mode: "development",
  devtool: "eval", // 개발 중에 발생하는 문제를 빠르게 파악하고 해결할 수 있게 설정한다.
  devServer: {
    host: "localhost",
    historyApiFallback: true, // SPA에서 경로에 따른 라우팅 처리를 위해 사용한다.
    port: 3000,
    hot: true
  }
});
```

```js
// webpack.prod.js
const { merge } = require("webpack-merge");
const config = require("./webpack.config.js");

module.exports = merge(config, {
  mode: "production",
  devtool: "hidden-source-map"
});
```

++ webpack에서 `devtool` 설정은 개발자 도구에서 디버깅을 용이하게 하기 위해 소스 맵(Source Map)을 생성하는 방법을 결정하는 옵션이다.

### 4. tsconfig.json 파일 설정

마지막으로 tsconfig.json 파일을 만들어서 설정을 추가해준다. 그럼 끝났다!

```
{
  "compilerOptions": {
    "target": "es2016",
    "jsx": "react-jsx", // JSX코드의 해석형식 지정,
    "module": "esnext",
    "lib": ["dom", "dom.iterable", "esnext"],
    "strict": true,
    "allowJs": true, // JS파일을 TS파일에 불러올 수 있는지 설정
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,

    "baseUrl": "./", // 상대 경로를 해석할 기준 경로 설정,
    "paths": {
      "@src/*": ["src/*"],
    },
    "outDir": "./dist"
  },
  "include": ["src"], // 프로젝트 내에서 컴파일할 대상 지정,
  "exclude": ["node_modules"] // 컴파일 제외 대상 지정,
}
```

### 5. package.json 설정

마지막으로 package.json에 script를 수정해준다.

```
 "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server webpack.dev.js --open --hot",
    "build": "webpack serve webpack.prod.js",
    "start": "webpack serve webpack.dev.js"
  },
```

이제 터미널에 `npm start` 명렁어를 입력해 실행해보자 그럼 아래와 같이 정상적으로 실행된 결과를 볼 수 있다.

<img width="1166" alt="스크린샷 2023-08-28 오후 3 08 19" src="https://github.com/shyunju7/shyunju7/assets/38373150/93b89fb5-0838-4b99-aba0-95bb49264807">

### 마무리

지금까지 React + Typescript에서 webpack을 설정하는 과정을 정리해보았다.
아직 부족한 부분은 많지만, 꾸준히 개선해봐야겠다!

참고
[webpack-kr](https://webpack.kr/configuration/resolve/) <br/>
[webpack 설정하기](https://yogjin.tistory.com/53)
