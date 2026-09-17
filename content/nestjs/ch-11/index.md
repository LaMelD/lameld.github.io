---
title: "11. 예외 필터"
date: 2026-09-17
weight: 11
tags: [nodejs, nestjs, exception-filter]
description: "내장 HttpException과 커스텀 예외 필터 작성, 핸들러·컨트롤러·전역 단위 적용 범위를 정리한다."
---

> **예외 필터(Exception Filter)** 는 처리되지 않은 예외를 가로채 일관된 에러 응답으로 변환한다.

## 11.1. 내장 HttpException

```typescript
throw new HttpException('권한 없음', HttpStatus.FORBIDDEN);
throw new NotFoundException('사용자를 찾을 수 없습니다');  // 단축 예외
```

- `BadRequestException`, `UnauthorizedException`, `NotFoundException` 등 다수 제공

## 11.2. 커스텀 예외 필터

```typescript
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const res = ctx.getResponse();
    const status = exception.getStatus();

    res.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      message: exception.message,
    });
  }
}
```

## 11.3. 적용 범위

```typescript
@UseFilters(HttpExceptionFilter)          // 컨트롤러/핸들러 단위
// 또는 main.ts 에서 전역 적용
app.useGlobalFilters(new HttpExceptionFilter());
```
