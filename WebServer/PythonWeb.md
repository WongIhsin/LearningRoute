#### PythonWeb开发

**WSGI** Web Server Gateway Interface

Python内置了一个WSGI服务器，这个模块叫wsgiref，是一个参考实现，符合WSGI标准，但是不考虑任何运行效率

常见的Python Web框架有：
**Flask**、**Django**、**web.py**、**Bottle**、**Tornado**

**Django**：全能型Web框架
**web.py**：一个小巧的Web框架
**Bottle**：和Flask类似的Web框架
**Tornado**：Facebook的开源异步Web框架

模板引擎有：**jinja2**、**Mako**、**Cheetah**、**Django**

#### WSGI

WSGI要求实现一个函数

```python
"""hello.py"""
def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    return [b'<h1>Hello, web!</h1>']
```

```python
"""server.py"""
from wsgiref.simple_server import make_server
from hello import application
httpd = make_server('', 8000, application)
print('Server HTTP on port 8000...')
httpd.serve_forever()
```

#### Flask

安装flask

```
pip install flask
```

```python
from flask import Flask, request, render_template

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def home():
    return render_template('home.html')

@app.route('/signin', methods=['GET'])
def signin_form():
    return render_template('form.html')

@app.route('/signin', methods=['POST'])
def signin():
    username = request.form['username']
    password = request.form['password']
    if username == 'admin' and password == 'password':
        return render_template('signin-ok.html', username=username)
    return render_template('form.html', message='Bad usrname or password', username=username)

if __name__ == '__main__':
    app.run()
```

flask通过**render_template()**函数来实现模板的渲染
模板有很多种，flask默认支持的是**jinja2**
模板支持修改保存后即可刷新浏览器看到最新的效果

其中需要将html放置在templates目录下，templates和app.py在同级目录下