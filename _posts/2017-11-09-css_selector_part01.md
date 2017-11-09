---
title: 'CSS Selector Part.01'
date: 2017-11-09 12:00:00
categories:
- css
tags:
- css
- selector
thumbnail: https://www.w3.org/TR/2011/REC-CSS2-20110607/selector.html#q5.0
---

* 작성자 : 이영범

#CSS Selector Part.01
이번에는 웹프로그래밍에서 빼놓을 수 없는 CSS selector에 대해서 알아보겠습니다.
첫 시간이니만큼 가벼운 내용으로 시작해서 점차 심화해 가도록 하겠습니다

## 연장부터 챙기자
Selector를 쓰려면 도구가 필요하겠죠? <br>
몇 가지 Selector에 사용되는 주요 도구들을 시대 흐름순으로 가볍게 훑어 보면서 현시점에서 '어떤' 도구를 '어떻게' 사용하는게 좋을지부터 생각해 보겠습니다.

#### document.getElement*()
```javascript
document.getElementById('아이디')
document.getElementsByClassName('클래스')
document.getElementsByName('Name')
document.getElementsByTagName('Tag name')
document.getElementsByTagNameNS('Namespace', 'Tag name')
```
한 때는 DOM에서 제공하는 위에 기술한 정도의 매우 단순하고 제한적인 메서드밖에 없었습니다.<br> 
만약에 '선택된 element의 자손들 중 마지막 (직계)자식의 자손의 3번째 li'를 Select하려고 했을 때 아래의 메서드만 있다면 정말 막막하겠죠       

#### Sizzle 
```javascript
$('.cls > :nth-last-child(1) li:nth-of-type(3)')
```
Sizzle이라는 매우 강력하고 직관적인 조회가 가능한 Selector 엔진이 나옵니다. <br>
Sizzle은 jQuery에 탑재되어 jQuery의 인기에 큰 역할을 하게 됩니다. <br>
위에서 이야기 한 복잡한 조회가 위의 한 줄로 가능해지죠.
  
#### document.querySelector*
```javascript
document.querySelector('.cls > :nth-last-child(1) li:nth-of-type(3)')
document.querySelectorAll('.cls > :nth-last-child(1) li')
```
jQuery의 Sizzle은 매우 강력했지만 사용하기 위해서는 페이지에 매번 import 해야하는 번거로움이 있었습니다.<br>
하지만 Selector API 1에서 복잡한 CSS Selector를 한 번에 처리가 가능한 document.querySelector와 document.querySelectorAll이 소개됩니다.<br>
위의 코드처럼 강력한 selector 기능을 제공하죠.<br>

## 개선해보기
document.querySelector*가 등장하면서 Selector를 위해 특정 라이브러리를 import하는 일은 피할 수 있게 되었습니다.<br>
하지만 여전히 불편한 점은 남아 있습니다. 그건 바로 메서드의 이름이 너무 길다는거죠.<br>
'$'에 길들여진 많은 개발자들에게 메서드 치다가 죽을 것 같은 'document.querySelectorAll'이라는 메서드명은 너무 가혹하죠...

그래서 해당 메서드를 새로운 변수에 할당해서 메서드명의 길이를 줄여보겠습니다
```javascript
var $ = $ || document.querySelector.bind(document);
var $$ = $ || document.querySelectorAll.bind(document);
```
이미 해당 페이지에서 $(jQuery)가 사용되고 있으면 $를 유지하고 없으면 $라는 변수에 document.querySelector 할당합니다.<br>
이때 중요한 점은 document.querySelctor을 그냥 할당하지 않고 bind로 this를 고정시켜 준 점입니다.<br>

document.querySelctor 메서드 내부에서 사용될 this는 document.querySelctor()로 호출할 경우 document가 되지만, 해당 메서드를 다른 변수에 할당하고 그 변수를 다른 곳에서 (예 전역인 window) 호출할 경우 this의 값이 달라지게 됩니다.<br>

그래서 'bind(document)'를 해주지 않고 그냥 'var $ = document.querySelector;'라고 할당하고 호출해보면 에러가 발생하게 됩니다.<br>
이런 에러를 피하기 위해 'bind(document)'를 추가해줘서 this를 명시적으로 document로 고정시켜줍니다.

이제 완성되었습니다
```javascript
$('.cls > :nth-last-child(1) li:nth-of-type(3)');   // 조건을 만족하는 첫 번째 element만 select
$$('.cls > :nth-last-child(1) li:nth-of-type(3)');  // 조건을 만족하는 모든 element select
```


이제 도구가 준비 되었으니 다음에는 selector의 구체적인 용법들에 대해서 알아보겠습니다 :) 