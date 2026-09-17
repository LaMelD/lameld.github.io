---
title: "패키지 관리: package.json, npm, pnpm, yarn"
date: 2026-09-17
weight: 2
tags: [nodejs, npm, pnpm, yarn]
description: "package.json 구조와 npm 기본 명령어, semver 버전 범위 기호, pnpm·yarn 같은 대안 패키지 매니저를 정리한다."
---

> Node.js는 **package.json** 을 중심으로 의존성을 관리한다. 기본 도구는 **npm**(Node 설치 시 함께 제공)이며, 더 빠르거나 디스크 효율이 좋은 **pnpm**·**yarn** 을 대안으로 쓴다.

## 1. package.json — 프로젝트 매니페스트

프로젝트의 의존성·스크립트·메타데이터를 담는다.

```bash
npm init -y   # package.json 생성
```

```json
{
  "name": "myapp",
  "scripts": { "dev": "node index.js" },
  "dependencies": { "express": "^5.0.0" },
  "devDependencies": { "vitest": "^2.0.0" }
}
```

- `dependencies` — 런타임에 필요한 패키지
- `devDependencies` — 개발·빌드·테스트에만 필요 (`npm i -D`)
- `package-lock.json` — 설치된 정확한 버전을 고정하는 잠금 파일 → **커밋한다**

## 2. npm 기본 명령어

```bash
npm install              # package.json 기준 전체 설치 → node_modules
npm install express      # 의존성 추가 (dependencies)
npm install -D vitest    # devDependencies 로 추가
npm uninstall express    # 제거
npm update               # 허용 범위 내 업데이트
npm run dev              # scripts 실행
npx create-vite@latest   # 설치 없이 패키지 실행 (npx)
```

## 3. semver — 버전 범위 기호

```text
^5.2.0  →  5.x.x   (major 고정, minor·patch 허용)  ← npm 기본값
~5.2.0  →  5.2.x   (patch 만 허용)
 5.2.0  →  정확히 그 버전만
```

## 4. 대안 패키지 매니저

| 도구 | 특징 | 설치 |
|---|---|---|
| `npm` | Node 기본 내장, 가장 범용 | 내장 |
| `pnpm` | 하드링크로 디스크 절약·빠름, 엄격한 의존성 격리 | `npm i -g pnpm` |
| `yarn` | 오래 널리 쓰임(berry 는 PnP 지원) | `npm i -g yarn` |

세 도구 모두 `install / add / remove / run` 형태로 사용법이 비슷하다.

```bash
pnpm add express   # = npm install express
yarn add express
```

> 한 프로젝트에서는 **매니저 하나만** 쓴다. 잠금 파일(`package-lock.json`·`pnpm-lock.yaml`·`yarn.lock`)이 섞이면 의존성이 어긋난다.
