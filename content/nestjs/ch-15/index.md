---
title: "15. CQRS"
date: 2026-09-17
weight: 15
tags: [nodejs, nestjs, cqrs]
description: "명령과 쿼리를 분리하는 CQRS 패턴을 @nestjs/cqrs의 CommandBus, QueryBus, EventBus로 구현하는 방법을 정리한다."
---

> **CQRS (Command Query Responsibility Segregation)** 는 상태를 바꾸는 **명령(Command)** 과 조회하는 **쿼리(Query)** 를 분리하는 패턴이다. 복잡한 도메인에서 읽기/쓰기 모델을 독립적으로 최적화한다.

## 15.1. 설치

```bash
npm install @nestjs/cqrs
```

## 15.2. 명령(Command)과 핸들러

```typescript
export class CreateUserCommand {
  constructor(public readonly name: string) {}
}

@CommandHandler(CreateUserCommand)
export class CreateUserHandler implements ICommandHandler<CreateUserCommand> {
  async execute(command: CreateUserCommand) {
    // 쓰기 로직
  }
}
```

```typescript
// 호출
this.commandBus.execute(new CreateUserCommand('Dexter'));
```

## 15.3. 쿼리·이벤트

- **QueryBus** — 읽기 전용 조회를 별도 핸들러로 분리
- **EventBus** — 명령 처리 후 도메인 이벤트를 발행해 후속 작업 트리거

> CQRS 는 구조적 오버헤드가 있다. 단순 CRUD 에는 과하고, 도메인 규칙이 복잡하거나 읽기/쓰기 부하가 크게 다를 때 도입한다.
