---
title: "9. JWT 인증/인가"
date: 2026-09-17
weight: 9
tags: [nodejs, nestjs, jwt, passport, auth]
description: "Passport와 JWT 조합으로 토큰을 발급하고 전략과 가드로 인증하는 방법, 역할 기반 인가를 정리한다."
---

> **인증(Authentication)** 은 "누구인지" 확인, **인가(Authorization)** 는 "무엇을 할 수 있는지" 확인이다. NestJS는 **Passport** + **JWT** 조합을 주로 쓴다.

## 9.1. 설치

```bash
npm install @nestjs/passport passport @nestjs/jwt passport-jwt
```

## 9.2. JWT 발급

```typescript
@Injectable()
export class AuthService {
  constructor(private jwt: JwtService) {}

  login(user) {
    const payload = { sub: user.id, email: user.email };
    return { access_token: this.jwt.sign(payload) };
  }
}
```

## 9.3. JWT 전략과 가드

```typescript
@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      secretOrKey: process.env.JWT_SECRET,
    });
  }
  validate(payload) {
    return { userId: payload.sub, email: payload.email };
  }
}
```

```typescript
@UseGuards(AuthGuard('jwt'))   // 토큰 없거나 유효하지 않으면 401
@Get('profile')
getProfile(@Req() req) { return req.user; }
```

## 9.4. 역할 기반 인가

```typescript
@UseGuards(AuthGuard('jwt'), RolesGuard)
@Roles('admin')                // 커스텀 데커레이터 + 가드로 권한 체크
@Delete(':id')
remove(@Param('id') id: string) {}
```
