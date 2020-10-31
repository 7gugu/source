---
title: JavaScript笔记
original: false
date: 2020-05-25
updated: 2020-05-25
tags: 
  - JavaScript
urlname: javascript-notes
---
复习回顾JavaScript. 摘录一些内容. 
<!--more-->



使用这种方法对所有按钮添加事件是低效且污染HTML代码的: 
~~~
const buttons = document.querySelectorAll('button');

for(let i = 0; i < buttons.length ; i++) {
  buttons[i].addEventListener('click', createParagraph);
}
~~~
最好是在需要js应用到的button里添加: 
~~~
onclick="createParagraph()"
~~~

# 脚本加载策略Script loading strategies

## defer

引入外部js时使用defer来确保HTML和JS能顺利同时加载: 
~~~
<script src="script.js" defer></script>
~~~
老式的解决方案是把脚本放在body的底部, 这样它会在HTML加载后执行. 这样做会导致加载或者解析脚本被完全阻止, 直到HTML DOM被加载完. 在拥有多js的大型网站上这会导致性能问题. 

## async

如果你的脚本没有任何依赖(例如jquery), 而且希望它立即加载, 可以使用async异步加载: 
~~~
<script src="script.js" async></script>
~~~
比如一些游戏中用到的图片资源. 

参考: 
[Mozilla: What is JavaScript?](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript)