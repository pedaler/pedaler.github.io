---
layout: post
title: [RECT]react-boostrap을 사용해보자.
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2016-12-9
tags: [sample post]

---


##  [RECT]react-boostrap을 사용해보자.

<br>
리액트는 앵귤러.js 2와 함께 매우 빠르게 퍼져나가고 이에 발 맞추어 프론트앤드의 다양한 라이브러리도 리액트를 보다 쉽게 이용할 수 있도록 포팅되어가고 있다.

이번엔 react-boostrap을 사용해볼 것이다.







1.react-Boostrap 설치.
----------


react-boostrap의 사용은 무지막지하게 쉽다.

~~~
npm install react-bootstrap --save
~~~

프로젝트에 위와같이 react-boostrap을 설치하고.

~~~
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/latest/css/bootstrap.min.css">

<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/latest/css/bootstrap-theme.min.css">
~~~

메인 html에 스타일시트를 추가해준다.

2.Component 사용
----------


리액트는 참으로 많은 컨포넌트들을 제공해주고 쉽게 가져다 쓸 수 있다.
필자는 react Modal 을 사용하려 했고 이를 쓰기위해선.
Popover,Button,Modal등의 클래스가 필요하였다.

그리하여

~~~
import { Popover,Button,Modal } from 'react-bootstrap';

~~~

이렇게 최초 사용할 컨포넌트의 클래스들을 추가해주고.

boostrap에서 제공해주는 컨포넌트 예제들을 그대로 가져다 추가해주면된다.


3.주의
----------

* 리액트를 어느정도 다루어본 사람이라면 크게 문제가 되진 않겠지만.. 필자와같이 초보자에겐 es5,es6 표기법이 조금 달라 혼동이 있을 수있다.

* boostrap에서 제공해주는 컨포넌트 예제들은 모두 es5기준으로 제공하고 있으니 해당 es6로는 어떻게 표기되고 작동되는지를 명확히 알아야할것이다.

예를 들면 아래와같은 것들이 있다.

~~~
es5
//getInitialStat(){~~}

es6
//constructor(){~~}

//es5
open(){
	this.setState(~~);
}
<Button onClick={this.open}>

es/6
open(){
	this.setState(~~);
}
<Button onClick={this.open.bind(this)}>
~~~





참고  
<https://react-bootstrap.github.io/getting-started.html>
