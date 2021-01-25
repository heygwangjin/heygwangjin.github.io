---
layout: post
title: "[Git]Git이란?(Basic of Git)"
tags: Git
categories: Git
---

![Schematic](/images/github.png)
<p>오늘은 소스코드 관리에 용이한 버전 관리 시스템(VCS; Version Control System)인 Git에 대해 알려드리고자 한다.
git을 겉햝기 식으로 알고 있는데 어떻게 돌아가는지 자세히 알고 싶은 분에게 도움이 되었으면 한다.</p>

바로 본론으로 들어가 보자.

## Git이란?
- Git은 간단히 말하면, 우리가 작성하는 **소스코드를 다른 프로그래머와 공유하여 협업**하기 위해 사용하는 분산 버전 관리 시스템이다. Git은 두 개의 저장소가 있는데, **로컬 저장소(Local repository)**와 **원격 저장소(Remote repository)**로 나뉘어진다.

그럼 이제, 깃을 이해하기 위해 필수로 알아야하는 것들에 대해 살펴보자.
 
## Git 필수 개념

- 작업 트리(Working Tree): 작업 트리는 우리가 깃을 설치할 디렉토리를 의미한다. **흔히 우리가 아는 컴퓨터에 있는 모든 디렉토리는 작업트리가 될 수 있다.** 단, 조건이 있는데, 그 조건으 바로 디렉토리에 **.git이라는 로컬저장소(Local repository)**를 설치해 줘야 한다.
- 로컬 저장소(Local repository): **로컬 저장소는 .git 디렉토리**이다. 우리가 작업트리에서 작업한 코드들을 **커밋(commit)이라는 것을 하면 이 .git이라는 로컬저장소에 저장**된다. 즉, 이 디렉토리에는 **모든 커밋이 존재**한다.
- 원격 저장소(Remote repository): 원격 저장소는 **저장소가 내 PC의 프로젝트 디렉토리 안이 아닌 다른 서버에 위치**해 있는 것을 말한다. 원격 저장소를 이용하면 다른 프로그래머와 협업이 가능하여 편리하다. 보통 우리는 Github이라는 회사의 서버를 빌려서 원격 저장소를 생성한다. 
- 인덱스(index): git add 명령어를 수행 후, 우리의 소스코드가 **커밋될 자격이 주어지게 하는 공간**이라고 생각하면 된다. commit을 하기 위해서는 반드시 이 공간에 존재해야 한다. stage area라고도 부른다.
- add: Untracked file(새로 생성된 파일)이나 Unstaged file(수정된 파일)을 Stage area라고도 불리우는 인덱스라는 공간으로 보내는 명령어이다.
- commit: 커밋은 스냅샷처럼 **작업트리의 어떠한 한 시점을 저장하는 것**이라고 생각하면 좋을 것 같다. commit을 수행할 때는 메시지를 필히 작성해 주어야 하는데, **변경 사항과 변경 이유**를 작성해 주면 좋다. 작업트리의 파일과 데이터를 복사해서 **.git폴더에 저장**한다. 직접 .git 폴더를 살펴봐도 좋을 것 같다. 
- push: 원격저장소에 내 로컬 저장소를 올리는 행위이다. 로컬 저장소에 있는 커밋이 다 원격 저장소에 업데이트 된다. **Local to Remote**
- fetch: 원격저장소에서 업데이트된 내용을 받아와서 로컬 저장소를 갱신하는 행위 **Remote to Local**
- pull: fetch 후, 나의 작업 트리도 갱신하는 행위 **Local to Worktree**
 
자, 지금까지 이해하지 못해도 상관없다. 이제 그림을 통해서 비교해 보면서 이해하자.

## Git 도식화 
![Schematic](/images/SchematicPictures.png)

위에서 설명한 용어들의 전체적인 도식화이다.
이제부터 하나하나 설명하겠다. 

<center><img src="/images/Worktrees_directory.png" width="80%" height="88%"></center>
- 우리가 아는 이러한 폴더가 바로 작업 공간(Work tree)이다.  여기에는 소스코드, 소스코드를 포함한 디렉토리, 그리고 .git이라는 로컬 저장소가 존재한다.

<center><img src="/images/Worktree_terminal.png" width="80%" height="88%"></center>
- terminal에서 확인하면 .git과 posts와 같은 디렉토리가 존재하는 것을 확인할 수 있다.

<center><img src="/images/Unstaged.png" width="80%" height="88%"></center>
- 해당 포스트를 작성하기 위해서 만든 .md파일이 Unstaged라고 적혀있다. 위에 욜어에서 설명했었는데, add라는 명령어가 바로 이 Unstaged한 파일을 index라고 불리우는 Stage area로 보내도록 한다.

<center><img src="/images/Staged.png" width="80%" height="88%"></center>
- add를 하고 난 후의 모습이다. Staged가 되었다. 즉, index에 들어갔다는 뜻이다. 위에서 말했듯이, index에 있는 파일들은 커밋(commit)이라는 명령어를 실행할 수 있다. 커밋은 꼭 메시지를 포함하여야 한다.
<center><img src="/images/Committed.png" width="80%" height="88%"></center>
- commit을 하고 난 후의 모습이다. commit은 .git이라는 로컬저장소에 저장된다고 했다. index에 있던 파일이 .git이라는 로컬 저장소에 commit으로 생성되었고, Test라는 메시지를 입력받았다. 
<center><img src="/images/Pushed.png" width="80%" height="88%"></center>
- push하고 난 후의 모습이다. 로컬저장소인 .git에서 원격저장소에 등록된 모습을 볼 수 있다.  


![Schematic](/images/SchematicPictures.png)

- 마지막으로 정리를 해 보자. 우리는 작업공간에 존재하는 posts라는 디렉토리 하위에 있는 md파일을 add해서 index라는 공간으로 보냈다. Stage area에 존재하게 된 md파일을 커밋함으로써 로컬저장소인 .git에 Test라는 메세지를 달고 생성 되었다. 그리고 마지막으로 원격저장소에 저장하기 위해 push라는 명령어를 실행했다.
- pull은 push의 반대 과정이라고 생각하면 된다. 원격에 있는 소스들을 작업 공간에 가져다 줘서 우리의 PC에서 볼 수 있게 해 준다.  
