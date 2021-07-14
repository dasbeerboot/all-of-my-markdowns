# 빌드와 번들(feat. Webpack & Babel)

## 번들링은 왜 필요할까? 웹팩은 왜 사용하는걸까?
React에서는 컴포넌트를 이용해 기능 및 UI를 모듈화시키게 되는데, 번들링은 이렇게 모듈화 시킨 자바스크립트 파일들을 묶어준다. 웹팩과 같은 번들러들은 서로 의존성을 가진 여러 JS/TS파일(모듈)들을 하나의 번들 파일로 묶어주는 역할을 한다.

1. ### 네트워크 요청/응답 시간을 줄일 수 있다
    서버에 같은 타입의 파일들을 묶어서 한번에 요청하고 응답을 받게되면, 네트워크 코스트가 줄어들어 속도가 더 빨라진다.
2. ### 웹팩의 development & production mode
    웹팩은 development와 production 두 가지 모드를 지원하는데 dev으로 번들을 하면 개발자들이 알아볼 수 있는, 줄바꿈이 된 파일로 번들이 되고 prod 모드로 번들하게 되면 uglified & minified 된 파일로 번들된다.
    > 웹팩을 이용한 결과물의 예: index.html & index.js  
    > - 번들러는 예를들어 var a = "천"+"주"+"원" 으로 할당된 변수를 var = "천주원" 으로 바꿔주는 것 과 같은 작업을 한다.
    > - 크로스브라우징을 위해 번들 과정에서 바벨을 통해 트랜스파일링을 통해 es버젼을 내릴 수 있다.
3. ### 바벨과 연결해 바벨의 코드 트랜스파일링과 웹팩을 이용한 번들링을 동시에 할 수 있어서 효율적이다.  
<br/>

## 웹팩을 조금 더 알아보자.
웹팩의 중요한 구성 요소는 entry, output, module, plugins 다.  
웹팩 config파일을 열어 웹팩의 동작을 알아보려면 entry에서 output까지의 중간에 어떤 동작을 하도록 설정해뒀는지를 보면 된다.  
아래 codesketch-gui의 webpack.config.js 코드를 예시로 들어 보자.
```javascript   
    module.exports = {
    mode: 'production',
    entry: {
        main: './src/index.tsx',
    },
    resolve: {
        extensions: ['.ts', '.tsx', '.js'],
    },
    devtool: 'eval-cheap-source-map',
    devServer: {
        hot: true,
        overlay: true,
        writeToDisk: true,
    },
    output: {
        filename: 'react-template.min.js',
        path: path.resolve(__dirname, 'dist'),
    },
```
웹팩은 번들을 시작할때 config 파일의 entry에 설정돼 있는 파일, 즉 여기서는 `./src` 하위의 `index.tsx`를 바라보는데, 요즘의 리액트같은 SPA 개발환경의 경우 최종적으로 모든 모듈들이 의존성을 갖고 src 하위의 index 파일에 모이기 때문에 이처럼 entry를 설정한다. **(리액트 cra 보일러플레이트를 사용할 경우에 웹팩은 react-script 모듈 안에 숨겨져있다.)**    
output은 최종적으로 번들링 된 결과물 파일의 이름과 해당 파일이 담길 path를 지정해주는 곳이다.   

<br/>

이제 아래 코드와 함께 module을 살펴보자.
```javascript
module: {
    rules: [
      // all files with a `.ts` or `.tsx` extension will be handled by `ts-loader`
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
      {
        test: /\.s[ac]ss$/i,
        use: [
          // Creates `style` nodes from JS strings
          'style-loader',
          // Translates CSS into CommonJS
          'css-loader',
          // Compiles Sass to CSS
          'sass-loader',
        ],
      },
      {
        test: /\.(png|jpe?g|gif|svg)$/i,
        use: [
          {
            loader: 'file-loader',
          },
        ],
      },
    ],
  },
```
웹팩은 자바스크립트밖에 알아들을 수 없다. 그래서 로더를 이용해야 한다. 로더는 자바스크립트 이외의 다른 리소스들, 예를들어 타입스크립트 파일이나 css나 svg파일 등 과 같은 파일들 또한 웹팩이 이해할 수 있게끔 바꿔주는 역할을 한다.  
간단한 예로 `ts-loader`와 `babel-loader`를 들 수 있다. 앞서 얘기한 것 처럼 타입스크립트를 ES5로 변환할때 `ts-loader`를 이용할 수 있고, ES6를 ES5로 변환할 때는 바벨을 사용할 수 있는데 위의 코드상에는 없지만, 웹팩 설정내에서 바벨을 사용하기 위해서는 `babel-loader`를 이용할 수 있다. 
위 코드블럭에 쓰인 `style-loader`, `css-loader`, `sass-loader` 또한 style sheet의 자바스크립트로의 변환 및 적용을 위해 많이 사용되며, svg-loader 를 이용해 svg파일또한 자바스크립트로 변환할 수 있다.  
~~*(이 역시 리액트애는 숨겨져있어서, 난 웹팩을 공부하기 전엔 svg파일 변환이 타입스크립트 => 자바스크립트 변환될 때 알아서 되는 줄 알았다)*~~  
  
<br/>
마지막으로 plugins를 살펴보자.

```javascript
plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: './public/index.html',
    }),
  ],
```
플러그인은 빌드의 결과물을 처리한다. 웹팩의 번들 마무리를 도와주는 셈이다. 위 코드에는 없지만 이 과정에서 앞서 말한 uglify와 minify도 설정할 수 있다. 위에서 사용된 `CleanWebpackPlugin`은 번들링 이전 결과물을 제거하는 플러그인이다. `HtmlWebpackPlugin`은 index.html에 번들링된 css파일과 js파일을 각각 link, script로 자동으로 추가해주는 플러그인이다.


## 그럼 바벨은 뭔데?
바벨은 공식문서에서 스스로를 자바스크립트 [컴파일러/트랜스파일러](https://developer.mozilla.org/ko/docs/Glossary/Compile)다. 위에서 적은 바와 같이, 크로스브라우징 및 자바스크립트가 동작하는 각기다른 여러 환경에서 각각 다른 자바스크립트 버젼을 필요로 할 경우가 많기때문에 바벨을 이용해 통해 모든 자바스크립트 환경에서 정상동작할 수 있도록 트랜스파일링 할 수 있다. 웹팩과 함께 주로 사용되며, 웹팩 내부에서는 babel-loader를 이용해 사용할 수 있다. 웹팩을 통해서만 사용될 경우 바벨을 설정하는 .babelrc 파일은 필요없다.