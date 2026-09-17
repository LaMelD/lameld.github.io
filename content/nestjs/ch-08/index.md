---
title: "8. 미들웨어"
date: 2026-09-17
weight: 8
tags: [nodejs, nestjs, middleware]
description: "라우트 핸들러 이전에 실행되는 클래스 미들웨어 작성과 MiddlewareConsumer 적용, 요청 생명주기 순서를 정리한다."
---

> **미들웨어(Middleware)** 는 라우트 핸들러 **이전**에 실행되는 함수다. 요청·응답 객체에 접근해 로깅·인증 전처리·CORS 등을 처리한다. (Express 미들웨어와 동일 개념)

## 8.1. 클래스 미들웨어

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`${req.method} ${req.originalUrl}`);
    next();   // 반드시 호출해야 다음 단계로 진행
  }
}
```

## 8.2. 적용 (MiddlewareConsumer)

모듈에서 `configure()` 로 적용 범위를 지정한다.

```typescript
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('users');    // 특정 경로에만 적용
  }
}
```

## 8.3. 요청 생명주기 순서

```text
미들웨어 → 가드 → 인터셉터(전) → 파이프 → 핸들러 → 인터셉터(후) → 예외 필터
```

NestJS 요청 처리에서 미들웨어가 가장 먼저 실행된다.
