## 12. Asset/inline Module

- resource 모듈은 jpg나 png같은 용량이 큰 파일에 적합한 반면,
- inline 모듈은 svg와 같은 용량이 작은 파일에 적합하다.
- 나아가 resource 모듈은 파일을 불러올 때 이미지당 한 번의 http 호출을 하지만, inline은 한 번에 20개를 호출한다.

## 13. general asset module

- 기본값이며 8킬로바이트 보다 적으면 inline을 적용하고, 그보다 많으면 resource를 적용한다.
- 물론 필요하다면 기준이 되는 숫자를 변경할 수 있다.

```javascript
      {
        test: /\.(png|jpg)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 3 * 1024, // 3 kilobytes
          },
        },
      },
```

## 14. Asset/source module type

- inline과 비슷하지만 txt와 같은 파일을 번들링 한다.

```javascript
      {
        test: /\.(png|jpg)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 3 * 1024, // 3 kilobytes
          },
        },
      },
      {
        test: /\.txt/,
        type: 'asset/source',
      }
```
