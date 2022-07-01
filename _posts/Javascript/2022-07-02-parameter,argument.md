---
title: "매개변수/인자(Parameter) 와 인수(Argument)"

categories:
  - Javascript
tags:
  - [Javascript]

toc: true
toc_sticky: true
---

예시로 이런 함수가 있다고 하면,

```javascript
function calculate(x, y) {
	return x + y;
}

console.log(calculate(2,3));
```

함수 정의 시 나열되는 **변수**인 **x**와 **y**를 **매개변수** / **인자 (Parameter)** 라고 하고,  
함수 호출 시 전달되는 실제 **값**인 **2**와 **3**을 **인수 (Argument)** 라고 한다.  

---

~~... 라고 깔끔하게 정리하고 넘어가고 싶은데 영 찜찜하다. 왜냐하면 검색해보면 모두 다 설명하는게 다르기 때문이다.~~  
~~위에 내가 적어놓은 것 처럼 구분하는 사람도 있고, 매개변수(Parameter) / 인자(Argument) 로 구분한 사람도 있고, 다들 제각각이다.~~  
  
[모던 자바스크립트 Deep Dive] 202 페이지에 Parameter(매개변수=인자) / Argument(인수) 라고 언급되어있는 것을 확인했다. 다른 언어에서는 어떻게 사용되는지는 모르겠지만 일단 자바스크립트 사용할 때는 이렇게 알고 있으면 될 것 같다.