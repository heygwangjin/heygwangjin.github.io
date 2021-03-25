---
layout: post
title: "[CA] MIPS 의 3 가지 포맷과 분기, 반복문과 관련된 MIPS 명령어(R-format, I-format, J-format)"
tags: CA 
categories: CA 
---
본 포스팅에서는 MIPS 의 3 가지 포맷과 C 언어로 작성된 조건, 반복문이 어떻게 MIPS 코드로 변환 되는지 알아본다.

본 포스팅은 **크롬 브라우저**에 최적화 되어 있기 때문에, 가급적 크롬 브라우저를 이용해주시길 바랍니다.  
## Representing Instructions
* * *
우선, 인스트럭션(instruction)이란 하드웨어의 CPU 가 메모리에서 읽어 들이는 명령어이다. 이 인스트럭션들은 기계어(machine code)라고 불리는 0 과 1 로 이루어진 **바이너리 형태**(binary digits)로 인코딩 되어 있다.  

모든 MIPS 인스트럭션들은 32-bit, 즉 **1 word** 형태인데, 이러한 인스트럭션들은 각각 자신만의 포맷을 가지고 있다. MIPS 인스트럭션들의 포맷에는 3 가지가 존재한다. 

바로, **R**-format, **I**-format, **J**-format 이다. MIPS 의 모든 인스트럭션들은 **규칙성**(regularity)을 지키기 위해서 32-bit 의 wide 형태를 가진다.(이 규칙성 덕분에 CPU 가 더욱 빠른 성능을 낼 수 있다.)

### MIPS <span id="r-format">R-format</span> Instructions
* * *
MIPS 의 모든 인스트럭션들은 32 비트 라고 했다.  

아래 그림을 보시다시피, R-format 은 32 비트 를 **6 개의 필드**(field)로 쪼개어 구성이 되며, 각각의 필드에서 필요한 비트의 수도 명시되어 있다.
```
                                R-format
+------------+----------+----------+----------+----------+------------+
|     op     |    rs    |    rt    |    rd    |   shamt  |   funct    |
+------------+----------+----------+----------+----------+------------+
    6 bits      5 bits     5 bits     5 bits     5 bits      6 bits   
```
MIPS R-format 의 대표적인 인스트럭션들은 **add / sub** 인스트럭션이 있다.  

필드를 쪼갠 이유는 기본적으로 MIPS 라는 하드웨어에서 이런 식으로 인스트럭션을 인코딩 해서 쓰겠다는 규칙을 정했기 때문이다.(하드웨어는 정해진 인스트럭션 포맷을 해석하기만 하면 된다.)  

op : operation code (opcode) // add, sub, etc.
rs : **first source** register number // rs, rt, rd 는 각 5 비트 씩이므로  
rt : **second source** register number // 2^5 인 32 개의 register number 표현 가능  
rd : **destination** register number  
shamt : shift amount (00000 for now) // shift 연산  
funct : function code (extends opcode) // 2^6 의 오퍼레이션이 부족할 때 사용  

### MIPS R-format Example
```
add $t0, $s1, $s2 // op, rs, rt, rd
```
위의 MIPS 어셈블리 코드는 R-format 에 해당하는 예제이다.  

어셈블리 코드로 작성 된 코드가 어떻게 바이너리 형태로 표현 되는지 알아보자.  

첫 번째로는 MIPS green sheet 라는 ISA manual 을 참조해서 **add 인스트럭션**이 R, I, J 포맷 중 **어떤 포맷에 속하는지 판단**을 해야한다.(add 는 R-format)  

그 다음에는 아래의 그림과 같은 add **인스트럭션 테이블**을 살펴본다. add 와 sub 인스트럭션은 다른 OPCODE 와 FUNCT 을 가진다.
```
CORE INSTRUCTION SET                                OPCODE
                                                    /FUNCT
NAME, MNEMONIC   FORMAT    OPERATION(in Verilog)     (Hex)
Add        add     R       R[rd] = R[rs] + R[rt]  (1) 0/20hex // 0 은 add OPCODE 를 뜻하고, 20hex 는 16진수로 표현한 FUNCT
```

