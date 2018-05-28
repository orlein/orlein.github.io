loopback.io
```
npm install -g loopback-cli
```
하면 된다는데 잘 안됨


설치중 문제:

파이썬 문제 -> npm install --global --production windows-build-tools 로 윈도우 빌드툴즈를 설치해야한다
```Powershell
C:\Users\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa>if not defined npm_config_node_gyp (node "C:\Program Files\nodejs\node_modules\npm\node_modules\npm-lifecycle\node-gyp-bin\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild )  else (node "C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\bin\node-gyp.js" rebuild )
gyp ERR! configure error
gyp ERR! stack Error: Can't find Python executable "C:\Python36\python.exe", you can set the PYTHON env variable.
gyp ERR! stack     at PythonFinder.failNoPython (C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\lib\configure.js:483:19)
gyp ERR! stack     at PythonFinder.<anonymous> (C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\lib\configure.js:508:16)
gyp ERR! stack     at C:\Program Files\nodejs\node_modules\npm\node_modules\graceful-fs\polyfills.js:284:29
gyp ERR! stack     at FSReqWrap.oncomplete (fs.js:166:21)
gyp ERR! System Windows_NT 10.0.16299
gyp ERR! command "C:\\Program Files\\nodejs\\node.exe" "C:\\Program Files\\nodejs\\node_modules\\npm\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild"
gyp ERR! cwd C:\Users\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa
gyp ERR! node -v v9.4.0
gyp ERR! node-gyp -v v3.6.2
gyp ERR! not ok
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: ursa@0.9.4 (node_modules\loopback-cli\node_modules\ursa):
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: ursa@0.9.4 install: `node-gyp rebuild`
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: Exit status 1

+ loopback-cli@4.2.0
updated 8 packages in 38.591s
```
python을 못찾아서 생기는 일이다. 환경설정변수에 python을 해야한다느니 뭐니 말이 있지만, 그렇게 해서 해결 되지 않았다. 
```
npm install -g --production windows-build-tools
```
를 해보자. 그러면 Visual Studio Build Tools와 Python2.7을 설치한다. 아무래도 Python 3버전이 아니라 2버전을 써야 맞나보다. 나는 2.7을 쓰지 않으니 안되었을 수도 있다.
주의할 점은 다른 npm 설치과정과 다르게,
```
Status from the installers:
---------- Visual Studio Build Tools ----------
Successfully installed Visual Studio Build Tools.
------------------- Python --------------------
Successfully installed Python 2.7
```
이런 메시지가 확실하게 뜨는지 확인해야한다. Visual Studio Build Tools는 생각보다 설치가 오래걸린다.


ssl문제 -> SSL을 다운받아 C:\에 풀고 폴더이름을 적절히 바꿔줘야한다
```
..\src\ursaNative.cc(157): warning C4244: 'argument': conversion from 'ssize_t' to 'int', possible loss of data [C:\Use
rs\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa\build\ursaNative.vcxproj]
..\src\ursaNative.cc(172): warning C4244: 'argument': conversion from 'ssize_t' to 'int', possible loss of data [C:\Use
rs\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa\build\ursaNative.vcxproj]
LINK : fatal error LNK1181: cannot open input file 'C:\OpenSSL-Win64\lib\libeay32.lib' [C:\Users\Username\AppData\Roaming
\npm\node_modules\loopback-cli\node_modules\ursa\build\ursaNative.vcxproj]
```
python 문제를 해결하고 나면 ssl문제가 생긴다. 이 문제는 ssl을 설치하면 해결된다.
[여기](https://code.google.com/archive/p/openssl-for-windows/downloads)를 눌러 적당한 버전을 다운받는다. 그리고 그 압축파일을 바로 C에 풀어버리고, 폴더이름을 OpenSSL-Win64로 바꿔준다.
```
npm WARN deprecated nodemailer@2.7.2: All versions below 4.0.1 of Nodemailer are deprecated. See https://nodemailer.com/status/
C:\Users\Username\AppData\Roaming\npm\lb -> C:\Users\Username\AppData\Roaming\npm\node_modules\loopback-cli\bin\loopback-cli.js

> ursa@0.9.4 install C:\Users\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa
> node-gyp rebuild


C:\Users\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa>if not defined npm_config_node_gyp (node "C:\Program Files\nodejs\node_modules\npm\node_modules\npm-lifecycle\node-gyp-bin\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild )  else (node "C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\bin\node-gyp.js" rebuild )
이 솔루션의 프로젝트를 한 번에 하나씩 빌드합니다. 병렬 빌드를 사용하려면 "/m" 스위치를 추가하십시오.
  ursaNative.cc
  win_delay_load_hook.cc
..\src\ursaNative.cc(157): warning C4244: 'argument': conversion from 'ssize_t' to 'int', possible loss of data [C:\Use
rs\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa\build\ursaNative.vcxproj]
..\src\ursaNative.cc(172): warning C4244: 'argument': conversion from 'ssize_t' to 'int', possible loss of data [C:\Use
rs\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa\build\ursaNative.vcxproj]
     Creating library C:\Users\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa\build\Release\urs
  aNative.lib and object C:\Users\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa\build\Release\
  ursaNative.exp
  Generating code
  All 188 functions were compiled because no usable IPDB/IOBJ from previous compilation was found.
  Finished generating code
  ursaNative.vcxproj -> C:\Users\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa\build\Release\\
  ursaNative.node
  ursaNative.vcxproj -> C:\Users\Username\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\ursa\build\Release\u
  rsaNative.pdb (Full PDB)
+ loopback-cli@4.2.0
added 3 packages and updated 1 package in 29.127
```
에러 없이 잘 설치된다.

```
> lb
```
를 commandline에 입력하여 loopback을 실행해보자. 그러면...
```
Error: Cannot find module 'ieee754'
    at Function.Module._resolveFilename (module.js:555:15)
    at Function.Module._load (module.js:482:25)
    at Module.require (module.js:604:17)
    at require (internal/module.js:11:18)
    at Object.<anonymous> (C:\Users\Accura\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\swagger-client\dist\index.js:7:11355)
    at t (C:\Users\Accura\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\swagger-client\dist\index.js:1:177)
    at Object.<anonymous> (C:\Users\Accura\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\swagger-client\dist\index.js:7:15)
    at Object.<anonymous> (C:\Users\Accura\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\swagger-client\dist\index.js:7:11111)
    at t (C:\Users\Accura\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\swagger-client\dist\index.js:1:177)
    at Object.<anonymous> (C:\Users\Accura\AppData\Roaming\npm\node_modules\loopback-cli\node_modules\swagger-client\dist\index.js:1:48717)
```
이런 문제가 생기는데, 그냥 ieee754가 없어서 그런다. 
```
npm install -g ieee754
```
로 설치하면 일단 loopback을 실행할 수 있다.
설치하고 나서도,
```
npm install --save ieee754
```
로 ieee754를 프로젝트 내에 설치해야한다.



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
