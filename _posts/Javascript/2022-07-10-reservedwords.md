---
title: "Javascript 변수명 작성 규칙 및 예약어"

categories:
  - Javascript
tags:
  - [예약어]

toc: true
toc_sticky: true
---

# 변수명 기본 규칙

### 불가능

* 첫 번째 문자에는 숫자 사용 불가
```javascript
let 2apple;  // SyntaxError: Invalid or unexpected token
```

* 하이픈( - ) 사용 불가
```javascript
let apple-juice;  // SyntaxError: Unexpected token '-'
```

### 가능
* 언더바( _ ) 와 달러( $ ) 사용 가능(첫 번째 문자로도 사용 가능)
```javascript
let _apple_juice;
let $apple$juice;
```

---

# 예약어(Reserved Words)
예약어란 컴퓨터 프로그래밍 언어에서 이미 문법적인 용도로 사용되고 있기 때문에 식별자로 사용할 수 없는 단어들이다.
아래와 같이 자바스크립트에서 몇몇 키워드들은 현재 특수한 목적으로 사용중이거나 다음 버전에서 사용될 가능성이 있기 때문에 이러한 키워드들은 변수명이나 함수명으로 사용하는 것을 피해야한다.


|abstract|arguments|await|boolean|
|break|byte|case|catch|
|char|class|const|continue|
|debugger|default|delete|do|
|double|else|enum|eval|
|export|extends|false|final|
|finally|float|for|function|
|goto|if|implements|import|
|in|instanceof|int|interface|
|let|long|native|new|
|null|package|private|protected|
|public|return|short|static|
|super|switch|synchronized|this|
|throw|throws|transient|true|
|try|typeof|var|void|
|volatile|while|with|yield|

[https://www.w3schools.com/js/js_reserved.asp](https://www.w3schools.com/js/js_reserved.asp){:target='_blank'}

```javascript
let break;  // SyntaxError: Unexpected token 'break'
let class; // SyntaxError: Unexpected token 'class'
```

---

# 네이밍 기법

* Camel Case ```camelCase```  
카멜 케이스는 첫번째 단어는 소문자로 시작하고 두번째 단어부터는 대문자로 시작하는 방식을 말한다.  
**변수나 함수의 이름에서 사용한다**
```javascript
let myName;
function getStudentsName(){}
```

* Pascal Case ```PascalCase```  
파스칼 케이스는 각 단어의 첫번째 글자를 대문자로 적는 방식이며, 대문자로 시작하는 Camel Case 라고 봐도 될 것 같다.  
**생성자와 클래스명에 사용한다**
```javascript
class StudentData {
    constructor(age, name, address) {
        this._age = age;
        this._name = name;
        this._address = address;
    }
}
```
멤버 변수는 지역 변수와 구분하기 위해 ```_age``` 나 ```_name``` 처럼 변수명 앞에 언더바( _ )를 붙여줄 때도 있다고 한다.
>**클래스 필드(class field)**
>
>클래스 내부의 캡슐화된 변수를 말한다. 데이터 멤버 또는 **멤버 변수**라고도 부른다. 클래스 필드는 인스턴스의 프로퍼티 또는 정적 프로퍼티가 될 수 있다. 쉽게 말해, 자바스크립트의 생성자 함수에서 this에 추가한 프로퍼티를 클래스 기반 객체지향 언어에서는 클래스 필드라고 부른다.
>
>[https://poiemaweb.com/es6-class](https://poiemaweb.com/es6-class){:target='_blank'}
>
>---
>
>"protected 프로퍼티 명 앞엔 밑줄 _이 붙습니다.  
>자바스크립트에서 강제한 사항은 아니지만, 밑줄은 프로그래머들 사이에서 외부 접근이 불가능한 프로퍼티나 메서드를 나타낼 때 씁니다."
>
>[https://ko.javascript.info/private-protected-properties-methods](https://ko.javascript.info/private-protected-properties-methods){:target='_blank'}


* Snake Case ```snake_case``` ```SNAKE_CASE```  
스네이크 케이스는 각 단어들을 언더바( _ )로 이어주는 방식을 말한다.  
**상수명(const)은 대문자 스네이크 케이스 형식으로 작성한다.**
```javascript
const COLOR_GREEN = "#008000";
```

* Kebab Case ```kebab-case```  
케밥 케이스는 각 단어들을 하이픈( - )으로 이어주는 방식을 말한다.  
하지만 Javascript에서는 하이픈을 사용할 수 없기 때문에 변수명으로 사용할 수 없는 기법이다.  
다만 객체 Property의 이름(key)으로 사용할 수 있는데, 이때에는 대괄호의 형태로만 불러올 수 있다.  
```javascript
let obj = {
    'apple-color': 'red'
}
console.log(obj["apple-color"]) // 'red'
console.log(obj.apple-color) // ReferenceError: color is not defined
```

---

이와 별개로 함수의 매개변수가 중요하지 않을 때 관습적으로 언더바를 쓰는 경우가 있다고 하니 참고해야할 것 같다.  
(이 경우에는 lodash 라이브러리에서도 언더바를 차용하고 있으니 주의해야한다.)
```javascript
const fruits = ['apple', 'orange', 'grape', 'melon']

//forEach 메소드는 currentValue, index, array 3개의 매개변수가 있는데,
//만약 이때 currentValue를 사용하지 않을 상황이라면 아래와 같이 표현할 수 있다.
fruits.forEach( (_, index, array) => {
    console.log(`index: ${index} // array: [${array}]`);
})
```
[https://stackoverflow.com/questions/27637013/what-is-the-meaning-of-an-underscore-in-javascript-function-parameter](https://stackoverflow.com/questions/27637013/what-is-the-meaning-of-an-underscore-in-javascript-function-parameter){:target='_blank'}