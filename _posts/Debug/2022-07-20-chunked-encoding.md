---
title: "Transfer-Encoding: chunked"

categories:
  - Debug
tags:
  - [HTTP]

toc: true
toc_sticky: true
---

# chunked 인코딩과의 만남

개인 프로젝트 작업 중에 GET 요청으로 robots.txt 를 읽어오게 한 적이 있었는데, UTF-8로 인코딩된 녀석들은 텍스트가 정상적으로 (예시)```User-agent``` 이라고 표시되었지만 어떤 녀석은 ```\x00U\x00s\x00e\x00r\x00-\x00a\x00g\x00e\x00n\x00t``` 이렇게 알 수 없는 코드로 표시됐었다. 처음에는 흔히 알고 있는 다른 방식의 인코딩(UTF-16 이라던지..)인가 싶어서 인코딩 변환 사이트에 돌아다니면서 디코딩을 시도해 봤는데 실패했었다. 도대체 이 녀석의 존재는 무엇인가 싶어서 GET 요청의 응답으로 온 데이터의 header를 살펴보니 차이점이 있었다.  

정상적으로 표시되는 녀석의 header 안에는  
```
'content-type': 'text/plain; charset=utf-8',
'content-length': '201',
```
이렇게 UTF-8로 인코딩 되었음이 표시되어있지만,  


문제의 녀석은 아래와 같다.  
```
'content-type': 'text/plain',
'transfer-encoding': 'chunked',
```

도대체 chunked는 무엇인가?  


# Transfer-Encoding: chunked 란?

[https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Transfer-Encoding](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Transfer-Encoding){:target='_blank'}  
[https://en.wikipedia.org/wiki/Chunked_transfer_encoding](https://en.wikipedia.org/wiki/Chunked_transfer_encoding){:target='_blank'}  

이해하는데에 도움되는 쉬운 글  
[https://withbundo.blogspot.com/2017/08/http-20-http-ii-transfer-encoding.html](https://withbundo.blogspot.com/2017/08/http-20-http-ii-transfer-encoding.html){:target='_blank'}  
[https://goyunji.tistory.com/8](https://goyunji.tistory.com/8){:target='_blank'}  

그 외  
[https://feel5ny.github.io/2020/01/05/HTTP_015_01/](https://feel5ny.github.io/2020/01/05/HTTP_015_01/){:target='_blank'}  
[https://eminentstar.tistory.com/48](https://eminentstar.tistory.com/48){:target='_blank'}  
[https://b.pungjoo.com/entry/Transfer-Encoding-chunked-VS-Content-Length](https://b.pungjoo.com/entry/Transfer-Encoding-chunked-VS-Content-Length){:target='_blank'}  

내가 이해한 바로는 데이터를 전송할 때 여러 개의 덩어리(Chunk)로 나누어서 전송하는 방식을 말하는 것 같다.
예를 들어 어떤 가사의 내용을 전달한다고 하면, (빛과 소금 - 내 곁에서 떠나가지 말아요)  
```
'제발 내 곁에서 떠나가'

'지 말아요 
그대 없는 밤은 너무 싫'

'어 
돌이킬 수 없'

'는 그대 마음 
이제 와서 다'

'시 어쩌려나 
슬픈 마음도 이젠 소용없네'
```
이렇게 나누어서 전송하는 것 같다.


# 어이없는 나름의 해결책...
그런데 문제는 저것을 어떻게 디코딩 해야 하는 것이냔 말이다! 아무리 찾아봐도 디코딩 하는 방법을 알 수가 없었다.  

그러다가 [https://www.reddit.com/r/Python/comments/28fr54/what_type_of_string_encoding_is/](https://www.reddit.com/r/Python/comments/28fr54/what_type_of_string_encoding_is/){:target='_blank'} 에서 보다가 갑자기 깨달은 사실..  

위에서 눈치챈 사람이 있었는지는 모르지만 ```User-agent``` 부분에서 ```\x00U\x00s\x00e\x00r\x00-\x00a\x00g\x00e\x00n\x00t``` 이렇게 변환되어 나온다고 했었는데... 자세히 보면 ```\x00``` 부분만 모두 제거해 보면 ```User-agent``` 가 된다.. 그냥 \x00 만 모두 삭제해주면 되는 거였다.

그리고 결과적으로는 다른 작업 없이 response 응답 객체안에 data값을 직접 불러오면 콘솔로그와는 다르게 알아서 UTF-8로 변환해서(?) 읽어올 수 있는 것 같다.
