# 08-Database Storage Models & Layout (CMU databases/Spring 2020)

# 一 Data Layout

**数据库本质上就是一个byte array or char array**

一个数据库的表的表示如图所示，这个表中有一个主键（32-bit INT）和一个value （BIGINT/64-bit INT）：

header存储了地址指示我们要去哪里找table，**所有的header都是同样的位长**，所以数据库会知道如何跳过header，然后后面的32 bit会被解释为id，再后面的64 bit会被解释为value。



![image-20210707220836544](images/image-20210707220836544.png)

## 1 Variable-Length Fields

现在我们要存储一条变长的数据，那么该条insert指令会有64 bit的指针，指向这个数据的开头，LENGTH存储了一个固定长度的指针指向数据，NEXT指向数据的剩余部分的指针。

**所以如果我们的数据overflow了，无法用一个array存储，我们就要用这种形式把所有的数据connect起来。**

![image-20210708102204168](images/image-20210708102204168.png)

![image-20210708102828622](images/image-20210708102828622.png)

## 2 NULL Data Types

第三种方法显然不好，因为这样需要额外的空间开销，而且绝对不是用一个bit就行了，针对不同类型的数据，为了能够让数据对齐，我们需要增添不同bit位的空间开销。

![image-20210708103402758](images/image-20210708103402758.png)

如果选择第三种方式，那么各个类型的Null的Size会变成如下图所示：

![image-20210708105827409](images/image-20210708105827409.png)

## 3 Word-alignment

![image-20210708114716210](images/image-20210708114716210.png)

### 3.1 eg 

![image-20210708114808078](images/image-20210708114808078.png)

![image-20210708114832210](images/image-20210708114832210.png)

### 3.2 没有实现Word-Aligned的问题

- 第一个问题会造成数据读取的缓慢
- 第二个问题是可能读到一些我们不期望的组合
- 第三个问题可能是拒绝访问

![image-20210708115003275](images/image-20210708115003275.png)

### 3.3 Solutions★

①PADDING

![image-20210708115533080](images/image-20210708115533080.png)

②REORDERING，给数据重新排序，让他们组成64 bit的对齐，但仍有可能使用Padding

![image-20210708115752377](images/image-20210708115752377.png)

### 3.4 实验结果

![image-20210708120201476](images/image-20210708120201476.png)

# 二 Storage Models

## 1 Three types of storage

![image-20210708135147802](images/image-20210708135147802.png)

![image-20210708140405122](images/image-20210708140405122.png)

### 1 N-ary Storage Model(NSM)

一个tuple（一条数据）的所有attributes都被连续的存储在一起。

- 对增删改查方便
- 对取表或属性集的一部分不方便

![image-20210708135231239](images/image-20210708135231239.png)

![image-20210708135435871](images/image-20210708135435871.png)

### 2 Decomposition Storage Model(DSM)

所有数据的同一属性分别连续地存储在一起

- 减少了查询一部分数据时不必要的工作
- 更好的压缩
- 但是由于属性需要分裂，减慢了增删改查

![image-20210708135532479](images/image-20210708135532479.png)

![image-20210708135650744](images/image-20210708135650744.png)

### 3 Hybrid Storage Model

基于日常工程中的==Observation==，我们要对"hot data"和“cold data"采取不一样的存储方式。

![image-20210708143731217](images/image-20210708143731217.png)

![image-20210708144437638](images/image-20210708144437638.png)

![image-20210708144509867](images/image-20210708144509867.png)

#### 3.1 Fractured Mirrors

所有的更新先进入NSM，然后再DSM中存储镜像。

![image-20210708144725001](images/image-20210708144725001.png)

#### 3.2 Delta Store

![image-20210708145040920](images/image-20210708145040920.png)

### 4 Peloton Adaptive Storage

![image-20210708145310792](images/image-20210708145310792.png)

![image-20210708145338100](images/image-20210708145338100.png)

### 5 Experiment

![image-20210708145537732](images/image-20210708145537732.png)

## 2 DSM: Design Decisions

![image-20210708140651787](images/image-20210708140651787.png)

### 2.1 Tuple Identification

我们是如何找到Tuple的？如何确认Tuple的起始位置？

![image-20210708140825208](images/image-20210708140825208.png)

### 2.2 Data Organization

![image-20210708141329348](images/image-20210708141329348.png)

#### 2.2.1 Insertion Order

按照插入顺序组织

![image-20210708141619810](images/image-20210708141619810.png)

#### 2.2.2 Soted Order

按照排序算法组织：按照升序排序组织，先排好第一个属性A，再排第二个属性B，依此类推。如此排序可以提高查询的效率，比如我们要查的一个tuple，它的A属性值为10，如果我们一直找到了比A属性值比10大的tuple，都没找到等于10的，说明要查询的数据不存在。

但是这样对插入数据并不方便，因为每次插入数据都要做重新排序，当我们有成千上万的数据时就会很麻烦。

![image-20210708141645632](images/image-20210708141645632.png)

![image-20210708142538886](images/image-20210708142538886.png)

#### 2.2.3 Partitioned

把表中数据根据某种散列函数分成多个切分（partition）。至于在各个切分中是否再做排序等进一步操作取决于具体需求、切分的大小等要素。

![image-20210708142642927](images/image-20210708142642927.png)

![image-20210708142801542](images/image-20210708142801542.png)

## 3 Observation

新插入的数据可能在最近也会被更新。

![image-20210708143351372](images/image-20210708143351372.png)

## 4 System Catalogs

存储数据库的操作记录

![image-20210708145629577](images/image-20210708145629577.png)

## 5 Schema Changes

![image-20210708145809428](images/image-20210708145809428.png)

## 6 Indexes

![image-20210708150013661](images/image-20210708150013661.png)

## 7 Sequences

![image-20210708150124733](images/image-20210708150124733.png)
