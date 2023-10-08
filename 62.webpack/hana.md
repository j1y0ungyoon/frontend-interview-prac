# webpack에 대해서 아는 만큼 설명해주세요

## 웹팩이란?

웹팩은 모듈 번들러 중에 하나이며, 의존성을 가진 여러 개의 파일(모듈)로 구성된 복잡한 어플리케이션의 처리를 돕는 역할을 함.
웹팩은 웹 어플리케이션을 구성하는 여러 파일 및 모듈의 의존성을 관리하고 이를 번들링해 최적화된 자바스크립트 파일을 생성하는 도구임(번들링)
즉, 요약하자면 여러 개의 파일을 하나로 합쳐주는(번들링) 모듈 번들러라고 할 수 있음

**번들링이란?**

사전적인 의미로는 어떠한 것들을 묶는다는 뜻이고 프로그래밍적인 의미로는 모듈화했던 자바스크립트 파일을 묶어준다는 뜻으로 사용됨
참고로 웹팩에서 지칭하는 모듈은 자바스크립트 모듈뿐만 아니라 htm;,css,image파일도 포함함
웹팩은 loader(로더)를 통해 다양한 타입의 파일들도 번들링이 가능함

## 장단점

- 장점

  - 성능 최적화와 자동화
    코드 축소 및 사용하지 않는 코드를 제거하는 tree-shaking과 같은 최적화 수행 -> HTTP 요청 수 감소 -> 웹사이트 성능 향상
  - 편의성
    css가 아닌 Sass나 stylus 등을 사용할 경우나 타입스크립트 사용 시 번들러가 컴파일 과정에서 필요한 플러그인을 추가하고 번들러를 실행해줌
  - 필요할 때만 필요한 스크립트 로드
    code splitting을 통해 원하는 떄에 적절히 모듈을 로딩할 수 있는 Dynamic Loading 과 Lazy Loading이 가능함
  - 종속성 문제(dependency issue) 해결

- 단점
  - 복잡한 구성(configuration)
  - 프로젝트가 크면 클수록 개발 모드 속도가 느림(하지만 웹팩5에 추가된 캐싱옵션으로 해결할 수 있음)
  - 웹팩의 번들 크기
    웹팩을 사용하면 번들 크기가 증가할 수 있으므로 번들 크기 관리와 최적화가 필요함. -> 코드 스플리팅과 트리 쉐이킹을 통해 번들 크키를 최소화할 수 있음

## 웹팩의 대표적인 요소 4가지

### entry

웹팩은 번들링 과정에서 dependency graph를 그림. 특정 지점에서부처 출발해 어플리케이션에 필요한 모든 모듈을 포함하는 그래프를 재귀적으로 완성해감. 한 파일이 다른 파일을 필요로 하면 이를 dependency(의존성)이 있다고 해석하는데, 그래프를 모두 그리고 하념 닝 모든 모듈을 하나의 번들로 묶어서 브라우저에 로드될 준비를 마침. 이때 웹팩이 어디를 출발점으로 잡아 그래프를 그려나가야 하는지 알려주어야함. config 파일에서 entry 속성을 설정해서 웹팩이 어떤 모듈로부터 시작해 디펜던시 그래프를 그려나갈지 명시해줄 수 있음. entry 속성에서 기본값으로 되어있는 값 말고도 원하는 대로 entry point를 지정할 수 도 있음(여러 개도 지정가능함)

```
// webpack.config.js


module.exports = {
  mode: process.env.mode,
  entry : './App.js',

}
```

엔트리 포인트를 분리하는 경우는 SPA 어플리케이션이 아닌 특정 페이지로 진입했을 때 서버에서 해당 정보를 내려주는 형태의 MPA 어플리케이션에 적합함

```
// webpack.config.js


  entry : {
    login: './src/Login.js',
    main : './src/MainView.js'
}
```

### output

웹팩이 번들링한 결과물을 어디에 생성할 건지와 어떤 이름으로 만들건지 정의하는 요소를 말함

```
// webpack.config.js

// root 디렉토리에 미리 dist라는 폴더를 만들어두고 bundle.js라는 이름으로 output 파일이름을 지정함
module.exports = {

  entry : './App.js',
  output : {
    path: './dist',
    filename : 'bundle.js'
  }

}
```

### loader

기볹거으로 웹팩은 자바스크립트 및 JSON 파일만 이해함. loader를 사용하면 웹팩이 다른 유형의 파일을 처리하고 이를 어플리케이션에서 사용하고, depencency graph에 추가할 수 있는 유효한 모듈로 변환할 수 있음

즉, 웹팩이 이해할 수 있도록 해주는 것이 loader의 역할임
또한 ES6의 형식으로 작성된 자바스크립트를 ES5로 바꾸어 컴파일 시켜주는 babel이라는 loader도 있음

```
// webpack.config.js

// module 안에서 loader에 대한 정의를 함
// module의 rules 안의 test는 적용할 파일의 타입을 선언(정규식 사용 가능)
// use는 test에 선언한 파일 타입에 맞는 loader를 정의함
// exclude에는 loader 적용을 하지 않을 파일을 정의

module.exports = {

  entry : './App,js',
  output : {
    path: './dist',
    filename : 'bundle.js'
  },
  module: {
    rules : [
      {
        test: /\.txt$/,
        use: 'raw-loader',
        exclude: /node_modules/
      },
    ]
  }

}
```

### plugins

loader는 파일 단위로 작업이 이루어지는 반면, plugin은 번들링된 결과물에 대해 적용할 수 있는 속성임. 예를 들면 번들링된 자바스크립트 파일에 대해 난독화를 하거나 번들된 css,js 파일들을 html 파일에 주입하는 역할을 함

loader와의 차이점을 요약하자면 아래와 같음

- loader
  파일을 해석하고 변환하는 과정에 관여해 모듈을 처리함

- plugin
  해당 결과물(번들링된 결과물)의 형태를 바꾸는 역할을 함 -> 번들링된 파일을 처리함

=> 번들된 파일을 압축할 수도 있고, 파일 복사, 추출, 병칭 사용 등의 부가 작업, 파일별 커스텀 기능을 위해 사용함

```
// webpack.config.js

const HtmlWebpackPlugin = require('html-webpack-plugins')

module.exports = {

  entry : './App,js',
  output : {
    path: './dist',
    filename : 'bundle.js'
  },
  module: {
    rules : [
      {
        test: /\.txt$/,
        use: 'raw-loader',
        exclude: /node_modules/
      },
    ]
  },
  plugins: [
    new HtmlWebpackPlugin(),
  ]

}
```
