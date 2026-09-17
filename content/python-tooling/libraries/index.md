---
title: "대표 라이브러리"
date: 2026-09-17
weight: 3
tags: [numpy, pandas, requests, pydantic, pytest]
description: "데이터·과학, 웹·네트워크, 데이터 검증·ORM, 개발·테스트 분야별 Python 대표 라이브러리 정리."
---

> Python 생태계의 **대표 라이브러리**를 분야별로 정리했다. 설치는 모두 `pip install <이름>` (또는 `uv add <이름>`).

## 데이터·과학

| 라이브러리 | 설명 |
|---|---|
| `NumPy` | 다차원 배열·수치 연산의 기반. 대부분의 과학 라이브러리가 의존 |
| `pandas` | 표(DataFrame) 기반 데이터 분석·가공 |
| `matplotlib` | 기본 시각화(그래프) 라이브러리 |
| `scikit-learn` | 전통적 머신러닝(분류·회귀·클러스터링) |

## 웹·네트워크

| 라이브러리 | 설명 |
|---|---|
| `requests` | 사람이 쓰기 쉬운 동기 HTTP 클라이언트 |
| `httpx` | requests 호환 + 비동기(async) 지원 HTTP 클라이언트 |
| `beautifulsoup4` | HTML 파싱·웹 스크래핑 |

## 데이터 검증·ORM

| 라이브러리 | 설명 |
|---|---|
| `pydantic` | 타입 힌트 기반 데이터 검증·설정 관리 (FastAPI 의 기반) |
| `SQLAlchemy` | 파이썬 대표 ORM / SQL 툴킷 |

## 개발·테스트

| 라이브러리 | 설명 |
|---|---|
| `pytest` | 사실상 표준 테스트 프레임워크 |
| `ruff` | 초고속 린터 + 포매터 (Rust) |
| `black` | 무설정 코드 포매터 |

## 간단 사용 예 (requests)

```python
import requests

r = requests.get("https://api.github.com")
print(r.status_code, r.json()["current_user_url"])
```

## 개별 문서

- [PyQt6](/python-tooling/pyqt6/)
- [PyInstaller](/python-tooling/pyinstaller/)
