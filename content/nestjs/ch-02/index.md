---
title: "2. 인터페이스"
date: 2026-09-17
weight: 2
tags: [nodejs, nestjs, controller, dto]
description: "HTTP 요청의 진입점인 컨트롤러와 라우트 핸들러 데커레이터, 계층 간 데이터 형태를 정의하는 DTO를 정리한다."
---

> **인터페이스 계층**은 들어오는 HTTP 요청을 받아 결과를 응답하는 진입점이다. NestJS 에서는 **컨트롤러(Controller)** 가 라우팅을 맡아 이 역할을 한다.

## 2.1. 컨트롤러

요청 경로(라우트)와 그것을 처리할 메서드를 연결한다. 클래스에 `@Controller()`, 메서드에 HTTP 메서드 데커레이터를 붙인다.

```bash
nest g controller users
```

```typescript
import { Controller, Get, Post, Param, Query, Body } from '@nestjs/common';

@Controller('users')           // 공통 경로 prefix: /users
export class UsersController {
  @Get()                       // GET /users
  findAll(@Query('page') page: number) {
    return `page ${page}`;
  }

  @Get(':id')                  // GET /users/:id
  findOne(@Param('id') id: string) {
    return `user ${id}`;
  }

  @Post()                      // POST /users
  create(@Body() dto: CreateUserDto) {
    return dto;
  }
}
```

### 라우트 핸들러 데커레이터

| 데커레이터 | 대상 |
|---|---|
| `@Get` `@Post` `@Put` `@Patch` `@Delete` | HTTP 메서드별 라우트 |
| `@Param(key)` | 경로 파라미터 (`/users/:id`) |
| `@Query(key)` | 쿼리스트링 (`?page=1`) |
| `@Body()` | 요청 본문(JSON) |
| `@Headers()` `@Req()` `@Res()` | 헤더·요청·응답 객체 |

## 2.2. DTO (Data Transfer Object)

계층 간에 오가는 데이터의 형태를 정의하는 객체. 보통 `class` 로 만들어 유효성 검사(6장)와 함께 쓴다.

```typescript
export class CreateUserDto {
  name: string;
  email: string;
}
```
