---
title: "16. 클린 아키텍처"
date: 2026-09-17
weight: 16
tags: [nodejs, nestjs, clean-architecture]
description: "도메인을 프레임워크와 인프라로부터 격리하는 클린 아키텍처의 계층, 의존성 규칙, 포트와 어댑터를 정리한다."
---

> **클린 아키텍처**는 비즈니스 규칙(도메인)을 프레임워크·DB·외부 세부사항으로부터 격리한다. 의존성은 항상 **바깥에서 안쪽(도메인)으로** 향한다.

## 16.1. 계층

| 계층 | 책임 |
|---|---|
| 도메인 (Entity) | 핵심 비즈니스 규칙. 어디에도 의존하지 않음 |
| 애플리케이션 (Use Case) | 도메인을 조합한 시나리오 |
| 인터페이스 어댑터 | 컨트롤러·프레젠터·리포지토리 구현 |
| 인프라 | DB·외부 API·프레임워크 세부사항 |

## 16.2. 의존성 규칙

- 안쪽 계층은 바깥 계층을 **모른다**.
- 바깥 → 안쪽 의존만 허용. 도메인은 NestJS·TypeORM 을 import 하지 않는다.

## 16.3. 포트와 어댑터

도메인은 인터페이스(포트)만 정의하고, 인프라가 이를 구현(어댑터)한다. NestJS의 DI로 구현체를 주입한다.

```typescript
// 도메인 포트
export interface UserRepository {
  findById(id: string): Promise<User>;
}

// 인프라 어댑터
@Injectable()
export class TypeOrmUserRepository implements UserRepository { /* ... */ }

// 모듈에서 토큰으로 바인딩
{ provide: 'UserRepository', useClass: TypeOrmUserRepository }
```
