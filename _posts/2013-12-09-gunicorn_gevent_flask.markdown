---
layout: post
title:  "Запускаем gunicorn c gevent"
date:   2013-12-09 22:00:00
categories: python virtualenv virtualenvwrapper
---
В мире Python сушествуют 2 наиболее известных сервера приложений: <a href="#">uWSGI</a> и <a href="#">Gunicorn</a>. Последный это форк рубишного Unicorn на pure python. uWSGI написан на C и казалось бы должен быть супербыстрым, но не всегда. Gunicorn может быть запущен в связке с <a href="#">Gevent</a>, и в этом случае он будет сравним со скоростью работы uWSGI, а в некоторых случаях быстрее. Gevent это библиотека, позволяющая реализовывать "зеленые" потоки. Это такие очень легкие потоки, которые не управляются операционной системой. Проще говоря вы можете писать асинхронные приложения используя синхронный API. Можно провести аналогию с сопрограммами, которые можно остановить во время выполнения, а потом запустить снова (продолжить ее выполнение).
Попробуем развернуть обычное Flask веб-приложение на связке nginx + gunicorn + gevent.
Поставим Flask

{% highlight bash %}
pip install flask
{% endhighlight %}

В файле hello.py создадим минимальное Flask приложение:

{% highlight python %}
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run()
{% endhighlight %}

Создадим скрипт gevent_run.py для запуска Flask через gevent:

{% highlight python %}
from gevent.wsgi import WSGIServer
from hello import app

http_server = WSGIServer(('', 5000), app)
http_server.serve_forever()
{% endhighlight %}

Поставим gevent

{% highlight bash %}
sudo apt-get install libevent-dev
{% endhighlight %}

{% highlight python %}
pip install gevent
{% endhighlight %}

Теперь запустим наше приложение через gevent

{% highlight bash %}
python gevent_run.py
{% endhighlight %}

Установим связку с gunicorn

{% highlight bash %}
pip install gunicorn
{% endhighlight %}

И запустим

{% highlight bash %}
python hello:app -k gevent
{% endhighlight %}

Запускать gunicorn вручную не всегда удобно, и тем более следить чтобы он был запущен в системе.
Поэтому делать это будем через Supervisor

{% highlight bash %}
pip install supervisor
{% endhighlight %}

Пример конфиг файла для Supervisor можно взять <a href="https://github.com/benoitc/gunicorn/blob/master/examples/supervisor.conf">тут</a>

{% highlight bash %}
gunicorn \
-b :8091 -w 1 -k gevent \
--worker-connections=2000 \
--backlog=1000 -p gunicorn.pid \
--log-level=critical hello:app
{% endhighlight %}
