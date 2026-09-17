---
title: "부록 D : 환경별 Docker 설치 (macOS · Ubuntu · Rocky Linux)"
date: 2026-09-17
weight: 14
tags: [docker, install, macos, ubuntu, linux]
description: "macOS의 Docker Desktop과 colima, Ubuntu의 apt, Rocky Linux의 dnf로 Docker를 설치하고 설치 후 설정과 확인 방법을 정리한다."
---

> 환경별 Docker 설치 가이드. **macOS** 는 Docker Desktop(또는 colima), **Ubuntu·Rocky Linux** 는 Docker 공식 저장소에서 Docker Engine 을 설치한다.

## 1. macOS

### Docker Desktop (권장)

GUI + 엔진 + CLI 가 한 번에 설치되는 공식 배포판.

```bash
brew install --cask docker
```

설치 후 **Docker Desktop 앱을 한 번 실행**해야 엔진(데몬)이 뜬다. 메뉴바에 고래 아이콘이 나타나면 준비 완료.

> 수동 설치: [docker.com](https://www.docker.com/products/docker-desktop) 에서 칩셋(Apple Silicon / Intel)에 맞는 `.dmg` 를 받아 설치해도 된다.

### 가벼운 대안 — colima (선택)

Docker Desktop 없이 CLI 만 쓰고 싶을 때. 라이선스·리소스 부담이 적다.

```bash
brew install colima docker docker-compose
colima start   # 리눅스 VM + 도커 엔진 기동
```

## 2. Ubuntu (apt)

Docker 공식 apt 저장소를 등록해 최신 Docker Engine 을 설치한다.

```bash
# 1) 이전 버전 제거
sudo apt-get remove docker docker-engine docker.io containerd runc

# 2) 저장소 및 GPG 키 등록
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 3) 설치
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 3. Rocky Linux (dnf)

RHEL 계열은 Docker 의 CentOS 저장소를 그대로 사용한다.

```bash
# 1) 저장소 등록
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 2) 설치
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 4. 리눅스 설치 후 (Ubuntu · Rocky 공통)

```bash
# 데몬 시작 + 부팅 시 자동 실행
sudo systemctl enable --now docker

# sudo 없이 docker 실행 (현재 사용자를 docker 그룹에 추가)
sudo usermod -aG docker $USER
newgrp docker            # 또는 로그아웃 후 재로그인
```

> `docker` 그룹은 사실상 root 권한과 같다. 신뢰할 수 있는 사용자에게만 추가한다.

## 5. 설치 확인

```bash
docker --version         # 클라이언트 버전
docker compose version   # Compose v2
docker run hello-world   # 테스트 컨테이너 실행
```

`hello-world` 가 인사 메시지를 출력하면 정상이다. (리눅스에서 docker 그룹 설정 전이면 `sudo` 필요)

## 6. 설치 후 기본 명령어

```bash
docker ps -a                    # 컨테이너 목록
docker images                   # 이미지 목록
docker pull nginx               # 이미지 내려받기
docker run -d -p 8080:80 nginx  # 백그라운드 실행 + 포트 매핑
```

> **라이선스** — **Docker Desktop**(macOS·Windows)은 직원 250명 초과 또는 연 매출 1천만 달러 초과 기업의 상업적 사용 시 유료 구독이 필요하다(개인·소규모·교육·오픈소스는 무료). **리눅스의 Docker Engine** 은 Apache 2.0 오픈소스로 이런 제약이 없다.
