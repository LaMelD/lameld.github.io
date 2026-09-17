---
title: "7. 영속화"
date: 2026-09-17
weight: 7
tags: [nodejs, nestjs, typeorm, database]
description: "TypeORM 연결 설정, 엔티티 정의, 리포지토리 주입과 CRUD, synchronize 옵션의 주의점을 정리한다."
---

> **영속화(Persistence)** 는 데이터를 DB에 저장·조회하는 계층이다. NestJS는 주로 **TypeORM**(또는 Prisma)과 연동하며 리포지토리 패턴으로 접근한다.

## 7.1. TypeORM 연결

```bash
npm install @nestjs/typeorm typeorm pg
```

```typescript
TypeOrmModule.forRoot({
  type: 'postgres',
  host: 'localhost',
  port: 5432,
  database: 'app',
  entities: [User],
  synchronize: true,   // 개발용. 운영에서는 마이그레이션 사용
});
```

## 7.2. 엔티티

```typescript
import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column({ unique: true })
  email: string;
}
```

## 7.3. 리포지토리 주입과 CRUD

```typescript
@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User) private repo: Repository<User>,
  ) {}

  findAll() { return this.repo.find(); }
  create(dto: CreateUserDto) { return this.repo.save(dto); }
}
```

모듈에서 `TypeOrmModule.forFeature([User])` 로 리포지토리를 등록해야 주입된다.

> `synchronize: true` 는 스키마를 자동 동기화해 편하지만 데이터 유실 위험이 있어 **운영 환경에서는 마이그레이션**을 쓴다(부록 참고).
