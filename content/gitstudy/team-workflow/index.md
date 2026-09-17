---
title: "팀 단위 Git 협업 전략: GitHub Flow + Rebase (5인 팀 기준)"
date: 2026-09-17
weight: 6
tags: [git, github, workflow]
description: "5인 규모 팀에서 GitHub Flow와 Rebase로 충돌을 최소화하는 워크플로우, 실전 명령어, 협업 규칙과 설계 관점의 조언을 정리한다."
---

> **핵심 원칙:** Main 브랜치는 언제나 배포 가능한 상태를 유지하며, 모든 변경은 `Feature Branch` → `Rebase` → `Pull Request` 과정을 거친다.

---

## 1. 기본 워크플로우: GitHub Flow + Rebase

5명 규모의 팀에서 코드 충돌을 최소화하고 히스토리를 정렬하는 가장 타당한 시퀀스입니다.

1. **Ticket 확인:** 할당된 작업 확인
2. **Branch 생성:** 최신 `main`에서 분기
3. **Local Dev:** 코드 수정 및 커밋
4. **Sync (Rebase):** `main`의 최신 변경사항을 내 작업에 수시로 반영
5. **PR (Pull Request):** 팀원에게 리뷰 요청 및 승인
6. **Merge & Cleanup:** `main` 병합 후 브랜치 삭제

---

## 2. 실전 커맨드라인 시뮬레이션

### STEP 1. 작업 시작 (최신 상태 보장)

기존 로컬 `main`이 구 버전일 수 있으므로 반드시 업데이트 후 분기합니다.

```bash
git checkout main
git pull origin main
git checkout -b feature/issue-번호 # 예: feature/login-api
```

### STEP 2. 작업 중 수시 동기화 (Rebase)

다른 팀원의 작업 결과물을 내 브랜치에 미리 합쳨 충돌을 예방합니다.

```bash
# 1. 변경사항 임시 저장
git add .
git commit -m "작업 중간 저장"

# 2. 원격의 최신 이력 가져오기
git fetch origin

# 3. 내 작업의 시작점을 최신 main 끝으로 재설정
git rebase origin/main
```

### STEP 3. 충돌 발생 시 해결법

`rebase` 도중 `CONFLICT` 발생 시 대응 절차입니다.

```bash
# 1. 충돌 파일 수정 (에디터에서 수정)
# 2. 수정 완료 알림
git add <파일명>
# 3. Rebase 계속 진행
git rebase --continue
# (참고) 너무 꼬였다면 중단: git rebase --abort
```

### STEP 4. PR 생성 및 반영

```bash
# 원격 저장소에 업로드
git push origin feature/issue-번호

# 만약 Rebase 후 push가 거절된다면 (본인 브랜치인 경우만)
git push origin feature/issue-번호 --force
```

---

## 3. 냉정한 협업 규칙 (Checklist)

| 규칙 | 세부 내용 |
|---|---|
| **Main Protection** | `main` 브랜치 직접 Push는 기술적으로 차단 (설정 필수) |
| **Atomic Commit** | 커밋은 하나의 논리적 단위로 작게 쪼개기 |
| **Code Review** | 최소 1명 이상의 승인(Approve) 후 Merge |
| **Squash & Merge** | 자잘한 커밋들을 하나로 합쳤서 `main` 히스토리를 깔낍하게 유지 |

---

## 4. 아키텍처적 조언 (냉정한 진단)

> 만약 특정 파일에서 **충돌이 매일 반복**된다면 이는 Git 사용법의 문제가 아니라 **설계의 문제**입니다.

- **결합도 낮추기:** 5명이 한 파일에 집중된다면 클래스나 모듈을 더 작게 쪼개야 합니다.
- **인터페이스 중심 개발:** 협업 지점의 규격(Interface)을 먼저 정의하고 각자 내부 로직을 구현하면 충돌 확률이 획기적으로 줄어듭니다.
