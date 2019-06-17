---
layout: post
title: JavaScript의 Array함수들
categories: [Tech, JavaScript]
---

JavaScript의 Array함수들
-------------------

**node.js 10.16.0과 ES2015를 사용합니다.** 


# 1. 개요

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/prototype 를 확인하면 Array prototype들에 대해 확인할 수 있습니다. JavaScript에서의 Array를 선언할 경우 쓸 수 있는 메서드 들이다. 크게 세 가지로 나눌 수 있습니다.

## 1. 변경자 메서드
변경자 메서드는 배열을 수정하는 메서드입니다.

## 2. 접근자 메서드
접근자 메서드는 배열을 수정하지 않고 배열을 반환하는 메서드입니다.

## 3. 순회 메서드
순회 메서드는 각 배열요소를 콜백으로 넘겨서 처리하여 배열을 반환하는 메서드입니다. 배열을 수정하는지 아닌지에 대해서는 콜백을 정의하는 것에 따라 다릅니다. 

**이 문서에서는 순회 메서드의 일부에 대해서 다룹니다.**

(제네릭 메서드에 대해서는 이야기하지 않겠습니다.)


# 2. 순회 메서드

## 1. map
map은 우리 말로 '지도'이고, 수학적으로 말하면 '사상', 또 수학적으로 다른말로 '함수' 입니다. 함수에는 여러 종류가 있는데 단사 함수, 전사 함수, 전단사 함수가 있습니다. JavaScript에서의 map은, 전사 혹은 전단사 함수가 됩니다. 예시를 들어보겠습니다.
```JavaScript
const arr = [ 1, 3, 5, 7, 9];
const plusTen = arr.map( x => x + 10)
// output: Array [ 11, 13, 15, 17, 19 ]
```
이 함수는 [ 1, 3, 5, 7, 9] 라는 배열이 [ 11, 13, 15, 17, 19 ]라는 배열을 리턴합니다. 어떻게 리턴할까요? 각 배열의 배열요소를 순회하여 그 배열요소들을 콜백에 집어넣고, 그 결과값을 또 배열에 집어넣어 리턴한 겁니다. 어릴 때 배우던 그 함수로 나타내면, 그 콜백의 형태는 이렇게 생겼습니다.
```
y = f(x) = x + 10
```
즉 배열요소에 대한 '사상'을 콜백 형태로 정의하여 치역을 리턴한겁니다. 그림으로 표현하면 아래와 같습니다.

![전단사 함수의 예시(출처: 위키백과 '함수' 항목)](/assets/Bijection.svg.png)

이 그림에서의 사상은 어떻게 되어 있는지 모릅니다. 어쩌면 이럴수도 있죠.

```JavaScript
const arr = [ 1, 2, 3, 4 ];
const wierdMap = arr.map( x => {
  if (x === 1) {
    return 'D';
  } else if (x === 2) {
    return 'B';
  } else if (x === 3) {
    return 'C';
  } else if (x === 4) {
    return 'A';
  }
});
```

중요한건 어쨌든 사상(=함수)가 있고, 그 사상때문에 두 배열이 서로 대응하고 있다는 겁니다. 그리고 그 콜백은 이렇게 생겼습니다.
```JavaScript
callback(currentValue [, index[, array ]])
```
람다 혹은 함수로 정의될 수 있고, []은 줘도 되고 안줘도 되는 parameter입니다. 즉, 저처럼 그냥 현재 값만 받아다가 순회할 수도 있고, 아니면 인덱스와 배열 모두를 받아서 다른 값을 참조할 수도 있습니다. 그렇기 때문에 처음 학습하면 index와 array까지 써야 하나? 라는 생각이 들고, 결국 map을 원래 array를 변형해가며 쓰는 경우가 생깁니다. 이렇게 쓰는 것이 **틀린 것은 아니지만** 권장하고 싶지는 않습니다. 메모리 관리 관점에서는 새 배열을 리턴하여 참조하지 않고 구 배열을 조작해서 쓰는 방법도 좋지만, 그럴바에는 위에 있는 변경자 메서드에서 찾아서 쓰는게 낫지 않을까요? 가독성을 위해서요.

### 1. 실제 사용 (1)

