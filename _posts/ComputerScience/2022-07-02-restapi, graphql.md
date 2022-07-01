---
title: "REST API 와 GraphQL"

categories:
  - ComputerScience
tags:
  - [API, REST API, GraphQL]

toc: true
toc_sticky: true
---

## API 란?

**API**는 **Application Programing Interface**의 약자로,  
"컴퓨터나 컴퓨터 프로그램 사이의 연결이다. 일종의 소프트웨어 인터페이스이며 다른 종류의 소프트웨어에 서비스를 제공한다." 라고 [위키백과](https://ko.wikipedia.org/wiki/API){:target='_blank'} 나오지만... 이렇게 봐서는 이해하기가 어려울 것 같다. (나도..)  
정확하지는 않지만 아주 쉽게 이해하도록 설명을 해보자면, 어떤 프로그램이 다른 프로그램과 데이터를 주고 받을 수 있게 도와주는 매개체를 말한다.  

예를들어 내가 어떤 게임 사이트에서 랭킹 페이지를 본다고 해보자. 그 사이트 메뉴들 중에 '랭킹' 이라는 페이지의 버튼이 있을 것이다.
이 버튼을 누르면 내부에서는 "게임 사이트의 서버에게 [**랭킹 목록 보여주기** API]를 요청합니다" 라는 코드가 동작하고,  
게임 사이트의 서버는 해당 API **요청**이 들어오면 랭킹 데이터들을 **응답** 한다(보내준다). 서버에게 랭킹 데이터들을 받게되면 내 화면에서는 그 데이터들로 랭킹 화면이 표시되게 된다. 이처럼 항상 **요청**과 **응답**이 존재하고 상호작용을 하게된다.  

어떤 방식으로 요청을 해야 원하는 데이터를 받을 수 있는지 각 API 마다 규격과 규칙이 있기 때문에, 기상청 API 같은 공공 API와 네이버지도, Kakao맵 등 공개된 API들은 이를 API Docs(Documents) 로 문서/메뉴얼로 공개해서 사용자들이 잘 사용할 수 있게 한다.



## REST API란? (Representational State Transfer)

2000년도 로이 필딩 (Roy Fielding) 의 박사학위 논문에서 최초로 소개된 '웹에서 데이터를 전송하고 처리하는 설계방식' 을 말한다. 대표적인 2가지 성격을 보면

1. URI만 보고도 의미하는 바를 알 수 있게 주소를 작성하기
"https://school.com/class/02/student/24" 만 보고도 대략적으로 "어떤 학교 홈페이지에 2반 24번 학생에 대한 페이지를 보여주는 것 같군!"이라는 생각이 들게끔 한눈에 이해할 수 있게 작성해야 한다. 그리고 단어의 선택도 add, find, delete 처럼 동사가 아니라 명사만 사용해야 한다.

2. 데이터에 대한 처리는 HTTP Method(GET, POST, PUT, DELETE)로 표현하기  
Create: 데이터 생성 (POST)  
Read: 데이터 조회 (GET)  
Update: 데이터 수정 (PUT)  
Delete: 데이터 삭제 (DELETE)  

이런 형식이다. 위에 1, 2번을 합쳐서 정리해보면 "어떤 학교 홈페이지에서 2반 24번 학생의 정보를 추가"하는 페이지가 있다고 할 때, 이 페이지는  
"https://school.com/addData/class/02/student/24" 이런식으로 만들기 보다는,  
[요청은: Create] [페이지는: "https://school.com/class/02/student/24"] 이런식으로 구성을 만들자는 것이다.  
이렇게 REST의 규칙을 잘 지키며 만든 구조를 "RESTful 하다"라고 표현하고 이를 REST API 라고 부른다.

도움되는 글:  
[https://www.ibm.com/kr-ko/cloud/learn/rest-apis](https://www.ibm.com/kr-ko/cloud/learn/rest-apis){:target='_blank'}  
[https://www.youtube.com/watch?v=4DxHX95Lq2U](https://www.youtube.com/watch?v=4DxHX95Lq2U){:target='_blank'}



## GraphQL 이란? (Graph Query Language)

GraphQL 은 페이스북이 개발한 쿼리언어이다. 쿼리문의 작성이 직관적이며 API를 호출할 때 쿼리문을 원하는 형식으로 작성하여 원하는 데이터만 응답받도록 할 수 있다.  
GraphQL 구조에는 크게 Query 와 Mutation 두가지가 있는데, Query 는 데이터를 읽는(Read)데에 사용하고, Mutation 은 데이터를 생성/수정/삭제(Creat/Update/Delete) 하는데에 사용된다.

### Apollo Server?

Apollo Server는 GraphQL 기반의 서버를 만들 수 있는 라이브러리로 프론트와 백엔드에서 모두 사용할 수 있다.  
[GraphQL Playground](https://www.apollographql.com/docs/apollo-server/v2/testing/graphql-playground/){:target='_blank'} 를 사용하면 Apollo Server 내의 API들을 메뉴얼처럼 확인할 수 있다.

#### GraphQL 과 REST API 비교

#### 1. Endpoint의 차이

REST API 는 URL 과 HTTP Method 등을 조합하기 때문에 다양한 Endpoint가 존재하는 반면, GraphQL 은 단 하나의 Endpoint가 존재한다.

* REST API

```javascript
// 3반 19번 학생의 정보를 불러오기
axios.get("https://school.com/class/03/student/19").then((res) => {
    return res;
});

// 4반 12번 학생의 정보를 삭제하기
axios.delete("https://school.com/class/04/student/12").then((res) => {
    return res;
});

// Jason 선생님의 정보를 수정하기
axios.update("https://school.com/teacher/jason").then((res) => {
    return res;
});
```
이처럼 상황에 따라 요청을 할 때 Endpoint(https://school.com/ 뒤에 부분) 의 변화가 많다. 


* GraphQL

```javascript
await axios.post(
  "https://school.com/graphql",
  {
    query: `
        mutation {
          getStudentsData(class: 03) {
            id,
            name,
            address
          }
        }
      `,
  }
);
```
요청 방식은 **POST** 로 항상 고정되며 Endpoint 는 **/graphql** 로 끝난다. 안에 query문 형식에 따라 원하는 API 요청을 할 수 있기 때문에 항상 모든 Endpoint 들을 외우지 않아도 된다.


#### 2. Overfetching, Underfetching

* Overfetching

학생의 개인정보(이름, 생년월일, 주소, 연락처)가 담겨있는 데이터에서 REST API 는 학생들의 정보들 중에 연락처만을 가져오고 싶어도 모든 데이터(이름, 생년월일, 주소, 연락처)를 가져오게 되기 때문에 불필요한 리소스가 낭비되지만, GraphQL은 학생들의 정보들 중에서 원하는 데이터만 골라서 가져올 수 있다.  

* Underfetching

학생과 선생님, 관리자들의 연락처를 모두 가져오고 싶을 때 REST API 는 각각 한번씩 요청을 해야하기 때문에 총 3번을 요청해야하지만, GraphQL은 한번의 요청 안에서 학생, 선생님, 관리자 등 필요한 여러개의 쿼리문을 담아서 전달할 수 있다.

---

이렇게 마무리하면 왠지 REST API 가 GraphQL 보다 항상 안좋은 것 처럼 보이겠지만, 둘다 상황에 따라 장단점이 있기 때문에 잘 파악해서 사용해야하는 것 같다.

