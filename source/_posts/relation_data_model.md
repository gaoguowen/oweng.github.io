---
title: 关系数据模型
date: 2019-09-09 21:37:35
categories: 
- Datebase basis
tags:
- relationDataModel 
---
 

## 概叙
关系数据库的基本特征是使用关系模型的组织数据，20世纪80年代以后，在商用DBMS中，关系模型逐步取代早期的网状模型和层次模型。

## 关系数据模型
作为数据模型，关系模型包含三个组成要素：关系数据结构、关系操作集合和关系完整性约束。

### 关系数据结构  <label style = "color:red; ">重点</label>

 结构只包含单一的数据结构（关系），现实世界的实体与实体间的各种联系均用关系来表示。关系模型是吧数据库比赛为关系的集合，并以二维表格的形式组织数据。

 录入一张二维表格如：


| 学号 | 姓名 | 性别 | 籍贯 | 民族 | ... |
| ------ | ------ | ------ | ------ | ------ | ------ |
| 001 | 张三 | 男 | 陕西 | 汉 | ... |
| 002 | 李四 | 男 | 湘西 | 苗 | ... |
| 003 | 王五 | 男 | 河北 | 汉 | ... |
| 004 | 赵六 | 男 | 东北 | 汉 | ... |
| ... | 

#### 基本术语
1. 表(Table)：也称为关系，是二维数据结构，由表名、构成表的各列及若干行数据组成，每个表由唯一的表名，每一行数据描述一条具体的记录值。
2. 关系（Relation）：一个关系逻辑上对应一张二维表，可以为每个关系取一个名称来标识。关系有三种类型：基本关系、查询表和视图表。