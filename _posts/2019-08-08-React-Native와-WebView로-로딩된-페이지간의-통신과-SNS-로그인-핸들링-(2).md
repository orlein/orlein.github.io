---
layout: post
title: React Native와 WebView로 로딩된 페이지간의 통신과 SNS 로그인 핸들링 (2)
categories: [Tech, JavaScript, ReactNative]
---


App 구축 방법 (ReactNative cli, Expo 아님)
--------------

순서

0. 시작하기 전에 node.js와 npm이 적절히 깔려있는지 확인한다. 

1. cli 툴을 깔자.
    
    ``` $ npm i -g react-native-cli ```

2. 적절한 폴더로 이동해서, 

    ``` $ react-native init SampleProject ```

    ``` $ cd SampleProject ```

    ``` $ react-native run-android ``` (또는 맥에서 ``` $ react-native run-ios ```)

를 전부 완료하고 제대로 돌아가는지 보자.

3. 

![스크린샷](/assets/react-native-screenshot.png);

이런게 뜨면 성공. 해당 스크린샷은 맥에서 iOS 시뮬레이터 띄웠음