마지막으로 위에서 설명한 6 개의 필드로 나눠진 [R-format](#r-format) 에 따라 인스트럭션을 인코딩한다.
```
      op          rs         rt         rd        shamt      funct
+------------+----------+----------+----------+----------+------------+
| 0(special) |   $s1    |   $s2    |   $t0    |     0    |    add     |
+------------+----------+----------+----------+----------+------------+
```

위의 표를 하드웨어의 CPU 가 읽을 수 있으려면 바이너리 코드로 변환되어야 한다. 그 과정은 아래와 같다.  

참고로 rs, rt, rd 자리에 들어간 숫자는 레지스터 오퍼랜드의 번호를 뜻한다.
```
                                R-format
      op          rs         rt         rd        shamt      funct
+------------+----------+----------+----------+----------+------------+
|     0      |    17    |    18    |    8     |     0    |   0x20(32) |
+------------+----------+----------+----------+----------+------------+

                               To Binary

      op          rs         rt         rd        shamt      funct
+------------+----------+----------+----------+----------+------------+
|   000000   |  10001   |  10010   |  01000   |  00000   |   100000   |
+------------+----------+----------+----------+----------+------------+
    6 bits      5 bits     5 bits     5 bits     5 bits      6bits
```
위의 각 필드에 있는 값을 붙이면 하나의 이진수 덩어리가 된다.  

CPU 는 아래의 이진수 덩어리를 RAM 으로 부터 읽어 와서 연산을 수행할 수 있고, 그 덩어리는 곧 16 진수로도 표현할 수 있다.  
```
00000010001100100100000000100000(2) = 0x02324020
```

### MIPS I-format Instructions
* * *
```
                                I-format
+------------+----------+----------+----------------------------------+
|     op     |    rs    |    rt    |        constant or address       |
+------------+----------+----------+----------------------------------+
    6 bits      5 bits     5 bits                 16 bits
```
MIPS I-format 은 **4 가지 필드**로 구성되어 있다. I-format 은 하위 16 비트 를 **상수**나 **주소**를 표현한다.  

I-format 의 대표적인 인스트럭션들은 **load / store** 인스트럭션이 있다.

load (lw) : 메모리 주소에 있는 데이터를 레지스터로 **가져오는** 인스트럭션  
store (sw) : 레지스터에 있는 데이터를 메모리 주소에 **저장**하는 인스트럭션

두 인스트럭션 모두 레지스터와 메모리에 접근해야 하는데, 레지스터에 접근하려면 우선 **레지스터 번호**를 알아야하고, 메모리에 접근하기 위해서는 **메모리 주소**를 알아야한다.

rt : destination or source register number // 데이터를 메모리 주소로부터 읽어와서 레지스터에 넣어주는 용도
rs + constant or address : 메모리의 어느 주소에서 데이터를 읽어올지 결정하는 용도

**Design Principle 4 : Good design demands good compromises**
- 만약 포맷이 서로 다르면 디코딩을 복잡하게 만든다. (CPU 가 읽기 힘들다)
- R-format 과 I-format 은 서로 다른데, 다르다는 의미는 하드웨어가 R 과 I 포맷에 속하는 인스트럭션을 서로 다르게 처리해줘야 한다는 뜻이다. // I-format 인데 6 개의 필드로 쪼개면 큰일난다.
- 그러나 MIPS 의 장점은 모든 인스트럭션의 **포맷이 32-bit 로 동일**하다. (x86 과 같은 것은 인스트럭션마다 조금씩 달라서 비용이 많이 든다)
- MIPS 의 3 가지 포맷들의 앞의 첫 6 비트 는 op 즉, **Opcode** 를 의미한다. // 6 비트만 보고 하드웨어가 3 가지 포맷 중에서 어떤 포맷인지 파악할 수 있다.

### MIPS J-format Instructions
* * *
```
                                J-format
+------------+--------------------------------------------------------+
|     op     |                         address                        |
+------------+--------------------------------------------------------+
    6 bits                             26 bits
```
J-format 은 **2 가지 필드**로 구성되어 있다. Opcode 6 비트와 그를 제외한 나머지 26 비트로 주소를 표현한다.  

J-format 의 대표적인 인스트럭션들은 **j / jal** 인스트력선이 있다.  

이 두 가지 Opcode 는 26 비트 주소 필드에 적힌 곳으로 **이동**할 때 사용되는 인스트럭션이다.
### Stored Program Computers
* * *
```
                     +---------------------------+
                     |          Memory           |
                     | +-----------------------+ |
                     | |   Accounting program  | |
                     | |     (machine code)    | |
                     | +-----------------------+ |
                     |                           |
                     | +-----------------------+ |
                     | |     Editor program    | |
        C P U        | |     (machine code)    | |
    +-----------+    | +-----------------------+ |
    |           |    |                           |
    |           |    | +-----------------------+ |
    |           |    | |       C compiler      | |
    | Processor |    | |     (machine code)    | |
    |           |    | +-----------------------+ |
    |           |    |                           |
    |           |    | +-----------------------+ |
    +-----------+    | |                       | |
                     | |      Payroll data     | |
                     | |                       | |
                     | +-----------------------+ |
                     |                           |
                     | +-----------------------+ |
                     | |                       | |
                     | |       Book text       | |
                     | |                       | |
                     | +-----------------------+ |
                     |                           |
                     | +-----------------------+ |
                     | |    Source code in C   | |
                     | |   for editor program  | |
                     | +-----------------------+ |
                     +---------------------------+
```
컴퓨터는 프로세서(processor) 형태의 **연산장치**가 있고, 메모리(memory) 형태의 **기억장치**가 있다.  

우리가 사용하는 프로그램은 다 메모리 안에 **프로세스**로 존재한다.(디스크에 있으면 프로그램, RAM 에 올라오면 프로세스)

- 하드웨어의 CPU 관점에서는 인스트럭션이나 데이터나 모두 0101 형태의 **바이너리 데이터**이다. (CPU 는 인스트럭션인지 데이터인지 구분하지 못하는 멍청이다. 그냥 주는대로 읽을 뿐)
- 인스트럭션과 데이터는 메인 메모리(RAM)에 저장되어 있다.
- 프로그램은 프로그램을 운영할 수 있다. // compilers, linkers
- 바이너리 호환성(binary compatibility)은 컴파일 된 프로그램이 다른 컴퓨터에서 동작할 수 있게 한다.

맨 마지막 문장이 무슨 의미냐면, 우리가 A 라는 컴퓨터에서 컴파일 했는데, **ISA 가 같은** 다른 컴퓨터(B)에서도 exe 파일을 수행할 수 있다.

## Logical Operations
* * *
![images](/images/ca02-image00.png)

### Shift Operations
* * *
```
                                R-format
+------------+----------+----------+----------+----------+------------+
|     op     |    rs    |    rt    |    rd    |   shamt  |   funct    |
+------------+----------+----------+----------+----------+------------+
    6 bits      5 bits     5 bits     5 bits     5 bits      6 bits
```
add, sub 와 같은 Opcode 는 R-format 의 shamt 필드를 사용하지 않았는데, shift 연산자는 shamt 필드를 사용한다.  

shamt : 얼마나 많은 비트 포지션을 왼쪽 혹은 오른쪽으로 쉬프트 할 것인가에 대한 정보  

shift 연산자에는 **sll** 과 **srl** 두 가지가 있다.  

Shift left logical (sll)
- 왼쪽으로 비트를 옮긴 다음에, 나머지는 0 으로 채운다.
- sll by i bits **multiples** by 2^i

Shift right logical (srl)
- 오른쪽으로 비트를 옮긴 다음에, 나머지는 0 으로 채운다.
- srl by i bits **divides** by 2^i (unsigned only)

```
1 = 0001(2)
2 = 0010(2) // 왼쪽으로 하나 shift(sll) 할 때마다, 2 가 곱해지는 것을 알 수 있다.
4 = 0100(2) // 반대로 오른쪽으로 하나 shift(srl) 할 때마다, 2 가 나눠지는 것을 알 수 있다.
```

### AND Operations
* * *
```
and $t0, $t1, $t2
```
`$t1` 과 `$t2` 에 있는 값을 and 연산해서 `$t0`에 넣어라
![and](/images/ca02-image01.png)

### OR Operations
* * *
```
or $t0, $t1, $t2
```
`$t1` 과 `$t2` 에 있는 값을 or 연산해서 `$t0`에 넣어라
![or](/images/ca02-image02.png)

### XOR Operations
* * *
```
xor $t0, $t1, $t2
```
`$t1` 과 `$t2` 에 있는 값을 xor 연산해서 `$t0`에 넣어라
![xor](/images/ca02-image03.png)

### NOT Operations
* * *
```
nor $t0, $t1, $zero
```
`$t1` 에 있는 값을 `$t0` 에 넣는 건데 0 은 1으로 1 은 0으로 바꿔주는 연산  
a Nor b == NOT (a OR b)
![not](/images/ca02-image04.png)

## Instructions for Making Decisions
* * *
판단을 내리는 인스트럭션들에 대해서 알아보자. C 언어에서 **분기문**이나 **반복문**과 관련된 인스트럭션이다.

### Conditional operations
```
beq rs, rt, L1 // branch equal
```
if (rs == rt) 를 어셈블리로 변환한 코드이다.  

위의 어셈블리 코드는 rs 와 rt 에 있는 값이 **같다**면, L1 이라는 레이블로 점프하라는 의미이다.
```
bne rs, rt, L1 // branch not euqal
```
if (rs != rt) 를 어셈블리로 변환한 코드이다.  

위의 어셈블리 코드는 rs 와 rt 에 있는 값이 **같지 않다**면, L1 이라는 레이블로 점프하라는 의미이다.
```
j L1 // J-format
```
beq 와 bne 는 **conditional jump** 이고, j 는 **unconditional jump** 이다.  

즉 beq 와 bne 는 **조건에 맞으면** 레이블로 점프하고, j 는 조건에 상관없이 **무조건** 레이블로 점프한다.  

### Compiling IF Statements
* * *
우리가 컴파일러라고 생각하고, 아래의 C 코드가 있을 때, 어떻게 MIPS 가 이해할 수 있는 어셈블리 코드로 변환시킬 수 있을지 생각해 보자.
#### C code
```
if (i == j)
	f = g + h;
else 
	f = g - h;
```
f, g, h, i, and j in `$s0, $s1, $s2, $s3, and $s4`
#### Compiled MIPS code (Assembly)
```
        bne $s3, $s4, Else // i != j, jump to Else
        add $s0, $s1, $s2  // i = j
        j   Exit           // 조건에 상관없이 jump to Exit
Else:   sub $s0, $s1, $s2 
Exit:   ...                // 뭐가 올지 몰라서 ... 으로 표현 
```

### Compiling Loop Statements
* * *
#### C code
```
while (save[i] == k)
    i += 1;
```
i in `$s3` , k in `$s5` , address of save in `$s6`  
Suppose the data type of the save array is word // 4-byte array
#### Compiled MIPS code (Assembly)
```
Loop:   sll     $t1, $s3, 2     // i 에 있는 값을 왼쪽으로 2-bit 이동 시키고 그 값을 $t1 에 저장, 즉 i << 2 와 동일 (인덱스 i에 4를 곱한 값)
        add     $t1, $t1, $s6   // $t1 = &save[i]; 
        lw      $t0, 0($t1)     /* $t0 = save[i]; 메모리 상에 t1 이 가지고 있는 값에서 0 만큼 떨어진 주소에서 1 word 만큼 로드한다.
                                 * 즉, &save[i] 의 시작 주소에서 1 word 만큼 읽어서 $t0 에 가져온다.
                                 */
        bne     $t0, $s5, Exit  // save[i] != k, jump to Exit 
        addi    $s3, $s3, 1     // i 에 상수값 1을 더한다.
        j       Loop 
Exit:   ...
```
`$s3`, 즉 i 에 있는 값을 sll 인스트럭션을 사용하여 왼쪽으로 2-bit 이동 시키고 그 값을 `$t1` 에 저장한다. 이것은 C 언어에서 i << 2 와 동일하다.  

왜 2-bit 만큼 sll 하는 것일까? save 배열이 메모리 상에서 아래의 주소값을 가지고 존재한다고 가정하자.
```
      +----------------+
0x100 |                |\
      +----------------+ \
0x101 |                |  \
      +----------------+   save[0]
0x102 |                |  /
      +----------------+ /
0x103 |                |/
      +----------------+
0x104 |                |\
      +----------------+ \
0x105 |                |  \
      +----------------+   save[1]
0x106 |                |  /
      +----------------+ /
0x107 |                |/
      +----------------+
```
우리 입장에서는 save[0]에서 save[1]로 갈 때, 그냥 인덱스를 1 만 올려주면 된다. 그러나, CPU 입장에서는 다르다.  

save 배열은 word 타입의 데이터이기 때문에 CPU 입장에서는 메인 메모리 상에서 **4 바이트**만큼 움직여야 한다.  

0x100 ~ 0x103 이라는 주소값을 save[0]이 차지한다. 그래서 save[0]에서 save[1]로 이동시키려면 save[0]의 시작 주소인 0x100 에서 4 를 더해야 0x104, 즉 **save[1]의 주소**에 도달할 수 있다. 그래서, 우리는 save[**0**]에서 **0** 의 값이 이진수로 0001 이라고 가정하면 save[**1**]의 **1** 은 0001 에서 sll 인스트럭션을 통해 왼쪽으로 두 번 쉬프트한 0100, 즉 4 를 곱한 값인 **4** 가된다.  

쉬프트 연산을 통해 얻은 값인 4 를 가진 `$t1` 과 save 배열의 시작 주소, 즉 **0x100** 을 가진 `$s6` 을 더한 값을 `$t1` 에 저장한다.  

그렇다면 우리는 이제 save[1]의 주소인 **0x104** 를 `$t1` 에 가지게 되었다.  

이제 우리는 lw Opcode 를 통해, 메인 메모리 상에서 save[1]의 주소(`$t1`)인 0x104 로 간 다음에 거기서 0 만큼 떨어진 위치, 즉 **save[1]의 시작 주소**인 **0x104** 에서 1 word 만큼인 0x107(포함)까지 로드해서 `$t0` 라는 레지스터 변수에 save[1]의 **값**을 저장한다.   

그리고 이제 bne 인스트럭션(branch not equal)을 통해서 `$t0` 와 `$s5` 의 값을 비교하여 값이 다르면 **Exit** 레이블로 이동하고, 같으면 j 인스트럭션을 통해 **Loop** 레이블로 이동해서 이전 과정을 반복한다.(우리가 사용하는 반복문이 이렇게 동작한다..)  

### Basic Blocks
* * *
Basic Block 은 인스트럭션의 집합(sequence of instructions)인데, 브랜치(branch)가 없다.  

그 의미는 어떠한 input 이 있고 그에 따른 output 이 있는데, output 이 여러 개로 갈라질 수 있다.(if, else 에 의해)  

이렇게 **브랜치가 없는 인스트럭션의 집합**을 Basic Block 이라고 한다.  

컴파일러는 처음에 우리 코드를 스캔하면서 Basic Block 단위로 코드를 자르고, Basic Block 단위 안에서는 브랜치가 없어서 이동이 없다.
### More Conditional Operations
* * *
slt 라는 인스트럭션을 알아본다. 이 인스트럭션은 어떠한 **조건에 맞으면 1 아니면 0 **을 저장하는 명령어이다.
```
slt rd, rs, rt
```
if (rs<rt) rd = 1, else rd = 0;  
rs 가 rt 보다 작다면 rd 라는 레지스터에 1 이라는 값을 저장하고, 아니면 0 이라는 값을 저장한다.
```
slti rt, rs, constant
```
상수(constant)를 사용해 if (rs<constant) rt = 1, else rt = 0 으로 응용 가능하다.

```
slt $t0, $s1, $s2   // if ($s1 < $s2)
bne $t0, $zero, L1 // branch to L1
```
예를 들어 `$s1` 이 `$s2` 보다 작다고 가정하면, `$t0` 은 1 의 값이 저장된다. 그리고 `$t0` 와 $zero 의 값이 다르니까 L1 레이블로 점프한다.

### Branch Instruction Design
* * *
CPU 관점에서, 두 값 중 어떠한 것이 더 크고 작은지에 대해 비교하는 것 보다 **동일 여부를 체크**하는 것이 훨씬 쉽다고 한다. 그래서 beq 와 bne 같은 것을 사용한다.  

즉, CPU 관점에서는 =, != 이 <, >= 같은 것들보다 훨씬 **빠르게** 처리할 수 있다.

### Signed vs Unsigned
* * *
Signed comparison : slt, slti  
Unsigned comparison : sltu, sltui  

우리가 둘 중 어떠한 인스트럭션을 사용하느냐에 따라서, 해당 값을 담고 있는 레지스터의 32 개의 bit 가 음수처럼 이용될 수도 있고 unsigned 로 이해되서 처리될 수도 있다.
