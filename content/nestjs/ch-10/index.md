---
title: "10. 로깅"
date: 2026-09-17
weight: 10
tags: [nodejs, nestjs, logging, winston]
description: "내장 Logger 사용법과 로그 레벨 설정, nest-winston을 이용한 winston 연동을 정리한다."
---

> **로깅**은 앱의 동작·오류를 기록해 문제를 추적한다. NestJS는 내장 `Logger` 를 제공하며, 운영에서는 `winston` 같은 전용 로거로 교체한다.

## 10.1. 내장 Logger

```typescript
import { Logger } from '@nestjs/common';

@Injectable()
export class UsersService {
  private readonly logger = new Logger(UsersService.name);

  findAll() {
    this.logger.log('전체 조회');
    this.logger.warn('경고 메시지');
    this.logger.error('에러 발생', stackTrace);
  }
}
```

- 로그 레벨: `log`, `error`, `warn`, `debug`, `verbose`

## 10.2. 로그 레벨 설정

```typescript
const app = await NestFactory.create(AppModule, {
  logger: ['error', 'warn', 'log'],   // 운영에선 debug/verbose 제외
});
```

## 10.3. winston 연동

```bash
npm install nest-winston winston
```

```typescript
WinstonModule.forRoot({
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'app.log' }),
  ],
});
```

파일 저장·외부 로그 수집(ELK 등)으로 보낼 때 유용하다.
