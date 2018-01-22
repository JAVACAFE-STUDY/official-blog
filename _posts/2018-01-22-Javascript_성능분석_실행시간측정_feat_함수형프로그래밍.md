---
title: 'Javascript 성능분석 - 실행시간측정 feat. 함수형프로그래밍'
date: 2018-01-22 12:00:00
categories:
- javascript
tags:
- javascript
- javascript닌자
- 성능분석
- 함수형프로그래밍
author : 이영범
---

### Javascript 코드의 성능(실행시간)을 측정하는 함수를 만들고 이를 점차 함수형 프로그래밍으로 추상화 혹은 조합하여 기능을 확장시키는 과정을 정리하여 공유합니다.<br>

알고리즘을 공부하다보니 코드의 성능(실행시간)을 측정하는 일이 필요해 졌습니다.<br>
요즘 자바스크립트를 공부해보는 중이라, 처음에는 테스트 라이브러리를 사용해서 측정하려고 했는데 뭔가 제 입맛에 딱 맞고 이거다 싶은 라이브러리가 없었습니다.<br>
좀 더 가볍고, 내 목적에 맞게 이것 저것 계속 기능수정도 하고 싶었던터라 그냥 만들어서 써보기로 결정하고 기존에 Javascript Ninja에서 봤던 코드를 가져왔습니다.

## Javascript Ninja의 성능측정 코드
```javascript
start = new Date().getTime();
for (var n=0; n<maxCount; n++) {
    /* 측정할 연산을 수행한다. */
}
elapsed = new Date().getTime() - start;
assert(true, "소요된 시간: " + elapsed);
```

코드는 매우 심플합니다.<br>
먼저 코드의 시작부분에 현재시간(1970년 1월 1일 이후에 경과한 millisecond)을 start에 저장합니다.<br> 
그리고 코드 실행이 끝난 후에 현재시간을 elapsed에 저장하고, elapsed - start하여 소요시간을 구합니다.<br>
그런데 해당 코드는 문제가 있습니다. 위의 코드는 성능측정을 위한 코드를 for문 안에 직접 기술해야 되는, 다시 말해 확장성이 전혀 없는 코드라 재사용이 불가능합니다.

## 리팩토링 : 재사용 가능한 함수로 만들기

그렇다면 위의 코드를 재사용 할 수 있게 바꾸려면 어떻게 해야 할까요?<br>
먼저 해당 코드는 실행환경이 브라우져든, 노드든 컴파일 되자마자 즉시 실행되고 끝나버리기 때문에 재사용할 수 없습니다.<br>
재사용을 위해 처리의 지연이 필요하죠. 그럴 수 있는 가장 간단한 방법은 함수로 만드는 것입니다.

```javascript
function timer(func, maxCount) {
    var start = new Date().getTime();
    maxCount = maxCount? maxCount: 1;
    for (var n = 0; n < maxCount; n++) {
        /* 측정할 연산을 수행한다. -> 함수로 바꿉니다 */
        func();
    }
    var elapsed = new Date().getTime() - start;
    return elapsed;
}
```

간단한 추상화를 통해 문제가 해결된 것 같습니다.<br>
하지만 위의 코드에는 한 가지 문제가 있습니다. 찾으셨나요?<br>
<br>
측정할 연산을 func로 추상화하는 것까지는 좋습니다. 하지만 막상 어떻게 실행시킬지 감이 오시나요?<br>
만약에 간단히 덧셈을 해주는 함수 (a,b)=>a+b가 있고 테스트로 1, 2를 인자로 넘기고 싶다면 해당 코드에서 timer를 실행할때 넘겨주는 func는 어떤 형태여야 할까요?<br>
힌트를 드리자면 넘겨주는 func는 실행이 끝난 함수가 아니라 실행했을때 다음번에 호출했을때 바로 실행할 수 있게 미리 준비해주는 '함수를 리턴하는 함수'여야 합니다.<br>
단 5분이라도 직접 고민해 보시고 아래의 내용으로 진행하시기 바랍니다.

