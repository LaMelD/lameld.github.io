---
title: "13. 태스크 스케줄링"
date: 2026-09-17
weight: 13
tags: [nodejs, nestjs, schedule, cron]
description: "@nestjs/schedule로 Cron, Interval, Timeout 작업을 선언적으로 등록하고 SchedulerRegistry로 동적 스케줄을 다루는 법을 정리한다."
---

> **태스크 스케줄링**은 정해진 시각·주기로 작업을 실행한다. `@nestjs/schedule` 로 cron·인터벌·타임아웃을 선언적으로 등록한다.

## 13.1. 설치와 등록

```bash
npm install @nestjs/schedule
```

```typescript
@Module({ imports: [ScheduleModule.forRoot()] })
export class AppModule {}
```

## 13.2. Cron / Interval / Timeout

```typescript
@Injectable()
export class TasksService {
  @Cron('0 0 * * *')                    // 매일 자정 (cron 표현식)
  handleDailyBatch() {}

  @Cron(CronExpression.EVERY_30_SECONDS)
  handleFrequent() {}

  @Interval(10000)                      // 10초마다
  handleInterval() {}

  @Timeout(5000)                        // 부팅 5초 후 1회
  handleTimeout() {}
}
```

## 13.3. 동적 스케줄

`SchedulerRegistry` 를 주입하면 런타임에 잡을 추가·삭제·중지할 수 있다.
