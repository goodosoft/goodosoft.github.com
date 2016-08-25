---
layout: post
title: "内部项目研发规范"
description: "internal project style guide"
category: [learn]
tags: [django]
---
{% include JB/setup %}

## 开发语言：python3.4 ##
- 下载地址：https://www.python.org/ftp/python/3.4.5/Python-3.4.5.tar.xz

## 后端框架：Django 1.8 LTS版本 ##

## 前端框架：Bootstrap3/AdminLTE/Jquery ##

## 数据库：Mysql 5.5.28 ##
- 统一使用DBA提供的开发测试数据库；
    开发数据库可以用作日常调试开发；
    QA环境数据库可以用作内部项目的生产环境；
- 数据库研发规范：http://172.21.200.34/pages/viewpage.action?pageId=17990135

## 版本控制：Git ##
- Git简明教程：https://git-scm.com/book/zh/v2
- Git工作流参考GitHub Flow（https://guides.github.com/introduction/flow/）
    **要及时commit，不要让一个commit承担过多内容；**
    1.从默认master分支根据issue来创建分支；
    2.在本地开发环境确保issue完成后commit代码；
    3.进行git fetch --all，拉取下来master分支的代码，合并到自己的分支，解决冲突;
    4.本地测试没问题后push代码到远程仓库；
    5.发起merge request，请求将该分支合并入master分支；
    6.代码经过评审后合并入master分支；
    
- 引入GitLab提供Git仓库管理、里程碑、代码审查、问题跟踪、动态订阅、wiki、持续集成交付等服务。

