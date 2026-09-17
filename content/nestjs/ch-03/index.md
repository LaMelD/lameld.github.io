---
title: "3. 프로바이더"
date: 2026-09-17
weight: 3
tags: [nodejs, nestjs, provider, di]
description: "비즈니스 로직을 담는 프로바이더와 @Injectable, 생성자 기반 의존성 주입, 토큰 기반 커스텀 프로바이더와 스코프를 정리한다."
---

> **프로바이더(Provider)** 는 비즈니스 로직을 담는 클래스로, NestJS의 **의존성 주입(DI)** 을 통해 다른 곳에 주입된다. 대표적으로 **서비스(Service)** 가 프로바이더다.

## 3.1. 서비스와 @Injectable

```bash
nest g service users
```

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()               // DI 컨테이너가 관리하는 프로바이더로 등록
export class UsersService {
  private users = [];
  findAll() { return this.users; }
  create(user) { this.users.push(user); return user; }
}
```

## 3.2. 의존성 주입

컨트롤러 생성자에 타입을 선언하면 Nest가 인스턴스를 자동으로 넣어 준다.

```typescript
@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll() { return this.usersService.findAll(); }
}
```

## 3.3. 커스텀 프로바이더

토큰 기반으로 값·클래스·팩토리를 주입할 수 있다.

```typescript
{
  provide: 'CONFIG',                  // 토큰
  useValue: { apiUrl: 'https://...' } // 값 주입
}
// 주입: constructor(@Inject('CONFIG') private cfg) {}
```

- `useClass` — 클래스 주입 (인터페이스 구현 교체)
- `useFactory` — 런타임에 계산해서 주입 (비동기 가능)
- `useValue` — 상수·목(mock) 객체 주입

> 기본 스코프는 **싱글턴**이다. 요청마다 새 인스턴스가 필요하면 `@Injectable({ scope: Scope.REQUEST })` 를 쓴다.
