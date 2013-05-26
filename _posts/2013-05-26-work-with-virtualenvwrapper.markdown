---
layout: post
title:  "Работа с virtualenvwrapper"
date:   2013-05-26 22:00:00
categories: python virtualenv virtualenvwrapper
---
В <a href='http://webus.github.io/2013/05/work-with-virtualenv/'>предыдущем посте</a> я рассказал про основные принципы работы с virtualenv. Возможно кому-то этого будет достаточно, но если вы за день работаете с большим количеством python проектов то информация ниже для вас.

Для более быстрого переключения между окружениями проектов был создан скрипт <a href='http://virtualenvwrapper.readthedocs.org/en/latest/'>virtualenvwrapper</a>.
Я не буду здесь рассказывать про все приемущества этой утилиты, это за меня уже написали на страничке проекта. Давайте посмотрим как это выглядит на деле.

Установим:
{% highlight bash %}
sudo pip install virtualenvwrapper
{% endhighlight %}

Поправим наш .bash_profile
{% highlight bash %}
vim ~/.bash_profile
{% endhighlight %}

Добавим в него строки:
{% highlight bash %}
export WORKON_HOME="$HOME/.virtualenvs"
source /usr/local/bin/virtualenvwrapper.sh
{% endhighlight %}

Применим изменения к текущей shell-сессии:
{% highlight bash %}
source .bash_profile
{% endhighlight %}

Создадим окружение для проекта my_project
{% highlight bash %}
mkvirtualenv my_project
{% endhighlight %}

Все! Сразу же можно работать.
Для переключения между существующими окружениями используем команду workon. Пример:
{% highlight bash %}
workon my_project1
{% endhighlight %}

Для того чтобы закончить работу с окружением, деактивируем его командой deactivate.

На этом все.