---
title: "재귀함수(Recursive Function)"

categories:
  - Algorithm
tags:
  - [재귀함수]

toc: true
toc_sticky: true
---

### 재귀함수란?
<a href="https://ko.wikipedia.org/wiki/%EC%9E%AC%EA%B7%80" target="_blank">재귀(Recursion)</a> : 재귀는 어떠한 것을 정의할 때 자기 자신을 참조하는 것을 뜻한다  

```javascript
// 1 ~ num 까지의 곱을 구하기
function calculate(num){
  if(num === 1) return 1
  
  return num * calculate(num - 1)
}

calculate(4) // 24  <-  4 * 3 * 2 * 1 의 결과
```

재귀함수는 함수 안에서 자기 자신(함수)을 호출하는 함수를 말한다. 그렇기 때문에 한번 함수를 호출하게되면 내부적으로 해당 함수가 반복적으로 다시 동작하게되고, 정해진 조건에 만족하게되면 함수의 결과를 리턴하게 된다.  
while 반복문과 흡사하기 때문에 while로 구현한 코드는 재귀함수로도 구현할 수 있다.  
다만, 복잡한 조건이 걸려있는 경우 재귀함수로 구현하는게 효율적일 수 있기에 상황에 맞게 사용하면 될 것 같다.  

---

### 스택 오버플로우(Stack Overflow) 와 꼬리재귀(Tail Recursion)
재귀함수는 함수를 호출 할 때 마다 메모리의 스택(Stack)이라는 영역에 저장되는데, 함수를 호출하는 횟수가 많아져서 지정된 스택 영역을 초과하면 [Stack Overflow](https://ko.wikipedia.org/wiki/%EC%8A%A4%ED%83%9D_%EC%98%A4%EB%B2%84%ED%94%8C%EB%A1%9C) 라는 에러가 발생하게 된다. 그래서 이런 문제를 보완하고자 **꼬리재귀** 라는 기법이 나오게 되었다.

```javascript
// 일반 재귀 형식
function calculate(num){
  if(num === 1) return 1
  
  return num * calculate(num - 1)
}
calculate(4) // 24


//  꼬리 재귀 형식
function calculateTail(num, total = 1){
  if(num === 1) return total
  
  return calculateTail(num - 1, num * total)
}
calculateTail(4) // 24
```

일반 재귀 형식은 return 이후에 재귀 함수의 **연산**이 시작되는데,  
꼬리 재귀 형식은 return 하기 전에 재귀 함수를 **연산**하고 결과를 반환한다.
**연산**이 이루어지는 위치의 차이가 있다고 보면 될 것 같다.

참고 링크:  
[https://yeonjewon.tistory.com/80](https://yeonjewon.tistory.com/80){:target="_blank"}  
[https://bentist.tistory.com/57](https://bentist.tistory.com/57){:target="_blank"}  
[https://catsbi.oopy.io/dbcc8c79-4600-4655-b2e2-b76eb7309e60](https://catsbi.oopy.io/dbcc8c79-4600-4655-b2e2-b76eb7309e60){:target="_blank"}


---

### 재귀함수로 구할 수 있는 것

* #### 팩토리얼([factorial](https://ko.wikipedia.org/wiki/%EA%B3%84%EC%8A%B9){:target="_blank"}, 1에서 n까지의 곱)

```javascript
function calculate(num){
  if(num === 1) return 1
  
  return num * calculate(num - 1)
}

calculate(4) // 24
```

* #### 거듭제곱 구하기 (x^n)

```javascript
function calculate(x, num){
  if(num === 1) return x
  
  return x * calculate(x, num - 1)
}

calculate(2, 10) // 1024
```


* #### 2진수 구하기

```javascript
const 진법 = 2

function binary(number){
  let result = ""

  function calculate(num){
    if(num === 0) return ''
    calculate(Math.floor(num / 진법))
    result += num % 진법
  }
  
  calculate(number)
  
  return result
}

binary(20) // '10100'
```

* #### 최대공약수 구하기 - 유클리드 호제법(Euclid's Algorithm)

```javascript
function calculate(a, b){
  if(a < b){
    let tmp
    tmp = a;
    a = b;
    b = tmp;
  }
  
  if(a % b === 0) return b
  else return calculate(b, a%b)  
}

calculate(4, 15) // 1
calculate(4, 16) // 4
```
> 최소공배수는 두 수의 곱을 최대공약수로 나누면 구할 수 있다. ```a * b / caculate(a, b)```

참고 링크:  
[https://wayhome25.github.io/cs/2017/04/15/cs-16-1-recursion/](https://wayhome25.github.io/cs/2017/04/15/cs-16-1-recursion/){:target="_blank"}

