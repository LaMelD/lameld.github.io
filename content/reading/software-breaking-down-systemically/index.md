---
title: "소프트웨어가 구조적으로 무너지고 있다: Breck-McKye의 2026년 장애 진단 읽기"
date: 2026-11-17T16:00:00+09:00
weight: 10
tags: [reading, reliability, supply-chain, decoherence, php2shell]
description: "Jimmy Breck-McKye는 2026년의 대형 장애와 보안 사고가 원인은 달라도 같은 구조에서 나왔다고 본다. 클라우드·오픈소스로 통제를 내준 데 이어 AI가 비정합성(decoherence)을 쌓는다는 주장이다. 원문이 든 사고 4건을 블로그 기록과 대조하고, HN 토론의 반론을 함께 정리했다."
---

> 이 글은 GeekNews [「소프트웨어가 구조적으로 무너지고 있다」](https://news.hada.io/topic?id=37303)(2026-11-17)에서 소개한 Jimmy Breck-McKye의 에세이 [Software is breaking down at a systemic level](https://www.breck-mckye.com/blog/2026/11/software-is-breaking-down-at-a-systemic-level/)(2026-11-15)를 읽고 정리한 글이다. 원문은 해결책을 내놓지 않는 **진단·전망 에세이**다. 원문 주장과 이 블로그 AI 브리핑에 기록한 사실이 다르면 둘 다 적었다. [Hacker News 토론](https://news.ycombinator.com/item?id=50520866)(조회 시점 362점, 댓글 226개)의 반론도 함께 실었다. 2026-11-17에 확인했다.

## 한눈에 보기

- **주장:** 2026년의 장애와 해킹은 직접 원인이 서로 다르다. 하지만 아무도 전체를 이해하지 못하는 복잡한 시스템이 서로 얽혀 있다는 구조는 같다. 원문: "AI is both a cause and a symptom of the problem here."
- **어떻게 여기까지 왔나:** 약 10년 전부터 클라우드와 오픈소스 패키지로 개발 속도를 샀다. 그 대가로 세부 지식과 통제력을 내줬다. 전문 직군은 범용 인력으로 평준화됐다.
- **AI가 더한 것:** 저자는 환각보다 **비정합성(decoherence)** 이 더 큰 위험이라고 본다. 변경 하나하나는 타당해 보여도, 쌓이면 서로 맞지 않는 모델이 시스템 안에 겹친다.
- **결론:** 회사는 "조직적 인지(organisational cognition)"를 잃을 위험에 있다. 사회는 이해할 수 없는 소프트웨어에 너무 많은 것을 맡겼다. 원문: "This is not a blip, and it's not an aberration - it's the new normal."
- **반론(HN):** 시기 설정이 틀렸고, 든 사례 중 AI 탓이 아닌 것이 많다. 과거에도 Heartbleed·log4j·CrowdStrike 같은 사고가 있었다는 지적이 나왔다.

## 원문이 든 사고 4건과 블로그 기록

저자는 서문과 첫 장에서 사고를 차례로 들며 "구조적 실패"의 사례로 삼는다. 이 블로그 AI 브리핑에서 이미 다룬 사고가 대부분이라 기록과 나란히 놓았다.

| 사고 | 원문의 서술 | 블로그에 기록한 사실 | 다룬 곳 |
|---|---|---|---|
| Microsoft 장애와 FAA 지상 정지 | "2 day global Microsoft outage, then an FAA ground-stop". Microsoft 장애는 데이터센터 간 통신을 막은 네트워크 설정 실패에서 시작됐을 **가능성**이 있다고 봤다 | Wired는 Microsoft 365가 **24시간** 장애 뒤 복구됐다고 보도했다. Microsoft는 "not related to cyber or AI/agents"라며 앞선 네트워크 용량 증설과 최근 네트워크 변경을 원인으로 들었다. FAA 정지는 별개 사건이다. 구리선을 광케이블로 바꾸던 중 라우터 한 대가 구형 관제 장비에 연결 요청을 쏟아 냈다. 근본 원인 판정은 아직이다 | [11-15 브리핑](/ai-briefing/2026-11-15/), [11-16 브리핑](/ai-briefing/2026-11-16/) |
| PHP2Shell | Wiz가 10월 21일 PHP 사이트 어디서든 임의 코드를 실행하는 익스플로잇을 공개했다. 런타임 메모리 관리 버그가 10년 넘게 숨어 있었다 | Calif·OpenAI·Wiz가 AI 모델로 찾은 PHP 엔진 자체의 무인증 원격 코드 실행 2건이다. 앱 코드와 무관하게 10년치 PHP가 영향을 받았다. 이후 CISA KEV에 올랐고 Check Point 장비 와이퍼 공격에도 쓰였다 | [10-22](/ai-briefing/2026-10-22/), [10-28](/ai-briefing/2026-10-28/), [11-14 브리핑](/ai-briefing/2026-11-14/) |
| eMudhra 사고 | 지나치게 약한 개인키 때문에 SSL 인증서 신뢰가 무너졌다 | 브라우저와 OS가 신뢰하던 eMudhra **중간 CA 인증서 8개**의 개인키가 약해서 노출됐다. 누구든 아무 도메인의 인증서를 위조할 수 있었다. eMudhra는 이후 SSL/TLS 사업에서 철수했고, Python·certifi·AI 도구들이 신뢰 저장소를 갱신했다 | [10-22](/ai-briefing/2026-10-22/), [10-23](/ai-briefing/2026-10-23/), [11-02 브리핑](/ai-briefing/2026-11-02/) |
| Shai Hulud (원문 기준 2026년 5월) | GitHub Actions CI 설정 오류로 포크한 누구나 상위 프로젝트에 코드를 넣을 수 있었다. 캐시된 라이브러리의 설치 스크립트가 비밀 토큰을 빼냈다 | 이 블로그는 5월 사건을 직접 다루지 않았다. 10월에 Shai-Hulud 변종("Mini Shai-Hulud")으로 귀속된 npm 패키지 사건을 기록했다 | [10-11 브리핑](/ai-briefing/2026-10-11/) |

대조해 보면 두 가지가 눈에 띈다.

- **원인이 AI가 아닌 사례가 섞여 있다.** Microsoft와 FAA 운영사는 원인 발표 전에 AI를 배제하는 문장을 따로 넣었다. 다만 저자의 논지는 "AI가 장애를 냈다"가 아니다. "복잡성이 통제를 넘어섰다"는 쪽이다. 두 장애 모두 네트워크 변경 한 번이 넓게 번진 사례라, 오히려 그 논지에 가깝다.
- **원문은 FAA 정지를 Microsoft 장애 탓으로 쓰지 않았다.** 같은 시기의 두 사건을 "double-whammy"로 나란히 놓았을 뿐이다. 장애 기간 "2일"은 블로그가 기록한 Wired 보도(24시간)와 다르다.

## 저자가 말하는 "통제를 잃은 과정"

**약 12년 전.** 소프트웨어 개발은 한 사람이 전체를 머릿속에 담아야 하는 일이었다. 저자는 시계를 손으로 조립하는 일에 비유한다. 사람으로 확장하기 어려웠고, 관리자에게는 골칫거리였다. SCRUM 같은 방법론도 효과가 제한적이었다고 본다.

**약 10년 전.** 클라우드(AWS 등)와 오픈소스 라이브러리가 퍼졌다. 2014년 무렵부터 NPM 같은 도구가 `package.json`에 한 줄을 넣는 재사용을 쉽게 만들었다. 저자는 이 편의의 대가로 세부 지식과 통제력을 내줬다고 본다.

**문화의 변화.** 운영·백엔드·모바일·테스트 전문가가 범용 인력으로 평준화됐다. 저자는 배관공·전기공 비유로 노동자와 경영진의 힘의 균형이 바뀌었다고 주장한다. 이 판단은 정치적 입장에 따라 달라질 수 있다는 단서도 직접 붙였다. 2016년 left-pad 사고와 XZ Utils 백도어 같은 아찔한 일이 있었다. 그래도 대체로 버틴 것은 성실한 개발자와 운영 인력 덕분이었다고 본다.

**그리고 AI.** 저자는 모델이 요청받은 일은 꽤 잘한다고 인정한다. 문제는 맡기는 범위다. 테스트와 함수에서 시작해 이제 설계와 전체 구상까지 에이전트에 넘긴다. 최악의 경우는 이렇게 본다. 원문: "At its worst, AI turns tech workers into “meat proxies” who spend their days massaging prompts". 학습된 무기력과 잘못된 자신감도 함께 생긴다고 경고한다.

## 핵심 개념: 비정합성(decoherence)

저자는 Kristen Maevyn의 에세이 [The Decoherence Trap](https://maevyn.codes/decoherence)에서 정의를 가져온다. 원문: "Decoherence: the accumulation of locally valid but mutually incompatible or redundant models within a system"

- **국소적으로는 타당하다.** AI가 만든 변경 하나하나는 리뷰에서 그럴듯해 보인다.
- **전체로는 맞지 않는다.** 같은 개념을 다르게 모델링한 코드, 중복된 구현, 서로 충돌하는 가정이 쌓인다. 기능 하나를 바꿔도 어느 모델이 진짜인지 확인하는 시간이 늘어난다.
- **속도에 비례해 커진다.** 변경이 빠를수록 빨리 쌓이고, 지수적으로 늘 수 있다고 본다.

환각은 틀린 출력이라 눈에 띈다. 비정합성은 각각 맞는 출력의 합이라 드러나지 않는다. 저자가 이쪽을 더 위험하게 보는 이유다.

## 조직적 인지와 걸린 것

**조직적 인지.** 저자는 극단적인 업무 사슬을 예로 든다. 프롬프트로 티켓을 쓰고, 티켓을 AI가 요약하고, 요약으로 문서를 만들고, 그 문서로 다시 계산한다. 이렇게 되면 회사가 하나의 단위로 생각하고 대응하는 능력이 사라진다. 원문: "I believe that tech companies risk losing their “organisational cognition”". 복잡성은 의도나 통제 없이, 통제할 수 있는 속도보다 빨리 만들어진다. 그 복잡성이 비용 절감, 과잉 설계, 무보상 오픈소스 노동에 기대며 부품 하나하나를 약하게 만든다.

**걸린 것.** 항공, 금융, 급여, 주유기, 길 찾기까지 모두 소프트웨어 위에서 돈다. 원문: "The software we build is not legible enough to bear the responsibility the world is giving it." 저자는 사회·정부·제도가 2026년 같은 사고가 일상이 되는 것에 대비되어 있지 않다고 본다. 원문: "I think we are only at the beginning of a very painful phase."

## HN 토론의 반론과 동의

| 쪽 | 요지 | 원문 |
|---|---|---|
| 반론 | 시기 설정이 틀렸다. 클라우드는 10년보다 훨씬 오래됐다 | "Cloud computing is much older than 10 years ago." |
| 부분 동의 | 연대기는 엉망이지만 흐름은 맞다. 소수 클라우드와 SaaS로 몰리면서 공통 장애 지점이 생겼다 | "The timeline is very confused, but the arc is mostly right." |
| 반론 | 문제는 의존성 자체가 아니다. 좋은 패키지가 없고, 유지보수에 돈을 안 내고, 공급망 신뢰 도구가 부족한 것이 문제다 | "the problem isn't using dependencies, that is the solution" |
| 반론 | PHP2Shell 버그는 PHP 5.x 시절부터 있었다. AI가 만든 버그가 아니라 AI가 드러낸 버그다 | "The PHP2Shell bug existed more than 15 years ago, spanning releases as early as 5.x." |
| 동의 | AI가 공격에 필요한 기술 수준을 크게 낮춘다 | "AI massively reduces the required skill to exploit." |

이 밖에 이런 의견도 나왔다.

- **과장이라는 지적:** FAA 정지는 케이블 교체 중에 났고, 인증서 사건은 명백한 사람의 실수였다.
- **해법 제안:** 명세·테스트·문서화가 해법이라는 의견이 많았다. 의사 코드로 명세를 쓰거나, AI가 읽을 주석과 참조 문서를 두자는 제안도 있었다.
- **역사적 비교:** Heartbleed, log4j, CrowdStrike 장애가 있었으니 지금이 특별히 나쁘다고 보기 어렵다는 반론이 있었다. 의존성이 복잡해져 장애 양상이 다양해졌다는 재반론도 나왔다.
- **케슬러 증후군 비유:** 시스템 간 결합이 커져 한 번의 실패가 연쇄로 번진다는 비유가 나왔다.

## 읽고 나서

- **사실과 의견을 나눠 읽어야 한다.** 사고 목록은 대부분 확인된 사건이다. 하지만 Microsoft 장애 원인은 저자의 추정이고, "12년 전·10년 전"이라는 시기는 HN에서 가장 많이 반박됐다. AI 모델이 보조금 성격의 컴퓨팅에 기댄다는 대목도 추측이다.
- **AI 원인설보다 복잡성 논지가 단단하다.** 블로그에 기록한 Microsoft·FAA·eMudhra 사고의 공식 원인은 AI가 아니다. 그래도 "한 지점의 변경이 넓게 번진다"는 구조는 네 사고 모두에 들어맞는다. PHP2Shell은 반대로 AI가 공격 쪽 탐색 비용을 낮춘 사례다.
- **비정합성은 실무에서 바로 쓸 수 있는 렌즈다.** 에이전트에게 변경을 맡길 때 리뷰 기준을 "이 변경이 맞나"에서 "이미 있는 모델과 겹치거나 충돌하지 않나"로 넓히라는 이야기로 읽힌다. HN에서 나온 명세·문서화 제안과도 이어진다.
- **원문은 처방을 내놓지 않는다.** 토론에 나온 제안을 저자 주장과 섞어 읽지 않도록 주의한다.

## 참고

- 원문: [Software is breaking down at a systemic level](https://www.breck-mckye.com/blog/2026/11/software-is-breaking-down-at-a-systemic-level/) (Jimmy Breck-McKye, 2026-11-15)
- GeekNews: [소프트웨어가 구조적으로 무너지고 있다](https://news.hada.io/topic?id=37303)
- Hacker News: [토론](https://news.ycombinator.com/item?id=50520866)
- 원문이 인용한 글: [The Decoherence Trap](https://maevyn.codes/decoherence) (Kristen Maevyn) · [Axel Rischin의 Azure 비판](https://isolveproblems.substack.com/p/how-microsoft-vaporized-a-trillion) · [Wiz의 PHP2Shell 공개](https://www.wiz.io/blog/php2shell-php-rce)
