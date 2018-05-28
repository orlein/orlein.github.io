loopback.io

npm install -g loopback-cli

하면 된다는데 잘 안됨


설치중 문제:

파이썬 문제 -> npm install --global --production windows-build-tools 로 윈도우 빌드툴즈를 설치해야한다

ssl문제 -> SSL을 다운받아 C:\에 풀고 폴더이름을 적절히 바꿔줘야한다 (지금 기억 안남)

ieee754문제 -> npm install -g ieee754하면 일단은 되는데, 그 다음엔 lb로 생성되는 프로젝트마다 npm install --save ieee754 해줘야한다

dependency vulnerability 문제 -> 해결 못함



loopback의 장점:

1. 알아서 Authentication을 만들어준다.
2. API가 모델 중심으로 자동 작성된다. 모델의 특성과 관계만 잘 입력해주면 알아서 API를 슥슥 만들어준다.
3. cli가 굉장히 간편하고 한국어로 번역이 잘 되어있다.
4. express 대체이다.
5. backend 자동화이지만 frontend를 위한 배려도 잘 해놨다. express처럼 template engine으로 된 페이지 몇개 떨렁 던져주고 마는 것이 

loopback의 단점:

1. 설치과정에서 몇가지 문제점이 생겼다.
2. api scaffolding이 아니라, json만 생성하고 나머지는 알아서 생성한다. API의 실제 모습을 들여다보기 힘든 구조다. 어떻게든 들여다볼 수 있겠지만, 프로젝트 폴더에서 보이지 않는다.
3. platform마다 sdk를 각각 깔아줘야한다.


결론:

1. 공부해 볼 가치는 있지만, express같은 다른 node.js 프레임워크를 공부하는게 우선이다.
2. 작은 프로젝트를 간단하게 만들어 볼 때 정말 유용한 것 같다. mockup 작성보다 빠른듯.