실제 사용할 때는, 배열과 같은 개수의 배열을 리턴하되 원래 배열요소가 콜백에 의해 처리된 형태였으면 할 때 쓰면 좋습니다. 예를 들면 이렇습니다.
```JavaScript
const getAllUsers = async () => {
  try {
    const users = await axios.get('/users');
    // users에는 User의 배열이 있다고 가정하고, User에는 firstName, lastName, birthDate, locale이 있다고 생각해봅시다.
    const displayUsers = users.map(user => {
      if (user.locale === 'ko') {
        return { 
          displayName: user.lastName + user.firstName,
          displayAge: new Date().getFullYear() - birthDate.getFullYear() + 1 //한국나이
          locale: locale //지역 
        }
      } else if (user.locale === 'en'){
        return {
          displayName: user.firstName + ' ' + user.lastName,
          displayAge: (new Date(Date.now() - birthday)).getFullYear() - 1970 //만 나이, 공식 틀렸으니 참고만..
          locale: locale
        }
      }
    });
    return displayUsers;
  } catch(e) {
    // error handling
  }
}
```
이 함수는 유저들의 배열을 받아서 유저 locale에 따라 표시되는 이름과 표시되는 나이를 다르게 표현하는 함수입니다. 유저의 locale은 ko와 en 둘밖에 없다고 가정하고, 한국사람은 한국식으로 /성+이름/으로 표현되고 미국사람은 서양식으로 /firstName lastName/으로 표현됩니다. 백엔드에서 받아온(혹은 미래에 받아 올) 유저들 데이터는 항상 일정합니다. 빠지거나 추가되는 일 없이, 그대로 표시되는 배열만 바뀌게 됩니다. map을 쓰기 좋은 사례입니다.

