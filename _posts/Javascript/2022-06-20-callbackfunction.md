---
title: "콜백 함수 (Callback Function)"

categories:
  - Javascript
tags:
  - [Javascript]

toc: true
toc_sticky: true
---

### 콜백 함수의 구조

콜백 함수는 **다른 함수의 매개변수로 들어가는 함수** 를 말한다. 매개변수로 넘겨받은 함수는 일단 넘겨받고, 때가 되면 나중에 호출(called back)한다.  
특정 기능이 있는 함수를 콜백 함수라고 정의하는 것이 아니라 일반 함수를 콜백의 용도로 사용하는 것이다.  

```javascript
function plus(a, b, callback) {  // callback 은 콜백 함수가 들어갈 자리
  callback(a + b);
}

function result(number) {  // 콜백으로 사용될 함수
  console.log(number);
}

plus(10, 5, result); // 15
```

위에 코드에서는 ```plus()``` 라는 함수가 먼저 호출이 되고, 그 후에 ```plus()``` 안에서 ```result()``` 함수가 **나중에 호출**이 된다.  

---

### 콜백 함수를 쓰는 이유

콜백 함수는 콜백 함수만의 특별한 기능이 있다기 보다는 함수를 활용하는 하나의 방식이라고 이해하면 될 것 같다.  

* #### 코드의 재사용성

```javascript
function random(callback) {
    const number = Math.floor(Math.random() * 10)
    callback(number)
}


// 콜백 함수들
function 노래방점수(getNumber) {
    console.log(`당신의 점수는 ${getNumber*10}점 입니다!`);
}

function 오늘의운세(getNumber) {
    if(getNumber > 5){
        console.log('오늘은 운세가 좋으시군요!');
    } else {
        console.log('오늘 하루는 조심하셔야겠습니다...');
    }
}

random(노래방점수)
random(오늘의운세)
/*
당신의 점수는 80점 입니다!
오늘은 운세가 좋으시군요!

당신의 점수는 60점 입니다!
오늘 하루는 조심하셔야겠습니다...

당신의 점수는 30점 입니다!
오늘은 운세가 좋으시군요!
*/
```
이렇게 하면 ```노래방점수()``` 와 ```오늘의운세()``` 각각 함수 안에 숫자를 생성하는 ```Math.floor(Math.random() * 10)``` 기능을 넣지 않아도 된다.


* #### 코드를 원하는 순서로 순차적으로 진행

아침, 점심, 저녁 순서를 꼭 지켜서 식사해야 하는 스케줄을 짜야한다.  
아침식사 코드는 1초,  
점심식사 코드는 4초,  
저녁식사 코드는 2초 가 걸린다고 가정한다.  

```javascript
function 식사패턴A() {
    setTimeout(() => {
        console.log('07:00 아침식사');
    }, 1000);
  setTimeout(() => {
        console.log('13:00 점심식사');
    }, 4000);
  setTimeout(() => {
        console.log('20:00 저녁식사');
    }, 2000);
}

식사패턴A(); // 결과 순서 => '07:00 아침식사', '20:00 저녁식사', '13:00 점심식사'

//-------------

function 식사패턴B() {
    setTimeout(() => {
        console.log('07:00 아침식사');
        setTimeout(() => {
            console.log('13:00 점심식사');
            setTimeout(() => {
                console.log('20:00 저녁식사');
            }, 2000);
        }, 4000);
    }, 1000);
}

식사패턴B(); // 결과 순서 => '07:00 아침식사', '13:00 점심식사', '20:00 저녁식사'
```


```식사패턴A()``` 의 경우에는 코드 자체는 순차적으로 작성되어 있지만, 실제로 각 동작들이 소요되는 시간이 달라서 완료되는 순서에 문제가 생긴다.  
(아침 -> 저녁 -> 점심)  

```식사패턴B()``` 의 경우에는 각 동작들이 소요되는 시간이 다르더라도 순차적으로 진행이된다.  
(아침 -> 점심 -> 저녁)  

다만 식사패턴B() 와 같이 작성을 할 경우에는 코드의 가독성이 떨어지고 유지보수가 어려워지는 [**콜백지옥**](https://librewiki.net/wiki/%EC%BD%9C%EB%B0%B1_%EC%A7%80%EC%98%A5){:target="_blank"}이라는 현상이 나타나게 된다. 이 부분은 Javascript ES6의 **Promise** 와 **async/await** 문법의 도움을 받아 해결할 수 있다.  



참고 링크:  
[https://www.youtube.com/watch?v=-iZlNnTGotk](https://www.youtube.com/watch?v=-iZlNnTGotk){:target="_blank"}