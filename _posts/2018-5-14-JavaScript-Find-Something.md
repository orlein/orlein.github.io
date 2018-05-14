---
layout: post
title: JavaScript-Find-Something snippet
categories: [Tech, JavaScript, Snippet]
---


JavaScript-Find-Something

----------------------

<pre><code>
var data = [...];
var id = "...";
var find = function(arr, key) {
  for (var i = 0; i<arr.length; i++) {
    if(arr[i].some_key === key){
      return arr[i].hiddenInfo;
    }
  }
}
console.log(find(data, id));
</code></pre>


A very simple O(n) finding algorithm for JavaScript

Be careful: this snippet shouldn't be used to a larget set of data.
