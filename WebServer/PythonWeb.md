#### PythonWeb开发

**WSGI** Web Server Gateway Interface

Python内置了一个WSGI服务器，这个模块叫wsgiref，是一个参考实现，符合WSGI标准，但是不考虑任何运行效率

WSGI要求实现一个函数

WSGI要求实现一个函数

```python
def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    return [b'<h1>Hello, web!</h1>']
```

