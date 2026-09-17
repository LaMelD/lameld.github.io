---
title: "웹 프레임워크: Django, Flask, FastAPI"
date: 2026-09-17
weight: 4
tags: [django, flask, fastapi, web]
description: "풀스택 Django, 마이크로 Flask, 비동기 API FastAPI의 성격과 선택 기준, 최소 시작 코드."
---

> Python 대표 **웹 프레임워크** 세 가지를 성격·사용법과 함께 정리했다. 규모·목적에 따라 고른다.

## 선택 기준

| 프레임워크 | 성격 | 언제 쓰나 |
|---|---|---|
| Django | 풀스택 (batteries-included) | ORM·admin·인증까지 한 번에, 규모 큰 서비스 |
| Flask | 마이크로 | 가볍게 시작, 필요한 것만 붙여 확장 |
| FastAPI | 비동기 API | 타입 기반 REST API, 고성능, 문서 자동 생성 |

## Django

ORM·admin·인증·템플릿을 기본 내장한 풀스택 프레임워크.

```bash
pip install django
django-admin startproject config .
python manage.py migrate
python manage.py runserver   # http://127.0.0.1:8000
```

```python
# models.py — ORM 모델 정의
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    created = models.DateTimeField(auto_now_add=True)
```

## Flask

최소 구성으로 시작하는 마이크로 프레임워크.

```bash
pip install flask
```

```python
# app.py
from flask import Flask

app = Flask(__name__)

@app.get("/")
def index():
    return {"hello": "world"}
```

```bash
flask run   # http://127.0.0.1:5000
```

## FastAPI

타입 힌트 기반 비동기 API 프레임워크. Pydantic 으로 검증하고 Swagger 문서를 자동 생성한다.

```bash
pip install fastapi uvicorn
```

```python
# main.py
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str | None = None):
    return {"item_id": item_id, "q": q}
```

```bash
uvicorn main:app --reload   # http://127.0.0.1:8000/docs 에서 자동 문서 확인
```
