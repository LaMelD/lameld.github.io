---
title: "대표 라이브러리"
date: 2026-09-17
weight: 3
tags: [nodejs, npm, typescript]
description: "HTTP·유틸리티, 데이터 검증·ORM, 실시간·로깅·테스트 분야의 대표 npm 라이브러리와 axios + zod 사용 예를 정리한다."
---

> Node.js / npm 생태계의 **대표 라이브러리**를 분야별로 정리했다. 설치는 `npm install <이름>`.

## HTTP·유틸리티

| 라이브러리 | 설명 |
|---|---|
| `axios` | Promise 기반 HTTP 클라이언트 (브라우저·Node 공용) |
| `lodash` | 배열·객체 유틸리티 함수 모음 |
| `dayjs` | 가볍고 immutable 한 날짜 처리 (moment 대체) |
| `dotenv` | `.env` 파일로 환경변수 로드 |

## 데이터 검증·ORM

| 라이브러리 | 설명 |
|---|---|
| `zod` | TypeScript 우선 스키마 검증 (타입 자동 추론) |
| `prisma` | 타입 안전 ORM + 마이그레이션 |
| `typeorm` | 데코레이터 기반 ORM |

## 실시간·로깅·테스트

| 라이브러리 | 설명 |
|---|---|
| `socket.io` | WebSocket 기반 실시간 양방향 통신 |
| `winston` | 구조적 로깅 |
| `vitest` / `jest` | 테스트 프레임워크 (vitest 는 ESM·TS 친화·고속) |

## 간단 사용 예 (axios + zod)

```typescript
import axios from "axios";
import { z } from "zod";

const User = z.object({ id: z.number(), name: z.string() });

const { data } = await axios.get("https://api.example.com/user/1");
const user = User.parse(data);   // 런타임 검증 + 타입 확정
```
