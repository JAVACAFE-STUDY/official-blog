---
title: 'CSS Selector Part.01'
date: 2017-11-09 12:00:00
categories:
- css
tags:
- css
- selector
thumbnail: https://www.w3.org/TR/css3-selectors/
author : 이영범
---



[지난 시간](http://tech.javacafe.io/css/2017/11/09/css_selector_part01/)에 CSS selector에 사용할 수 있는 라이브러리/메서드에 대해서 알아봤습니다<br>
이번 시간에는 실제로 CSS Selector에는 어떤 것들이 있는지, 그리고 어떤 식으로 사용하는게 좋을지에 대해서 생각해 보겠습니다.

# 기본 
[지난 시간](http://tech.javacafe.io/css/2017/11/09/css_selector_part01/) 만들었던 Selector를 활용하여 
가장 많이 사용되는 간단한 Selector부터 알아보겠습니다.<br>
그냥 하면 재미가 없으니까 간단한 
```javascript
var $$ = $ || document.querySelectorAll.bind(document);     // select 하고자 하는 대상이 복수
var $ = $ || document.querySelector.bind(document);     // select 하고자 하는 대상이 단수

$('#id')    // id로 조회
$$('.class')    // class로 조회
$('#id')    // id 조회
```
<a href="">test</a>