---
title: "[LV.2] 소수 찾기 (코딩테스트 고득점 Kit - 완전탐색)"

categories:
  - Algorithm
tags:
  - [완전탐색, 소수, 재귀함수, 프로그래머스]

toc: true
toc_sticky: true
---


## [LV.2] 소수 찾기 (코딩테스트 고득점 Kit - 완전탐색)

[https://programmers.co.kr/learn/courses/30/lessons/42839](https://programmers.co.kr/learn/courses/30/lessons/42839){:target="_blank"}  


```javascript
function solution(numbers) {
    const arr = numbers.split('').map((e) => Number(e))
    const resultAll = new Set();
    let count = 0;
   
    // 소수 구하는 함수
    function isPrime(num){
        if(num <= 1) return false

        for(let i = 2; i <= Math.sqrt(num); i++){
            if(num % i === 0) return false
        }

        return true
    }
    
    // 모든 경우의 수의 조합을 구하는 함수
    function calculate(arr, number) {
        const results = [];

        if (number === 1) return arr.map((i) => {
            resultAll.add(i)
           return [i]
        }); 
        
        for (let i = 0; i < arr.length; i++) {
            const rest = [...arr.slice(0, i), ...arr.slice(i + 1)];
            const getResult = calculate(rest, number - 1);
            const resultNum = getResult.map((e) => {
                return [arr[i], ...e]
            });
            results.push(...resultNum);
        }

        return results;
    };
    
    // 모든 경우의 수의 조합을 Set 객체에 추가하기 (중복 값 처리)
    for(let i = 1; i <= arr.length; i++){
        const result = calculate(arr, i)
        
        for(let x = 0; x < result.length; x++){
            const num = Number(result[x].join(''));
            if(resultAll.has(num) === false){
                resultAll.add(num)
            }
        }
    }

    // Set 객체에 모인 모든 결과값들 중에 소수만 판별해서 카운팅하기
    resultAll.forEach((e) => {
        if(isPrime(e)) count++;
    })
    
    return count
}
```

## 풀이 과정

#### 소수 구하는 함수 만들기

>[소수(Prime number) 구하기](https://sunghwancode.github.io/algorithm/primenumber/){:target="_blank"}  


#### 모든 경우의 수의 조합을 구하는 함수 만들기

우선 어떤식으로 코드를 만들어야 할지 설명을 해보자면,  
```"123"``` 이라는 값이 주어졌을 때, 이 숫자들로 만들 수 있는 모든 조합을 찾아내고,   
각 숫자들 중 소수에 해당되는게 총 몇개인지 카운트를 해야한다.  

모든 경우의 수들을 나열해보면  

```
1  
2  
3  
 
1 2  
1 3  
2 1  
2 3   
3 1  
3 2  

1 2 3  
1 3 2  
2 1 3  
2 3 1  
3 1 2  
3 2 1  
```

이렇게 나오게 된다.  
이 숫자들을 구하기 위해서는 한 자리수, 두 자리수, 세 자리수 기준으로 모든 조합을 구해야하기 때문에  
쉽게 정리해보면 **한 자리수에서 모든 조합을 찾을 수 있는 함수를 만든 후,
for 반목문으로 각 자리수에 해당 함수를 동작**하게 만들면 된다.  

```javascript
// 모든 경우의 수의 조합을 구하는 함수
    function calculate(arr, number) {
        const results = [];

        if (number === 1) return arr.map((i) => {
            resultAll.add(i)
            return [i]
        }); 
        
        for (let i = 0; i < arr.length; i++) {
            const rest = [...arr.slice(0, i), ...arr.slice(i + 1)];
            const getResult = calculate(rest, number - 1);
            const resultNum = getResult.map((e) => {
                return [arr[i], ...e]
            });
            results.push(...resultNum);
        }

        return results;
    };
```

```calculate```함수에서는 매개변수로 arr(주어진 숫자들의 배열), number(자리수) 를 받는다.  

```javascript
// 모든 경우의 수의 조합을 Set 객체에 추가하기 (중복 값 처리)
    for(let i = 1; i <= arr.length; i++){
        const result = calculate(arr, i)
        
        for(let x = 0; x < result.length; x++){
            const num = Number(result[x].join(''));
            if(resultAll.has(num) === false){
                resultAll.add(num)
            }
        }
    }
```

이 코드에서 ```calculate(arr, i)```는 반복문을 통해 여러번(3번) 호출하게 되고,  
```calculate([1,2,3], 1)``` 이 함수는 1,2,3 의 숫자들로 1 자리수의 모든 조합을  
```calculate([1,2,3], 2)``` 이 함수는 1,2,3 의 숫자들로 2 자리수의 모든 조합을  
```calculate([1,2,3], 3)``` 이 함수는 1,2,3 의 숫자들로 3 자리수의 모든 조합을 찾아낼 것이다.  


예시로 아래에서 ```calculate([1,2,3], 3)``` 함수가 어떻게 동작하는지 알아보자.  

---

* 1-1-1.  

```calculate()``` 내부의 ```for (let i = 0; i < arr.length; i++)```문까지 진입한다.  
```i = 0``` 일 때 ```const rest``` 는 ```[...arr.slice(0, 0), ...arr.slice(0 + 1)]``` 라는 값을 받게 되고, 이는 ```[...[], ...[2,3]]``` -> ```[2,3]``` 과도 같다.  

```rest = [2,3]``` 이기 때문에 ```const getResult``` 는 재귀함수로 호출된 ```calculate([2,3], 3-1)``` 의 결과값을 받게될 것이다.

---

* 1-2-1. 

```calculate([2,3], 2)``` 내부에서 ```for (let i = 0; i < arr.length; i++)```문까지 진입한다.  
```i = 0``` 일 때 ```const rest``` 는 ```[...arr.slice(0, 0), ...arr.slice(0 + 1)]``` 라는 값을 받게 되고, 이는 ```[...[], ...[3]]``` -> ```[3]``` 과도 같다.

```rest = [3]``` 이기 때문에 ```const getResult``` 는 재귀함수로 호출된 ```calculate([3], 2-1)``` 의 결과값을 받게될 것이다.  

---

* 1-3-1.

```calculate([3], 1)``` 내부에서 ```for (let i = 0; i < arr.length; i++)```문까지 진입한다.  
여기서는 number 인수 값이 1이기 때문에
```javascript
if (number === 1) return arr.map((i) => {
    resultAll.add(i)
    return [i]
}); 
```
이 코드에 진입하게 되고, 결과적으로 ```resultAll```에 3을 추가하고 ```[3]```을 1-2-1으로 return 하고 함수를 종료하게 된다.

---

* 1-2-1. ( ```[3]``` return 받음 )

```const getResult``` 에 ```[3]```의 값을 받게 되었다.  

```javascript
const resultNum = getResult.map((e) => {
    return [arr[i], ...e]
});
results.push(...resultNum);
```

이 코드에 진입한 후에는 ```[arr[i], ...e]```의 값 -> ```[2, ...[3]]``` -> ```[2, 3]``` 이 map의 결과로 인해 ```[[2,3]]``` 으로 ```resultNum```에 들어가게 되고 ```results``` 배열에 ```[2,3]``` 형태로 추가된다.  

1-2-1의 반복문이 끝났기 때문에 이제 다음 반복문(```i = 1```)을 이어서 진행한다.  

---

* 1-2-2. 

```calculate([2,3], 2)``` 내부에서 ```for (let i = 0; i < arr.length; i++)```문까지 진입한다.  
```i = 1``` 일 때 ```const rest``` 는 ```[...arr.slice(0, 1), ...arr.slice(1 + 1)]``` 라는 값을 받게 되고, 이는 ```[...[2], ...[]]``` -> ```[2]``` 와도 같다.

```rest = [2]``` 이기 때문에 ```const getResult``` 는 재귀함수로 호출된 ```calculate([2], 2-1)``` 의 결과값을 받게될 것이다.  

---

* 1-3-2.

```calculate([2], 1)``` 내부에서 ```for (let i = 0; i < arr.length; i++)```문까지 진입한다.  
여기서는 number 인수 값이 1이기 때문에
```javascript
if (number === 1) return arr.map((i) => {
    resultAll.add(i)
    return [i]
}); 
```
이 코드에 진입하게 되고, 결과적으로 ```resultAll```에 2을 추가하고 ```[2]```을 1-2-2으로 return 하고 함수를 종료하게 된다.  

---

* 1-2-2. ( ```[2]``` return 받음 )

```const getResult``` 에 ```[2]```의 값을 받게 되었다.  

```javascript
const resultNum = getResult.map((e) => {
    return [arr[i], ...e]
});
results.push(...resultNum);
```

이 코드에 진입한 후에는 ```[arr[i], ...e]```의 값 -> ```[3, ...[2]]``` -> ```[3, 2]``` 이 map의 결과로 인해 ```[[3,2]]``` 으로 ```resultNum```에 들어가게 되고 ```results``` 배열에 ```[3,2]``` 형태로 추가된다.  

1-2의 모든 반복문이 끝났기 때문에 그동안 ```results``` 배열에 모아진 ```[[2,3],[3,2]]``` 값을 1-1로 return 한다.  

---

* 1-1-2. ( ```[[2,3],[3,2]]``` return 받음 )

```const getResult``` 에 ```[[2,3],[3,2]]```의 값을 받게 되었다.  

```javascript
const resultNum = getResult.map((e) => {
    return [arr[i], ...e]
});
results.push(...resultNum);
```

이 코드에 진입한 후에는 ```[arr[i], ...e]```의 값 -> ```[1, ...[2,3]]``` 와 ```[1, ...[3,2]]``` 가 map의 결과로 ```[[1,2,3], [1,3,2]]``` 으로 ```resultNum```에 들어가게 되고 ```results``` 배열에 ```[1,2,3]```, ```[1,3,2]``` 각각의 형태로 추가된다.  


이제 1의 과정이 모두 끝났으며, 나머지 2와 3번째의 반복문까지 완료하면 ```results``` 배열에는 ```[[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]``` 의 값들이 들어가있게 되고 이 값을 return 하게된다.  

---

* 마지막 ( ```[[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]``` return 받음 )


```javascript
// 모든 경우의 수의 조합을 Set 객체에 추가하기 (중복 값 처리)
    for(let i = 1; i <= arr.length; i++){
        const result = calculate(arr, i)
        
        for(let x = 0; x < result.length; x++){
            const num = Number(result[x].join(''));
            if(resultAll.has(num) === false){
                resultAll.add(num)
            }
        }
    }
```

앞서 얻어진 ```[[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]``` 의 결과값은  
```const result = calculate(arr, i)``` 코드에서 ```calculate([1,2,3], 3)``` 이었을 때 (3자리수로 만들어지는 모든 경우의 수의 조합) 얻어진 결과이다.  

이제 새로운 반복문에서 result의 값들을 차례대로 숫자 형태로 바꾼 후 ```resultAll``` 이라는 Set 객체에 추가하게된다.
```javascript
for(let x = 0; x < result.length; x++){
    const num = Number(result[x].join(''));
    if(resultAll.has(num) === false){
        resultAll.add(num)
    }
}
```

Set 객체는 중복된 값이 존재하지 않기 때문에  
```calculate([1,2,3], 1)```  -> ```[1,2,3]```
```calculate([1,2,3], 2)```  -> ```[12, 13, 21, 23, 31, 32]```
```calculate([1,2,3], 3)```  -> ```[123, 132, 213, 231, 312, 321]```

이 모든 과정에서 생기게 되는 중복값들(계산하다보면 몇 번 발생하게된다)을 따로 처리하지 않아도 ```resultAll``` 안에 고유한 값들만 모을 수 있게 된다.  

최종적으로 ```resultAll``` 안에는

```javascript
Set(15) {
  1,
  2,
  3,
  12,
  13,
  21,
  23,
  31,
  32,
  123,
  132,
  213,
  231,
  312,
  321,
}
```

총 15개의 경우의 수 결과값들이 모이게 되고,  

```javascript
// Set 객체에 모인 모든 결과값들 중에 소수만 판별해서 카운팅하기
resultAll.forEach((e) => {
    if(isPrime(e)) count++;
})

return count
```

이 값들을 하나하나씩 소수를 판별하는 함수에 대입해서 소수로 확인될 시 ```count```를 1씩 올리게되고,

```
2
3
13
23
31
```

총 5개의 소수들이 존재하기 때문에
최종적으로 ```count```값 5를 return 하고 마무리하게 된다.