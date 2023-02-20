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
