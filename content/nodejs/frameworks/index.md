---
title: "웹 프레임워크: Express, Fastify, NestJS"
date: 2026-09-17
weight: 4
tags: [nodejs, express, fastify, nestjs]
description: "Express·Fastify·NestJS의 성격과 선택 기준, 각 프레임워크의 최소 서버 예제를 정리한다."
---

> Node.js 대표 **웹 프레임워크**를 정리했다. 미니멀부터 구조적 프레임워크까지 목적에 맞게 고른다.

## 선택 기준

| 프레임워크 | 성격 | 언제 쓰나 |
|---|---|---|
| Express | 미니멀·사실상 표준 | 자유로운 구성, 학습·프로토타이핑 |
| Fastify | 고성능·플러그인 | 처리량 중요, 스키마 기반 검증 |
| NestJS | 구조적 (TS·DI) | 규모 큰 팀 프로젝트, Angular 식 아키텍처 |

## Express

가장 널리 쓰이는 미니멀 프레임워크.

```bash
npm install express
```

```javascript
// index.js
const express = require("express");
const app = express();

app.get("/", (req, res) => res.json({ hello: "world" }));
app.listen(3000, () => console.log("http://localhost:3000"));
```

## Fastify

스키마 기반 검증과 높은 처리량이 강점.

```bash
npm install fastify
```

```javascript
// index.js
const fastify = require("fastify")();

fastify.get("/", async () => ({ hello: "world" }));
fastify.listen({ port: 3000 });
```

## NestJS

TypeScript·의존성 주입(DI) 기반의 구조적 프레임워크. 자세한 내용은 아래 개별 문서 참고.

```bash
npm i -g @nestjs/cli
nest new project
npm run start:dev   # http://localhost:3000
```

## 개별 문서

- [NestJS](/nestjs/)
