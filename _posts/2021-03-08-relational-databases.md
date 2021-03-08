---
layout: post
title: "[Database] 관계형 데이터베이스와 릴레이션(Relational Databases and Relation)"
tags: Database
categories: Database 
---
# Relational Databases and Relation 
* * *
이번 포스팅에서는 관계형 데이터베이스가 뭔지 알아보고, 관계형 데이터베이스에서 사용되는 용어(Terminology)들을 정리한다.  
정리하는데 참고한 사이트는 포스팅 최하단의 Reference 부분에 작성했다. 감사의 인사를 전합니다.

## Relational Database
관계형 데이터베이스란 데이터를 **Relation(Table) 형태로 저장**하는 데이터베이스를 말한다.  

**Schema** - 관계형 데이터베이스에서 Schema는 **Relation(table) 형태로** 되어있다.  
**Instance** - **actual contents** at given point in time   
**Database** - set of named **Relations** (or tables) 쉽게 말해, **table들의 집합** ex) student table, college table  

Schema는 **Rleation 형태로** 되어있고, Instance는 실제 contents를 뜻하며, Database는 이름이 명시된 **여러 Relation의 집합**이다.  

## Relation Terminology
* * *
Relation은 한 마디로 우리가 아는 **엑셀의 table과 동일**하다.

![example](/images/relation.png) 
 
그렇다면, 위의 그림은 **College, Apply, Student 이렇게 3 개의 Relation을 가지고 있는 관계형 데이터베이스**라고 할 수 있다.  

지금부터, Relation에서 사용되는 용어들을 위의 그림을 통해서 알아보자.  

**Attribute**
- Attribute는 Relation에서 엑셀의 **열(column)**과 같은 의미이다.
- **Relation은 반드시 이름이 있는 Attribute를** 가지고 있다.
- 위 그림에서 Student Relation은 sID, sName, GPA, sizeHS라는 **4개의 Attribute**를 가지고 있다.

**Tuple**
- Tuple은 Relation에서 엑셀의 **행(row)**과 같은 의미이다.
- Relation은 **각각의 attribute에 대한 값(value)**을 tuple 형태로 가지고 있다.  

**Domain**
- Relation의 **각 Attribute들이 가질 수 있는 값들의 집합** ex) Attribute가 GPA라면 **0 ~ 4.5**의 값이 domain이다. 
- Domain의 이름은 Attribute 이름과 같을 수도 있고 다를 수도 있다.

**Domain이 필요한 이유**  
- Relation에 저장되는 데이터 값들이 본래 의도했던 값들만 저장되고 관리하기 위해서이다.  
- 예를 들어 **'sex'**라는 Attribute가 있다면, 이 Attribute가 가질 수 있는 값은 **'male'**, **'female'**일 것이다. 데이터베이스 설계자는 Attribute인 sex의 domain으로 같은 이름인 sex로 설정하고 그 값으로 'male'과 'female'을 지정하면 **사용자가 실수로 다른 값을 입력하는 것을 방지**할 수 있다.

**NULL**
- NULL은 값으로만 명시될 수 있다. 
- NULL은 **'unknown'**과 **'undefined'** 두 가지 의미를 가진다.
- 맥락 상 파악해야 하는데, **아예 관련이 없는 사항에 대한 NULL은 undefined**를 뜻하고, **관련이 있는 사항이지만 아직 값을 입력하지 않은 경우에는 unknown**을 뜻한다고 생각하면 된다.

**Key** 
- Relation에서 각 **데이터를 분류하는 기준**의 역할을 한다.
- **각 tuple마다 다른 값을 가지는 Attribute**이다.
- **한 개 or 여러 개**의 Attribute들이 key가 될 수 있다.
- 위 그림의 Student Relation에서 **sID Attribute는 key가 될 수 <u>있다</u>**. --> 각 tuple의 value가 unique하기 때문이다.
- 위 그림의 Student Relation에서 **sName Attribute는 key가 될 수 <u>없다</u>**. --> **Amy가 중복**된다. 

## Relational Data Model의 특징
* * *
- 각 Relation은 **반드시 한 개 이상의 key를 가져야 한다**.
- **중복 tuple은 절대 존재하지 않는다**. --> 모든 Relation은 key를 가지고 있기 때문이다.

## Reference
* * *
<a href="https://jhnyang.tistory.com/108">릴레이션 용어</a>  
