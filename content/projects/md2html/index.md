---
title: "md2html: 마크다운을 저장하면 HTML이 따라 생기는 Claude Code 플러그인"
date: 2026-05-26
weight: 2
tags: [project, python, claude-code, markdown]
description: "표준 라이브러리만 쓰는 단일 파일 변환기와, .md 저장 시 자동으로 .html을 만드는 PostToolUse 훅 플러그인."
---

저장소: [github.com/LaMelD/md2html](https://github.com/LaMelD/md2html)

`.md` 파일을 작성하거나 수정할 때마다 같은 디렉토리에 `.html`이 자동으로 생기는 Claude Code 플러그인이다. 산출물은 두 개다.

- `md2html.py` — Python 단일 파일 CLI 변환기. 외부 의존성 0, 표준 라이브러리만 쓴다.
- `plugin/` — Claude Code 플러그인. `Write`/`Edit`/`MultiEdit` 결과가 `.md`면 PostToolUse 훅이 변환기를 돌린다.

사내 관리자 화면에 있던 JS 위젯(md2html.js)의 테마 10종을 그대로 보존하면서 Python으로 옮긴 것이 출발점이다.

## 동작

```
Claude가 .md 작성/수정 → PostToolUse 훅 발화 → md2html.py 실행 → 같은 이름의 .html 생성
```

설계 원칙은 셋이다.

- **비차단**: 훅은 무슨 일이 있어도 `exit 0`. 변환이 실패해도 Claude 작업을 막지 않는다.
- **단일 진실 원본**: `.md`가 원본이고 `.html`은 매번 덮어쓰는 파생물이다.
- **제외 규칙 코드화**: `CLAUDE.md`, `README.md`, `SKILL.md` 같은 메타 파일과 `.git`, `node_modules`, `dist`, `build`, `.venv` 같은 디렉토리는 훅과 스킬이 같은 규칙으로 건너뛴다.

## 테마 10종

| ID | 이름 | 특징 |
|---|---|---|
| `manual` | 매뉴얼 (플러그인 기본) | 기술 문서, 다크 코드블록, 알림 박스 |
| `meeting` | 회의록 (CLI 기본) | 안건·결정사항 강조, 청색 톤 |
| `white` | White | 화이트, 파란 액센트 |
| `dark` | Dark | VS Code 다크 톤 |
| `release` | 릴리즈 노트 | 다크 헤더, RELEASE 라벨 |
| `report` | 업무보고 | 표 강조, 절제된 흑백 |
| `proposal` | 제안서 | 임팩트 표지, 푸른 그라데이션 |
| `notice` | 공지사항 | 빨강 헤더, 노랑 강조 |
| `plan` | 기획안 | 보라 메인, 섹션 자동 번호 |
| `quotation` | 견적서 | 비즈니스 그린, 금액 우측 정렬 |

한글 이름(`릴리즈노트`, `회의록` 등)도 CLI에서 그대로 인식한다.

## 사용

```bash
# 기본 — 같은 디렉토리에 .html 생성
python3 md2html.py meeting-notes.md

# 테마 지정 (ID 또는 한글 이름)
python3 md2html.py release-2025q2.md -t release
python3 md2html.py release-2025q2.md -t 릴리즈노트

# stdin → stdout
cat notes.md | python3 md2html.py -t notice > notes.html
```

라이브러리로도 쓴다. `convert()`는 본문 조각만, `convert_document()`는 인라인 CSS까지 포함한 전체 문서를 돌려준다.

```python
from md2html import convert_document
html = convert_document("# 회의\n\n- 안건 1", title="정기회의", theme="회의록")
```

지원 문법은 ATX·Setext 헤딩, 굵게·기울임·취소선·인라인 코드, 링크·이미지, 중첩 리스트와 인용, 펜스·들여쓰기 코드블록, 정렬 지원 파이프 표까지다.

## 설치

```text
/plugin marketplace add LaMelD/md2html
/plugin install md2html
/reload-plugins
```

새 세션부터 자동으로 동작한다. CLI만 쓰려면 `md2html.py` 한 파일만 복사하면 된다.

## 만들면서 배운 것

커밋 이력이 그대로 제작 노트다.

1. 변환기와 플러그인 초안을 한 커밋으로 냈다.
2. 한 줄 설치를 위해 저장소 루트에 `marketplace.json`을 추가했다.
3. 첫 배포 환경에서 훅이 로드되지 않았다. 플러그인의 `hooks.json`은 settings.json 모양이 아니라 최상위 `"hooks"` 키로 한 번 더 감싸야 했다. 같은 버전은 마켓플레이스 캐시가 갱신되지 않아 버전을 올려야 했다.
4. 운영 서버 일부가 Python 3.6이었다. `from __future__ import annotations`를 빼고, `dataclasses`는 `requirements.txt`의 환경 마커(`; python_version < "3.7"`)로 3.6에서만 백포트를 깔게 했다.
5. 그러자 `re.Match` 타입 힌트가 3.6/3.7에서 즉시 평가되며 깨졌다. 문자열 리터럴 어노테이션(`"re.Match"`)으로 바꿔 모든 버전에서 동작하게 했다.

교훈은 하나로 모인다. `from __future__ import annotations`를 제거할 때는 어노테이션에 쓰인 타입의 도입 버전을 전부 확인해야 하고, 의심스러우면 문자열 어노테이션이 안전하다.
