---
title: "6. 파이프와 유효성 검사"
date: 2026-09-17
weight: 6
tags: [nodejs, nestjs, pipe, validation]
description: "내장 파이프, ValidationPipe와 class-validator를 이용한 DTO 검증, 커스텀 파이프 작성법을 정리한다."
---

> **파이프(Pipe)** 는 컨트롤러 핸들러에 도달하기 전에 입력값을 **변환**하거나 **검증**한다. 잘못된 입력은 여기서 걸러 예외로 응답한다.

## 6.1. 내장 파이프

```typescript
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) {  // 문자열 → 숫자 변환·검증
  return id;
}
```

- `ParseIntPipe`, `ParseBoolPipe`, `ParseUUIDPipe`, `DefaultValuePipe` 등

## 6.2. ValidationPipe + class-validator

DTO에 데커레이터로 규칙을 선언하고 전역 파이프로 자동 검증한다.

```bash
npm install class-validator class-transformer
```

```typescript
// main.ts
app.useGlobalPipes(new ValidationPipe({ whitelist: true }));
```

```typescript
// create-user.dto.ts
import { IsEmail, IsString, MinLength } from 'class-validator';

export class CreateUserDto {
  @IsString()
  @MinLength(2)
  name: string;

  @IsEmail()
  email: string;
}
```

검증 실패 시 자동으로 `400 Bad Request` 를 반환한다.

## 6.3. 커스텀 파이프

```typescript
@Injectable()
export class TrimPipe implements PipeTransform {
  transform(value: any) {
    return typeof value === 'string' ? value.trim() : value;
  }
}
```
