---
layout: post
title: "[Database] Relational Algebra - select, project, join"
tags: Database
categories: Database 
---
# Relational Algebra Operators 
* * *
이번 포스팅에서는 Relational Algebra의 3가지 operator를 알아본다.  
정리하는데 참고한 사이트는 포스팅 최하단의 Reference 부분에 작성했다. 감사의 인사를 전합니다.

## Example Relations 
* * *
Query on set of relations **produces relation as a result**.  

Example  
**Collge**(***cName***, state, enrollment)  
**Student**(***sID***, sName, GPA, size of HS)  
**Apply**(***sID***, ***cName***, ***major***, decision)  

italic, bold체인 Attribute들은 각 relation의 **key**이다.  

![example](/images/relational-algebra01.png) 
 
## Select Operator (σ)
* * *
Select operator의 기호는 **σ 시그마이며, 특정한 Tuple**을 가져온다.  

```
Students with GPA>3.7  
> σ(GPA>3.7)(Student)
```
* * *
```
Students with GPA>3.7 and HS<1000  
> σ(GPA>3.7^HS<1000)(Student)
```
* * *
```
Applications to Stanford CS major  
> σ(cName='stanford'^major='cs')(Apply)
```

**Notation -> σ(condition)(Relation name)**

## Project Operatoa (π)
* * *
Project operator의 기호는 **π 파이이며, 특정한 Attribute**를 가져온다.   

```
ID and decision of all applications  
> π(sID^dec)(Apply)
```

**Notation -> π(A1, A2, ..., An)(Relation name)**

## Select & Project
* * *
Attribute와 Tuple을 동시에 가져온다.  

```
ID and name of students with GPA>3.7  
> π(sID, sName)σ(GPA>3.7)(Student)
```

## Duplicate
* * *
SQL과 Relational Algebra의 차이점은 **중복된 값을 가진 tuplel의 제거 여부**이다.  

- SQL에서는 중복된 값을 가진 Tuple을 제거하지 않는다.  
- Relational Algebra 에서는 **중복된 값을 가진 Tuple을 제거한다**.

```
List of application majors and decision  
> π(major, dec)(Apply)
```

## Cross-Product
* * *
Cross Product는 두 개의 Relation을 combine한다.  

Querying에 의해 새롭게 생성된 relation은 **두 relation의 attribute의 개수를 더한 개수의 attribute**를 가지고, tuple은 **두 릴레이션의 tuple 수를 곱한 개수를 가진다**.  

```
Names and GPAs of students with HS>1000 who applied to CS and were rejected.  
```
π(sName, GPA)σ(student.sID=Apply.sID^HS>1000^major='cs'^dec='R')**(Student X Apply)**
 
## Natural Join (⋈)
* * *
- **Enforce equality on all atributes with same name**  
cross-product에서 condition으로 동일한 이름을 가진 attribute를 필터링 하는 것이 필요 없어진다. (Natural Join에 내장됨)

- **Eliminate one copy of duplicate attributes**  
서로 다른 relation이 서로 동일한 attribute name을 갖는다면 Natural Join 연산 결과로 생성되는 Relation은 중복된 attribute가 단 1개의 attribute를 가진다.  

Natural Join의 연산자 기호는 **⋈**이다.  

```
Names and GPAs of students with HS>1000 who applied to CS and were rejected.  
```
π(sName, GPA)σ(HS>1000^major='cs'^dec='R')**(Student⋈Apply)**

```
Names and GPAs of students with HS>1000 who applied to CS at college with enr>20,000 and were rejected.  
```
π(sName, GPA)σ(HS>1000^major='cs'^dec='R'^enr>20,000)**(Student⋈(College⋈Apply))**

## Theta Join
* * *
- Exp1 ⋈θ Exp2 == σ(θ)(Exp1 X Exp2)
- theta = condition
- join이라는 용어는 theta join을 의미한다. 
- DBMS의 기본 join 연산이다.

## Reference
* * *
<a href="https://hack-gogumang.tistory.com/577?category=822435">Algebra operators</a>  
<a href="https://www.youtube.com/watch?v=tii7xcFilOA&list=PL6hGtHedy2Z4EkgY76QOcueU8lAC4o6c3&index=9">05-01-relational-algebra-1.mp4</a>
