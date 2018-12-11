---
layout: post
title: JavaScript의 this키워드에 대하여
categories: [Tech, JavaScript]
---

JavaScript의 this키워드에 대하여



# 1. Default Binding
```javascript
var myFunction = function() {
   console.log(this);
}

myFunction();    // Window 
```  
그냥 this를 부르면 window 객체를 부른다.

# 2. Implicit Binding
```javascript
function foo(){
	console.log(this.a);
}

var obj = {
	a:2,
	foo:foo
};

obj.foo();  // 2
```  
foo:foo 하면, foo에서 정의된 this는 왼쪽 foo가 속한 객체 즉 obj를 가리킨다.


```javascript
var john = {
	name: 'John',
	greet: function(person) {
      console.log("Hi " + person +", my name is " + this.name);
	}
}

john.greet("Mark");  // Hi Mark, my name is John

var fx = john.greet;
fx("Mark");   // Hi Mark, my name is  
// 첨언: chrome에서는 위 결과가 나오지만, 브라우저에 따라 Hi Mark, my name is undefined 가 나올 수 있습니다.
// this를 window로 받는 브라우저에서 그렇습니다.
// 함수형 자바스크립트 방의 ENvironmentSet님 감사합니다.
```
이 방법에서는 function을 객체 안에 넣어서 this키워드의 혼란을 방지하였다.

# 3. Explicit Binding
```javascript
function greet() {
	console.log(this.name);
}

var person = {
	name:'Alex'
}

greet.call(person, arg1, arg2, arg3, ...); // Alex
```
이런 식으로 하면 call의 첫번째 argument가 확실히 명시적으로 this가 된다.

```javascript
function greet() {
	console.log(this.name);
}

var person = {
	name:'Alex'
};

var greetPerson = greet.bind(person); 
greetPerson(); // Alex
```
이렇게 좀 더 직관적이고 명시적인(=Explicit) 방법으로 this가 어떤지 확실하게 가리킬 수 있다.

# 4. 'New' keyword Binding

```javascript
function Foo() {        
        /*
	     1- create a new object using the object literal 
            var this = {};
       */

	  // 2- add properties and methods 
	    this.name = 'Osama';
	    this.say = function () {
		return "I am " + this.name; 
	   };
	  // 3- return this;
}

var name = 'Ahmed';
var result = new Foo();
console.log(result.name);  //3 
```
이런 식으로 new 키워드를 통해 바인딩을 할 수도 있다.

# 5. binding 우선순위

new keyword > explicit > implicit > default

# 6. Arrow Function에서의 this

```javascript
var group = {
  title: "Our Group",
  students: ["John", "Pete", "Alice"],

  showList() {
    this.students.forEach(
         (student) => { 
         	       // this here refer to group object
      	          console.log(this.title + ': ' + student);
                }
    );
  }
};

group.showList();
```
arrow function은 자신의 this, arguments, super 또는 new.target을 바인딩하지 않는다. 그러므로 생성자로서 사용할 수 없으며, new 키워드 앞에 올 수 없다. 
showList() 바깥에서의 this.title은 showList()안쪽에 있는 arrow function에서 쓰이는 this.title과 같다.
애초에 this가 없기때문에 call()이나 apply()메서드는 인자만 전달한다.  
```javascript
var adder = {
  base : 1,
    
  add : function(a) {
    var f = v => v + this.base;
    return f(a);
  },

  addThruCall: function(a) {
    var f = v => v + this.base;
    var b = {
      base : 2
    };

    return f.call(b, a);
  }
};

console.log(adder.add(1));         // 이는 2가 콘솔에 출력될 것임
console.log(adder.addThruCall(1)); // 이도 2가 콘솔에 출력될 것임
```

# 7. function vs arrow function 비교

## 1. setInterval
```javascript
function Person() {
  // Person() 생성자는 `this`를 자신의 인스턴스로 정의.
  this.age = 0;

  setInterval(function growUp() {
    // 비엄격 모드에서, growUp() 함수는 `this`를
    // 전역 객체로 정의하고, 이는 Person() 생성자에
    // 정의된 `this`와 다름.
    this.age++;
  }, 1000);
}

var p = new Person();
```

vs

```javascript
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // this 는 person 객체를 참조
  }, 1000);
}

var p = new Person();
```

(아래는 MDN에서 인용)
>>>
화살표 함수는 전역 컨텍스트에서 실행될 때 this를 새로 정의하지 않습니다. 대신 코드에서 바로 바깥의 함수(혹은 class)의 this값이 사용됩니다. 이것은 this를 클로저 값으로 처리하는 것과 같습니다. 따라서 다음 코드에서 setInterval에 전달 된 함수의 this는 setInterval을 포함한 function의 this와 동일한 값을 갖습니다.
>>>

## 2. 메소드로 사용되는 화살표 함수
```javascript
var obj = {
  i: 10,
  b: () => console.log(this.i, this),
  c: function() {
    console.log( this.i, this)
  }
}
obj.b(); // prints undefined, Window
obj.c(); // prints 10, Object {...}
```  
obj.b()는 this가 바인드되어있지를 않은 function을 부른다.
obj.c()는 obj가 this로써 이미 bind되어있다.


# 8. class에서의 this
class는 사실 function이다. 
(아래는 MDN 인용)
>>>
JavaScript class는 ECMAScript 6을 통해 소개되었으며, 기존 prototype 기반의 상속 보다 명료하게 사용할 수 있습니다. Class 문법은 새로운 객체지향 상속 모델을 제공하는 것은 **아닙니다**. JavaScript class는 객체를 생성하고 상속을 다루는데 있어 훨씬 더 단순하고 명확한 문법을 제공합니다.
>>>

# 9. References

https://medium.com/tech-tajawal/javascript-this-4-rules-7354abdb274c
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes
https://bestalign.github.io/2015/10/18/first-class-object/