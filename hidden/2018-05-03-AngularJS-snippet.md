---
layout: post
title: AngularJS redirection snippet
categories: [Tech, JavaScript, Snippet]
---


AngularJS Redirection Snippet

----------------------

1. Add dependency $window in controller function parameter

<pre><code>

var Controller = function ($scope, $window) { ... }

</code></pre>

2. Make Function

<pre><code>

function() {
		...
		if ( ... ) {
			$window.location.href = "(url)";
		}
        ...
	}

</code></pre>

* url can start with basis

>  $window.location.href = "https://google.com"

* url can start with project root

> $window.location.href = "menu1/menu2"

=> redirect to http://localhost:3000/menu1/menu2


