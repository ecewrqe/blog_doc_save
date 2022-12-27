django使用python内置的http和threadTCPMix来实现底层的socket。通过get_environ()获取到父类已经构造完的environ。整合给META
django的wsgi在core->http->basehttp.py
django继承simple_server.WSGIRequestHandler


update