### 2. 실제 사용(2)
```JavaScript
//index.js
import { render } from 'react-dom';
import React from 'react';
const x = [ 1,2,3,4,5 ] //받아온 데이터
const App = () => (
  <div>
  {x.map((v, i) => (<div key={i}>The number is: {v}</div>)) /* 유저가 보는 부분 */} 
  </div>)
render(<App />, document.getElementById('root'));
```
( https://stackblitz.com/edit/react-eyapgt 에서 실제로 보실 수 있습니다)

React.js에서, 데이터를 Array형태로 받아왔을 때 이를 x라고 한다면, 데이터 그대로 뿌려주는 경우는 없을겁니다. 데이터를 잘 바인딩하여 유저가 볼 수 있게 해야하는데, 이럴 때 유용한 것이 map입니다. 이 예제에서는 데이터를 순회하여 jsx객체로 만들고, React에서 render할 수 있게 합니다. 덤으로 div에 key도 줄 수 있고요. 이렇게 하다가 복잡해진다면, map함수를 renderArray같이 정의할 수 있겠습니다.
```JavaScript
import { render } from 'react-dom';
import React from 'react';
const x = [ 1,2,3,4,5]
const renderArray = (arr) => arr.map((v) => (<div>The number is: {v}</div>))
const App = () => (
  <div>
  {renderArray(x)}
  </div>)
render(<App />, document.getElementById('root'));
```

## 2. reduce
reduce는 한국말로 '줄이다'인데, 이건 차라리 다른 프로그래밍 언어에서 reduce와 같은 역할을 하는 함수 이름을 보는 것이 더 낫습니다. 이름은 fold입니다. https://wiki.haskell.org/Fold 배열을 배열요소 하나씩 접어올리다가 결국 하나의 값만 남게 되는데, Haskell같은 언어에서는 오른쪽에서 왼쪽으로 접을지 아니면 왼쪽에서 오른쪽으로 접을지를 함수 이름에 따라 결정할 수도 있습니다. JavaScript에서는 reduce()와 reduceRight()으로 결정합니다. 

접는다 접는다 했는데, 아직 무슨소린지 모르시겠다면 한번 따라해보세요.

1. A4용지를 하나 준비합니다.
2. 반으로 접습니다.
3. 2.에서 접힌 종이를 반으로 접습니다.
4. 3.에서 접힌 종이를 반으로 접습니다. 칸이 8칸 생겼습니다.
5. 각 칸에, 순서대로 1부터 8까지의 숫자를 적습니다.
6. 1이 적힌 칸을 접어서 2가 적힌 칸에 포개어 접읍시다. 그 위에 1 + 2라고 적습니다.
7. 6.에서 적힌 칸을 1 + 2 이 적힌 칸에 포개어 접고, 그 위에 (1 + 2) + 3 라고 적습니다.
8. 7.에서 적힌 칸을 (1 + 2) + 3 이라고 적힌 칸에 포개어 접고, 그 위에 ((1 + 2) + 3) + 4 라고 적습니다.
9. 이런 식으로 8까지 적게 되면, ((((((1+2)+3)+4)+5)+6)+7)+8 이 적힙니다. 이게 바로 fold 혹은 reduce의 원리입니다. (실제 컴퓨터에서의 연산 방향은 어떻게 될지 한번 생각해보세요)

즉 배열이 하나씩 접혀가면서, 콜백을 수행하게 됩니다. JavaScript에서의 예시를 보겠습니다.

```JavaScript
const arr = [ 1,2,3,4,5,6,7,8 ];
const result = arr.reduce((acc, cur) => acc + cur);
// output: 36
```
콜백을 설명하자면 acc는 누적 값이고, cur은 현재 값입니다. 순회하면서 현재 값이 1부터 8까지 바뀌게 되고, acc에다가 현재 값을 더합니다. 하지만 센스가 있으신 분이라면 여기서 이상한 점을 눈치챌텐데 누적 값의 '초기 값'은 없는걸까요? 여기서는 0으로 초기화 되어 있습니다. number배열을 reduce하실 때는 초기값을 신경쓰지 않으셔도 됩니다. 그래도 reduce 할 때 이런 숫자만 있는 것이 아닙니다.
```JavaScript
const arr = [{ x: 'w'}, {x: 'o'}, {x: 'r'}, {x:'l'}, {x: 'd'}]
const result = arr.reduce((acc, cur) => acc + cur.x);
// output: '[object Object]orld'
```
당연히 결과는 world를 의도했습니다. 이 결과가 이렇게 나오는 것은, 그냥 초기값이 전달되지 않아서 그렇습니다. (더 정확히 파고드시려면 JavaScript가 객체를 그냥 console.log 했을 때 어떤 일이 일어나는지를 보세요)
```JavaScript
const arr = [{ x: 'w'}, {x: 'o'}, {x: 'r'}, {x:'l'}, {x: 'd'}]
const result = arr.reduce((acc, cur) => acc + cur.x, 'hello ');
// output: 'hello world'
```
여기에서는 'hello '(띄어쓰기 포함)이 시작부분에 있게끔 의도했습니다. 이렇게 하여 'hello world'가 출력되었습니다. 초기값을 지정해서 특정 배열이 하나의 값으로 잘 '접혀진'걸 볼 수 있습니다. 

reduce의 콜백은 map과 마찬가지로 index와 array를 받을 수 있습니다.

### 실제 사용
```JavaScript
const user = {
  name: 'alice',
  items: [
    { name: 'pencil', price: 12 },
    { name: 'book', price: 213 },
    { name: 'bag', price: 2512 },
    { name: 'phone', price: 3144 },
    { name: 'computer', price: 2878 },
    { name: 'book', price: 225 }
  ]
}
const newUser = {
  name: user.name,
  money: user.items.reduce((acc, cur) => acc + cur.price, 0),
  items: user.items.reduce((acc, cur) => {
    console.log(acc, cur)
    if (cur.name in acc) {
      acc[cur.name]++;
    } else {
      acc[cur.name] = 1
    }
    return acc;
  }, {})
}
/* output: { 
  name: 'alice', 
  money: 8984, 
  items: {
    bag: 1,
    book: 2,
    computer: 1,
    pencil: 1,
    phone: 1
    } 
  } */
```
이런 식으로 받아온 데이터를 처리하고 싶을 때 쓸 수도 있습니다. 이런 단편적인 용도 이외에도, Array를 순회하면서 하나의 object로 만드는 것은 상당히 유용합니다. 

## 3. filter
필터는 굉장히 직관적입니다. '걸러내기'인데, 기준에 맞지 않는 배열요소를 걸러냅니다. 결국에는 기존 배열의 개수와 적거나 같은 배열이 리턴됩니다. 
```JavaScript
const arr = [ 1, 44, 6456, 243, 66, 68645, 3433 ];
arr.filter( x => x > 555);
// output: [ 6456, 68645, 3433 ]
```
배열요소 중 555가 넘는 것만 추려서 리턴합니다. 아주 직관적이고 간단합니다. 역시 map과 마찬가지로 콜백에서 index와 array를 받을 수 있습니다. filter는 그냥 걸러내고 싶은 기준에 따라 걸러내면 됩니다.

## 4. map, reduce, filter의 연계
이 세 메서드는 연계하기 아주 좋습니다. 예를 들어 다음과 같습니다.
```JavaScript
// 배열의 각 값을 제곱하고, 제곱한 값이 10 이상인 것들만 서로 모아서 더하기
const arr = [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 ,12 ]
const result = arr.map(x => x * x)
                  .filter(x => x > 10)
                  .reduce((acc, cur) => acc + cur, 0);
// output: 636
```
이렇게 메서드 체이닝으로 연계하면 배열을 맘대로 조작하고 값을 쉽게 리턴할 수 있습니다. 같은 동작을 for문으로 짠다면...
```JavaScript
const arr = [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 ,12 ]
let sum = 0;
for(let i = 0; i < arr.length; i++) {
  const x = arr[i] * arr[i];
  if (x > 10) {
    sum += x;
  }
}
```
차이가 없어 보일수도 있습니다. 하지만, 만약에 콜백이 조금 더 복잡하다면? 지금 이 콜백에서 정의하는 동작이 다른 곳에서도 계속 쓰여서 함수를 따로 빼야한다면? 그럴때는 밑처럼 쓰는 것이 직관적이지 않을 수 있습니다. 특히나 위처럼 쓸 경우의 가독성이 약간 더 높은 것이, 여러 줄에 쓰이게 된다면 당연히 코드의 전체적인 가독성이 높아져 코드 품질이 좋아지는 효과를 낳습니다. 재사용성은 말할것도 없죠. 순수 함수로 정의된 콜백은 다른데서 정의해서 재활용해도 문제없습니다.


