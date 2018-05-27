---
layout: post
title: JavaScript Snippet for KendoTreeview - from flat table to nested algorithm
categories: [Tech, Snippet, JavaScript]
---

JavaScript Snippet for KendoTreeview
--------------


<pre><code>
var treeviewDatasource = function(dt){
    var keys = Object.keys(dt[0]);
    var result = [];
    var node = [];
    dt.map(function(row, index) {
        node = result;
        keys.map(function(key, i) {
            if (node.length === 0 || node[node.length - 1].text !== row[key]){
                var newElement = {text: row[key]};
                if (keys.length - 1 !== i){
                    newElement.items = [];
                }
                node.push(newElement);
            }   
            node = node[node.length - 1].items;           
        });
    });
    return result;
}
</code></pre>

In kendoUI for jQuery, there is no library for rearranging data for its treeview datasource. 

