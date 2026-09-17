---
title: "PyQt6로 데스크톱 GUI 만들기"
date: 2026-09-17
weight: 5
tags: [pyqt6, qt, gui]
description: "Qt6 파이썬 바인딩 PyQt6의 설치, 창 띄우기 최소 예제, 핵심 개념과 시그널/슬롯, PySide6와의 라이선스 차이."
---

> **PyQt6** 는 크로스플랫폼 GUI 툴킷 **Qt6** 의 파이썬 바인딩이다. 데스크톱 애플리케이션(창·버튼·입력폼 등)을 만들 때 쓴다.

## 설치

```bash
pip install PyQt6
```

## 최소 예제 — 창 띄우기

```python
import sys
from PyQt6.QtWidgets import QApplication, QLabel, QWidget, QVBoxLayout

app = QApplication(sys.argv)     # 모든 PyQt 앱은 QApplication 하나로 시작

window = QWidget()
window.setWindowTitle("Hello PyQt6")
layout = QVBoxLayout()
layout.addWidget(QLabel("안녕하세요 👋"))
window.setLayout(layout)
window.show()

sys.exit(app.exec())             # 이벤트 루프 시작
```

## 핵심 개념

| 개념 | 설명 |
|---|---|
| `QApplication` | 앱 전체를 관리하는 최상위 객체 (하나만 생성) |
| `QWidget` | 모든 UI 요소의 기반 클래스 (창·버튼·라벨 등) |
| Layout | 위젯 배치 관리 (`QVBoxLayout`·`QHBoxLayout`·`QGridLayout`) |
| Signal / Slot | 이벤트(시그널)를 처리 함수(슬롯)에 연결하는 방식 |
| `QMainWindow` | 메뉴바·툴바·상태바를 갖춘 표준 메인 창 |

## 시그널/슬롯 — 버튼 클릭 처리

Qt는 "무슨 일이 일어났다(시그널)"를 "실행할 함수(슬롯)"에 `connect` 로 연결한다.

```python
from PyQt6.QtWidgets import QPushButton

button = QPushButton("클릭")
button.clicked.connect(lambda: print("버튼이 눌렸습니다"))
```

> **PySide6 와 비교** — 둘 다 같은 Qt6 를 감싸 API 가 거의 동일하다. 차이는 라이선스다. PyQt6 는 GPL/상용, PySide6 는 LGPL(Qt 공식 바인딩). 상용 배포 시 라이선스를 확인한다.
