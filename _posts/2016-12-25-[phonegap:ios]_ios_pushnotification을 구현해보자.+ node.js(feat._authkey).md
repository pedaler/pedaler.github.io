---
layout: post
title: [phoneGap/ios] pushNotification을 사용해보자.
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2016-12-25
tags: [sample post]

---


##  [phoneGap/ios] ios pushNotification을 구현해보자.+ node.js (feat. AuthKey)


<br>
0. 무엇을 할테냐?
----------

cordova push plugin을 통해 푸시 노티피케이션을 구현해본다.

기본적으로 react src 에서 어떻게 설정해야하는지와.

해당 소스가 어떻게 ios에 설정되는지.

그리고 순수하게 ios push Notification 설정법 에대해 알아보고자 한다.

참고로 기존 방법이아닌. authKey를 이용한 push 설정방법을 이야기하고자 한다.





<br>  


1. react 에서 pushNotification이용하기
----------


react 에서 push Notification을 사용하기위해 기본적인 설정들이 필요하다.

이는 당연히 `ios native`에서 하는 과정들을 단지 javascript로 표현된것이다.

ios native를 다뤄본 개발자라면 쉽게 이해할 수 있다.


페달러는 처음 실행히 window load => deviceReady check => react render의 과정을 거친다.(기본 보일러프로젝트 예제)
~~~
window.onload = function(){
	document.addEventListener('deviceready', startApp, false);
~~~

이제 startApp에서 화면 랜더전에 pushNotification 객체를 만들어주고 등록하는 과정을 진행한다.

윈도우객체에는 기존 코르도바 플러그인의 설치 객체들이 존재하고 이객체중 pushNotification 을 쓰기로한다.(코르도바 푸시플러그인은 설치되어있다고 가정하고 진행한다.)

필자는 startApp내부에 initPlugin이라는 함수를 두어 시작시 설정해줘야할 플러그인의 정보를 모아두었다.

~~~
function initPlugin(){
	console.log('initPlugin');
    pushNotification = 	window.plugins.pushNotification;

}
~~~

이제 설정된 pushNotification으로 몇가지 등록을 해주어야한다.

먼저 플랫폼을 설정해줘야한다.

push notification은 native 파라미터에 종속될 수 밖에 없다. 즉 native에서 해주는 설정을 그대로 해줘야할 것이다.

아래코드는 플랫폼이 ios일 경우 푸시를 등록하고, 디바이스토큰 및 에러 핸들러 그리고 뱃지,사운드,알람을 사용할지 를 설정하는 코드이다. ecb는 푸시가 디바이스에 왔을때 받는 함수를 말한다.

* 디바이스토큰은 후에 서버에서 푸시알림을 주게될 고유 기계 id값이된다.

~~~
if(device.platform == 'iOS'){
	    pushNotification.register(
	    tokenHandler,
	    errorHandler,
	    {
	        "badge":"true",
	        "sound":"true",
	        "alert":"true",
	        "ecb":"onNotificationAPN"
	    });

	}
}
~~~

아래와같이 관련 당연히 모두 존재하여야한다.


~~~
function errorHandler (error) {
    alert('error = ' + error);
}

function tokenHandler (result) {
	console.log('device tokec = >',result);
}


// iOS
function onNotificationAPN (event) {
    if ( event.alert )
    {
    	//알랏이 있다면.
        navigator.notification.alert(event.alert);
    }

    if ( event.sound )
    {
    	//사운드가 있다면.    
        var snd = new Media(event.sound);
        snd.play();
    }

    if ( event.badge )
    {
       	//뱃지가 있다면.    
        pushNotification.setApplicationIconBadgeNumber(successHandler, errorHandler, event.badge);
    }
}


~~~


간단히 정리하면

1. 앱 실행 & deviceReady 된 후,

2. initPlugin을 통해 notification객체를 바인드하고,

3. 푸시에 필요한 설정을 세팅해준 것이다.


이와같이 설정해두면 react에서의 setting은 마무리 된것이다.




<br>
2. ios Push 인증서 설정하기.
----------

