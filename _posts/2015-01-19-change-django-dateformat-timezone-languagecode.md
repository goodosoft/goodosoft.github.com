---
layout: post
title: "修改django默认语言，时区，日期格式"
description: "change django default LANGUAGE_CODE TIME_ZONE DATE_FORMAT"
category: [learn]
tags: [django]
---
{% include JB/setup %}

### Django 默认值： 
>LANGUAGE_CODE = 'en-us'  
>TIME_ZONE = 'UTC'  
>DATE_FORMAT   = 'N j, Y' (e.g. Feb. 4, 2003)  

### 修改setting.py：  
>LANGUAGE_CODE = 'zh-cn'  
>TIME_ZONE     = 'Asia/Shanghai'  
>DATE_FORMAT   =  'Y,m,d'  

### 参考资料：
- [django document settings](https://docs.djangoproject.com/en/1.7/ref/settings/)
- [django document templates builtins templatefilter-date](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#std:templatefilter-date)



