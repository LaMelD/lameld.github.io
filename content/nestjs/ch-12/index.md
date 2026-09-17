---
title: "12. 인터셉터"
date: 2026-09-17
weight: 12
tags: [nodejs, nestjs, interceptor, rxjs]
description: "핸들러 실행 전후에 로직을 끼워 넣는 인터셉터의 기본 구조와 적용, 응답 표준화·로깅·캐싱 활용 예를 정리한다."
---

> **인터셉터(Interceptor)** 는 핸들러 실행 **전후**에 로직을 끼워 넣는다. 응답 변환·로깅·캐싱·타임아웃 등에 쓰며 RxJS 스트림을 다룬다.

## 12.1. 기본 구조

```typescript
@Injectable()
export class TransformInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const now = Date.now();
    return next
      .handle()                          // 핸들러 실행
      .pipe(
        map(data => ({ data })),         // 응답을 { data } 로 감싸기
        tap(() => console.log(`${Date.now() - now}ms`)),
      );
  }
}
```

## 12.2. 적용

```typescript
@UseInterceptors(TransformInterceptor)
@Controller('users')
export class UsersController {}
```

## 12.3. 활용 예

- **응답 표준화** — 모든 응답을 `{ data, timestamp }` 형태로 통일
- **로깅** — 요청 처리 시간 측정
- **캐싱** — `CacheInterceptor` 로 GET 응답 캐시
