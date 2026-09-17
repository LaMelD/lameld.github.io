---
title: "17. 테스트 자동화"
date: 2026-09-17
weight: 17
tags: [nodejs, nestjs, jest, testing]
description: "Test.createTestingModule을 이용한 단위 테스트, 의존성 모킹, e2e 테스트와 실행 명령을 정리한다."
---

> NestJS는 **Jest** 기반 테스트를 기본 제공한다. 단위 테스트는 프로바이더 로직을, e2e 테스트는 요청→응답 전체 흐름을 검증한다.

## 17.1. 단위 테스트

`Test.createTestingModule` 로 테스트용 모듈을 구성하고 의존성을 모킹한다.

```typescript
describe('UsersService', () => {
  let service: UsersService;

  beforeEach(async () => {
    const module = await Test.createTestingModule({
      providers: [UsersService],
    }).compile();
    service = module.get(UsersService);
  });

  it('should return users', () => {
    expect(service.findAll()).toEqual([]);
  });
});
```

## 17.2. 모킹

외부 의존성(리포지토리 등)은 목 객체로 대체해 로직만 검증한다.

```typescript
{ provide: getRepositoryToken(User), useValue: { find: jest.fn() } }
```

## 17.3. e2e 테스트

```typescript
const app = moduleFixture.createNestApplication();
await app.init();

return request(app.getHttpServer())
  .get('/users')
  .expect(200);
```

## 17.4. 실행

```bash
npm run test       # 단위 테스트
npm run test:e2e   # e2e 테스트
npm run test:cov   # 커버리지
```
