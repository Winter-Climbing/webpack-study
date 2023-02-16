## 15. what is webpack loader

- Loader는 JS뿐만 아니라 css, sass, xml, handlebar와 같은 다양한 도구들을 빌드할 수 있게 해준다.

## 16. handling CSS With webpack

- asset은 webpack 안에 저장되어 있는 모듈이기에 딱히 설치할 필요가 없지만 Loader는 npm에서 다운 받아야 된다. (npm i -D style-loader css-loader)
- rules에 loader 모듈을 추가했다.

```javascript
  module: {
    rules: [
      {
        test: /\.(png|jpg)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 3 * 1024, // 3 kilobytes
          },
        },
      },
      // loader 모듈
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
    ],
  },
```

## 17. Handling SASS

- loader 모듈은 use안에 배열을 오른쪽부터 왼쪽 방향으로 처리한다.
- 즉 sass-loader가 처리된 후 css-loader로 넘어가고 그다음 style-loader로 주입된다.
- 당연히 sass-loader도 없기에 다운 받아야 된다. (npm i -D sass-loader sass)

## 18. Using latest JS features with BABEL

- 바벨 로더이다 (module안에 css loader와 똑같이 포함)
- npm i -D @babel/core babel-loader @babel/preset-env @babel/plugin-proposal-class-properties
- core와 loader는 핵심기능을 가지고 있고
- preset-env는 문법들을 모아 놓고 있다.
- 최신 문법이 아직 preset-env에 포함되어 있지 않다면 plugin을 통해 설치해야 한다. (밑의 예시는 class 문법을 설치했다. 최근 버전에는 이미 preset-env에 class 문법이 추가되어 사실 설치할 필요가 없다.)
- 즉, preset-env에 포함되어 있지 않은 문법을 사용했다면 공식 문서를 찾아보고 plugin을 통해 삽입해줘야 한다.

```javascript
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/env"],
            plugins: ['@babel/plugin-proposal-class-properties']
          },
        },
      },
```
