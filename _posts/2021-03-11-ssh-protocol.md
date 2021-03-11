---
layout: post
title: "[Network] SSH 프로토콜(Secure Shell Protocol)"
tags: Network Protocol
categories: Network 
---
# Secure Shell Protocol (SSH)
* * *
이번 포스팅에서는 OSI 7계층에서 5계층을 담당하는 Session layer의 **SSH(Secure Shell Protocol) 프로토콜**에 관해 알아본다. 정리하는데 참고한 사이트는 포스팅 최하단의 [Reference](#ref)부분에 작성했다. 감사의 인사를 전합니다.

## What is SSH?
* * *  

<div><img style="margin: auto;" src="/images/ssh.png"></div><br>  
SSH란 Secure Shell Protocol, 즉 네트워크 프로토콜 중 하나로 **컴퓨터와 컴퓨터가 인터넷과 같은 Public Network를 통해 서로 통신**을 할 때 **보안적으로 안전하게 통신**을하기 위해 사용하는 프로토콜이다.  

그렇다면 이 SSH라는 프로토콜은 언제 사용할까?  

대표적인 사용의 두 가지 예시가 있는데 첫 번째는 **데이터 전송**이고, 두 번째는 **원격 제어**이다.  

먼저 데이터 전송의 예로는 원격(remote) 저장소인 깃헙이 있다. 우리가 로컬에서 작성한 소스 코드를 원격 저장소인 깃헙에 푸쉬할 때, 우리는 SSH를 활용해 파일을 전송한다.  

다음으로 원격 제어의 예로는 우리의 로컬 쉘을 통해서 우리가 **제어하고자 하는 서버 컴퓨터에 SSH 프로토콜을 통해 접속하여 서버 컴퓨터의 쉘을 통해 OS를 제어**하는 것이다.  

SSH는 FTP나 Telnet과 같은 다른 컴퓨터 통신 프로토콜보다 **보안** 측면에서 성능이 우수하여 많이 사용한다.  

해당 포스팅에서는 **SSH는 어떤 방식으로 서로 다른 컴퓨터가 안전하게 통신**하는지 알아 보고, **SSH 프로토콜을 사용해 원격으로 서버 컴퓨터를 제어하는 예시**를 통해 여러분의 이해를 돕고자 한다.

## Public Key and Private Key
SSH는 한 쌍의 Key를 가지고 있는데, 그것은 바로 **Public key**와 **Private key**이다. 이 두 가지 키를 통해 접속하려는 서버 컴퓨터와 인증 과정을 거치게 된다.  

**Public key**는 공개되어도 비교적 안전한 Key이다. Public key를 통해 메세지를 전송하기 전 암호화를 한다. (복호화 불가능)  

**Private key**는 외부에 노출이 되어서는 안되는 Key로 본인의 컴퓨터 내부에 저장하게 되어 있다. (복호화 가능) 

이러한 Public, Private key들을 통해 다른 컴퓨터와 통신을 하기 위해서는 아래와 같은 절차를 진행한다.  
1. 클라이언트 사이드 컴퓨터가 서버 사이드 컴퓨터에 접속 시도를 함으로써 연걸을 시작한다.
2. 서버 사이드 컴퓨터가 클라이언트 사이드 컴퓨터에게 server public key를 보낸다.
3. 양 사이드 컴퓨터가 협상을하고 시큐어 채널을 연다.
4. 사용자(User)가 서버 호스트 운영체제 로그인한다.

이렇게 서로 관계를 맺고 있는 Key라는 것이 증명이 되면, 두 컴퓨터 사이에 암호화된 **채널**이 형성되어 Key를 활용해 메시지를 암호화 하고 복호화하며 데이터를 주고 받는다.

## How to Access to a Server Using SSH 
* * *
**참고: Linux / Unix 환경에서만 터미널을 통해 사용할 수 있다**
```
ssh [Username]@[Destination IP or Domain]
```
터미널을 실행하고, 위와 같이 **username과 접속하고자 하는 서버의 도메인 주소나 IP**를 입력하면 서버의 쉘을 조작할 수 있게 된다.  

```zsh
ssh -p 22 [username]@[Destination IP or Domain]
```
기본적으로 **ssh 프로토콜은 port number 22**를 사용한다. 그래서 22번 포트라면 옵션을 작성할 필요가 없지만, 만약 **다른 포트번호를 사용하고 싶다면 -p 옵션을 주고 포트번호를 작성**하면 된다.  

![ssh](/images/ssh-complete.png)

이제 서버 컴퓨터의 쉘을 조작할 수 있다.

```zsh
exit
```
서버 쉘을 종료시키고 싶으면 **exit 명령어**를 통해 나갈 수 있다.

<h2 id="ref">Reference</h2>
* * *
<a href="https://cheershennah.tistory.com/96">SSH란?</a>  
