## 20. what is webpack Plugin

- 플러그인은 로더가 컨트롤하지 못하는 라이브러리 같은 것을 사용하게 하고
- 그밖에 최적화를 가능하게 한다. (어글리파이, 용량 최적화 등)

## 21. Minification of the Resulting webpack bundle

- terserplugin의 경우 파일 용량을 최적화해주는 plugin으로 미리 웹팩에 다운 되어 있다.

```javascript
// webpack.config.js
const TerserPlugin = require("terser-webpack-plugin");
  plugins: [new TerserPlugin()],
```

## 22. Extracting Css into a Separate Bundle With mini-css-extract-plugin - 1

- css를 분할해서 번들에 집어 넣는 이유는

1. 그렇게 했을 때 사이즈가 가볍고 다운로드가 빨라진다.
2. 나눴을 때 개발 경험이 더 좋다.

- css-extract-plugin은 npm으로 다운 받아야 된다.(npm i -D mini-css-extract-plugin)

```javascript
//webpack


  // npm에서 설치 후 변수로 불러왔다.
  const MiniCssExtractPlugin = require("mini-css-extract-plugin");

  // use에 0번째 배열에 있던 style-loader를 MiniCssExtractPlugin.loader로 바꿨다
  {
    test: /\.css$/,
    use: [MiniCssExtractPlugin.loader, "css-loader"],
  },
  {
    test: /\.scss$/,
    use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
  },

  plugins: [
    new TerserPlugin(),
    new MiniCssExtractPlugin({
      filename: "styles.css",
    }),
  ],
```

## 23. Extracting Css into a Separate Bundle With mini-css-extract-plugin - 2

- 결국 아무리 css 파일을 놔눠도 css 파일은 하나의 파일로 묶여진다.
- 시멘틱이 중요한 이유 중 하나!

## 24. Browser Caching

- 캐시는 기본적으로 네트워크를 효율적이게 가동하는데 대단히 중요한 기능이다.
- 킹치만 생각해보자. 캐시가 다운됐는데 그 캐시가 잘못된 js파일이었다면? 사용자는 새로운 파일을 다운 받고 싶어도 못 받을 것이다.
- 그렇기에 요즘은 새로운 파일을, 새로운 이름으로 만든 후 다시 보낸다. (그럼 다운 안 받고는 못 베길테니까)

```javascript
//webpack

  output: {
    filename: "bundle.[contenthash].js",
    path: path.resolve(__dirname, "dist"),
    publicPath: "dist/",
  },

  plugins: [
    new TerserPlugin(),
    new MiniCssExtractPlugin({
      filename: "styles.[contenthash].css",
    }),
  ],

// 저렇게 [contenthash]를 붙여서 md5 해시를 랜덤하게 붙여서 캐시 문제를 해결한다.
```

<br>

## 25. How To clean Dist Folder Before Generating New Bundle

- 24에서 조금만 변경이 있어도 랜덤한 해시를 붙이는 파일을 생성했따.
- 근데 넘 지저분하다
- 이 부분을 해결하기 위한 플러그인이 필요하다.
- (npm i -D clean-webpack-plugin)

```javascript
//webpack

const { CleanWebpackPlugin } = require("clean-webpack-plugin");

  plugins: [
    new CleanWebpackPlugin(),
  ],

  ---

  // 아래와 같이 설정할 경우 다른 폴더에 있는 모든 파일을 output에 설정되어 있는 (지금은 dist) path로 다 집어 넣고 하나로 묶어 버린다.

  new CleanWebpackPlugin({
  cleanOnceBeforeBuildPatterns: [
    "**/*",
    path.join(process.cwd(), "build/**/*"),
  ],
}),
```

- npm run build 후, dist에 중복된 js, css 파일이 다 삭제되었다.

<hr>

- 하지만 웹팩 5.20 버전 부터는 위의 플러그인이 기존 버전 문법에 추가되었다. (output.clean)

```javascript
  output: {
    filename: "bundle.[contenthash].js",
    path: path.resolve(__dirname, "dist"),
    publicPath: "dist/",
    clean: {
      dry: true,
      keep: /\.css/
    }
  },
```

용례는 이곳을 참고하자

> https://jeonghwan-kim.github.io/2022/08/21/webpack-output-clean
