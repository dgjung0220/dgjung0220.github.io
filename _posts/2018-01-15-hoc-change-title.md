---
tags: ["react","web","javascript","HOC"]
title: 'React HOC(High Order Component)'
date: 2018.01.15
category: 'react'
---

# HOC(High Order Component)

## 개요

High Order Component의 약자로서 React가 제공해주는 api가 아닌 React를 이용한 응용 기술, 디자인패턴 입니다.  
HOC는 React에서 UI Component로서가 아닌 컴포넌트 로직을 재사용하기 위한 컴포넌트 입니다.  
즉, 다른 Component취해 새로운 Component를 반환하여 사용하게끔 해줍니다.  
React 공식홈페이지에서 가이드를 해줍니다.  
이미 react-redux나 react-router를 통하여 알게 모르게 사용되는 기술입니다.  

react-redux를 예로 들자면  

```javascript
import react from 'react'
import { connect } from 'react-redux'
// Class Comp extends react.Component {.....

Comp = connect(mapStateToProps, mapDispatchToProps)(Comp)

```

에서와 같이 Comp에서 react-redux의 cennect란 함수를 이용하여 redux로부터 state를 연결하여 값들을 사용하고 있습니다.  

~~심지어 내부에서  connectHOC란 함수명으로 사용합니다.~~  
~~return connectHOC(selectorFactory, _extends{methodName: 'connect'...~~  

이를 응용하면 아래 데이터호출, 인증처리와 같은 비지니스 로직을 분리한다건가,
react-redux, react-router와 같이 필요한 값들을 부여하거나 반대로 제한하는 등의 여러가지 일들을 할 수 있습니다.


## 사용해보기  
SPA(Single Page Application)에서는 각각의 페이지를 동적으로 받아 페이지 전환을 하게 됩니다.  
페이지마다 title를 바꿔주기위해서

```javascript
import React, { Component } from 'react'

class Page extends Component {
  componentDidMount(){
      document.title = "PageTitle"
  }

  componentWillDidMount(){
      document.title = "default title"
  }

  render() {
    return <div>페이지 컴포넌트</div>
  }
}
```

위와 같은 코드들을 페이지로서 사용하는 Component에 작성을 하게 됩니다.
바로 HOC를 만들어 적용하게 되면, title를 작성하기 사용되는 비지니스로직을 분리함과 동시에 중복되는 코드를 줄일 수 있게 됩니다.

```javascript
import React, { Component } from 'react'

class Page extends Component {
  render() {
    return <div>페이지 컴포넌트</div>
  }
}

function withTitle(title){
  return function(WrappedComponent){
    Class TitleComponent extends Component {
      componentDidMount(){
          document.title = title
      }

      componentWillDidMount(){
          document.title = "default title"
      }

      render(){
        <WrappedComponent {...this.props} />;
      }
    }

    return TitleComponent
  }
}
```

> withTitle('PageTitle')(Page)

위에 작성된 withTitle은 HOC로서 작성되어 받아낸 Component만을 랜더링 함으로서 UI적으로는 영향을 끼치지 않고 title만을 조작하는 로직을 처리합니다.  
Page컴포넌트는 title에 대해서는 신경쓰지 않고 랜더링하는 부분만 남게 되었습니다.
이로서 title을 조작하는 로직이 분리 되었습니다.

이와같이 HOC는 AOP스럽게 사용함으로서 코드 재사용성을 늘리고 분리합니다.
이러한 HOC작성 함로서 인증처리와 같은 문제나 로깅등에 대한 것도 분리할수 있게 됩니다.

## 단점

HOC는 Component를 Component가 감싸는 형식이기 때문에 그에 따른 단점도 생기게 됩니다.  
대표적인 문제로
1. displayName이 덮어씌워지는 문제
2. refs가 전달이 안되는 문제

위와 같은 문제는 HOC 코드 작성하는 부분에서 극복이 가능합니다. ~~번거로움 +1~~

```javascript  
Class TitleComponent extends Component {
  static displayName = `TitleComponent(${getDisplayName(WrappedComponent)})`;
  // ...
```
남발하게 될 경우 디버깅이 힘들어지는 등의 당연한 문제점들이 발생하게 됩니다.


# Etc..

decorator와 같이 사용하게 된다면

```javascript
@withTItle("PageTitle")
class Page extends Component {
  render() {
    return <div>페이지 컴포넌트</div>
  }
}
```

이런식으로도 사용가능합니다.

decorator는 es7문법으로서 현재는 babel-plugin-transform-decorators-legacy 플러그인을 설치해야 사용가능하며  
babel 7버전부터는 포함되게 된다고 합니다.  
사용법은 [여기](https://github.com/loganfsmyth/babel-plugin-transform-decorators-legacy).  



참고페이지 [여기](https://reactjs.org/docs/higher-order-components.html).  
