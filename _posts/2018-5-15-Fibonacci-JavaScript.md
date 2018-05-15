---
layout: post
title: Fibonacci in JavaScript
categories: [Tech, JavaScript]
---

Fibonacci in JavaScript
-------------------

문제: https://www.acmicpc.net/problem/1003
<pre><code>
var count0and1 = function(n) {
	var zeros = 0;
	var ones = 0;
  var fibonacci = function(n) {
    if (n <0 ){
      return null;
    } else if (n == 0) {
    	zeros++;
      return 0;
    } else if(n==1) {
    	ones++;
      return 1;
    } else {
      return fibonacci(n-1) + fibonacci(n-2);
    }
  }
  var result = fibonacci(n);
  return [zeros, ones, result];
	
}
var fibResult = count0and1(7); //숫자 크게 하지 말것! 엄청 오래걸림! Don't increase! It takes too long.
console.log(fibResult[0], fibResult[1], fibResult[2]);
</code></pre>
Implementing fibonacci function in recursive way


<pre><code>
var fibonacci_memo = function(n) {
  var memo = [0, 1];
  var zeros = 0;
  var ones = 0;
  var fib = function(n) {
		var result = memo[n];
   	if (n == 0){
      zeros++;
    }
    if (n == 1){
      ones++;
    } 
    if (typeof result !== 'number') {
      result = fib(n - 1) + fib(n - 2);
      memo[n] = result;
    }
    return result;
	}
  var r = fib(n);
  return [zeros, ones, r];
  //return r;
  //return fib;
}
var fibMemoResult = fibonacci_memo(55);
console.log(fibMemoResult[0], fibMemoResult[1], fibMemoResult[2]);
</code></pre>
Implementing fibonacci function in memoization way

This is much more faster than the recursive way.