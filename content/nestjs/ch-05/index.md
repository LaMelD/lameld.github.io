---
title: "5. 환경 변수 구성"
date: 2026-09-17
weight: 5
tags: [nodejs, nestjs, config, dotenv]
description: "@nestjs/config로 .env를 읽어 ConfigService로 제공하는 방법과 Joi 스키마로 환경 변수를 검증하는 방법을 정리한다."
---

> 설정값(DB 접속, 시크릿 등)은 코드에 하드코딩하지 않고 **환경 변수**로 분리한다. NestJS는 `@nestjs/config` 로 `.env` 를 읽어 `ConfigService` 로 제공한다.

## 5.1. 설치와 등록

```bash
npm install @nestjs/config
```

```typescript
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({ isGlobal: true }), // 전역에서 주입 가능
  ],
})
export class AppModule {}
```

```bash
# .env
DATABASE_URL=postgres://localhost:5432/app
JWT_SECRET=super-secret
```

## 5.2. ConfigService 사용

```typescript
constructor(private config: ConfigService) {}

get dbUrl() {
  return this.config.get<string>('DATABASE_URL');
}
```

## 5.3. 유효성 검사 (Joi)

필수 환경 변수 누락을 부팅 시점에 잡는다.

```typescript
import * as Joi from 'joi';

ConfigModule.forRoot({
  validationSchema: Joi.object({
    JWT_SECRET: Joi.string().required(),
    PORT: Joi.number().default(3000),
  }),
});
```

> `.env` 는 **커밋하지 않는다**(`.gitignore`). 대신 `.env.example` 로 키 목록만 공유한다.