```javascript
function lazyExec(func, arr) {
    return ()=>func.apply(null, arr);
}
```

위의 문제를 해결하기 위해서는 실행시킬 함수(func)와 인자(arr)를 받아서 실행시킬 준비만 하는, 즉, 함수를 리턴하는 함수인 lazyExec와 같은 형태의 함수가 필요합니다.<br>
lazyExec는 아래처럼 사용이 가능합니다.
```javascript
var f = lazyExec((a,b)=>a+b, [1,2]);    // 실행하면 함수와 인자가 바인딩된 실행가능한 함수가 리턴
f(); // 실제 함수 실행 : 3
```

이제 문제는 해결됐습니다.

```javascript
var loopCount = 10000000,
    timeCost = timer(lazyExec((a,b)=>a+b, [1,2]), loopCount);
console.log('덧셈 함수 ' + loopCount + '회 실행에 소요된 시간 : ' + timeCost + ' ms');
// 덧셈 함수 10000000회 실행에 소요된 시간 : 190 ms
```

## 한 번 더 추상화

여기서 그만둬야 됐는데... 문득 그러면 테스트의 정확도를 높이기 위해 10000000번 실행한 전체를 10번, 100번, ... 실행하고 싶으면 어떻게 하지? 하는 의문이 들었습니다.<br>
여기서 반복문 속에서 실행되는 영역은 timer(lazyExec((a,b)=>a+b, [1,2]), loopCount) 부분이니까, 이 부분을 한 번 더 처리지연 시키면 되겠지 싶은 생각이 들었습니다.<br>
아래와 같이 말이죠

```javascript
var lazyTimeCost = lazyExec(timer, [lazyExec((a,b)=>a+b, [1,2]), loopCount]);
// 실행
for(var i=0; i<10; i++) {
    timeCost = lazyTimeCost();
    console.log('덧셈 함수 ' + loopCount + '회 실행에 소요된 시간 : ' + timeCost + ' ms');
}
// 덧셈 함수 10000000회 실행에 소요된 시간 : 183 ms
// 덧셈 함수 10000000회 실행에 소요된 시간 : 177 ms
// 덧셈 함수 10000000회 실행에 소요된 시간 : 175 ms
// ...
```

함수를 리턴하는 추상화된 함수인 lazyExec 함수를 하나 만들어서 계속 재사용할 수 있다는게 놀랍지 않나요?<br>
지금까지 기존 함수를 바꾼 것은 하나도 없습니다. 함수의 조합으로 계속 기능을 확장시키고 있을 뿐이죠.
여기서는 멈췄어야 되는데 인간의 욕심은 끝이 없는지라... 다시 또 재사용 가능한 함수로 만들고 싶어졌습니다.

```javascript
// timer를 장착한 함수와 반복횟수를 받는 함수를 받아야 하는 규약이 생깁니다.
function getLoopTestTime(setTimerFunc, count) { 
    // 이제 결과값이 여러개가 오기 때문에 결과값을 담은 배열을 리턴해 줍니다
    var execTimes = [], timeCost;   
    for(var i=0; i<count; i++) {
        timeCost = setTimerFunc();  
        execTimes.push(timeCost);
    }
    return execTimes;
}

// 실제 실행되는 함수인 timer를 실행시킬 함수로 고정시켜주는 함수를 만듭니다
function setTimerFunc(func, params, loopCount) {
    return lazyExec(timer, [lazyExec(func, params), loopCount]);
}

var setTestFunc = setTimerFunc((a,b)=>a+b, [1,2], loopCount);
var loopLoopCount = 10;
var loopTestTime = getLoopTestTime(setTestFunc, loopLoopCount);
console.log('덧셈함수를 ' + loopCount + '번 실행을 ' + loopLoopCount + '번 수행한 시간 : ' + loopTestTime);
// 덧셈함수를 10000000번 실행을 10번 수행한 시간 : 203,173,179,206,187,174,196,169,179,181
```

드디어 완성되었습니다. 그런데...

## 사용자 편의성을 위한 은닉과 조합