authKey를 기반으로한 푸시 인증서를 생성해보자.

10.27 업데이트된 apple 개발문서에 authkey관련 push Noti를 하는 방법에대해 디테일한 설명이 되어있다.

기존은 개인키를 만들고 푸시인증서를 만들어 해당 인증서를 다시 PKCS#12(.p12) 파일로 변환해주고  해당 키를 서버로 전달해서 사용하는 다소 번거로운 방법이었다.

authKey기반의 인증서 등록방법은 굉장히 간단하다. 심지어 푸시 expired타임도 없이 무기한으로 사용할 수 있다.

<br>

~~~
1. apple devCenter APNs Auth Key를 하나 발급받는다.(이는 서버로 보낼 키가 된다)


2. develop이나 production 용도에 맞게 push Ceritifacte를 발급 받는다.



3. appIDs 에서 앱을 만들고 option 중 pushNotification을 on하면 2에서 발급된 certificatie가 자동으로 붙을것이다.

~~~


이제 개발센터에서의 설정은 끝이다.

디테일한 프로비전 설정 및 release/debug 모드 설정등은 생략한다.

이후 ios build하고 폰에서 런을 하여 token 값이 찍히는 것을 올바르게 확인해본다.




<br>
3. Node.js에서의 세팅
----------

서버에서의 세팅은 더욱 간단하다.

~~~
 * p8파일을 서버로 옮긴다.
~~~


다음 apns를 사용하기 위해 npm install을 한다.
~~~
 npm install apn --save
~~~

그리고 npm apn문서에 나와있는 그대로 설정을 하면되는데..


~~~
var apn = require('apn');


var apnProvider = new apn.Provider({  

     token: {

        key: './keys/APNsAuthKey.p8',
        // authKey를 확인해보면 keyId가 존재한다.
        keyId: 'KKKABCDE',        
        // https://developer.apple.com/account/#/membership/ 에서 팀아이디를 확인할 수 있다.
        teamId: 'D4H8VWYX2L',
    },
    //production 인지  develop인지 판단하는 부분이다.
    production: true // Set to true if sending a notification to a production iOS app
});

//위에서 발급받은 디바이스토큰을 입력한다.
var deviceToken = 'asdfasdf83fj0fj3jf0ajf039j00000';


var notification = new apn.Notification();

//내앱의 번들 id를 입력하면된다.
notification.topic = 'kr.co.helloword';

// 푸시를 만기시간이다 얼마만큼의 시간동안 보유할지에 대한이야기다.
// apns는 storeForward 방식을 사용하고 만약 단말기가 꺼져있다면 단말기가 온라인 될때 알림을 전달한다.
// expirytime 이 지나게되면 해당  알림을 지운다.
notification.expiry = Math.floor(Date.now() / 1000) + 3600;

//뱃지 수를 의미한다
notification.badge = 3;

//플레이 사운드를 의미한다
notification.sound = 'ping.aiff';

//알랏 문구를 의미한다.
notification.alert = 'Hello World \u270C';

//디바이스에 전달할 내부 페이로드다.
didReceiveRemoteNotification
notification.payload = {id: 123};

//실제 보내지고 result에 다양한 정보가 찍힌다.
apnProvider.send(notification, deviceToken).then(function(result) {  
    console.log(result);
    console.log(result['failed'][0]['response']);
});

~~~

<br>
4. 에러사항
----------


필자는 서버에서 세팅을하고 테스트를 하는데  

서버에서 아래와같은 오류가 떴다.

~~~
const CLIENT_PRELUDE = Buffer.from("PRI * HTTP/2.0\r\n\r\nSM\r\n\r\n");
                              ^

TypeError: this is not a typed array.
~~~

구글링이 나를 도왔다.

단순히 node버전이 낮아서 생긴 이슈라고 버전을 올려보란다.

가장 최신버전에 stable한 버전을 받았더니 해당 에러가 사라졌다.






<br>  
참고  
<https://github.com/node-apn/node-apn>

<https://eladnava.com/send-push-notifications-to-ios-devices-using-xcode-8-and-swift-3/>

++ 많은 구글링..
