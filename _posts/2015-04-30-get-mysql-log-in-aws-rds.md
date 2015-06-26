---
layout: post
title: "在AWS-RDS中使用MySQL数据库日志文件"
description: "Get Mysql log file in aws rds with cli"
category: [learn]
tags: [AWS,RDS,Mysql]
---
{% include JB/setup %}

### 确认系统是否安装java&设置JAVA_HOME： 
    rpm -qa | grep java 
    rpm -ql java-1.6.0-openjdk|grep bin 
    export JAVA_HOME=/usr/lib/jvm/java-1.6.0-openjdk-1.6.0.34.x86_64/jre


### 获取Amazon RDS Command Line Toolkit 
    wget http://s3.amazonaws.com/rds-downloads/RDSCli.zip
    unzip RDSCli.zip

#### 新建一个叫accounts的app 
> python manage.py startapp accounts 

#### 在view里添加一个自定义认证的类 
    class ActiveDirectoryBackend:
    def authenticate(self,username=None,password=None):
        try:
            if len(password) == 0:
                return None
            s = Server('ldaps://dc.example.msf', port = 636, get_info = GET_ALL_INFO)  # define an unsecure LDAP server, requesting info on DSE and schema
            c = Connection(s, 
                       auto_bind = True, 
                       client_strategy = STRATEGY_SYNC, 
                       user = username + '@example.msf', 
                       password = password, 
                       authentication = AUTH_SIMPLE, 
                       check_names = True)
            c.unbind()
            return self.get_or_create_user(username,password)
        except:
            return None
    def get_or_create_user(self, username, password):
        try:
            user = User.objects.get(username=username)
        except User.DoesNotExist:
            info=sys.exc_info()  
            print(info[0],":",info[1])
            print(username)
            mail = username + '@example.com'
            user = User(username=username,email=mail)
            user.is_staff = True
            user.is_superuser = False
            user.set_password('ldap a authenticated')
            user.save()
        return user
    def get_user(self, user_id):
        try:
            return User.objects.get(pk=user_id)
        except User.DoesNotExist:
            return None
> 其中域控服务器和用户名，邮箱地址根据实际情况修改。 

### 在settings.py里的INSTALLED_APPS添加accounts 
    INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'accounts',)

### 在setting.py里添加自定义认证 
	AUTHENTICATION_BACKENDS=('accounts.views.ActiveDirectoryBackend',
                         'django.contrib.auth.backends.ModelBackend',) 

### 参考资料： 
- [Customizing authentication in Django](https://docs.djangoproject.com/en/1.7/topics/auth/customizing/)
- [python3-ldap(ldap3) documentation](http://pythonhosted.org/python3-ldap)
- [django ldap authentication sample code](https://djangosnippets.org/snippets/901/)


