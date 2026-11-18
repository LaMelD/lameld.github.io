---
title: "SQLite의 확장은 더 큰 DB가 아니라 더 많은 DB: GeekNews 아티클 읽기"
date: 2026-11-18T13:40:00+09:00
weight: 20
tags: [reading, sqlite, database, turso, supabase]
description: "GeekNews 아티클은 SQLite를 한 대의 큰 DB가 아니라 사용자·사이트마다 작은 DB를 여러 개 두는 방식으로 쓰는 흐름을 정리했다. lobste.rs·Julia Evans의 단일 서버 운영기, Poke의 사이트별 DB, Supabase의 Turso 인수를 원 출처와 대조하고, 아티클이 빠뜨린 lobste.rs 데이터 손실 사고와 Durable Objects 한도를 덧붙였다."
---

> 이 글은 GeekNews가 직접 쓴 아티클 [「SQLite의 확장은 더 큰 DB가 아니라 더 많은 DB」](https://news.hada.io/article/sqlite-a-database-for-every-app)(2026-10-08, GeekNews Weekly 202641)를 읽고 정리한 글이다. 아티클은 외부 글 하나를 요약한 것이 아니다. GeekNews에 올라온 여러 글을 엮어 쓴 글이다. 그래서 핵심 사례 5건은 GeekNews 토픽을 거쳐 **원 출처**까지 따라가 대조했다. 원 출처와 다르거나 아티클이 빠뜨린 내용은 따로 표시했다. 2026-11-18에 확인했다.

## 한눈에 보기

- **주장:** SQLite를 키우는 방법은 DB 하나를 크게 만드는 것이 아니다. 사용자·테넌트·사이트마다 작은 DB를 하나씩 두는 것이다.
- **왜 되나:** SQLite는 DB 하나에 쓰기가 한 번에 하나만 돈다. DB를 나누면 그 제약이 DB마다 따로 걸린다. 제약이 사라지지는 않지만, 제약을 나눠 갖는 범위가 줄어든다.
- **단일 서버에서도 실용적이다.** lobste.rs는 MariaDB를 SQLite로 바꿨다. 다만 DB 서버를 없애도 운영 지식은 그대로 필요하다. 통계 갱신, 긴 트랜잭션, 백업이 그 예다.
- **사례:** Poke는 AI가 만든 웹사이트마다 Turso DB를 하나씩 붙인다. Supabase는 매주 DB를 100만 개 넘게 만든다며 2026-10-02에 Turso 인수를 발표했다.
- **기준:** 누가 같은 데이터를 함께 읽고 동시에 바꿔야 하는지 본다. 사용자 사이 조인·집계나 여러 소유자를 건드리는 트랜잭션이 핵심이면 중앙 Postgres가 낫다.

## 서버용 SQLite 도구는 백업·복제부터 자랐다

아티클은 SQLite가 서버에서 진지한 선택지가 된 과정을 GeekNews 글로 짚는다. 이 시기의 발전은 SQL 문법이 아니라 **백업·복제·복구** 같은 운영 도구 쪽이었다.

| 시기 | 도구·글 | 한 일 |
|---|---|---|
| 2019 | [Dqlite](https://news.hada.io/topic?id=441) | SQLite에 Raft 복제를 붙였다 |
| 2021 | [rqlite](https://news.hada.io/topic?id=3636) | SQLite + 분산 합의 + HTTP API |
| 2022 | [SQLite를 Primary DB로?](https://news.hada.io/topic?id=6498) | 주 DB로 써 봤는지 묻는 글이 화제가 됐다 |
| 2022 | [Ben Johnson의 서버사이드 SQLite 글](https://news.hada.io/topic?id=6557) | Fly.io 합류. Litestream으로 외부 스토리지에 계속 백업한다 |
| 2022 | [LiteFS](https://news.hada.io/topic?id=7058) | 파일시스템 계층 복제. 읽기는 가까운 노드, 쓰기는 지정 노드에서 한다 |

아티클이 붙인 단서가 중요하다. 백업이 있다고 다른 노드가 바로 쓰기를 넘겨받는 것은 아니다. 읽기 복제본은 최신 데이터를 보장하지 않는다.

## 단일 서버: lobste.rs와 Julia Evans

SQLite는 앱 프로세스 안에서 함수 호출로 쿼리를 돈다. 네트워크 왕복도, 따로 운영할 DB 서버도 없다. N+1 쿼리가 덜 아픈 이유다. 다만 쿼리 자체의 비용은 그대로 남는다([GN 11561](https://news.hada.io/topic?id=11561)).

**lobste.rs ([원문](https://lobste.rs/s/ko1ji1/lobste_rs_is_now_running_on_sqlite), 2026-07-13)**

- 2019년 이슈 #539에서 시작한 MariaDB → SQLite 전환이다. 2월 21일 첫 배포는 성능 문제로 되돌렸다. 원문: "so we decided to revert"
- full table scan 2건과 N+1을 고친 뒤 7월 11일 두 번째 배포에 성공했다.
- 원문: "cpu usage is down, memory usage is down". 비용은 원문: "1/2 the vps cost once mariadb vps is taken down"이다. 아티클의 "서버 비용 절감"은 **MariaDB VPS를 내린 뒤**의 이야기다.
- MariaDB 함수를 대신할 사용자 정의 함수(`regexp`, `if`, `stddev`)를 만들었다. unsigned bigint는 bigint로 바꿨다. 댓글 기준 DB 파일은 약 3.8GB다.
- **아티클이 빠뜨린 사고:** 운영자 댓글에 따르면 첫 마이그레이션이 댓글 투표를 전부 지웠다. 원문: "This initial migration unexpectedly deleted every vote on a comment, 3.2 million rows in the database." 복구 뒤에는 성능이 크게 떨어졌다. 봇 크롤링도 원인으로 꼽혔다. 운영자는 7월 21일 댓글에서 원문: "we're back in a fast, stable state"라고 알렸다. 아티클은 "복구 작업 뒤 성능 문제"라고만 적었다.

**Julia Evans ([원문](https://jvns.ca/blog/2026/07/17/learning-about-running-sqlite/), 2026-07-17)**

- Mess with DNS를 2022년부터 SQLite로 운영해 왔다. 테이블은 DB 파일 3개로 나눴다.
- FTS5 검색이 느렸는데 `ANALYZE`로 통계를 갱신하자 빨라졌다. 원문: "went from taking 5 seconds to like 0.05 seconds". 정확한 측정이 아니라 어림값이라고 본인이 적었다.
- 큰 `DELETE`를 긴 트랜잭션 하나로 돌리자 다른 쓰기가 기다리다 타임아웃이 났다. 원문: "My approach so far has been to just do these cleanup operations in small batches". DELETE가 왜 느렸는지는 아직 모른다고 했다.
- WAL은 원문: "turning on WAL mode like all the blog posts said to do"라고만 했다. 타임아웃은 5초다. 원문에 PRAGMA 줄은 없다.
- Litestream에 `retention: 400h`를 걸었지만 의도대로 도는지는 확인하지 못했다고 했다.

아티클의 결론은 이렇다. 원문: "DB 서버를 없애도 DB 운영 지식까지 없어지지는 않습니다."

## 더 많은 DB: 단일 writer를 쪼개는 방식

SQLite는 DB 하나에 쓰기 작업이 한 번에 하나만 진행된다. 그래서 서비스 전체를 DB 하나에 담으면 쓰기가 한 줄로 선다. 최근 흐름은 데이터를 독립 단위로 나누는 쪽이다. 서비스 전체가 커도 DB 하나가 맡는 범위는 작게 유지한다([GN 19565](https://news.hada.io/topic?id=19565)).

**Cloudflare Durable Objects ([문서](https://developers.cloudflare.com/durable-objects/best-practices/access-durable-objects-storage/))**

- 원문: "each individual Durable Object has its own SQLite database that you can read from and write to synchronously."
- SQL API는 생성자에 넘어오는 `ctx.storage.sql`이다. SQLite 백엔드 클래스에서만 쓸 수 있고, 새 네임스페이스에는 SQLite 백엔드가 권장된다.
- [한도 문서](https://developers.cloudflare.com/durable-objects/platform/limits/)(2026-11-13 갱신)의 표는 객체당 저장 한도를 10GB로 적었다. 같은 페이지 FAQ는 "10 GB on Workers Paid, or 1 GB on the Free plan"이라고 해서 표와 다르다. Free 플랜에서 쓸 거라면 직접 확인해야 한다.
- 한도를 넘으면 쓰기가 `database or disk is full: SQLITE_FULL`로 실패한다. 읽기와 DELETE는 계속 된다.

**Poke ([Turso 고객 사례](https://turso.tech/blog/how-poke-gives-every-user-their-own-database), 2026-08-20)**

iMessage 개인 비서 Poke는 AI가 만든 웹사이트마다 DB를 하나씩 붙인다. 원문: "A Turso database gets provisioned automatically, one per artifact." **Turso가 직접 쓴 고객 사례**라는 점은 감안해서 읽어야 한다.

- **중앙 DB:** 원문: "Poke already uses PlanetScale for its core system data: user accounts, agent state, and internal operations."
- **사이트별 DB:** 사이트를 Vercel에 배포할 때 DB URL을 환경변수로 넣는다. 사례 공개 시점까지 원문: "10,000 databases created to date"
- **나눈 이유:** AI가 짠 SQL의 원문: "really really bad database access patterns"와 예상하지 못한 트래픽이 공유 DB를 흔들지 않게 하려는 것이다. 보안과 비용도 이유로 들었다.
- **아직 없는 것:** 가비지 컬렉션이 없다. 버려진 사이트의 DB도 그대로 남는다. 아티클은 Turso의 서버리스 과금이라 유휴 DB 부담이 낮다고 봤지만, 근거 수치는 없다.
- **검토한 대안:** Neon, Supabase, Cloudflare, PlanetScale

아티클의 단서도 같이 적어 둔다. 원문: "DB를 나누는 것만으로 성능과 보안이 완전히 격리되지는 않습니다." 공유 자원 한도와 DB별 접근 권한은 따로 챙겨야 한다.

## 나누지 않는 편이 나을 때

공유 테이블은 `user_id`·`tenant_id` 열로 데이터를 구분한다. 사이트별 DB는 어느 DB를 여느냐가 곧 경계다. 한 사용자 데이터를 옮기거나 되살리기는 쉬워진다. 대신 수천 개 DB의 스키마 변경, 백업, 삭제가 새 일이 된다. 전체 사용자를 대상으로 한 검색·집계도 번거로워진다.

아티클이 든, 중앙 Postgres가 나은 경우는 다음과 같다.

- 여러 앱이 같은 데이터를 자주 갱신한다.
- 사용자 사이 조인·집계가 서비스의 핵심이다.
- 소유자가 다른 데이터를 한 트랜잭션으로 자주 바꾼다.

SQLite도 트랜잭션은 지원한다. 문제는 함께 바뀌어야 할 데이터를 다른 DB로 나눴는지다. Poke도 공유 핵심 상태는 중앙 DB에 두고, 서로 독립적인 생성 사이트만 떼어 냈다.

## Supabase는 왜 Turso를 샀나

Supabase는 Postgres를 중심으로 인증·파일 저장·실시간 갱신·서버 함수를 묶은 앱 백엔드다. 그런 Supabase가 SQLite 쪽 회사를 샀다([발표](https://supabase.com/blog/supabase-is-acquiring-turso), 2026-10-02. GeekNews 토픽은 10-03).

- **규모:** 원문: "Supabase is already launching over one million databases per week." 회사가 밝힌 수치다.
- **역할 분담:** 원문: "SQLite is well suited to these small, on-demand workloads". 앱이 커지면 원문: "a clear path into the broader Supabase ecosystem"으로 간다. 원문: "Turso will continue its work on SQLite."
- **구조:** 서버 한 대가 DB 수백만 개를 맡는다. 필요할 때 올리고, 쓰지 않으면 내린다.
- **사람:** Glauber Costa와 Pekka Enberg가 Supabase에 합류한다. Glauber는 agentic infrastructure 작업을 이끈다. Turso 사용처로는 Superhuman, Sauna.ai, CTO.new, Mastra를 들었다.
- **없는 것:** 인수 조건, 가격 변경, Turso의 Rust 엔진을 어떻게 할지는 발표에 없다. SQLite에서 Postgres로 자동 전환하는 기능이 나온 것도 아니다. 아티클도 이 점을 분명히 적었다.

## 실무에서 먼저 볼 SQLite 설정

아티클과 원 출처에 나온 문제는 대부분 아래 설정과 습관으로 미리 줄일 수 있다. 출처는 SQLite 공식 문서([WAL](https://www.sqlite.org/wal.html), [PRAGMA](https://www.sqlite.org/pragma.html), [ANALYZE](https://www.sqlite.org/lang_analyze.html))다.

```sql
PRAGMA journal_mode = WAL;   -- 읽기와 쓰기 하나가 서로 막지 않는다. DB 파일에 저장되는 설정이다
PRAGMA busy_timeout = 5000;  -- 잠겨 있으면 바로 SQLITE_BUSY를 내지 않고 5초까지 기다린다. 연결마다 건다
PRAGMA optimize;             -- 필요한 테이블만 ANALYZE한다. 주기적으로, 또는 연결을 닫기 전에 돌린다
```

- **통계:** 인덱스를 새로 만들었거나 데이터가 크게 바뀌었으면 `ANALYZE` 또는 `PRAGMA optimize`를 돌린다. Julia Evans의 5초 → 0.05초가 이 경우다.
- **큰 삭제·갱신:** 작은 배치로 나눈다. 쓰기는 하나뿐이라, 긴 트랜잭션은 다른 쓰기를 모두 기다리게 만든다.
- **마이그레이션 검증:** 옮긴 뒤 테이블별 행 수를 원본과 맞춰 본다. lobste.rs에서는 첫 마이그레이션이 투표 320만 행을 예상치 못하게 지웠다.

## 읽고 나서

- **"더 많은 DB"는 데이터 소유 구조에서 나온다.** 엔진 성능 이야기가 아니다. 데이터가 사용자·사이트별로 깔끔하게 갈리면 DB를 나누는 것이 쉽고 싸다. 서로 엮이는 데이터가 핵심이면 나눈 만큼 일이 늘어난다.
- **사례 출처의 무게가 다르다.** lobste.rs와 Julia Evans는 운영자 본인의 기록이다. Poke는 Turso가 쓴 고객 사례이고, Supabase 수치는 인수 발표의 회사 주장이다.
- **아티클은 낙관 쪽으로 다듬어져 있다.** lobste.rs의 데이터 손실과 Poke에 DB 정리 기능이 없다는 점은 원 출처에만 있다. 실제로 도입한다면 이 두 가지가 운영 부담의 실체다.
- **AI 시대와의 연결은 설득력이 있다.** 에이전트가 앱을 대량으로 찍어 내면 DB도 앱 수만큼 생긴다. 이때 기준은 "익숙한 서버 DB를 띄울까"가 아니다. 아티클의 말대로 "가까이에 가볍게 두는 것으로 충분한지"다.

## 참고

- GeekNews 아티클: [SQLite의 확장은 더 큰 DB가 아니라 더 많은 DB](https://news.hada.io/article/sqlite-a-database-for-every-app)
- 원 출처: [lobste.rs is now running on SQLite](https://lobste.rs/s/ko1ji1/lobste_rs_is_now_running_on_sqlite) · [Julia Evans, learning about running SQLite](https://jvns.ca/blog/2026/07/17/learning-about-running-sqlite/) · [Turso, How Poke gives every user their own database](https://turso.tech/blog/how-poke-gives-every-user-their-own-database) · [Supabase is acquiring Turso](https://supabase.com/blog/supabase-is-acquiring-turso) · [Cloudflare Durable Objects 스토리지](https://developers.cloudflare.com/durable-objects/best-practices/access-durable-objects-storage/) · [Durable Objects 한도](https://developers.cloudflare.com/durable-objects/platform/limits/)
- GeekNews 토픽: [Postgres 하나면 대부분의 애플리케이션을 만들 수 있다](https://news.hada.io/topic?id=26449) · [Poke](https://news.hada.io/topic?id=32891) · [lobste.rs 전환](https://news.hada.io/topic?id=31413) · [SQLite를 운영하며 새롭게 배운 몇 가지](https://news.hada.io/topic?id=31556) · [Supabase의 Turso 인수](https://news.hada.io/topic?id=34676)
