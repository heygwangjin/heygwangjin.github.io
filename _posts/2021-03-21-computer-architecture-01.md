---
layout: post
title: "[CA] MIPS 명령어 집합 구조 (MIPS Instruction Set Architecture)"
tags: CA 
categories: CA 
---
본 포스팅에서는 MIPS 의 몇 가지 명령어 집합(Instruction Set)에 대해 알아본다.  

참고한 사이트는 아래의 [Reference](#ref)에 작성했다. 감사의 인사를 전합니다.

## ISA and Microarchitecture 
* * *
![image04](/images/ca01-image04.png)

컴퓨터 시스템은 크게 소프트웨어(SW)와 하드웨어(HW)로 나뉜다. MIPS 의 명령어 집합에 대해서 알아보기 전에 우리는 두 가지 개념에 대해서 알야아 한다.
### ISA

첫 번째는 ISA 라고 불리우는 **Instruction Set Architecture** 이다. 우리가 C 언어와 같은 프로그래밍 언어로 작성한 프로그램은 컴파일러(Compiler), VM(Virtual Machine) 그리고 OS(Operating System) 에 의해, **Machine code** 로 변환 된다.

ISA 는 이러한 Machine code 를 하드웨어가 이해할 수 있도록 만들어진 명령어 집합 구조이다. 즉 **HW 와 SW 의 인터페이스**라고 할 수 있다. ISA 는 아래와 같은 특징들을 가진다.
- 호환성 문제(Compatibility issues) 때문에 변경되기 어렵다.
- 각 칩(chip) 마다 독자적인 ISA 를 가지고 있다. // AMD, ARM, Intel 등

### Microarchitecture

두 번째는 **Microarchitecture** 이다. Intel 과 AMD 는 x86 이라는 같은 ISA 를 사용하고 있는데 왜 다른 성능을 내는지 궁금할 수 있다.  

그 이유는 바로, **ISA 구현 방법**이 다르기 때문인데, 이러한 ISA 구현 방법을 바로 **Microarchitecture** 라고 한다.

### Mainstream ISAs

유명한 ISA 는 아래와 같은 회사들이 있다.

1. Intel - 주로 우리가 사용하는 컴퓨터나 노트북에 사용된다.
2. AMD - 이것도 컴퓨터와 노트북에 많이 사용된다.
3. ARM - Apple 이 사용하는 ISA 이다. 최근 Apple Silicon 에 의해 애플의 모든 기기는 ARM 아키텍처를 이용한다.
4. MIPS - PlayStation 에 사용되고, 교육용으로 많이 사용된다.

## Operations of Computer Hardware
* * *

컴퓨터는 **수동적**(Passive)이다. 그래서 컴퓨터는 명령어 집합(Instruction Sets)에 만들어진 연산(operation)들만 수행할 수 있다. 즉 하드웨어는 칩에 들어간 기능만 할 수 있다.  

컴퓨터마다 다른 명령어 집합을 가지고 있는데, 이 **명령어 집합이 컴퓨터 하드웨어**를 다룬다. MIPS 가 현대의 ISA 와 매우 흡사하고, 교육용으로 좋아서 많이 사용된다.  

### Arithmetic Operations

MIPS 에서 사용하는 Arithmetic Operation 들에 관해 알아 보기 전, MIPS 에는 **Operation code** 와 **Operand** 가 존재한다. Operation code 는 **연산자**로 생각하면 되고, Operand 는 말 그대로 **피연산자**이다.  

설명을 하기에 앞서 지금부터 모든 Operation code 는 편의상 **Opcode**라고 표현한다.   

MIPS 에는 **add** 와 **sub** 라는 Opcode 가 있다. add 는 덧셈을 의미하고, sub 는 뺄셈(subtract)을 의미한다. 이 두 Opcode 는 총 3 개의 Operand 를 가지는데, **2 개의 source** 와 **1 개의 destination** operand 로 이루어진다.  

```
add a, b, c // a = b + c
sub a, b, c // a = b - c
```

모든 Arithmetic operation 들은 위와 같은 형식이다.  

본 포스팅에서는 MIPS 의 여러가지 디자인 원리(Design Principle)에 관해 알아볼 것인데, 그 첫 번째는 바로 **Simplicity favors regularity** 이다.

이 말의 의미는 **규칙적으로 디자인 해 놓아서, 코드를 구현하기에 간단**하다는 것이다. 그래서 낮은 비용으로 높은 성능을 낼 수 있다.  
 
우리는 지금부터 C code 가 어떻게 MIPS code 로 컴파일 되는지 알아본다.  

## Operands of Computer Hardware
* * *

C code 가 어떻게 MIPS code 로 컴파일 되는지 알아보기 전에 **컴퓨터 하드웨어의 Operand** 에 대해서 알아본다.  

#### C code
```
f = (g + h) - (i + j);
```
f, g, h, i, j 는 모두 C 언어의 변수이다. C 언어의 변수들은 실제로 메모리(DRAM) 상에 저장되어 있다. 그러나, Arithmetic operation 은 **레지스터 operand** 나 **메모리 operand** 를 사용한다. 이 operand들은 결국 어떤 값을 저장하는 공간인데, 그 공간은 **레지스터**(register) 가 될 수도 있고, **메모리**(DRAM) 가 될 수도 있다는 말이다.  

위의 f, g, h, i, j 변수들은 f 부터 시작해서 각 `$s0, $s1, $s2, $s3, $s4` 이런 식으로 표현되는 레지스터 operand 들에 저장된다고 가정하자.  

#### Compiled MIPS code (Assembly)
```
add $t0, $s1, $s2 # $t0 = $s1 + $s2
add $t1, $s3, $s4 # $t1 = $s3 + $s4
sub $s0, $t0, $t1 # $s0 = $t0 - $t1 
```
C code 에서는 우리가 보기 편하게 g, h 와 같은 변수들을 그냥 사용했는데, 실제로는 위와 같이 레지스터 operand 를 표현하는 값으로 적용 된다.  

정리하면, C code 에서 선언된 변수들은 각자의 용도에 알맞는 `$s0, $s1` 과 같은 **레지스터 operand 들로 변환**된다.  

레지스터에 대해서 잘 모르는 사람을 위해 아래에 간략하게 설명한다.  

#### 레지스터(register)
- 레지스터는 프로세서 내부적으로 관리하는 어떤 저장공간이다. (CPU 내에 존재)
- 레지스터는 어떤 데이터를 담을 수 있는 가장 **기본적인 단위**이다.
- 레지스터는 **자주 사용되는 데이터**를 저장하는 공간이다.

레지스터는 32-bit data 이다. 레지스터가 32-bit data 인 이유는 컴퓨터가 한 번에 인식하고 처리할 수 있는 bits 의 그룹(group)을 **word** 라고 하는데, MIPS32 아키텍쳐(architecture)에서의 word 는 32 bits 이기 때문에, 32-bit register 를 사용한다.  

그렇다면, 64 개 혹은 128 개의 더 많은 레지스터를 제공하지 않는 이유는 무엇일까? 레지스터 수가 많아지면 read/write 가 느려져서 clock cycle time 이 증가하게 된다. 이 말 뜻은, 즉 레지스터에 대한 **접근(access) 속도**를 빠르게 하기 위해서이다. 레지스터 수가 많으면 필요한 주소 비트(bit)가 늘어나서 word 의 비트 수를 차지하게 된다.  

그렇다면, 32 개의 레지스터보다 더 적은 수를 두면 더 빠르지않을까? 라고 생각할 수 있다. 그런데 8 개나 16 개의 레지스터를 두면 물론 하드웨어 관점에서는 더 빨라지겠지만, 프로그래머 관점에서는 레지스터가 많아야 프로그래밍이 쉬워진다. 즉 하드웨어와 프로그래머 관점에서 적절한 **밸런스**(balance)를 맞춘 것이 MIPS 에서는 **32** 개의 레지스터이다.  

**Design Principle 2 : Smaller is faster** // 적은 양의 레지스터(32-bit data)를 둠으로써, 데이터에 빠르게 접근할 수 있다.

### Register Operands : MIPS 32 Registers

C code 에 선언된 변수들이 각자 알맞는 레지스터 변수로 변환된다고 했는데, 아래의 표는 32 개의 레지스터 변수에 대한 정보이다.

![MIPS 32](/images/ca01-image01.png)

'#' 은 레지스터 번호를 뜻한다.  
'Name' 은 레지스터 변수의 이름을 뜻한다.  
'Usage' 는 레지스터의 용도를 뜻하는데, 용도를 나누는 이유는 하드웨어의 구현을 쉽고 빠르게 구현하기 위해서이다.  

## MIPS Memory Operands
* * *

MIPS code 의 operand 로 레지스터 operand 가 아닌, 메모리(DRAM) operand 가 올 수도 있다. 모든 데이터(Arrays, Structures, Dynamic data)는 처음에 메모리에 존재한다.  

Arithmetic Operation 을 하기 위해서는, **메모리에서 레지스터**로 데이터를 가져오거나 **레지스터에서 메모리**로 데이터를 저장해야 하는데, 이 때 두 가지 중요한 연산이 필요하다.  

첫 번째는 **Load** 라는 Opcode 인데, Load Opcode 는 **메모리**에 있는 값을 **레지스터**로 가져오는 Opcode 이다. // **Load** values from memory into registers 

두 번째는 **Store** 라는 Opcode 인데, Store Opcode 는 **레지스터**에서 연산한 결과값을 **메모리**에 저장하는 Opcode 이다. // **Store** result from registers to memory  

여기서 중요한 점은 **메모리는 실제로 byte 단위로 접근**한다는 것이다. 레지스터는 32 bits, 즉 4 bytes 로 표현할 수 있는데, 이것은 1 word 와 동일하다.  

메모리상에 존재하는 어떤 데이터를 접근할 때, **한 바이트**씩 접근할 수 있다. 그래서 C 언어에서는 가장 최소의 단위 변수로 char 타입을 쓰는데, 얘가 1 byte 로 이루어진 것이다.  

C 언어에서 int 형 배열을 선언하면 그 배열은 메모리 상에서는 아래와 같은 구성을 가진다.  

![array](/images/ca01-image02.png)

컴퓨터 메모리에서는 메모리에 데이터를 저장할 때, **Big Endian** 혹은 **Little Endian** 이라고 표현할 수 있고, 이 두 가지 방식에 따라서 **값을 저장하는 순서**가 다르다. MIPS 는 **Big Endian** 방식을 채택했다. 다른 ISA 는 Little Endian 을 사용할 수도 있다. 

이렇게 메모리에 데이터를 저장하는 표현 방법이 두 가지로 나뉘어 있기 때문에, 메모리를 표현하는 방식이 서로 다른 두 컴퓨터끼리 데이터를 전송하는 경우에 문제가 발생한다. 

### MIPS Memory Operand Example (1)

C code 가 있을 때, 변수는 메모리에 저장되어 있는 값을 담고 있는 별명이다. 그런데, 그 값을 더하거나 빼려면 **레지스터**에 들어와서 연산을 수행해야 한다. 메모리에 있는 데이터를 레지스터로 가져와서 연산을 하려면 어떻게 해야할까??  

바로, 앞에서 설명한 **Load** 라는 연산을 사용해야 한다.

#### C code
```
g = h + A[8];
```

g in `$s1`, h in `$s2`, base address of A in `$s3` // base address of A 는 A 배열의 시작 주소를 의미한다.  

#### Compiled MIPS code (Assembly)
```
lw $t0, 32($s3) // load word
```
Opcode : lw // 메모리를 읽어 오는 load word 이다. 두 개의 operand 만 받는다.  
Operand : `$t0`, `32($s3)`

lw 는 어떤 **word** (4 byte) 를 가져온다. A[8] 을 가져오기 위해서는 메모리 위치를 알아야 한다.  

`$s3` 은 A[0]의 메모리 주소값을 가지고 있다. A 가 int 형 배열이므로, A[8] 은 A[0] 에서 4 * 8 인 32 byte 만큼 떨이진 곳에 위치한다.  

그러므로 위의 코드를 다시 설명하자면, `$s3` 에서 32 byte, 즉 **8 word** 떨어진 곳에 있는 A[8] 의 메모리 주소를 읽어서 `$t0` 에 넣으라는 의미이다.  

따라서, g = h + A[8] 이라는 C code 를 MIPS code 로 변환하면 아래와 같다.

#### Compiled MIPS code (Assembly)
```
lw $t0, 32($s3)
add $s1, $s2, $t0
```

### MIPS Memory Operand Example (2)

#### C code
```
A[12] = h + A[8];
```
h in `$s2`, base address of A in `$s3`  

A의 **메모리 시작 주소**는 `$s3` **레지스터 operand** 가 가지고 있다. h 의 메모리 주소는 `$s2` 레지스터 변수가 가지고 있다.  

#### Compiled MIPS code (Assembly)
```
lw $t0, 32($s3) // load word
add $t0, $s2, $t0 // $t0 = h + A[8]
sw $t0, 48($s3) // store word
```
sw 는 store word 의 준말인 Opcode 이다.

sw Opcode 는 `$t0` 레지스터 operand 에 있는 값을 A 의 메모리 시작 주소에서 **12 word** 만큼 떨어진 **메모리 주소에 저장**하라는 의미이다.

## Register vs Memory
* * *
레지스터를 operand 로 가지는 **add**, **sub** Opcode 를 알아보았다.  

메모리를 operand 로 가지는 **lw**, **sw** Opcode 를 알아보았다.

MIPS 에서 메모리에 있는 데이터를 연산하기 위해서는 메모리에 있는 값을 레지스터로 읽어(lw) 와서, 연산을 한 후에 그 결과 값을 메모리에 다시 저장(sw) 해야한다.  

이러한 과정을 거치는 이유는 **연산자**(Operaotr) 가 **프로세서**(Processor) 안에 있기 때문에, CPU 관점에서는 메모리에 접근하는 것보다 **레지스터에 접근**하는 것이 훨씬 **빠르기** 때문이다.  

그리고 컴파일러는 어떠한 변수에 대해 **가능한 많은 레지스터**를 사용해야한다. 왜냐하면 **메모리에 접근**하는 것이 레지스터에 접근하는 것보다 더 **느리기** 때문이다.

## MIPS Immediate Instructions
* * *

MIPS 에서는 상수(constant) 데이터 같은 자주 쓰이는 것을 어떻게 표현할까?

```
addi $s3, $s3, 4 // i 라는 postfix 가 있으면 constant operand를 연산
```

상수는 뺄셈 명렬(Subtract instruction)은 존재하지 않는다. 그냥 아래의 코드처럼 음수를 사용하면 된다. (컴퓨터는 2의 보수를 사용)

```
addi $s2, $s2, -1
```

**Design Principle 3: Make the common case fast**

### MIPS Constant

32 개의 register 중에서 0 번 register 의 이름은 `$zero`이며, 0 의 값을 의미한다. 이것은 **항상 0 의 값**을 가지고 **변경 불가능**하다.
```
add $t2, $s1, $zero
```

### Memory Hierarchy

![image3](/images/ca01-image03.png)

메모리는 계층적 구조를 이룬다. 위로 갈수록 연산 속도가 빠르고 크기가 작으며, 밑으로 갈수록 연산 속도가 느리고 크기가 크다.

## Summary of MIPS Design Principles
* * *
MIPS 의 3 가지 디자인 원리를 알아봤다.

### <strong>Simplicity favors regularity</strong>
Arithmetic operation 들은 모두 동일한 형태의 포맷을 가지고 있다. 그래서 하드웨어도 간단해질 수 있다.

### <strong>Smaller is faster</strong>
적은 수의 레지스터와 적은 수의 Instruction Set 을 가짐으로써, 빠르게 구현할 수 있다.

### <strong>Make the common case fast</strong>
Common case(상수)에 대해서 빠르게 만들 수 있다.

## Additional 
* * *
### 2의 보수법

컴퓨터는 [2의 보수법](https://modoocode.com/308)을 통해서 음수를 표현한다. 

+2 = 0000 0000 .... 0010(2)
-2 = 1111 1111 .... 1101(2) + 1 --> 1111 1111 .... 1110(2)

### MIPS-32 ISA

MIPS 에는 아래와 같은 명령들이 존재한다. 

<input type="checkbox" name="chk_info" values="Computational (add, sub)" checked="checked">Computational (add, sub)  
<input type="checkbox" name="chk_info" values="Load/Store" checked="checked">Load/Store  
<input type="checkbox" name="chk_info" values="Jump and Branch (switch, if)">Jump and Branch (switch, if)  
<input type="checkbox" name="chk_info" values="Floating point">Floating point  
<input type="checkbox" name="chk_info" values="Memory Management">Memory Management  
<input type="checkbox" name="chk_info" values="Special">Special  

MIPS 의 여러가지 Opcode 중 add, sub, lw, sw 를 알아봤다. 앞으로 나머지 Opcode 에 대해서도 포스팅할 기회가 되면 하겠다.  

마지막으로 중요한 점은 **모든 명령(Instruction)들은 동일하게 32 bits**의 크기를 가진다.

<h2 id="ref">Reference</h2>
* * *
[2의 보수법](https://modoocode.com/308)
[Istruction Set (2) Operands](https://m.blog.naver.com/PostView.nhn?blogId=mayooyos&logNo=220825101747&proxyReferer=https:%2F%2Fwww.google.com%2F)
