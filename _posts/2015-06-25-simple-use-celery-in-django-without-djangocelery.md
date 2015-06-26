---
layout: post
title: "在django中使用celery分布式任务队列"
description: "simple use celery in django without django-celery"
category: [learn]
tags: [django]
---
{% include JB/setup %}

在django中，如果在request中做一个费时的动作，会给用户带来非常不好的体验，所以考虑用一部的任务队列来实现。 
celery是一个不错的分布式任务队列，在3.1版本之前需要安装django-celery来和django进行整合，最新版本已经可以只安装celery来进行一些简单的任务了。

### 通过pip安装celery模块： 
    pip install celery==3.1.18

### 一个标准的django工程应该有如下目录
    - proj/
      - proj/__init__.py
      - proj/settings.py
      - proj/urls.py
    - manage.py

#### 添加 proj/proj/celery.py
    from __future__ import absolute_import
    
    import os
    
    from celery import Celery
    
    # set the default Django settings module for the 'celery' program.
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'proj.settings')
    
    from django.conf import settings
    
    app = Celery('proj')
    
    # Using a string here means the worker will not have to
    # pickle the object when using Windows.
    app.config_from_object('django.conf:settings')
    app.autodiscover_tasks(lambda: settings.INSTALLED_APPS)
    
    
    @app.task(bind=True)
    def debug_task(self):
        print('Request: {0!r}'.format(self.request)) 

#### 在proj/proj/__init__.py里添加：
    from __future__ import absolute_import
    
    # This will make sure the app is always imported when
    # Django starts so that shared_task will use this app.
    from .celery import app as celery_app 

#### 然后就可以在应用app里编写自己的task.py，例如demoapp/tasks.py:  
    from __future__ import absolute_import
    
    from celery import shared_task
    
    @shared_task
    def add(x, y):
        return x + y
#### 注意：这里并没有存储分布式任务的结果，如果你想把结果存在django数据库里，需要安装django-celery。  
  

### 参考资料： 
- [celery官方文档](http://celery.readthedocs.org/en/latest/django/first-steps-with-django.html#using-celery-with-django)

