[TOC]



# 13 Query Execution & Processing

# Why you should take this course

![](images/Screenshot from 2021-07-17 16-08-06.png)

![](images/Screenshot from 2021-07-17 16-09-28.png)

![](images/Screenshot from 2021-07-17 16-11-26.png)

![](images/Screenshot from 2021-07-17 16-13-44.png)

# I History of Databases

![](images/Screenshot from 2021-07-17 16-26-32.png)

![](images/Screenshot from 2021-07-17 16-27-34.png)

![](images/Screenshot from 2021-07-17 16-29-18.png)

## 1.1 Network Data Model

!Complex Queries

!Easily Corrupted (一份文件损坏，整个系统崩溃，那时候的人也不喜欢备份)

![](images/Screenshot from 2021-07-17 16-30-57.png)

## 1.2 IBM IMS （Hierarchical Data Model）

- Duplicated Data (Different supplier provides same Battery needs different data instances over and over again. `That means if the battery name changes, I need to go to find all the battery instances and change their name at the same time`)
- No independence (Supplier's parts point to PART)

![](images/Screenshot from 2021-07-17 16-33-38.png)

![](images/Screenshot from 2021-07-17 16-34-53.png)

## 1.3 Relational Model

**We will join these tables before we find the tuple we need.**

![](images/Screenshot from 2021-07-17 16-41-37.png)

![](images/Screenshot from 2021-07-17 18-02-56.png)

![](images/Screenshot from 2021-07-17 18-04-15.png)

![](images/Screenshot from 2021-07-17 18-05-52.png)

## 1.4 Object-oriented Model

- Complex Queries （进行复杂的查询并不方便）

- No Standard API （没有像SQL那样的标准）

![](images/Screenshot from 2021-07-17 18-10-32.png)

![](images/Screenshot from 2021-07-17 18-11-11.png)

![](images/Screenshot from 2021-07-17 18-11-46.png)

## 1.5 Internet Boom

![](images/Screenshot from 2021-07-17 22-58-46.png)

![](images/Screenshot from 2021-07-17 22-59-52.png)

![](images/Screenshot from 2021-07-17 23-05-06.png)

![](images/Screenshot from 2021-07-17 23-06-29.png)

![](images/Screenshot from 2021-07-17 23-07-39.png)

![](images/Screenshot from 2021-07-17 23-09-02.png)

![](images/Screenshot from 2021-07-17 23-10-41.png)

![](images/Screenshot from 2021-07-17 23-12-26.png)

![](images/Screenshot from 2021-07-17 23-13-35.png)

![](images/Screenshot from 2021-07-17 23-15-13.png)

## 1.6 The future

CMU教授个人对未来的设想

![](images/Screenshot from 2021-07-17 23-18-07.png)
