---
title: "14. 헬스 체크"
date: 2026-09-17
weight: 14
tags: [nodejs, nestjs, terminus, health-check]
description: "@nestjs/terminus로 헬스 컨트롤러를 만들고 DB, HTTP, 디스크, 메모리 인디케이터로 상태를 점검하는 방법을 정리한다."
---

> **헬스 체크**는 앱과 의존 서비스(DB·외부 API·디스크)의 상태를 점검하는 엔드포인트를 제공한다. 로드밸런서·쿠버네티스 프로브에 쓰인다. `@nestjs/terminus` 를 사용한다.

## 14.1. 설치

```bash
npm install @nestjs/terminus
```

## 14.2. 헬스 컨트롤러

```typescript
@Controller('health')
export class HealthController {
  constructor(
    private health: HealthCheckService,
    private db: TypeOrmHealthIndicator,
    private http: HttpHealthIndicator,
  ) {}

  @Get()
  @HealthCheck()
  check() {
    return this.health.check([
      () => this.db.pingCheck('database'),
      () => this.http.pingCheck('external', 'https://example.com'),
    ]);
  }
}
```

## 14.3. 인디케이터

- `TypeOrmHealthIndicator` — DB 연결 상태
- `HttpHealthIndicator` — 외부 URL 응답
- `DiskHealthIndicator` · `MemoryHealthIndicator` — 디스크·메모리 임계치
