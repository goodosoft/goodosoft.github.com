---
layout: post
title: "加解密django model field数据库字段"
description: "encryption decryption django model field"
category: [learn]
tags: [Django]
---
{% include JB/setup %}

在应用开发过程中，需要把敏感信息加密存储在数据库中，避免数据泄露带来的信息泄露。 
Django本身在用户管理中对用户的密码有一套很棒的加解密机制，但是自己系统的信息需要自定义开发。
经过信息收集，决定采用AES双向对称加密解密。

### 通过pip安装pycrypto模块： 
    pip install pycrypto

### windows用户可以下载网友编译好的版本： 
    https://github.com/axper/python3-pycrypto-windows-installer


### 修改django app的models.py 
    import base64
    from Crypto.Cipher import AES
    from Crypto import Random
    BS = 16
    aes_key = 'This is a key123'
    pad = lambda s: s + (BS - len(s) % BS) * chr(BS - len(s) % BS) 
    unpad = lambda s : s[:-ord(s[len(s)-1:])]
    
    
    class DBmodel(models.Model):   
    	db_password   = models.CharField(max_length=400)
    	def set_db_password(self,i_password):
    		raw = pad(i_password)
    		iv = Random.new().read( AES.block_size )
    		cipher = AES.new( aes_key, AES.MODE_CBC, iv )
    		self.db_password = base64.b64encode( iv + cipher.encrypt( raw ) ) 
    	def get_db_password(self):
    		enc = base64.b64decode(self.db_password)
    		iv = enc[:16]
    		cipher = AES.new(aes_key, AES.MODE_CBC, iv )
    		return unpad(cipher.decrypt( enc[16:] ))

#### set_db_password函数用来写入加密后的数据 

#### get_db_password函数用来读取解密后的数据 
    

### 参考资料： 
- [pycrypto](https://pypi.python.org/pypi/pycrypto)
- [Encrypt & Decrypt using PyCrypto AES 256](http://stackoverflow.com/questions/12524994/encrypt-decrypt-using-pycrypto-aes-256)
- [How to replace a Django model field with a property](http://www.stavros.io/posts/how-replace-django-model-field-property/)


