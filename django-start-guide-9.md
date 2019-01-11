---
title: Django入门与实践-第9章：静态文件设置
date: 2019-01-09 23:39:26
tags:
- python
- django
categories:
- Python
- Django
- Django入门指南
---

静态文件是指 CSS，JavaScript，字体，图片或者是用来组成用户界面的任何其他资源。

Django 提供了一些功能来帮助我们管理静态文件。
这些功能可在 `INSTALLED_APPS` 的 `django.contrib.staticfiles` 应用程序中找到

```myproject
myproject/
 |-- myproject/
 |    |-- boards/
 |    |-- myproject/
 |    |-- templates/
 |    |-- static/
 |    |    +-- css/
 |    |         +-- bootstrap.min.css    <-- here
 |    +-- manage.py
 +-- venv/
```

告诉Django在哪里可以找到静态文件。打开settings.py，拉到文件的底部，在STATIC_URL后面添加以下内容：

```py
STATIC_URL = '/static/'

STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'),
]
```

`templates/home.html`

```html
{% load static %}<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Boards</title>
    <link rel="stylesheet" href="{% static 'css/bootstrap.min.css' %}">
  </head>
  <body>
    <div class="container">
      <ol class="breadcrumb my-4">
        <li class="breadcrumb-item active">Boards</li>
      </ol>
      <table class="table">
        <thead class="thead-inverse">
          <tr>
            <th>Board</th>
            <th>Posts</th>
            <th>Topics</th>
            <th>Last Post</th>
          </tr>
        </thead>
        <tbody>
          {% for board in boards %}
            <tr>
              <td>
                {{ board.name }}
                <small class="text-muted d-block">{{ board.description }}</small>
              </td>
              <td class="align-middle">0</td>
              <td class="align-middle">0</td>
              <td></td>
            </tr>
          {% endfor %}
        </tbody>
      </table>
    </div>
  </body>
</html>
```

模板的开头使用了 Static Files App 模板标签 {% load static %}

显示效果：

![9](https://gitee.com/SCD-Wear/img-bed/raw/master/django/cd80cdec7f4ad5df6e4bdf95bb5dbdfa.jpg)