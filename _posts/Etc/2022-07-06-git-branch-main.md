---
title: "Git 기본 브랜치를 master에서 main으로 바꾸기"

categories:
  - Etc
tags:
  - [Git]

toc: true
toc_sticky: true
published: true
---


## Git의 기본 브랜치(master)를 Github의 기본 브랜치(main)로 맞추기

[Github 의 기본 브랜치가 main 으로 변경됨](https://github.blog/changelog/2020-10-01-the-default-branch-for-newly-created-repositories-is-now-main/){:target='_blank'}에 따라 내 컴퓨터의 git의 설정도 main으로 바꾸는것이 좋을 것 같다.  

실제로 Github에서 레포지토리를 생성할 때 Add a README file 에 체크하면 이런 안내문에 갑자기 표시된다.
> This will set ```main``` as the default branch. Change the default name in your settings.

---

### 1. Github의 기본 브랜치(Default branch) 설정하기

Github 메뉴에서, ```Settings``` - ```Branches``` - ```Default branch``` 에서 수정할 수 있다. 만약에 이 부분이 main 이라면 그대로 냅두면 된다.

---

### 2. git 기본 브랜치 설정을 main으로 바꾸어야 할 때

만약 ```git config --list``` 나 ```git config --get init.defaultbranch``` 을 터미널에 입력했을 때

```
init.defaultbranch=master
```

이렇게 ```defaultbranch``` 값이 master 로 나온다면

```
git config --global init.defaultbranch main
```

을 입력해주면 main으로 수정된다. (init.defaultbranch=main)

---

#### 작업중인 로컬 브랜치 이름을 main으로 변경해야 할 때
```
git branch -M main
```

여기서 ```-M``` 명령어는 강제로 이름을 수정한다 ```--move --force``` 의 단축된 표현이다.  

해당 내용은 여기 [https://git-scm.com/docs/git-branch](https://git-scm.com/docs/git-branch){:target='_blank'} 에서 참고할 수 있다.  

**```-m```**  
**```--move```**  
Move/rename a branch, together with its config and reflog.  


**```-f```**  
**```--force```**  
Reset \<branchname\> to \<startpoint\>, even if \<branchname\> exists already. Without ```-f```, git branch refuses to change an existing branch. In combination with ```-d``` (or ```--delete```), allow deleting the branch irrespective of its merged status, or whether it even points to a valid commit. In combination with ```-m``` (or ```--move```), allow renaming the branch even if the new branch name already exists, the same applies for ```-c``` (or ```--copy```).


**```-M```**  
Shortcut for ```--move --force```.




#### Github의 브랜치 삭제하기

```
git branch -a   // Github의 브랜치 먼저 확인

git push origin --delete 삭제할브랜치이름
```

