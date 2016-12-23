---
layout: post
title: [React] 구글맵 API Key는 script태그에서 안전할까?
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2015-12-23
tags: [react google map][api key security]
  
---

###[React] 구글맵 API Key는 script태그에서 안전할까?
<br><br>
구글맵을 사용하려면 구글맵에서 제공해주는 API key로 요청해야한다.

공식홈페이지에는 script태그 내부에 요청URL 쿼리스트링에 key를 붙여서 요청하게 되어있다.    

    <script src="https://maps.googleapis.com/maps/api/js?libraries=places&key=키부분"/>


이 방법은 보안상 아주매우무지 별로이므로 API key를 따로 빼서 요청하도록 하기로 했다.   
그래서 찾아보니 GoogleApi+ScriptCache 라이브러리를 사용해 요청하는 방법이 있길래..써봤다.
    
참고 : https://gist.github.com/auser/1d55aa3897f15d17caf21dc39b85b663

그런데 역시나.. 한번에 되는건 없다.   

    Warning: Make sure you've put a <script> tag in your <head> element to load Google Maps JavaScript API v3. If you're looking for built-in support to load it for you, use the "async/ScriptjsLoader" instead. See https://github.com/tomchentw/react-google-maps/pull/168

뭐 이런 에러가 났다. 잘 보면 use the 'async/ScriptjsLoader' instead 라고 나와있다.
뭔지모르겠다. 

script태그에 async속성을 사용하면 maps API가 로드되는 동안 브라우저가 나머지 웹사이트를 렌더링 할 수 있다고 한다. 
API가 준비되면 callback 매개변수를 사용해 지정된 함수를 호출한다.



참고 : script태그의 async와 defer에 관한 글    
https://muckycode.blogspot.kr/2015/01/javascript-html-script-async-defer.html








아..헐... 구글에서 리퍼러를 설정 해줄 수 있었다...이런..ㅎㅎㅎㅎㅎㅎ
참고:
http://stackoverflow.com/questions/1364858/what-steps-should-i-take-to-protect-my-google-maps-api-key


허무하게 끝이 나버렸다..    

결론: 구글 API설정에서 리퍼러 혹은 사용할 IP를 맵핑해주면 script태그에 써도 그다지 문제없다.   
배운점: Key같이 중요한 정보는 반드시 보안을 신경써야 한다는점!!

