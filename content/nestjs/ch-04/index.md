---
title: "4. 모듈 설계"
date: 2026-09-17
weight: 4
tags: [nodejs, nestjs, module]
description: "@Module 메타데이터의 imports, controllers, providers, exports와 기능 모듈, 공유 모듈, 전역 모듈, DI 경계를 정리한다."
---

> **모듈(Module)** 은 관련된 컨트롤러·프로바이더를 하나로 묶는 단위다. 앱은 루트 모듈(`AppModule`)에서 시작해 기능별 모듈로 뻗어 나간다.

## 4.1. @Module 메타데이터

```typescript
import { Module } from '@nestjs/common';

@Module({
  imports: [OtherModule],         // 이 모듈이 의존하는 다른 모듈
  controllers: [UsersController], // 라우팅 담당
  providers: [UsersService],      // 모듈 내부에서 주입 가능한 프로바이더
  exports: [UsersService],        // 다른 모듈에 공개할 프로바이더
})
export class UsersModule {}
```

| 속성 | 역할 |
|---|---|
| `imports` | 필요한 외부 모듈 등록 |
| `controllers` | 요청을 처리할 컨트롤러 |
| `providers` | 모듈 내부에서 주입 가능한 프로바이더 |
| `exports` | 외부 모듈에서 쓸 수 있게 공개 |

## 4.2. 기능 모듈 / 공유 모듈

- **기능 모듈** — 도메인 단위로 분리 (`UsersModule`, `OrdersModule`)
- **공유 모듈** — 자주 쓰는 프로바이더를 `exports` 해 여러 모듈이 재사용
- **전역 모듈** — `@Global()` 을 붙이면 `imports` 없이 어디서나 주입 (남용 금지)

## 4.3. DI 경계

프로바이더는 `exports` 로 공개해야만 다른 모듈에서 주입할 수 있다. 캐슐화가 기본이다.
