---
layout: post
title: React Native와 WebView로 로딩된 페이지간의 통신과 SNS 로그인 핸들링 (4)
categories: [Tech, JavaScript, ReactNative]
---

RN 프로젝트에서 WebView 설정
---------------------------

# 1. JavaScript 코드 인젝션

ReactNative에서 WebView를 띄우는 곳으로 가서, render()를 찾는다. 그리고
```javascript
render() {

  const patchPostMessageJsCode = `(${String(
    function() {
      var originalPostMessage = window.postMessage
      var patchedPostMessage = function(message, targetOrigin, transfer) {
          originalPostMessage(message, targetOrigin, transfer)
      }
      patchedPostMessage.toString = function() {
          return String(Object.hasOwnProperty).replace('hasOwnProperty', 'postMessage')
      }
      window.postMessage = patchedPostMessage
    }
  )})();`
  
  return (
    <WebView
      ref={wv=> this.wv = wv /* 2 */} 
      injectedJavaScript={patchPostMessageJsCode} 
      onMessage={(e)=>{ /* 3 */}} />
  )
```
이런 식으로 WebView에다가 JavaScript 코드를 inject 해줘야 한다. 2와 3 부분은 나중에 다룬다.

# 2. RN -> WebView 메시지 전송

/* 2 */ 부분에서 ref지정이 꼭 필요한데, 왜냐면 이 ref로 메시지를 보내기 때문이다. this 스코프를 가져갈 수 있는 곳에서,
```javascript
const data = {} //어느 JSON 데이터
this.wv.postMessage(JSON.stringify(data));
```
이런 식으로 data를 전송해줄 수 있다. data는 사전에 형태를 정해놓는 편이 좋다.

# 3. WebView -> RN 메시지 전송

WebView로 띄울 웹앱 또는 웹 페이지에서 전송하는것은 postMessage로 한다.

window는 https://developer.mozilla.org/ko/docs/Web/API/Window 
```javascript
sendPostMessageToRN = (message, payload) => {
  setTimeout(() => {
    window.postMessage(message, payload);
  }, 100);
}
//////////
this.sendPostMessageToRN(JSON.stringify({email, name}));
```
나는 이런식으로 setTimeout으로 처리했다. 이렇게 하지 않으면 사실 살짝 불안정하다. 

왜 그런지는 모르겠다. 좀 더 알아보면 해답이 금방 나올 것 같지만, 별로 하고싶지는 않다.

/* 1 */에서 WebView로부터 받은 값을 받아다 RN에서 처리하게 할 수 있다. 

onMessage의 callback을 보면, e에는 nativeEvent라는 것이 있고, 여기에 받아온 데이터를 처리한다.
e의 type설명은 생략한다.

```javascript
onMessage={(e)=> {
  const data = JSON.parse(e.nativeEvent.data);
}}
```
