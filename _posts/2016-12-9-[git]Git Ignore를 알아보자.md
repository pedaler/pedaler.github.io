 ---
layout: post
title: [git]Git Ignore를 알아보자
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2016-12-12
tags: [sample post]
  
---


##  [git]gitignore를 알아보자

<br>
git 을 쓰다보면 버전관리가 필요없는 파일들을 빼내고 싶을 떄가 있다.

예를 들어 빌드될때 잠깐 만들어지는 react의 bundle.js 라든가. 자바에선 컴파일될때 생기는 .class파일들.. 같은 것들, npm에서 package.json으로 관리되어지는 내부 node-modules 등은 버전관리에 등록할 필요가 없을 것이다.

이런 불필요한것들 트랙킹되지않도록 막는것을 바로 <code>.ignore</code>에서 하게 된다.
   
      
###1. .gitignore 파일 생성

첫째로 git이 관리되는 root폴더에서 <code>.gitignore</code>파일을 생성한다.

이는  숨김파일로관리됨으로 숨김파일 해제를 하고 사용하길 바란다.

###2. 파일 작성

<code>.gitignore</code> 파일에 제외될 설정을 적어준다.

아래를 설명을 참고하자.

~~~

# => 주석을 의미한다. 
test.txt => 모든 test.txt파일을 제외한다.
*.txt => 모든 txt확장자를 가지는 파일을 제외한다.
/*.txt => root에서의 *.txt파일만을 제외한다.(하위는 유지)
/test => test폴더만 제외한다.
test/ => test하위의 모든 파일을 제외한다.
*/test => test라는 이름을 가진 모든 폴더를 제외한다.

!test.txt => test.txt의 이름을 가지는 파일을 포함한다. !!! 미명령어는 다른 명령어보다 우선적이다.

~~~

위와같은 설정을 ignore에서 해주면 그의 맞게 파일들이 제외된다.



##3.적용

실제 프로젝트에서 해당 ignore를 적용하기위해선 꼭 수정된사항을 commit해주어야 그 설정이 먹게된다.

~~~
git add 
git commit -m ' add ignore'
~~~

그런데

이렇게해도 먹지 않는 경우가 있다.  기존에 이미 tracking된 파일은 ignore가 바로 먹지 않는다. 

그럴경우엔 깃에서 해당 파일을 제거해줘야한다. 

이말은 기존 tracking된 파일을 untracking시킨다는 말이되고, 최초 버전관리에서 제거 한다는 의미이다

깃에서 제거하기위해 rm 명령어를 사용한다. 
예를들어 test폴더(하위포함)를 ignore하고싶다면

~~~
git rm test/
~~~

와 같이 제거해준다.

이후 git add 등을 통해 test폴더를 확인해보면 ignore된것을 확인할 수 있다.
