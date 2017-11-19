---
title: 'CSS Selector Part.02'
date: 2017-11-16 12:00:00
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
그냥 하면 재미가 없으니까 간단한 퀴즈로 시작해 보겠습니다.<br>
아래 문제를 보고 해당하는 selector를 작성한 후에 '정답제출'을 눌러서 현재 수준을 진단해 보세요.


<div id="container">
    <div class="card selector-root">
        <div class="code">
            &lt;div class="code">
            <div class="a" data-x="pre1-x-1suf" id="id1">&lt;div class="a" data-x="pre1-x-1suf" id="id1">&lt;/div></div>
            <div class="a b" data-x="pre2-x-2suf">&lt;div class="a b" data-x="pre2-x-2suf">&lt;/div></div>
            <div class="a b c" data-x="pre3-x-3suf" id="id2">&lt;div class="a b c" data-x="pre3-x-3suf" id="id2">&lt;/div></div>
            <div id="container1">
                &lt;div id="container1">
                <div class="b" data-x="x-suf">&lt;div class="b" data-x="x-suf">&lt;/div></div>
                <div class="b c" data-x="pre-x">&lt;div class="b c" data-x="pre-x">&lt;/div></div>
                &lt;/div>
            </div>
            <div id="container2">
                &lt;div id="container2">
                <div class="c" data-x="x">&lt;div class="c" data-x="x">&lt;/div></div>
                &lt;/div>
            </div>
            &lt;/div>
        </div>
    </div>
    <div class="card quiz">
        <!-- TODO  문제에 focusing 되면 코드 영역 강조표시 -->
        <div>
             Selector 확인 : <input type="text" id="selector-test"/>
        </div>
        <div class="quiz-answer">
            <input type="hidden" value="#id1"/>
            <div class="quiz-area">1. id가 "id1"인 selector</div>
            <div class="answer-area"><input type="text" class="custom-answer"/></div>
            <div class="answer-check"></div>
        </div>
        <div class="quiz-answer">
            <div class="quiz-area">1. id가 "id1"인 selector</div>
            <div class="answer-area"><input type="text" class="custom-answer"/></div>
            <div class="answer-check"></div>
        </div>
    </div>
</div>
<script>
    function addClass(selector, className) {
        [].forEach.call(document.querySelectorAll('.selector-root *'), (ele) => { ele.classList.remove(className); });
        try {
            if(document.querySelectorAll(selector)) {
                [].forEach.call(document.querySelectorAll('.selector-root ' + selector), (ele) => { ele.classList.add(className); });
            }
        } catch(e) {

        }
    }
    // TODO : 크롬과 같은 selector 만들어보기
    document.getElementById('selector-test').addEventListener('keyup', (eve)=>{	addClass(eve.currentTarget.value, 'highlight'); });
</script>


```javascript
var $$ = $ || document.querySelectorAll.bind(document);     // select 하고자 하는 대상이 복수
var $ = $ || document.querySelector.bind(document);     // select 하고자 하는 대상이 단수

$('#id')    // id로 조회
$$('.class')    // class로 조회
$('#id')    // id 조회
```