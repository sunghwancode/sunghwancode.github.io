---
title: "팀 프로젝트 - 1주차"

categories:
  - Diary
tags:
  - [Diary, GCP, Https]

toc: true
toc_sticky: true
---

처음으로 팀 프로젝트를 진행해보고있다.
열정적이고 착한 팀원들과 같이 머리를 꽁꽁 싸매면서 개발중인데, 본의아니게 https 설정에서 막혔어서 기록으로 남겨두려고 한다.

---

프론트엔드쪽에서 작업할 때 사용할 테스트용 API 를 만들려고 GCP (google cloud platform)로 배포하는중에 문제가 생겼다.

https로 백엔드 서버에 접속하게 하고싶은데 GCP에서 제공하는 구글 SSL인증서가 인식을 못하는 것이였다.
그래서 계속 "PROVISIONING" 이라는 상태에서 몇시간이고 변하질 않았다.
도메인 상태는 주황색 경고 마크와 함께 "FAILED_NOT_VISIBLE" 이 표시되었다.

<center>
<img src="../../assets/images/ssl%20%EC%9D%B8%EC%A6%9D%EC%84%9C.jpg" width="50%"/>
</center>
<br>

옆에 물음표를 클릭하면 어떠한 상황에서 FAILED_NOT_VISIBLE 이 표시되는지 <a href="https://cloud.google.com/load-balancing/docs/ssl-certificates/troubleshooting?authuser=2&_ga=2.33592745.-989404064.1647441538&_gac=1.118880379.1652370276.Cj0KCQjw4PKTBhD8ARIsAHChzRL6q5AWxkcK5bffVCFzgpGNInQZsfAOj6q-bVp3U3MzaP9UOVso8FEaAiwXEALw_wcB#domain-status" target="_blank">메뉴얼</a>이 나와있는데,

결과적으로는 내가 인증서에 입력해야할 도메인주소를 잘못 입력해서 발생한 문제였었다.

내가 연결할 도메인 주소가 예를들어 `sunghwan.com` 이라고 한다면 인증서에도 `sunghwan.com` 이라는 도메인을 입력해주어야 한다.
나는 서브도메인을 사용할 예정이였고 (`backend.sunghwan.com`), 그렇다면 인증서에도 서브도메인 자체를 넣었어야 했는데 나는 기본 도메인주소를 넣어야하는 줄 알고.. 그래서 인식을 못했던 것이였다.

---

아무튼 https 연결을 해결하고 지금은 열심히 API 작업에 들어갔다.
GCP에서 이것저것 설정하고 다시 배포하고 하다보니 크레딧이 금방금방 줄어들고 있다..!
다음 주 쯤에는 새로운 구글 계정을 파서 다시 셋팅을 해야할....것... 같...다...
