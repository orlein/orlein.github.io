---
layout: post
title: Promise Pattern Snippets Example
categories: [Tech, Snippet, JavaScript]
---

<pre><code>
var data = [, , , ...] // (data array);
var promises = [] // empty array
data.map((item) => {
  if (validation condition){
    var promise = (promise to http request or something...)
    promises.push(promise);
  }
});
Promise.all(promises)
.then((res) => {
  (refresh items or something)
});
</code></pre>

This fires multiple requests to update my data, and refresh items when the requests are all ended.
