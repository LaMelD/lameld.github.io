---
title: "PyInstaller로 실행 파일 만들기"
date: 2026-09-17
weight: 6
tags: [pyinstaller, packaging, gui]
description: "파이썬 스크립트를 인터프리터·의존성과 함께 하나의 실행 파일로 묶는 PyInstaller의 기본 사용, 자주 쓰는 옵션, 주의점."
---

> **PyInstaller** 는 파이썬 스크립트를 인터프리터·의존성과 함께 **하나의 실행 파일**로 묶어 준다. 파이썬이 설치되지 않은 PC 에도 배포할 수 있다.

## 설치

```bash
pip install pyinstaller
```

## 기본 사용

```bash
pyinstaller main.py
```

- `build/` — 빌드 중간 산출물
- `dist/` — **최종 실행 파일**이 들어가는 폴더
- `main.spec` — 빌드 설정 파일 (자동 생성, 재빌드 시 재사용 가능)

## 자주 쓰는 옵션

| 옵션 | 설명 |
|---|---|
| `--onefile` | 하나의 실행 파일로 묶기 (기본은 폴더 형태) |
| `--windowed` / `--noconsole` | GUI 앱에서 콘솔 창 숨기기 (PyQt 등) |
| `--name APP` | 실행 파일 이름 지정 |
| `--icon app.ico` | 실행 파일 아이콘 지정 |
| `--add-data "src:dest"` | 이미지·데이터 파일 포함 |
| `--hidden-import mod` | 자동 감지 안 되는 모듈 강제 포함 |

## GUI 앱 패키징 예 (PyQt6)

```bash
pyinstaller --onefile --windowed --name MyApp --icon app.ico main.py
```

> **주의점**
> - **크로스 컴파일 불가** — Windows 실행 파일은 Windows 에서, macOS 용은 macOS 에서 빌드해야 한다.
> - **안티바이러스 오탐** — `--onefile` 실행 파일이 백신에 오탐되는 경우가 있다.
> - **누락 모듈** — 동적 import 는 감지되지 않을 수 있어 `--hidden-import` 로 직접 추가한다.
