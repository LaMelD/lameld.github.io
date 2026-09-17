---
title: "버전 관리: nvm"
date: 2026-09-17
weight: 1
tags: [nodejs, nvm, shell]
description: "nvm 설치와 셸 설정, 버전 설치·전환, .nvmrc, 자주 쓰는 명령어와 pyenv와의 차이를 정리한다."
---

> **nvm (Node Version Manager)** 는 여러 Node.js 버전을 설치·전환하는 도구다. pyenv 와 달리 미리 컴파일된 **바이너리를 내려받고**, `nvm` 자체는 실행 파일이 아니라 셸에 로드되는 **셸 함수**로 동작한다.

## 1. 설치 (설정)

### 공식 설치 스크립트

Homebrew 설치는 nvm 공식 문서가 권장하지 않는다. 공식 스크립트를 사용한다.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

> 스크립트 URL의 `v0.40.3` 은 최신 릴리스 태그로 교체한다.

### 셸 설정 (zsh)

설치 스크립트가 `~/.zshrc` 에 아래를 자동으로 추가한다. 없으면 직접 넣고 셸을 다시 로드한다.

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

```bash
source ~/.zshrc   # 또는 터미널 재시작
command -v nvm    # 'nvm' 이라고 나오면 정상 설치
```

## 2. 버전 설치

```bash
nvm ls-remote          # 설치 가능한 원격 버전 목록
nvm install --lts      # 최신 LTS 설치
nvm install 22         # 특정 메이저 버전 설치 (예시)
nvm ls                 # 설치된 버전 목록
nvm uninstall 20       # 버전 제거
```

## 3. 버전 적용 (사용성)

```bash
nvm use 22             # 현재 셸에서 사용할 버전 전환
nvm use --lts
nvm alias default 22   # 새 셸이 기본으로 쓸 버전 지정
nvm current            # 현재 활성 버전
```

> `nvm use` 는 **현재 셸에만** 적용된다. 새 터미널을 열면 `nvm alias default` 로 지정한 버전으로 돌아간다.

### 프로젝트별 버전 — .nvmrc

프로젝트 루트에 `.nvmrc` 파일을 두고 버전을 적는다.

```bash
echo "22" > .nvmrc   # 또는 lts/jod 같은 별칭
nvm use              # .nvmrc 의 버전으로 전환
```

> pyenv 와 달리 **폴더 진입 시 자동 전환은 기본 제공되지 않는다.** 자동화하려면 nvm 문서의 `cd` 훅(zsh `chpwd`)을 `~/.zshrc` 에 추가해야 한다.

## 4. 자주 쓰는 명령어

| 명령어 | 설명 |
|---|---|
| `nvm install <ver>` | 지정 버전 설치 (`--lts` 로 최신 LTS) |
| `nvm ls` | 설치된 버전 목록 |
| `nvm ls-remote` | 설치 가능한 원격 버전 목록 |
| `nvm use <ver>` | 현재 셸의 버전 전환 |
| `nvm alias default <ver>` | 새 셸의 기본 버전 지정 |
| `nvm current` | 현재 활성 버전 표시 |
| `nvm install <new> --reinstall-packages-from=<old>` | 전역 npm 패키지를 이전 버전에서 이관 |
| `nvm uninstall <ver>` | 버전 제거 |

## 5. 사용성 특성 & 주의점

- **바이너리 설치** — 미리 빌드된 Node 바이너리를 받는다. pyenv 의 소스 빌드보다 빠르다.
- **셸 함수** — `nvm` 은 `nvm.sh` 를 source 해서 얻는 셸 함수다. 그래서 셸마다 로드가 필요하고, 스크립트·`Makefile` 같은 비대화형 셸에서는 안 잡힐 수 있다.
- **전역 npm 패키지 분리** — 버전마다 global 패키지가 따로 관리된다. 전환 후 다시 깔거나 `--reinstall-packages-from` 으로 옮겨야 한다.
- **자동 전환 없음** — `.nvmrc` 자동 적용은 셸 훅을 직접 추가해야 한다(pyenv 의 자동 전환과 가장 큰 차이).
- **아키텍처** — Apple Silicon(arm64)에서 아주 오래된 Node 는 arm64 바이너리가 없어 Rosetta 나 소스 빌드가 필요할 수 있다.

## 6. pyenv(Python)와 비교

| 항목 | nvm (Node.js) | pyenv (Python) |
|---|---|---|
| 설치 방식 | 바이너리 다운로드 (빠름) | 소스 빌드 (느림) |
| 동작 원리 | 셸 함수 (source) | shim (PATH 가로채기) |
| 폴더 자동 전환 | 훅 직접 추가 필요 | 기본 지원 |
| 프로젝트 파일 | `.nvmrc` | `.python-version` |

pyenv 는 [버전 관리: pyenv](/python-tooling/pyenv/) 글에서 따로 다룬다.
