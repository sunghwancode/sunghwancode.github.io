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


... 라고 깔끔하게 정리하고 넘어가고 싶은데 영 찜찜하다. 왜냐하면 검색해보면 모두 다 설명하는게 다르기 때문이다.  
위에 내가 적어놓은 것 처럼 구분하는 사람도 있고, 매개변수(Parameter) / 인자(Argument) 로 구분한 사람도 있고, 다들 제각각이다.  
그래서 정확하게 정리해놓은 것을 찾으려고 애를 쓰고있던 도중.. 명쾌한 댓글 하나를 읽고 여기서 멈추기로 했다.  
  
> '지금은 너무 힘쓰지 말고, 우선 내가 이해하기 쉬운대로 받아들이고 나중에 더 정확하게 이해하도록 해요.'