# 14 Query Compilation & Processing

# Agenda

![image-20210718140549807](images/image-20210718140549807.png)

# Preliminary

## 1 Definition of transiplation

Q:

![image-20210718141007209](images/image-20210718141007209.png)

A:

![image-20210718140928365](images/image-20210718140928365.png)

![image-20210718140915894](images/image-20210718140915894.png)

## 2 Hekaton Remark

![image-20210718141218048](images/image-20210718141218048.png)

![image-20210718141408243](images/image-20210718141408243.png)

### 2.1 Example Database

![image-20210718141601175](images/image-20210718141601175.png)

![image-20210718141903255](images/image-20210718141903255.png)

![image-20210718142035897](images/image-20210718142035897.png)

### 2.2 Predicate Interpretation

Database will interpret `B.val = ? + 1` as a tree execution as it's shown as below:

- if this kind of query needs to be executed for 10B times, there will be 4*10B times jumps, which is slow.

![image-20210718142258363](images/image-20210718142258363.png)

# I Code Specialization

- Access Methods 很少有系统做
- Predicate Evaluation is quite common

![image-20210718143111328](images/image-20210718143111328.png)

## 1.1 Benefits

![image-20210718143348906](images/image-20210718143348906.png)

## 1.2 Architecture Overview

Today we only care much about the Physical Plan generated by the Optimizer, other procedures will affect the system performance though.

**Compiler will receive the Plan and compile it into machine code**

![image-20210718143630318](images/image-20210718143630318.png)

# II Code Generation

![image-20210718144643409](images/image-20210718144643409.png)

## 1 Transpilation

- Write code that converts a relational query plan into imperative language **source code** and then run it through a conventional compiler to generate native code.

Please refer to the <a href="#Preliminary">Preliminary</a> for its explanation from StackOverflow

### 1.1 HIQUE

![image-20210718145438007](images/image-20210718145438007.png)

## 2 JIT Compilation

- Generate an **intermediate representation (IR)** of the query that the DBMS then compiles into native code.

Translate you high-level code into **IR** like JVM bytecode
