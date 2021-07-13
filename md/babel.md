# Babel?

### uglify & minify를 해준다

- minify: 최적화!
- uglify: 소스를 사람들이 못알아보게하깅

### bundling = uglify + minify

- webpack: 번들러 / 결과물: ex)index.html & index.js파일을
  > 번들과정에서 바벨을 통해서 트랜스파일링을 해서 버전을 내릴 수 있다.
- 번들러: var a = "천"+"주"+ 원 => var a = "천주원" 처럼 최적화되게 한 번 바꿔줌

### google closer library => 퍼포먼스 최적화의 목적이 있음! 찾아봐야함

### TypeScript 가 생긴 이유?

<details>
컴파일러를 사용하면 무겁고 컴파일러도 따로 만들어조야한다.
(컴파일해서 실행파일 만들고 떨궈주고 하는애들 = c, c++)
개발할 당시는 인터프리터가 빠르지만 다 개발하고 배포할때는 컴파일러 방식으로 된 애가 빠르다. 안정성도 ㅅㅌㅊ <br/>
컴파일을 안하는 자바스크립트는, 런타임시에만 발생하는 에러가 있고 예외가 자꾸 터져서 타입스크립트를 쓰게됨<br />
타입스크립트의 모토? super-set. 타입스크립트는 ? 자바스크립트 + ES6 상위에 타입스크립트가 있다 <br />
브라우저는 ts를 못알아들어서 tsc(typscript compiler)를 써서 es5로 바꾸는데, tsc가 babel을 이용한다. <br />
리액트에서 쓰는 jsx => es5 해주는것도 바벨이다. <br />
바벨이 이 모든 기능을 갖고있지 않아서 preset 어쩌고 하는 plugin을 설치한다.
ts로 개발하고 => js로 바꿈 (via tsc) => 이렇게 바꿔서 나온 js를 번들(웹팩만 쓰는거 아니고 걸프 써도되고) <br />
... 위 방법대로 하면 귀찮으니까 웹팩으로 한번에 묶어서 할 수 잇다.<br />
웹팩은 그니깐 개발하고 배포할때만 필요한거야 <br />
웹팩 has plugins, and loader <br/>
plugin: add-ons 같은 기능. 웹팩에서 번들 하기를 도와줌. html-webpack-plugin => index.html에 js결과물을 묶어줌 <br/>
loader: 바벨 같은 애들. 번들하는 과정 자체에 필요한애들. babel-loader(transplile, 바벨의 역할을 해주는 애), scss-loader(scss to css), svg-loader etc... <br/>
+++ts config에서 target 및 preset 설정 가능~!~! <br/>
</details>

### 웹팩과 바벨은 배포된상태에서는 쓸모업다.

빌드 !== 번들
번들: 여러파일을 하나로 묶어주는거 <br />
빌드: 몰라 <br/>

### 웹팩은 개발이 아니다. 웹팩은 설정이다.

ex: webpack.config.js => mode: 'production' || 'development' <br/>
development 로 번들하면 우리가 알아볼 수 있는 코드로 나오고, production 으로 번들하면 uglify 랑 minify를 해줌 <br />
webpack은 entry 와 output 사이에 어떤걸 실행시키느냐가 중요함. <br/>
webpack은 번들을 시작할때 엔트리에 설정돼있는 파일을 보는데, 그 파일은 대부분 src루트에 index.js 보게 해둔다. <br/>
이렇게 할 수 있는 이유? 리액트건 뷰건 앵귤러건 현대 클라이언트들이 SPA를 쓰니까, index.html 하나만 뽑으면 되눈고양 <br/>
bundler 는 임포트 안되고 안쓰이는애들 트랜스파일링 안해줌 왜냐면 쓸모없는새끼니까. <br/>
entry를 보고 시작해서, output에 명시돼있는 파일로 떨궈준다! <br />
modules: loaders <br/>
plugins: plugins <br/>
웹팩내부에서 바벨을 쓰는 방법? : 바벨 loader. 그래서 웹팩을 통해서만 쓸라면 .babelrc 없어두댕

dev 와 prod를 구분할 줄 알아야해!
dev 디펜던시는 개발할때 만 쓰이고, 배포할때는 안들어감!
그냥 디펜던시는 배포할때도 들어가야함 <br/>
바벨, 웹팩은 dev dependencies <br />
웹팩과 바벨이 하는 역할은? 결과물만 만드는 것! 이과정이 CI <br />

### webpack 과 babel의 상관관계

### boilerplate === cra

boilerplate => 내가만든 프로젝트 템플릿~~!!
cra =>
