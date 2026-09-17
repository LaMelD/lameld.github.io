---
title: "패키지 관리: pip, venv, uv, poetry"
date: 2026-09-17
weight: 2
tags: [pip, venv, uv, poetry]
description: "pip와 requirements.txt, 표준 venv와 virtualenv, 가상환경·의존성·잠금을 통합 관리하는 uv와 poetry의 사용법과 비교."
---

> Python은 **가상환경으로 프로젝트를 격리**하고 그 안에서 **pip**(또는 uv·poetry)로 패키지를 설치한다. 전역에 바로 설치하면 프로젝트 간 버전이 충돌하므로, 프로젝트마다 가상환경을 두는 것이 기본이다.

## 1. pip — 기본 패키지 설치 도구

```bash
pip install requests            # 설치
pip install "django>=5,<6"      # 버전 범위 지정
pip install -r requirements.txt # 파일 기반 일괄 설치
pip uninstall requests          # 제거
pip list                        # 설치된 목록
pip show requests               # 특정 패키지 정보
```

### requirements.txt — 의존성 고정

```bash
pip freeze > requirements.txt   # 현재 환경을 파일로 저장
pip install -r requirements.txt # 다른 환경에서 그대로 재현
```

## 2. venv — 표준 가상환경 (Python 3.3+ 기본 내장)

```bash
python -m venv .venv        # .venv 폴더에 가상환경 생성
source .venv/bin/activate   # 활성화 (zsh/bash)
deactivate                  # 비활성화
```

## 3. virtualenv — 확장 기능

venv보다 빠르고 다양한 옵션·CLI를 제공한다.

```bash
pip install virtualenv
virtualenv --python=3.13 .venv
source .venv/bin/activate
deactivate
```

## 4. 현대적 통합 도구

가상환경 + 의존성 + 잠금(lock)을 한 번에 관리한다.

### uv — 초고속 (Rust 기반, 최근 표준으로 부상)

```bash
brew install uv
uv init myproject       # 프로젝트 생성 (pyproject.toml)
uv add requests         # 의존성 추가 (+ 자동 가상환경·lock)
uv run python main.py   # 가상환경 안에서 실행
uv sync                 # lock 기준으로 환경 재현
```

### poetry — pyproject.toml 기반 의존성·빌드 관리

```bash
pipx install poetry
poetry new myproject
poetry add requests
poetry install
poetry run python main.py
```

## 5. 도구 비교

| 도구 | 역할 | 특징 |
|---|---|---|
| `pip` | 패키지 설치 | 표준. 가상환경은 별도(venv)로 만들어야 함 |
| `venv` | 가상환경 | 표준 내장. 격리만 담당 |
| `virtualenv` | 가상환경 | venv 상위호환, 더 빠름 |
| `uv` | 통합 | 초고속. pip·venv·lock 통합 — 신규 프로젝트 추천 |
| `poetry` | 통합 | 의존성 + 패키징. pyproject 생태계 |

> `.venv` 폴더는 보통 `.gitignore` 대상이고, `requirements.txt`·`poetry.lock`·`uv.lock` 같은 잠금 파일은 커밋해서 팀이 같은 버전을 재현하게 한다.
