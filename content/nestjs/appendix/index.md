---
title: "부록"
date: 2026-09-17
weight: 99
tags: [nodejs, nestjs, typeorm, config]
description: "TypeOrmModule.forRootAsync와 ConfigService로 ormconfig.json을 대신해 DB 설정을 동적으로 구성하는 팁을 정리한다."
---

> 부록 — 실무에서 자주 쓰는 설정 팁 모음.

## 1. ormconfig.json 동적 생성

과거 TypeORM은 `ormconfig.json` 정적 파일로 DB 설정을 읽었지만, 환경 변수 기반으로 동적 구성하는 것이 안전하다. NestJS 에서는 `TypeOrmModule.forRootAsync` + `ConfigService` 조합을 쓴다.

```typescript
TypeOrmModule.forRootAsync({
  imports: [ConfigModule],
  inject: [ConfigService],
  useFactory: (config: ConfigService) => ({
    type: 'postgres',
    host: config.get('DB_HOST'),
    port: config.get<number>('DB_PORT'),
    username: config.get('DB_USER'),
    password: config.get('DB_PASSWORD'),
    database: config.get('DB_NAME'),
    entities: [__dirname + '/**/*.entity{.ts,.js}'],
    synchronize: config.get('NODE_ENV') !== 'production',
  }),
});
```

> 접속 정보(특히 비밀번호)를 `ormconfig.json` 에 커밋하지 않는다. `.env` 로 관리하고 위처럼 주입한다.
