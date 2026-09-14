---
title: "RP-CHAT: 캐릭터와 1:1로 대화하는 RP 챗봇"
date: 2026-05-29
weight: 1
tags: [project, python, fastapi, react, typescript, llm, docker]
description: "미리 정의한 캐릭터와 대본 형식으로 대화하는 학습용 챗봇. FastAPI + SQLite + React/Vite + nginx를 Docker Compose로 묶었다."
---

저장소: [github.com/LaMelD/RP-CHAT](https://github.com/LaMelD/RP-CHAT)

미리 정의된 캐릭터와 1:1로 대화하는 학습용 챗봇이다. 응답은 소설 대본처럼 **지문(이탤릭) + 대사(굵게, 큰따옴표)** 형식으로 스트리밍된다. 처음에는 도구 호출을 하는 ReAct 어시스턴트로 시작했다가, 캐릭터 일관성을 우선하기로 하고 캐릭터 RP 전용으로 방향을 바꿨다.

## 무엇이 다른가

- 세션이 아니라 **캐릭터**가 중심이다. 캐릭터마다 이름, 한 줄 요약, 페르소나, 말투, 오프닝 장면을 가진다.
- 캐릭터당 대화는 하나뿐이다(1:1 누적). 새로 시작하고 싶으면 대화를 리셋한다.
- 도구 호출(웹 검색, 계산기)은 일부러 뺐다. 캐릭터가 갑자기 검색 결과를 읽어 주기 시작하면 몰입이 깨진다.
- 첫 부팅 때 시드 캐릭터 3종(`제인`, `루카스`, `미아`)이 자동으로 들어간다.

## 스택과 구성

| 계층 | 선택 |
|---|---|
| 백엔드 | FastAPI, SQLAlchemy(asyncio) + SQLite, OpenAI SDK, SSE 스트리밍 |
| 프론트엔드 | React + TypeScript, Vite, Tailwind CSS, react-markdown |
| 인그레스 | nginx (같은 오리진으로 API·SSE·Vite HMR을 모두 통과) |
| 실행 | Docker Compose (dev / prod 두 파일) |
| 테스트 | pytest(백엔드), vitest(프론트), Playwright MCP로 E2E 시나리오 |

브라우저는 nginx 하나만 바라본다. 백엔드에 CORS 미들웨어를 두지 않았기 때문에 호스트에서 백엔드와 프론트를 따로 띄우는 방식은 지원하지 않는다. 실행 경로를 하나로 줄이면 "내 환경에서는 되는데"가 사라진다.

## 데이터 모델

테이블은 세 개다.

| 테이블 | 역할 |
|---|---|
| `characters` | 슬러그 id, 이름, 요약(≤80자), 페르소나, 말투, 오프닝 장면, 아바타 색 |
| `conversations` | 캐릭터당 하나. `character_id`에 UNIQUE를 걸어 1:1을 DB에서 강제 |
| `messages` | `user` / `assistant` 메시지. 지문과 대사는 파싱하지 않고 한 문자열로 저장하고 프론트가 마크다운으로 렌더링 |

`conversations`를 따로 둔 이유는 다음 버전의 멀티 캐릭터 룸 때문이다. 그때 이 테이블이 룸이 되고 캐릭터 결합표가 붙는다. 지금은 FK 하나와 UNIQUE 한 줄만 부담한다.

## 프롬프트 전략

시스템 프롬프트는 캐릭터 필드(요약, 페르소나, 말투)와 출력 포맷 규칙을 합쳐서 만든다. 오프닝 장면은 프롬프트에 넣지 않고 **첫 어시스턴트 메시지로 DB에 시드**한다. 그래야 대화 리셋 뒤에도 같은 장면에서 다시 시작하고, 프롬프트와 화면이 어긋나지 않는다.

## API

- 캐릭터 CRUD: `GET/POST/PUT/DELETE /api/characters`
- 대화: `GET /api/characters/{id}/conversation`, 리셋은 `DELETE .../conversation/messages` (메시지만 지우고 오프닝을 다시 넣는다)
- 채팅: SSE로 토큰을 스트리밍하고, 완료·오류 이벤트를 따로 보낸다
- 모델: 사용 가능한 모델 목록 조회

## 실행

```bash
cp .env.example .env        # OPENAI_API_KEY 입력
docker compose -f docker-compose.dev.yml up --build
# http://localhost:8081
```

프로덕션은 `docker-compose.prod.yml`을 쓴다. 프론트엔드가 빌드되어 nginx 이미지에 들어간다.

## v1의 한계와 다음

- 한 세션에 캐릭터 한 명. 여러 캐릭터가 한 방에서 대화하는 모드는 다음 버전.
- 토큰 한도 슬라이딩 윈도우가 없어서 대화가 아주 길어지면 LLM 호출이 실패할 수 있다.
- 설계 문서와 구현 계획은 저장소의 `docs/superpowers/` 아래에 있다. 설계를 먼저 쓰고 계획으로 쪼개 구현하는 흐름을 그대로 남겨 두었다.
