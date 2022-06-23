---
title: "소수(Prime number) 구하기"

categories:
  - Algorithm
tags:
  - [소수]

toc: true
toc_sticky: true
---

## 소수(Prime number) 란?

2, 3, 5, 7, 11, 13... 처럼 **1과 자기 자신만을 약수로 가지는 수**를 말한다.
* 음수는 소수가 아니다
* 1은 소수가 아니다
* 소수는 2보다 크거나 같다

즉, n이 소수가 되려면 **2보다 크거나 같고, 자신 이외의 자연수로는 나눌 수 없어야 한다**.


## 소수 찾는법

* #### 주어진 숫자 n까지 직접 계산하기

```javascript
function isPrime(num) {
  if(num <= 1) return false
  
  for(let i = 2; i < num; i++){
    if(num % i === 0) return false
  }
  
  return true
}

isPrime(17) // true
```

소수는 2이상부터 해당되기 때문에 2에서 num 바로 전 까지의 수들 중에, 나머지 연산자(%)로 나누었을 때 정확하게 떨어지는 숫자가 확인되지 않았을 경우에만 true를 리턴한다.


* #### 주어진 숫자 n의 절반 까지만 계산하기

```javascript
function isPrime(num) {
  if(num <= 1) return false
  
  for(let i = 2; i <= num/2; i++){
    if(num % i === 0) return false
  }
  
  return true
}

isPrime(17) // true
```

맨 처음의 방법과 달라진 부분은 num을 절반으로 나누었다는 것과, 등호를 ```<=``` 으로 바꾼 것.  
계산 과정을 반절로 줄임으로써 조금 더 빠른 결과를 기대할 수 있다.  
(등호를 바꾼 이유는 num = 4 를 기준으로 계산해보면 ```i < num/2``` 일 때는 2를 포함하지 않게되서 잘못된 결과를 얻게된다.)

* ### 제곱근(Square root) 사용하기

>[Math.sqrt()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/sqrt){:target="_blank"} 함수는 숫자의 제곱근을 반환합니다.

```javascript
function isPrime(num) {
  if(num <= 1) return false

  for(let i = 2; i <= Math.sqrt(num); i++){
    if(num % i === 0) return false
  }
  
  return true
}

isPrime(17) // true
```

제곱근보다 작은 수에서 나눠지는 수가 안나온다면 제곱근보다 큰 수에서도 나눠지는 수가 나올 수 없기 때문에, 굳이 제곱근보다 큰 수까지 반복문을 돌릴 필요가 없어서 두번째 방법보다 더 적은 과정으로 결과를 얻을 수 있다.  

```isPrime(16)``` 의 Math.sqrt(num) 값은 4  
```isPrime(17)``` 의 Math.sqrt(num) 값은 4.123105625617661  
```isPrime(20)``` 의 Math.sqrt(num) 값은 4.898979485566356  
이므로, 16~20 까지는 동일하게 4 이내에서 소수를 판별할 수 있게된다.  
(계산해보니 28까지는 4 이내에서 소수인지 판별된다)




* #### 응용 - '[에라토스테네스의 체](https://ko.wikipedia.org/wiki/%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98_%EC%B2%B4){:target="_blank"}' 구현하기

<center>
<img src="https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif">
</center>

'에라토스테네스의 체' 의 원리는 **자기 자신(n)을 제외한 2부터 n까지의 배수를 제거하다 보면 소수만 남는다**는 원리이다. 이 또한 n의 제곱근 까지만 계산을 해도 결과는 똑같이 얻을 수 있다.  

위의 예시 이미지를 설명하자면,  
n 은 120이고, 120의 제곱근은 10.954... 이다.  
맨 처음에 **2는 소수이기 때문에** 먼저 소수로 체크한 후 아래의 표에 따라 진행하게된다.

| 색상 | 동작 |
|:-:|:-|
| **빨간색** | 2를 제외한 2의 배수들을 제거 (4, 6, 8, 10, 12...) |
| **초록색** | 3의 배수들을 제거 (3, 9, 12, 15, 18...) |
| | 4의 배수들은 2의 배수에서 이미 모두 제거됨 |
| **파란색** | 5의 배수들을 제거 |
| | 6의 배수들은 2와 3의 배수에서 이미 모두 제거됨 |
| **노란색** | 7의 배수들을 제거 |
| | 8의 배수들은 2의 배수에서 이미 모두 제거됨 |
| | 9의 배수들은 3의 배수에서 이미 모두 제거됨 |
| | 10의 배수들은 2와 5의 배수에서 이미 모두 제거됨 |
| * **보라색** | **남아있는 모든 수들은 소수에 해당됨** |

```javascript
function calculate(n){
  // n + 1 만큼의 배열 생성 (index의 최대값이 n이 포함될 수 있게)
  let arr = Array(n + 1).fill(true)

  // 소수는 2이상부터 존재하기 때문에 0, 1은 제외
  arr[0] = false 
  arr[1] = false
  
  for(let i = 2; i <= Math.sqrt(n); i++){
    if(arr[i]){ // 제거 되지(false 처리 되지)않은 숫자만 진행
      for(let x = i+i; x <= n; x+=i){
        // x = i+i 인 이유는 x = i 이면 배수의 기준이 되는 숫자까지 포함되서 제거가 되어버린다.
        // 그래서 애초에 기준 숫자의 다음 배수부터 확인하도록 해야한다.
        arr[x] = false;
      }
    }
  }
  
  return arr
}

const result = calculate(120);

const 소수_개수 = result.filter(e => e).length;
console.log(소수_개수) // 30

const 소수_목록 = result.map((v, i) => (v) ? i : 0).filter(e => e);
console.log(소수_목록)
/*
[ 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59,
61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113 ]
*/
```
 