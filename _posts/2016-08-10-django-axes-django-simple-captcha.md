---
layout: post
title: "django用户多次密码错误后需要输入验证码登录"
description: "show captcha when django user login fail too many times with django-axes & django-simple-captcha"
category: [learn]
tags: [django]
---
{% include JB/setup %}

在django中，出于安全考虑，如果用户登录多次密码错误，需要用户输入验证码才能继续操作。
做了一下功课，有开源的app可以实现。简单记录一下具体过程。

### 通过pip安装 django-axes & django-simple-captcha： ###
    pip install django-axes
    pip install django-simple-captcha

### 在django setting.py `INSTALLED_APPS`中添加django-axes & django-simple-captcha ###
	INSTALLED_APPS = (
	'django.contrib.admin',
	'django.contrib.auth',
	'django.contrib.contenttypes',
	'django.contrib.sessions',
	'django.contrib.sites',
	...
	'axes',
	'captcha',
	...
	)
### migrate数据库 ###
    python manage.py migrate

### 在django setting.py 设置django-axes自定义参数 ###
	AXES_LOCKOUT_URL = '/locked'

### 在url.py中添加  ###
    url(r'^captcha/', include('captcha.urls')),
	url(r'^locked/$', locked_out, name='locked_out'),

### 在form.py中添加captcha form ###
	from captcha.fields import CaptchaField
    class AxesCaptchaForm(forms.Form):
    	captcha = CaptchaField()

### 在view.py中添加对应的函数 ###
    from axes.utils import reset
	def locked_out(request):
	    x_forwarded_for = request.META.get('HTTP_X_FORWARDED_FOR')
	    if x_forwarded_for:
	        ip = x_forwarded_for.split(',')[0]
	    else:
	        ip = request.META.get('REMOTE_ADDR')
	    if request.POST:
	        form = AxesCaptchaForm(request.POST)
	        if form.is_valid():
	            reset(ip=ip)
	        return HttpResponseRedirect("/login/")
	    else:
	        form = AxesCaptchaForm()
	        return render_to_response('locked_out.html',
	                                  RequestContext(request,
	                                                 {'form': form,}))

### 创建模版 locked_out.html ###
    <form action="" method="post">
	    {% csrf_token %}
	
	    {{ form.captcha.errors }}
	    {{ form.captcha }}
	
	    <div class="form-actions">
	        <input type="submit" value="Submit" />
	    </div>
	</form>

#### 参考资料：  ####
- [django-axes官方文档](https://django-axes.readthedocs.io/en/latest/captcha.html)
- [django-simple-captcha官方文档](http://django-simple-captcha.readthedocs.io/en/latest/usage.html)

