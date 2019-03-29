---
layout: post
title: oracle知识梳理-执行计划分析
date: 2017-08-27
tags: oracle 
description: "oracle知识梳理-执行计划分析"
---

## 分析某条SQL的性能问题，通常我们要先看SQL的执行计划，看看SQL的每一步执行是否存在问题。常用的两种方法查看SQL执行计划。
#### 方法一：autotrace生成执行计划

#### 这种方式执行方便，但是当遇到执行时间长的SQL就变得不太现实，它是先产生结果再生成执行计划的。关于Autotrace几个常用选项的说明：
```
SET  AUTOTRACE  OFF ---------------- 不生成AUTOTRACE 报告，这是缺省模式
SET  AUTOTRACE  ON  EXPLAIN --------- AUTOTRACE只显示优化器执行路径报告
SET  AUTOTRACE  ON  STATISTICS ------ 只显示执行统计信息
SET  AUTOTRACE  ON ----------------- 包含执行计划和统计信息
SET  AUTOTRACE  TRACEONLY -----------同set autotrace on，但是不显示查询输出
```

### 步骤 1 sqlplus登录数据库
### 步骤 2 打开autotrace
```
SQL> set autotrace on explain
```
### 步骤 3 输入要查看的SQL
```
SQL> select po.charge, po.new_charge, po.avg_charge
         from lbi_ls_basic.t_l_acct_sum_po_sum_d  po,
             lbi_dim_basic.t_d_dyn_pkgname     pkg,
             lbi_dim_basic.t_d_dyn_tenant_def     def
 where po.subcosid = pkg.productkey(+)
         and po.tenantid = def.tenantid(+); 
返回执行计划结果
```
### 步骤 4 关闭autotrace
```
SQL> set autotrace off
----结束
```

#### 方法二、explain plan for 生成执行计划

#### 这种方式是直接产生执行计划，不会产生SQL结果。

### 步骤1 sqlplus登录数据库
### 步骤2 执行explain plan for语句
```
SQL> explain plan for 
select po.charge, po.new_charge,po.avg_charge
           from lbi_ls_basic.t_l_acct_sum_po_sum_d   po,
                lbi_dim_basic.t_d_dyn_pkgname       pkg,
                lbi_dim_basic.t_d_dyn_tenant_def     def
       where po.subcosid = pkg.productkey(+) and po.tenantid = def.tenantid(+)
       ;
```       
### 步骤3 查询执行计划
```
SQL> select * from table(dbms_xplan.display);
返回结果：略
----结束
```

## 以上都是不借助图形化的工具，借助图形化的工具后方便很多，比如:PL/SQL Developer强大的不行，或者Toad也行。