만들다 보니 한 가지 의문이 들었습니다.<br>
이 함수 만드느라 애는 썼는데 이걸 사용하는 사람한테 너무 어렵지 않을까?<br>
내일의 나는 쓸 수 있을 것 같은데 일주일 후의, 한 달 후의 나는 저 함수를 다시 쓸 수 있을까?<br>
지금은 저 함수의 코드와 과정이 다 눈에 보이지만 저걸 볼 수 없는 사람(마지막 실행부만 볼 수 있는 사람)은 저 함수를 어떻게 쓸 수 있을까?<br>
<br>
그렇습니다 저 함수는 규약이 너무나 많습니다.<br>
getLoopTestTime을 쓰기 위해서는 setTimerFunc로 만든 lazyExec(timer, [lazyExec(func, params), loopCount]) 형태가 와야되는...
당장 내일의 저도 사용하기 어려운 함수가 되어버린거죠<br>

자, 이제는 기존 함수들의 조합을 통해 사용자가 쓰기 편한 함수로 바꿔보겠습니다.<br>
그 과정에서 사용자가 몰라도 되는 알 필요가 없는 부분은 숨기고, 사용자가 알아야 되는 어떤 함수를 테스트할건지, 그때 파라미터로 뭐를 넘길지, 몇 번 반복할건지, 그 전체를 다시 몇 번 반복시킬지 여부만 입력하면 되도록 함수를 새로 만들어 보겠습니다.

```javascript
function getLoopLoopTestTime(func, params, loopCount, loopCountAll) {
    return getLoopTestTime(setTimerFunc(func, params, loopCount), loopCountAll);
}

var loopLoopTestTime = getLoopLoopTestTime((a,b)=>a+b, [1,2], 10000000, 10);
console.log('덧셈함수를 ' + loopCount + '번 실행을 ' + loopLoopCount + '번 수행한 시간 : ' + loopLoopTestTime);
// 덧셈함수를 10000000번 실행을 10번 수행한 시간 : 181,204,176,196,167,184,175,173,176,178
```

getLoopLoopTestTime((a,b)=>a+b, [1,2], 10000000, 10)에서 보듯이 이제는 (테스트 할 함수, 함수에 넘길 파라미터, 함수반복횟수, 함수반복횟수만큼 다시 반복횟수)만 알아도 사용이 가능해졌습니다.<br>
이제 사용자는 복잡한 규약을 몰라도 사용할 수 있는 범용 함수가 되었습니다. 끝~! :)<br>


## 부록 : 분석함수

값이 많아질 경우에 평균, 최소, 최대값 정도는 보고 싶어졌습니다
```javascript
function testTimeReport(costTimes) {
    var copied = costTimes.slice();     // 값만 복사해서 원본의 훼손을 막습니다
    return {
        'average': copied.reduce((a,b)=>(a+b)/2),
        'min': copied.sort((a,b)=>a>b)[0],
        'max': copied[copied.length-1],
        'originalData': costTimes
    }
}

console.log(testTimeReport(loopLoopTestTime));
// {average: 176.892578125, min: 167, max: 204, originalData: Array(10)}
```

## TODO

아래 목록은 만들면서 느낀 추후 과제들입니다.<br>
- getLoopLoopTestTime처럼 getLoopTestTime도 사용하기 편하게 리팩토링 해보기<br>
- 위의 코드는 함수간의 의존관계가 강하다. 그런 의존관계를 검증할 수 있는 validation을 만들거나 의존관계를 강제할 수 있는 클래스, 생성자함수, 더블바인딩기법 등을 통해 타입을 강제할 수 있는 방법을 찾아보기<br>
- 위의 과정에 기존의 코드를 고치지 않고 선행조건, 후행조건을 사용해서 검증해보기<br>
- 범용적으로 쓸 수 있는 타입체크 함수 만들어보기<br>
/*
    validate(func, [[valid1func, msg1], [valid2func, msg2], ...])
 */


# 참고자료
- 자바스크립트 Ninja - 인사이트 - 존 레식, 베어 바이볼트
- 함수형 자바 스크립트 - 한빛미디어 - 마이클 포거스

