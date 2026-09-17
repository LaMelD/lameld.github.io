---
title: "버전 관리: pyenv"
date: 2026-09-17
weight: 1
tags: [pyenv, homebrew, zsh]
description: "여러 Python 버전을 나란히 설치하고 전역·프로젝트·셸 단위로 전환하는 pyenv의 설치, 버전 적용 우선순위, 자주 쓰는 명령어와 nvm과의 비교."
---

> **pyenv** 는 여러 Python 버전을 한 시스템에 나란히 설치하고 전역·프로젝트·셸 단위로 전환해 주는 버전 관리 도구다. `~/.pyenv/shims` 를 PATH 앞에 끼워 넣어 `python`·`pip` 호출을 가로채는 **shim** 방식으로 동작한다.

## 1. 설치 (설정)

### Homebrew로 pyenv 설치

```bash
brew install pyenv
pyenv --version
```

### 셸 설정 (zsh)

`~/.zshrc` 마지막에 아래를 추가한 뒤 셸을 다시 로드한다.

```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - zsh)"
```

```bash
source ~/.zshrc   # 또는 터미널 재시작
```

> pyenv는 Python을 **소스에서 직접 빌드**하므로 빌드 의존성이 먼저 필요하다.

```bash
xcode-select --install
brew install openssl readline sqlite3 xz zlib tcl-tk
```

## 2. 버전 설치

```bash
pyenv install --list      # 설치 가능한 버전 목록
pyenv install 3.13.2      # 특정 버전 설치 (예시 — 최신 안정 버전 권장)
pyenv versions            # 설치된 버전 목록 (* 가 현재 버전)
pyenv uninstall 3.11.9    # 버전 제거
```

## 3. 버전 적용 (사용성)

```bash
pyenv global 3.13.2   # 전역 기본 버전
pyenv local 3.13.2    # 현재 폴더 + 하위 (.python-version 파일 생성)
pyenv shell 3.13.2    # 현재 셸 세션에만 (PYENV_VERSION 환경변수)
```

버전이 여러 곳에서 지정되면 **위쪽이 우선**한다.

| 우선순위 | 지정 방법 | 적용 범위 | 근거 |
|---|---|---|---|
| 1 (최상) | `pyenv shell` | 현재 셸 세션 | `PYENV_VERSION` 환경변수 |
| 2 | `pyenv local` | 폴더 + 하위 | `.python-version` (현재→상위 탐색) |
| 3 | `pyenv global` | 전역 | `~/.pyenv/version` |
| 4 (최하) | system | 전역 | pyenv 설치 이전의 시스템 Python |

## 4. 자주 쓰는 명령어

| 명령어 | 설명 |
|---|---|
| `pyenv install <ver>` | 지정 버전 설치 |
| `pyenv install --list` | 설치 가능한 버전 목록 |
| `pyenv versions` | 설치된 버전 목록 (`*` 가 현재) |
| `pyenv version` | 현재 적용된 버전과 그 근거 표시 |
| `pyenv which python` | 실제 실행되는 python 실행 파일 경로 |
| `pyenv rehash` | shim 재생성 (실행 파일이 안 잡힐 때) |
| `pyenv uninstall <ver>` | 버전 제거 |

## 5. 사용성 특성 & 주의점

- **shim 방식** — `~/.pyenv/shims` 가 실제 `python` 을 대신 받아 버전에 맞게 라우팅한다. 그래서 `which python` 은 shim 경로를 가리킨다.
- **소스 빌드** — `pyenv install` 은 소스를 내려받아 컴파일한다. 첫 설치가 수 분 걸리고 빌드 의존성이 필요하다.
- **프로젝트 자동 전환** — `.python-version` 이 있는 폴더에 들어가면 별도 훅 없이 자동으로 해당 버전으로 전환된다.
- **rehash** — pip 로 CLI 실행 파일을 새로 깔면 shim 이 없어 명령이 안 잡힐 수 있다 → `pyenv rehash`.
- **가상환경** — `pyenv-virtualenv` 플러그인을 얹으면 `pyenv virtualenv 3.13.2 myenv` 로 버전+가상환경을 함께 관리할 수 있다.

## 6. nvm(Node)과 비교

| 항목 | pyenv (Python) | nvm (Node.js) |
|---|---|---|
| 설치 방식 | 소스 빌드 (느림) | 바이너리 다운로드 (빠름) |
| 동작 원리 | shim (PATH 가로채기) | 셸 함수 (source) |
| 폴더 자동 전환 | 기본 지원 | 훅 직접 추가 필요 |
| 프로젝트 파일 | `.python-version` | `.nvmrc` |

nvm은 [Node.js 시리즈의 nvm 글](/nodejs/nvm/)에 따로 정리했다.
