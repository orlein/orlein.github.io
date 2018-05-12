---
layout: post
title: Promise Pattern Snippets
categories: [Tech, Snippet, JavaScript]
---


Promise Pattern Snippets
-------------------

<pre><code>
var PromiseA = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("hello");
    }, 3000);
});
var PromiseB = new Promise((resolve, reject) => {
    setTimeout(()=>{
        resolve("world");
    }, 4000);
});

function getExample(){
    var a = PromiseA;
    var b = PromiseB;
    return Promise.all([a, b]).then(function([resultA, resultB]){
        console.log(resultA, resultB); // prints hello world in 4 seconds later
    })
    
}
</code><pre>

You can use resultA and resultB simutaneously.

in ES8,

<pre><code>
var PromiseA = new Promise((resolve, reject) => {
    resolve("hello");
});
var PromiseB = new Promise((resolve, reject) => {
    resolve("world");
});
async function getExample2(){
    var resultA = await PromiseA;
    var resultB = await PromiseB;
    console.log(resultA, resultB);
}
</code></pre>
you can use like this.

