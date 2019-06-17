---
layout: post
title: React Native와 WebView로 로딩된 페이지간의 통신과 SNS 로그인 핸들링 (1)
categories: [Tech, JavaScript, ReactNative]
---


목차
--------------------------

1. 들어가기 앞서
2. App 구축 방법 (ReactNative cli, Expo 아님)
3. RN 프로젝트에서 firebase 등 dependency 추가하고 설정하기
4. firebase login dependency 설정하기
5. WebView에서 ReactNative측으로 정보 넘겨주기
6. ReactNative에서 WebView측으로 정보 넘겨주기

들어가기 앞서, Q&A
---------------------

1. 이 짓을 왜 합니까?

    - React.js로 웹 프론트를 개발해 놨고, 잘 돌아가는데, 모바일 앱을 당장에 모두 구축해서 내기 힘들 때
    - ReactNative로 만든 앱에서 띄운 WebView가 있는데, WebView에서 띄운 페이지와 앱이 통신해야 할 때
    - 굳이 ReactNative에서 띄운 WebView에서 로그인을 시도해야할 때
    - 등등...
    
2. 이 문서를 왜 썼습니까?

    - 찾아보니 굉장히 파편화되어있어서 찾기 골때리니까
    - 내가 기록하고 기억하려고
    - 이런 골때리는 짓도 있다고 소개하려고

3. 이런 일은 언제 했습니까?

    - 한 7월 중순 쯤? 꽤 됐음

4. 구체적인 기술 스택은 어떻게 됩니까?

    1) DynamoDB + Spring Boot + React.js로 만든 웹 앱을 aws 인스턴스에 올렸음
    
    2) cli로 만든 ReactNative App에서 WebView를 생성하고 위의 주소로 접속할 수 있게 했음. *Expo 아님!!!*
    
    3) ReactNative와 Firebase를 연결하고 로그인기록을 저장할 수 있게 구축해놨음

5. 독자에게 필요한 정보일까요?

    - 나도 모릅니다. RN <=> WV 통신 검색하다 오셨겠죠.



    

