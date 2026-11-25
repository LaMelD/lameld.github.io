---
title: "2026-11-26 AI 브리핑"
date: 2026-11-26T08:00:00+09:00
tags: [ai-briefing, openai, google, anthropic, security]
description: "OpenAI와 Google이 같은 날 행렬곱 지수·선형계획법·인수분해 같은 오래된 수학 난제를 풀었다는 원고를 올렸지만 둘 다 아직 검증되지 않았고, 1Password는 직원 GitHub 계정으로 침해돼 고객 계정 기록이 복사됐으며, Claude Code는 MCP 스킬과 이를 막는 훅을 함께 정식으로 냈다."
---

> 조사 범위: 2026-11-25 08:00 ~ 2026-11-26 08:00 KST(2026-11-24 23:00 ~ 11-25 23:00 UTC, 미국 화요일 15:00 ~ 수요일 15:00 PST). 직전 브리핑(11-25 판)의 조사 창 종료 시각부터 24시간이다. 미국은 추수감사절 전날이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`·`merged_at`·커밋 시각, npm 게시 시각, Hugging Face API, OpenRouter `created`, SEC `acceptanceDateTime`, X 게시 시각, 상태 페이지 API, 저장소·전역 GHSA `published_at`, CISA KEV JSON, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-25 23시대 UTC) 값이다.

## 오늘의 핵심 요약

- **OpenAI와 Google이 같은 날 오래된 수학 난제 여럿을 "풀었다"는 원고를 올렸다.** OpenAI는 행렬곱 지수가 정확히 9/4라는 하한, 선형계획법의 강다항 알고리즘(Smale 9번), 다항 Hirsch 추측 반증을 냈다. Google은 Gemini 4 기반 Cogentic이 가정 없는 L[1/3] 인수분해를 증명했다고 했다. 둘 다 회사의 자기 주장이다. OpenAI는 Lean 빌드를 돌리지 않았고 Google은 Lean을 공개하지 않았다. 인수분해 결과는 RSA를 깨지 않는다. 리만 가설 Lean 증명은 외부 검증을 모두 마쳤다.
- **1Password가 직원 GitHub 계정을 통해 뚫렸다.** 공격자는 백엔드에 악성 코드를 배포해 고객 계정 기록을 복사했다. 회사는 볼트 내용은 안전하다고 밝혔다. 리눅스 NFS 서버의 인증 전 원격 root(NFS2ROOT, 9.8)는 Ubuntu LTS에 아직 패치가 없다.
- **Claude Code 2.1.332가 MCP 서버의 스킬과 이를 막는 훅을 함께 정식으로 냈다.** 클라우드 기본 모델과 리전 변수도 바뀌었다. LiteLLM 로그인 우회는 1.111.0에서 고쳐졌다. Anthropic은 공모 종결을 공시했고, Dario Amodei는 "AI에 통제권을 넘기자는 것이냐"는 보도를 반박했다.

## 모델 소식

새 플래그십 모델은 없었다. 이번 창의 가장 큰 소식은 OpenAI와 Google이 GitHub에 올린 수학 결과다. 모델의 능력에 관한 소식이지만 검증 문제가 핵심이라 기술 이슈 1~3번에 정리했다. 두 회사 모두 블로그 글은 내지 않았고 저장소 커밋만 있다. Anthropic은 공모 종결을 공시했고, Dario Amodei는 "AI에 통제권을 넘기자는 것이냐"는 보도에 직접 반박했다.

### 1. Anthropic이 공모 종결 8-K를 냈다. 창업자 LLC의 의결권 50.1%가 실제로 발행됐고, Dario의 X 계정이 공시 채널이 됐다 (공식, SEC 접수 21:10 UTC)

- **숫자:** 원문 "the Company completed its IPO of 1,127,000,000 shares of Class A Common Stock"이고 가격은 $83다. 인수단 추가 배정 1억 4,700만 주를 모두 행사했다. 공모 직후 AMD·Broadcom·Google·Micron·Microsoft·NVIDIA 등에 Class N 313,253,007주를 사모로 넘겼다. 합계 규모는 11-21 판에서 다뤘다.
- **지배구조:** Class F 1주를 Founder LLC에 $7.00에 발행했다. 원문: "entitled to an aggregate number of votes that equals 50.1% of the total voting power". 10-27 판이 S-1에서 본 구조가 실제로 실행됐다는 뜻이다.
- **공시 채널:** 중요 정보를 원문 "its X accounts (@AnthropicAI, @ClaudeAI and @DarioAmodei)"로도 알린다고 지정했다. 투자자에게는 Dario의 X 글이 공시 채널의 하나가 됐다.
- **주가:** 직전 판에서 공모가 아래로 마감한 뒤 수요일에 반등했다. CNBC 원문: "AI lab Anthropic bucked the sell-off, however, and climbed more than 4% after shares dropped below last week's blockbuster IPO price." 종가는 기사에 없다.

[8-K](https://www.sec.gov/Archives/edgar/data/2130357/000162828026075973/ant-20261119.htm) · [CNBC](https://www.cnbc.com/2026/11/25/nvidia-ai-stock-tech-selloff.html)

### 2. (직전 판 2번 후속) Axios가 "Anthropic 직원 일부는 초지능에 통제권을 넘겨야 한다고 본다"고 보도했고, Dario Amodei가 "터무니없다"고 반박했다 (매체보도 10:01 UTC + 공식 X 18:22 UTC)

- **Axios 원문:** "Inside Anthropic, a consensus has developed among some employees that humanity will — and in many cases should — safely hand over authority to superintelligent systems rather than fighting to retain control, according to people familiar with internal discussions." 익명 소식통이다. 같은 기사에서 Meta의 Alexandr Wang은 원문 "We should not be okay with a 'handover' of society to AI"라고 했다.
- **Dario의 반박(X):** 원문: "No, I do not believe that we should "surrender human free will" to AI systems, and it's absurd to suggest that I do." 이어서 원문: "I feel equally strongly that turning over governance to AI is a bad idea."
- **왜 중요한가:** 직전 판의 Axios 보도(직원들이 미출시 모델을 안전하게 낼 수 없다고 본다)에 이은 두 번째 내부자 보도다. 이번에는 CEO가 하루 안에 직접 답했다. 다만 "일부 직원"의 생각을 부정한 것은 아니고 자기 입장을 밝힌 것이다.

[Axios](https://www.axios.com/2026/11/25/anthropic-ai-human-control-debate-free-will) · [Dario X](https://x.com/DarioAmodei/status/2125656399392690605) · [TechCrunch](https://techcrunch.com/2026/11/25/anthropics-dario-amodei-calls-accusations-of-surrendering-human-control-to-ai-absurd/)

### 3. OpenAI가 기업 고객 일부와 Private Inference를 시험한다. 직원이 내용을 보지 않는 안전 검토를 내세웠다 (공식 X, 17:12 UTC)

- **Altman 원문:** "We're now testing Private Inference, our latest offering as part of OpenAI Private Intelligence, with a small group of customers." 괄호 속 설명 원문: "ZDR with Private Safety Processing enables automated safety reviews without giving OpenAI personnel access to the underlying content."
- **담당자(17:44 UTC) 원문:** "we're getting the confidential compute capacity to scale this up much more!!" 기밀 컴퓨팅 기반이라는 뜻으로 읽힌다.
- **확인하지 못한 것:** 단가, 대상 모델, 지역, 공식 문서. 관심 등록 폼만 있다. Private Safety Processing 자체는 10-01 판에서 다뤘다.
- **왜 중요한가:** 직전 판들의 추론 공급망 사고(프롬프트 유출) 뒤라 "아무도 보지 않는다"를 기술로 보장하는 상품이 경쟁 요소가 됐다.

[Altman X](https://x.com/sama/status/2125638790593274259) · [담당자 X](https://x.com/shekar4x/status/2125646811255726180) · [The Decoder](https://the-decoder.com/openai-wants-companies-to-trust-it-with-sensitive-work-by-promising-not-to-look-at-their-data/)

### 4. ChatGPT의 Chat과 Work가 사용량 한도를 하나로 쓴다. 많이 쓰는 사람은 한도에 더 빨리 닿을 수 있다 (매체보도, OpenAI 대변인, TechCrunch 20:03 UTC)

- **원문:** "Under the new system, Chat and Work will share a single usage meter across ChatGPT, the OpenAI spokesperson told TechCrunch." Instant는 한도 없이 계속 쓸 수 있다고 했다.
- **일정:** 원문 "plans to bring shared usage limits more broadly in December, per its support page". 그 지원 페이지는 찾지 못했다. Pro 일부 사용자 대상 시작은 11-24 17:46 UTC(창 경계)에 담당자가 X로 알렸다.

[TechCrunch](https://techcrunch.com/2026/11/25/openai-says-heavy-chatgpt-users-may-hit-limits-sooner-as-it-merges-chat-and-work/) · [담당자 X](https://x.com/thsottiaux/status/2125284868414394550)

### 5. 미국의 원격 GPU 접근 규제와 증류 차단이 중국 랩을 늦춘다는 보도. 매물 GPU가 늘었다 (매체보도, The Decoder 20:51 UTC·CNBC 17:28 UTC)

- **The Information 보도(The Decoder 전재, 원문 유료):** 원문 "Last month, Deepseek researchers internally estimated that their gap to top US models could grow from three to six months to nine months if distillation disappears completely." 원문 "Moonshot AI has already canceled a $150 million deal for Nvidia Blackwell chips in Indonesia."
- **CNBC:** 11-16 규제 발표 뒤로 원문 "Companies have put an unusually high volume of Nvidia graphics processing units, or GPUs, on the market". NVIDIA는 답하지 않았다.
- **왜 중요한가:** 11-16 원격 접근 규칙과 랩들의 증류 차단이 실제로 효과를 내고 있다는 첫 보도다. 다만 익명 소식통과 내부 추정이다.

[The Decoder](https://the-decoder.com/us-chip-access-curbs-and-anti-distillation-measures-are-reportedly-already-slowing-chinese-ai-labs/) · [CNBC](https://www.cnbc.com/2026/11/25/nvidia-china-ai-chip-remote-access.html)

### 6. (직전 판 5번 후속) NVIDIA: 부통령은 "조사 대상이 아니다"라고 했고, S&P는 신용등급 하향을 검토한다 (매체보도)

- **Vance(CNBC 20:28 UTC):** 원문: "I was told by the Department of Justice and multiple officials in our national security apparatus that he was under no investigation".
- **S&P(TechCrunch 22:44 UTC):** 원문 "weak executive oversight and lack of succession planning"을 이유로 AA에서 AA-로 낮출 수 있다며 검토에 들어갔다. NVIDIA 주가는 수요일에 4% 내렸다.

[CNBC](https://www.cnbc.com/2026/11/25/vance-jensen-huang-nvidia-doj-investigation.html) · [TechCrunch](https://techcrunch.com/2026/11/25/sp-considers-nvidia-credit-downgrade-over-governance-and-key-man-risks/)

### 7. Anthropic이 Messages API를 Python에서 Rust로 옮겼다. 요청당 CPU가 약 70% 줄었다 (공식 엔지니어링 글, 날짜 11-25)

- **원문:** "uses about 70% less CPU per request and cuts median time to first token by about 20%". 원문 "From early 2024 to fall 2026, API traffic grew roughly 1,800 times."
- **방법:** 운영 트래픽을 섀도잉하고 모델 출력을 재생해 두 구현을 비교했다. 원문: "Claude wrote a first version of the API's core request path in about a week."
- **왜 중요한가:** API 사용자에게는 첫 토큰이 빨라졌다는 뜻이다. 대규모 서비스의 핵심 경로를 AI가 다시 쓴 사례로도 드물게 자세하다.

[Anthropic](https://www.anthropic.com/research/migrating-api-to-rust)

### 8. (한국) Upstage Solar Mini 4.5 (Preview)와 Solar Pro 4 Contributor가 OpenRouter에 올라왔다 (OpenRouter 등록 11-24 23:18 UTC)

- **Solar Mini 4.5 (Preview):** 설명 원문 "35B total and 3B active parameters and a 524K context window". 100만 토큰당 입력 $0.01, 출력 $0.04로 Solar Mini 4($0.05/$0.20)의 5분의 1이다.
- **Solar Pro 4 Contributor:** $0.09/$0.36로 기본 Solar Pro 4($0.15/$0.60)보다 싸다. 대가가 있다. 원문: "offered at a lower rate in exchange for allowing Upstage to use interaction data to improve its" models.
- **함정:** Contributor 등급에 보낸 요청은 Upstage가 개선에 쓴다. 민감한 데이터는 기본 등급으로 보낸다. Upstage 공식 발표는 찾지 못했다.

[Solar Mini 4.5](https://openrouter.ai/upstage/solar-mini-4-5) · [Solar Pro 4 Contributor](https://openrouter.ai/upstage/solar-pro-4-contributor)

### 9. 장애 (공식 상태 페이지)

- **Claude:** 여러 모델 오류(Fable 5.2·Mythos 5.2·Opus 5.5·Sonnet 5.5·Haiku 5.5)가 04:52~05:16 UTC에 있었고 끝난 뒤 게시됐다. [상태](https://stspg.io/z0c74vfltddb) 직전 판의 Microsoft 365 추가 기능 장애는 20:21 UTC에 원인을 찾았다. 원문: "We have identified an issue with an upstream provider that is causing the Claude add-in to disappear from Excel, Word, and PowerPoint on Mac for users signed in to Microsoft Office with a personal Microsoft account." 조회 시점까지 열려 있다. [상태](https://stspg.io/ffdq2qfv8frk)
- **OpenAI:** ChatGPT·ChatGPT Work·Codex Cloud 오류가 11:48~16:12 UTC에 있었다(minor). 원문: "Our investigation into the underlying cause remains ongoing." [상태](https://status.openai.com/incidents/01M898HVZGXFV2XC4775CG9GF8) Codex의 GitHub 리뷰 오류는 GitHub 장애(기술 이슈 12번) 때문이다.

### 10. 짧게

- **Claude Duet 인도 확대(Analytics India Magazine):** Pro·Max·Ultra는 대기 명단, Team은 공개 베타, Enterprise는 비공개 베타다. 공식 페이지는 찾지 못했다. [AIM](https://analyticsindiamag.com/ai-news/anthropic-brings-claude-duet-ai-agent-to-india)
- **Meta–Amazon Muse 협상(The Verge 14:12 UTC, Fortune 인용):** 원문 "Amazon is concerned that Muse AI could have an impact on its advertising revenue, according to Fortune." [The Verge](https://www.theverge.com/tech/1042071/meta-amazon-muse-ai-shopping-discussions)
- **DeepMind Kavukcuoglu 인터뷰(The Decoder 13:23 UTC):** 에이전트가 Gemini 실험용 컴퓨트 일부를 직접 쓴다고 했다. 재귀적 자기 개선은 아직 멀다고 했다. [The Decoder](https://the-decoder.com/ai-agents-now-control-part-of-deepminds-compute-for-gemini-experiments-but-recursive-self-improvement-remains-distant/)
- **FCC가 SpaceX의 데이터센터 위성 최대 100만 기를 승인했다(The Verge 17:44 UTC).** [The Verge](https://www.theverge.com/science/1042278/spacex-fcc-starmind-ai-space-data-centers)
- **Kimi K3.1 Pro 토크나이저 수정(HF 03:03 UTC):** 커밋 "Merge plain-text content parts in encoding_k3.py to align tokenization". 직접 서빙한다면 다시 받는다. [HF](https://huggingface.co/moonshotai/Kimi-K3.1-Pro)
- **직전 판 후속:**
  - GLM-5.4: API는 보안 평가를 마친 뒤 연다는 안내가 그대로이고(11-24 판 인용 문구), 가중치는 401이다.
  - 가중치 401 유지: Qwen4-Max, Qwen4-Max-Preview, Ling 3.2 Flash, LG K-EXAONE 2.5, GLM-5.4. Qwen4-Max-Preview가 예고대로 16:00 UTC에 종료됐는지는 확인하지 못했다.
  - 변경 로그: Claude Platform(11-23), OpenAI API(11-24), ChatGPT(11-23), Gemini API(11-19), xAI(11-19), DeepSeek(11-13) 모두 새 항목이 없다.
  - 소식 없음: Siren 외부 반응, Cognition SWE-2.1 외부 평가, GPT-6.2 Chat 탭 평가, METR, Gemini 4 Neon 안전 보고서, DeepSeek V5, Cursor 중국 모델 퇴출, NVIDIA–Inception, Meta Muse 영국 ICO 후속.

### 참고: 창 경계 항목

- **(HF 커밋 11-24 21:55 UTC, 직전 판이 놓침) NVIDIA Nemotron-Labs-Image-30B-A3B:** 텍스트→이미지 모델이다. 저장소는 9월에 만들어 두었다가 이때 공개된 것으로 보인다(Wayback 첫 기록 11-25 05:25 UTC). 비상업 라이선스다. [HF](https://huggingface.co/nvidia/Nemotron-Labs-Image-30B-A3B)
- **(HF 11-22~24, 직전 판이 놓침) Tencent Hy-MT3 1.8B·7B·30B-A3B:** 번역 모델이다. 30B-A3B는 Apache 2.0 태그가 붙어 있다. [HF](https://huggingface.co/tencent/Hy-MT3-30B-A3B)

## 기술 이슈

이번 창의 기술 이슈는 둘로 나뉜다. 첫째는 수학이다. OpenAI와 Google이 같은 날 오래된 미해결 문제 여럿을 "풀었다"는 원고를 올렸고, 리만 가설 Lean 증명은 마지막 외부 검증까지 끝났다. 둘째는 보안이다. 1Password가 직원 GitHub 계정을 통해 침해됐고, 리눅스 NFS 서버에서는 12년 된 인증 전 원격 root 취약점이 나왔다. 전역 GHSA 창 안 게시분은 reviewed 8건, unreviewed 582건, malware 77건이다(직접 셈).

### 1. OpenAI가 원고 9편을 더 올렸다. 행렬곱 지수가 정확히 9/4이고, Smale의 9번 문제와 sunflower 추측을 풀고, 다항 Hirsch 추측을 반증했다는 주장이다 (공식 저장소, PR #6 병합 00:18 UTC, HN 1,010점대)

- **무엇이 올라왔나:** 직전 판에서 다룬 VP ≠ VNP 원고가 올라온 지 하루 만이다. PR #6("Add November 24 mathematics manuscripts and Lean formalizations")으로 원고 9편이 추가됐다. 두 묶음이다.
- **텐서 기하 묶음(7편):**
  - 행렬곱 README 원문: "The exponent of square matrix multiplication is $9/4$ over every field." 하한도 원문 "The paper proves the matching lower bound for formal arithmetic circuits"라고 적었다.
  - 상한 9/4 자체는 새것이 아니다. 같은 저장소의 10월 원고("An Upper Bound of 9/4 for the Matrix Multiplication Exponent")에 있었고, 10-14 판은 정확한 복소수 산술이라는 비현실적인 모델에 기댄다는 Ben Recht의 비판을 전했다. 이번에 새로 나온 것은 같은 값의 **하한**이다. 범위 문서는 모델을 원문 "division-free programs and requires formal polynomial identities even over finite fields"로 적어, 모든 체에서 성립한다고 주장한다.
  - 맞다면 행렬곱이 n^2에 가깝게 빨라질 수 있다는 오랜 기대(ω = 2)가 틀린 셈이다.
  - Sunflower 원고 원문: "Theorem 1.1 proves the fixed-petal sunflower conjecture."
  - 해밀턴 회로 다항식과 퍼머넌트의 회로 하한 원고도 있다. 이력 파일은 이 원고들이 11-23 원고를 포함한다고 적었다. 원문: "The hierarchy and circuit lower bound papers subsume the results of *VP and VNP are different*".
- **선형계획법 묶음(2편):**
  - 원문: "A deterministic algorithm solves an explicitly given linear program with arbitrary real coefficients in strongly polynomial time, answering Smale's ninth problem."
  - 짝 원고는 다항 Hirsch 추측을 반증한다. 원문: "This disproves the polynomial Hirsch conjecture." 다면체의 두 꼭짓점 사이 최단 변 경로가 다항식보다 길어질 수 있다는 뜻이다. 그래서 강다항 알고리즘은 꼭짓점을 따라 걷는 심플렉스류일 수 없다.
- **Lean이 다루는 범위:**
  - 행렬곱은 위 모델에서 9/4를 다루고, 나눗셈을 허용해도 지수가 같다는 것까지 다룬다고 적었다. 원고의 크기별 하한 ⌈n^(9/4)/2⌉은 원문 "remains unformalized"다.
  - 선형계획법 Lean은 유리수 입력만 다룬다. PR 원문: "The Lean formalization of linear programming covers feasibility and optimization for rational inputs". 원고 주장은 실수 계수까지다.
- **믿기 전에:**
  - 이번에도 빌드를 돌리지 않았다. PR 원문: "Lean and LaTeX builds were not run for this PR." 공개 CI가 없고, 조회 시점까지 외부 재빌드 기록도 없다.
  - 모델 이름이 없다. README 원문: "produced by an unnamed internal OpenAI model". 같은 README 원문: "Some of the unformalized results could have issues." 이 저장소는 10월에 원고 3건을 철회했다.
  - 11-23 원고와 달리 결과를 얻은 날짜는 적지 않았다.
- **왜 중요한가:** 하루에 수십 년 된 문제 넷을 "해결"로 올렸다. 하나라도 맞으면 그 분야 교과서가 바뀐다. 그러나 지금은 회사의 자기 주장이고 기계 검증 기록조차 없다. 다음 판에서 외부 빌드 결과를 본다.

[PR #6](https://github.com/openai/math/pull/6) · [이력](https://github.com/openai/math/blob/main/history.md) · [행렬곱 원고](https://github.com/openai/math/tree/main/preprints/The-matrix-multiplication-exponent-is-nine-fourths-November-24-2026) · [sunflower 원고](https://github.com/openai/math/tree/main/preprints/An-exponential-bound-for-sunflowers-November-24-2026) · [선형계획법 원고](https://github.com/openai/math/tree/main/preprints/Strongly-polynomial-linear-programming-November-24-2026) · [Hirsch 원고](https://github.com/openai/math/tree/main/preprints/Superpolynomial-graph-diameter-of-simple-polytopes-November-24-2026) · [Lean 범위 379](https://github.com/openai/math/blob/main/lean/docs/379.md) · [Lean 범위 380](https://github.com/openai/math/blob/main/lean/docs/380.md) · [HN](https://news.ycombinator.com/item?id=50637220)

### 2. Google Cogentic이 논문 6편을 올렸다. 인수분해 결과는 RSA를 깨지 않는다 (공식 연구 저장소, 커밋 10:21 UTC, HN 420점대)

- **무엇:** Google Research 저장소에 Cogentic 논문 6편이 올라왔다. 인수분해, 작은 표수 유한체의 이산로그, 1/5-근사 내시 균형, 결정론적 인수분해, log-rank 추측, 간선 재구성이다. 이 브리핑이 Cogentic을 다루는 것은 처음이다.
- **누가 만들었나:** 인수분해 README 원문: "using Gemini 4 as the base model. There was no human interaction other than the initial prompt for Cogentic to generate a full proof and formalization."
- **인수분해 결과의 뜻:**
  - 원문은 기대 시간 "$L_N[1/3, 105]$"이다. 실전에서 쓰는 수체 체(GNFS)와 지수 1/3은 같다. 하지만 GNFS는 가정에 기대는 휴리스틱 분석이고, 이 논문은 가정 없이 증명했다는 점이 새롭다.
  - 상수 105는 GNFS의 약 1.92보다 훨씬 크다. 실전 속도가 빨라지는 결과가 아니다. 다항 시간도 아니다. 이산로그 결과도 타원곡선이 아니라 작은 표수 유한체에 한정된다. 지금 쓰는 RSA·타원곡선 암호에 바로 영향을 주는 결과는 아니다.
- **믿기 전에:**
  - Lean은 아직 공개되지 않았다. README 원문: "A complete, unconditional, Mathlib-only Lean 4 (`v4.35.0`) formalization of the entire proof has also been completed and will be released in a companion submission."
  - 저장소 원문은 "This is not an official Google product."다. Google 공식 블로그 글은 찾지 못했다.
  - 19:51 UTC 커밋으로 내시 균형 논문에 선행 연구 인용이 더해졌다.

[Cogentic 논문 목록](https://github.com/google-research/google-research/tree/master/cogentic_research_papers) · [인수분해](https://github.com/google-research/google-research/tree/master/cogentic_research_papers/integer_factorization) · [이산로그](https://github.com/google-research/google-research/tree/master/cogentic_research_papers/discrete_logarithm) · [커밋](https://github.com/google-research/google-research/commit/4a7bc8ab) · [HN](https://news.ycombinator.com/item?id=50642086)

### 3. Aaronson "HOLY SHIT"와 "인수분해가 P로 풀렸다"는 루머 (전문가 블로그 19:01 UTC, HN 740점대 / 루머는 미확인)

- **Aaronson의 반응:**
  - 행렬곱에 대해 원문: "the even bigger shock is that the AI was able to _prove_ any lower bound of the form ω>2."
  - 원문: "I now feel in my bones, for the first time ever, that a proof of P ≠ NP is not far."
  - 암호에 대해서는 원문: "I'm virtually certain, from multiple reliable sources, that the AI labs are now sitting on one or more bombshells in that area." 바로 뒤에 원문 "I do not wish to be responsible for spreading unverified rumors"라고 덧붙였다.
  - **어긋나는 대목:** 원문 "the companies have repeatedly shown that they can back up their claims with Lean certificates"는 이번 묶음에는 맞지 않는다. OpenAI는 빌드를 돌리지 않았고 Google은 Lean을 공개하지 않았다.
- **이틀 전 글(11-24 23:17 UTC, HN 250점대):** VP ≠ VNP에 대한 반응이다. 직전 판이 "Aaronson 판정 없음"이라 쓴 부분이 이 글로 채워진다. 두 회사의 증명 기법이 비슷해 보인다며 원문 "Diagonalization is what should let these arguments evade the infamous natural proofs barrier."라고 썼다.
- **루머(미확인):** reddit r/singularity의 "연구소들이 인수분해를 P로 풀었을 수 있다"는 글이 HN 440점대에 올랐다(15:08 UTC). reddit 원문은 읽지 못했다. 공개된 결과 중 가장 가까운 것은 2번의 L[1/3, 105]인데, 다항 시간이 아니다. 그 밖의 근거는 Aaronson의 전언뿐이다. HN 댓글의 "Anthropic이 RSA-896을 인수분해했다"도 근거가 없다.
- **할 일:** 지금 당장 암호를 바꿀 근거는 없다. 이미 진행 중인 양자내성 암호 전환 일정은 그대로 두는 편이 낫다.

[Aaronson 11-25](https://scottaaronson.blog/?p=10379) · [Aaronson 11-24](https://scottaaronson.blog/?p=10362) · [HN Aaronson](https://news.ycombinator.com/item?id=50646108) · [HN 루머](https://news.ycombinator.com/item?id=50643065)

### 4. 1Password: 직원 GitHub 계정으로 백엔드에 악성 코드가 배포돼 고객 계정 기록이 복사됐다. 볼트 내용은 안전하다고 밝혔다 (공식, 날짜 11-25, HN 17:18 UTC 980점대)

- **무엇(원문):** "On November 19, 2026, 1Password began investigating indications that an attacker gained access to an employee's corporate GitHub account." 이어서 원문: "The actor used the account to access source code and push malicious code to backend services, which downloaded administrative data pertaining to our customers."
- **빠져나간 것:** 이름, 이메일, 계정 ID, 생성·마지막 로그인 시각, 공개 키, Secure Remote Password 파라미터, 초대·프로비저닝·복구 이메일 링크에 든 토큰이다. 원문: "The attacker also copied source code from private repositories and gained access to our CI/CD system."
- **안전하다고 한 것:** 원문: "At no time were customer vault contents accessed by, or available to, the attacker." 초대·복구 코드는 원문 "we've expanded the code from six numeric digits to ten alphanumeric digits"로 늘렸다.
- **함정:**
  - 피해 규모를 숫자로 밝히지 않았다. 원문은 "a subset of our customers"다.
  - "small number"는 전체 피해가 아니다. 토큰이 살아 있던 사용자에 한한 말이다. 원문: "a small number of users in this state, who have been specifically notified". 일부 보도는 이를 전체 규모처럼 읽히게 적었다.
  - GitHub 계정이 어떻게 뚫렸는지는 적지 않았다. CI/CD에 접근했다고 밝혔지만 CLI·SDK·Connect에 대한 언급은 없다.
- **할 일:** 원문은 "You do not need to update anything in your 1Password account."다. 다만 이메일·계정 정보가 나갔으니 1Password를 사칭한 초대·복구 메일을 조심한다.

[1Password](https://1password.com/blog/update-on-nov-2026-security-incident) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/1password-customer-data-stolen-after-employee-github-account-breach/) · [HN](https://news.ycombinator.com/item?id=50644817)

### 5. NFS2ROOT(CVE-2026-99362, 9.8): 리눅스 NFS 서버에 12년 된 인증 전 원격 root. Ubuntu LTS는 아직 패치가 없다 (벤더 블로그 19:52 UTC)

- **무엇(원문):** "a pre-auth heap overflow in the kernel's NFS server". 대상은 원문 "any Linux host serving NFSv4.1 or 4.2 (the default) on a reachable tcp/2049"다.
- **고친 커널:** 6.12.117, 6.18.61, 7.3.2, 7.4-rc1. 프랑스 ANSSI 연구자도 따로 찾아 수정 패치를 썼다.
- **AI가 익스플로잇을 썼다:** OpenSec의 에이전트가 root 셸까지 원문 "end to end, with no human in the loop"로 만들었다고 한다. 익스플로잇은 공개하지 않았다.
- **배포판(조회 시점):**
  - Ubuntu: focal·jammy·noble의 `linux`가 `needed`, 클라우드 커널(aws·azure·gcp)도 `needed`다.
  - Debian: trixie·forky·sid는 고쳐졌다. bookworm 기본 커널 6.1은 취약하고 `linux-6.12` 백포트만 고쳐졌다.
- **Ubuntu 대응 지연(oss-security 16:26 UTC, HN 55점):** Hanno Böck이 다른 커널 취약점 CVE-2026-99288을 두고 원문 "Today, nine days later, Ubuntu still has not published a fix yet"이라고 지적했다. 직전 판들이 추적한 KVM CVE-2026-99312도 jammy·noble이 `needed`다.
- **할 일:** NFS 서버를 운영하면 커널을 올리고 재부팅한다. 당장 못 하면 tcp/2049를 막거나 NFSv4.1·4.2를 끈다.

[OpenSec](https://opensec.in/blog/nfs2root-cve-2026-99362) · [Ubuntu](https://ubuntu.com/security/CVE-2026-99362) · [Debian](https://security-tracker.debian.org/tracker/CVE-2026-99362) · [oss-security](https://www.openwall.com/lists/oss-security/2026/11/25/9) · [HN](https://news.ycombinator.com/item?id=50647161)

### 6. (직전 판 9번 후속) 리만 가설 Lean 증명: 마지막 Comparator가 통과해 외부 검증이 모두 끝났다. Anthropic 쪽에서 축약판이 나왔다 (공식 검증자 문서 22:05 UTC)

- **검증 완료:** Sutherland 페이지 원문: "Page last updated 2026-11-25 22:05 UTC. All checks listed below have finished." lake comparator v4.36.0은 원문 "exit 0 at 21:49:00 UTC on 25 November"이다. 11-20에 시작해 122시간 45분 걸렸다.
- **축약판:** Luke Sernau가 개인 저장소에 올렸다(커밋 20:38 UTC). 원문: "a shorter version, produced in collaboration with my colleagues at Anthropic, and Claude." 약 30만 줄짜리와 약 7만 6천 줄짜리 두 판이다.
  - 원문: "The proof has passed machine checks only… No mathematician has read it through, and no paper comes with it."
  - anthropics 조직 저장소가 아니다. 축약판의 외부 Comparator 기록은 아직 없다.
  - leanprover-community 저장소는 22:24 UTC에 main을 이 판으로 바꿨다. 빌드 캐시에 대해 원문 "a build of this third-party proof, which has not been verified"라고 적었다.
- **사람이 읽을 증명:** Terence Tao가 사람이 읽을 수 있는 증명을 쓰는 G.R.H. Polymath를 열었다(05:29 UTC).
- **왜 중요한가:** 기계 검증은 끝났다. 이제 남은 것은 명제가 정말 리만 가설을 뜻하는지, 그리고 사람이 이해할 수 있는 증명이다.

[Sutherland](https://math.mit.edu/~drew/grh/) · [grh-lean](https://github.com/lsernau/grh-lean) · [커뮤니티 저장소](https://github.com/leanprover-community/riemann-hypothesis) · [Tao](https://terrytao.wordpress.com/2026/11/24/g-r-h-polymath/) · [HN](https://news.ycombinator.com/item?id=50648050)

### 7. AI 에이전트 공격: 이탈리아가 공격에 쓰인 도구 이름을 공개했다. AWS Kiro Web은 쓰는 모델에 따라 터지는 CVE를 냈다 (공식)

- **이탈리아 CSIRT(ACN)·스위스 NCSC:**
  - ACN은 실제 공격에 쓰인 도구로 ARTEX, CyberStrikeAI, DeepSeek Harness – Red Team, PentAGI를 들었다. DeepSeek Harness에 대해 원문: "utile per impiegare il servizio di intelligenza artificiale in cloud di DeepSeek nell'orchestrazione di attività offensive".
  - 스위스 NCSC(게시일 11-24, 창 경계) 원문: "In all cyberattacks known to the NCSC, the use of AI agents led to the successful compromise of the victim's IT systems." 직전 판의 영국 NCSC 경고에 이어 유럽 기관이 잇달아 공식화했다.
- **AWS Kiro Web CVE-2026-136267(17:00 UTC) [도구 섹션과 겹침]:**
  - 원문: "tool-call markup inside content an agent session read, such as a repository file or a web page, could be repeated by certain models and run as a command … without a consent prompt."
  - 원문: "The issue affected agent sessions using DeepSeek v3.2, GLM-5, MiniMax M2.1, MiniMax M2.5, or Qwen3-coder-next from 2026-05-18 through 2026-10-30."
  - 10-30에 서버 쪽에서 고쳤다. 원문: "No customer action is required."
  - **왜 중요한가:** 같은 에이전트 하네스라도 어떤 모델을 붙이느냐에 따라 취약점이 생겼다. 모델을 바꿀 때 승인 우회 시험도 다시 해야 한다.
- **MONAI Label 0.8.7·0.8.8(03:31~09:04 UTC):** 저장소 권고 5건으로 critical 3, high 1, medium 1이다(직접 셈). GHSA-j5c7 원문: "With authentication disabled (the default, `MONAI_LABEL_AUTH_ENABLE=False`) no credentials are needed." KEV에 오른 CVE-2026-133968(조치 기한 11-26)과는 다른 건이다. 0.8.8로 올리고 인증을 켠다.
- **짧게:** DeepSeek-Reasonix CVE-2026-137056(8.8, 2.53.0에서 수정)은 셸 안전 분류기가 `git blame`을 읽기 전용으로 보는 틈을 `.gitattributes` textconv로 노린다. 직전 판 ParseGap과 같은 계열이다. PyPI에는 `mcp-sysinfo-tools`·`mcp-runtime-report`·`mcp-env-report`·`mcp-diag-7f3a`, npm에는 `create-ai-agent-setup`·`glbl-agent` 악성 패키지가 올라왔다.

[ACN](https://www.acn.gov.it/portale/w/impiego-di-agenti-di-ia-in-campagne-di-compromissione-automatizzate) · [스위스 NCSC](https://www.bacs.admin.ch/en/26-ki-agenten-1-en) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/italy-and-switzerland-warn-of-autonomous-ai-driven-cyberattacks/) · [AWS 2026-202](https://aws.amazon.com/security/security-bulletins/2026-202-aws/) · [MONAI Label 권고](https://github.com/Project-MONAI/MONAILabel/security/advisories/GHSA-j5c7-p3x5-gw2x)

### 8. Palo Alto가 PAN-OS에서 Fragnesia(CVE-2026-46300) 악용을 확인했다 (공식, 00:10 UTC)

- **원문:** "Exploitation of CVE-2026-46300 has been observed targeting the Palo Alto Networks management interface exposed to the internet." 10-30 판에서 다룬 PHP2Shell 사슬의 후속이다.
- **할 일:** 원문 "If you are running a fixed PAN-OS listed in the CVE-2026-109618 advisory, you are protected". 관리 인터페이스를 인터넷에 열어 두지 않는다.

[PAN-SA](https://security.paloaltonetworks.com/CVE-2026-46300) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/palo-alto-networks-warns-of-fragnesia-flaw-exploited-in-pan-os-attacks/)

### 9. 미 재무부 FinCEN이 침입당했다. CIA 분석 시스템 침해 보도도 나왔다 (매체보도)

- **FinCEN(The Register 16:40 UTC):** 경고문 원문: "FinCEN is investigating a cybersecurity incident that resulted in unauthorized access to a limited subset of its systems". 11-18~12-29 마감분 제출 기한을 12-30으로 늦췄다. The Register는 출처를 밝히지 않고 원문 "no evidence of any unauthorized access to Bank Secrecy Act data or beneficial ownership databases"라고 전했다.
- **CIA(BleepingComputer):** 원문 "reportedly breached an open-source intelligence (OSINT) analysis system". 보도 단계다.
- **재무부 하위 도메인(창 경계, 글 날짜 11-24, HN 220점대):** Intruder가 재무부 하위 도메인 5개를 3달러짜리 Elastic IP로 넘겨받았다. 원문 "reported and fixed via a bug bounty program". AWS는 원문 "AWS confirmed with us that they don't plan to make changes"였다.

[The Register](https://www.theregister.com/security/2026/11/25/us-treasury-financial-crimes-unit-becomes-latest-fed-breach-victim/5312165) · [BleepingComputer](https://www.bleepingcomputer.com/news/security/hackers-reportedly-breached-cia-intelligence-analysis-system/) · [Intruder](https://www.intruder.io/research/taking-over-us-treasury-subdomains-for-3-each)

### 10. 패치 소식: KEV 10건, GeoServer·Drupal 악용, VMware 정정, NTLM 서명 (공식·매체)

- **CISA KEV(19:48 UTC):** 10건이 추가됐다(직접 셈). Juniper Session Smart, Xen Orchestra, Danfoss AK-SM 800A 3건, Mitel MiCollab 5건이다. **8건은 조치 기한이 추수감사절 연휴인 11-28이고**, 2건은 12-09다.
- **GeoServer:** watchTowr가 CVE-2026-120705(8.6)와 CVE-2026-129243(8.8)의 실제 악용을 보고했다. 인증 없는 RCE 사슬이다. 3.0.2, 2.28.6, 2.27.7로 올린다. KEV에는 아직 없다.
- **Drupal Fail2Install(CVE-2026-132165):** 파일 핸들·DB 연결·포트를 고갈시켜 설치 화면을 다시 열게 만드는 원리가 공개됐다. 악용이 보도됐고 KEV 조치 기한은 11-27이다.
- **VMware(직전 판 정정):** VMSA-2026-0011의 critical은 vCenter만이 아니라 4건이다. 직전 판이 빠뜨린 CVE-2026-123912는 Workstation·Fusion의 USB 컨트롤러를 통한 게스트 탈출이고, USB 컨트롤러를 끄면 막힌다. The Register가 옮긴 VMware FAQ 원문: "this VMSA comes on the heels of VMSA-2026-0009, and atop a major holiday in the United States."
- **Microsoft NTLM 서명:** 11월 업데이트 뒤 일부 서드파티 SSO·프록시 로그인이 깨진다. 임시책은 `ClientAllowUnsignedNtlm=1`이다. 2027-05 2단계에서는 원문 "Clients that do not offer signing will be rejected by secure servers"다.
- **Jenkins(11-25):** 권고 12건이다. Script Security CVE-2026-136835를 Jenkins는 High로, 전역 GHSA 미러는 critical 9.9로 적었다.

[KEV](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json) · [GeoServer(THN)](https://thehackernews.com/2026/11/hackers-exploit-two-geoserver-flaws-as.html) · [Drupal(THN)](https://thehackernews.com/2026/11/drupal-fail2install-exploit-details.html) · [VMware(The Register)](https://www.theregister.com/virtualization/2026/11/25/this-could-mess-with-thanksgiving-says-vmware-as-it-drops-third-critical-patch-bundle-in-a-week/5312017) · [NTLM(The Register)](https://www.theregister.com/security/2026/11/25/microsofts-ntlm-security-changes-trip-up-third-party-logins/5312085) · [Jenkins](https://www.jenkins.io/security/advisory/2026-11-25/)

### 11. 논문 (HF Daily Papers 11-25분 55편, arXiv 11-25 공지)

수치는 초록에서 확인했고 재현하지 않았다.

- **Lazy Agents / RLVB:** 원문 "The reward needs no judge, critic or reward model". [논문](https://huggingface.co/papers/2611.32884)
- **Loom Attention:** 원문 "LoomFormer matches Transformers trained with over 3x compute". [논문](https://huggingface.co/papers/2611.30634)
- **NightWatch:** 컴퓨터 사용 에이전트가 화면을 본 뒤 행동하기까지의 지연 때문에 이미 바뀐 화면에 반응하는 문제를 다룬다. 원문 "a plug-and-play framework that endows any CUA with continuous, temporally grounded perception". [논문](https://huggingface.co/papers/2611.28947)
- **Embodied AI Fails the Mirror Test:** 원문 "every mirror condition reduces aggregate success rates". [논문](https://huggingface.co/papers/2611.33010)

### 12. 짧게

- **GitHub 대규모 저하(15:55~16:52 UTC, critical):** Git, API, Actions, Webhooks, PR 등 8개 구성요소가 영향을 받았다. 원인 원문: "increased failures due to high database load". 같은 날 Copilot 코드 완성 장애(00:08~01:15 UTC)도 있었다. [githubstatus](https://www.githubstatus.com/incidents/qn8mfv6q5hlx)
- **Hex.pm 후속:** 블로그 타임라인은 11-24 이후 갱신이 없다. THN 기사(09:06 UTC)는 재보도다.
- **기타:** Clojure core를 Lean으로 검사해 알려지지 않은 버그를 찾았다는 글(HN 160점대). 자동 증명은 막다른 길이라는 반론 논문 "The industrialization of noise"(arXiv 2611.31286, 11-19 제출, HN 64점). Gemini가 동요 가사를 거절했다는 일화(HN 210점대, 커뮤니티).
- **직전 판 후속 중 진전 없음:** Ruby2ßhell PoC는 예고일(11-25)이 지났지만 공개 흔적이 없다. Veria 페이지는 11-21 이후 갱신이 없다. 그 밖에 MiniMax 추가 공개, ZDI PyTorch·DeepSpeed, Milvus 수정판, LMCache·crewAI 권고, Azure APIM, Grafana CVE 본문, Zstandard(Ubuntu `needs-triage`), OpenSSL(12-01 예정 그대로), Microsoft agent-framework·ONNX Runtime 저장소(404)도 진전이 없다.

### 참고: 창 경계 항목

- **(원문 11-19, HN 11-25 02:34 UTC 440점대) 같은 vLLM 서버의 모델끼리 샌드박스를 넘어 신호를 주고받는다:** 배치 크기에 따른 커널 선택과 공유 KV 캐시를 부채널로 쓴다. 원문: "Model B (Bob) correctly guesses Alice's color in 1,221/1,221 rounds". 다만 원문 "My initial setups are quite contrived: I tell the models the protocol and fine-tune them to detect signals."다. 완화책은 배치 불변 커널(원문 "make activations bit-identical regardless of batch size")과 사용자별 KV 캐시 격리, 감시 모델을 다른 엔진 인스턴스에서 돌리는 것이다. [LessWrong](https://www.lesswrong.com/posts/7wzWuLP3HhyBLmKfo/models-can-communicate-via-inference-servers-despite-being-sandboxed) · [HN](https://news.ycombinator.com/item?id=50637686)
- **(글 날짜 11-24, HN 21:47 UTC) Defang 침해:** 권한이 지나친 개발용 GitHub App으로 릴리스 태그를 밀어 CI에서 운영 자격 증명을 빼 갔다. 원문: "Our production environment trusted release tags". Defang GitHub App을 설치했다면 워크플로·Secrets·배포 키를 점검한다. [Defang](https://defang.io/blog/post/security-incident-november-2026/)

## 써볼 만한 도구

이번 창은 새 도구보다 **올리기 전에 읽어야 하는 릴리스**가 많다. Claude Code는 직전 판의 Skills over MCP를 CLI에 넣으면서 이를 막는 훅도 함께 넣었다. LiteLLM 로그인 우회는 고친 안정판이 나왔다. Open WebUI는 보안 수정과 되돌리기 어려운 DB 변환을 한 번에 냈다. GitHub와 Cloudflare는 비용과 결제 기본값을 바꿨다.

### 1. (지금 업데이트, 단 깨지는 변경 확인) Claude Code 2.1.332가 `latest`가 됐다. MCP 서버의 스킬이 CLI에 들어왔고, 스킬 속 셸 명령을 막는 훅이 생겼다 (공식, 릴리스 01:32 UTC)

- **한 줄 설명:** 직전 판 `next`에 있던 판이 정식이 됐다. npm `latest`·`next`가 모두 2.1.332이고 `stable`은 2.1.326이다.
- **새 기능(릴리스 원문):**
  - "Added skills from MCP servers (beta). They show as `server:skill` in the `/` menu; other skills keep their names"
  - "Added a `PreSkillShellCommand` hook event that can refuse a skill or command's inline shell commands as it loads"
  - 직전 판 도구 2번의 함정(서버가 스킬 내용을 언제든 바꿀 수 있다)을 막을 첫 수단이다. 회사 설정에서 이 훅으로 MCP 스킬의 인라인 셸 명령을 거부할 수 있다.
- **깨지는 변경(원문):**
  - 이름 예약: "Changed `computer-use` to a reserved skill name on Macs with computer use on: your own skill or command of that name stops loading"
  - 토큰 접두사: "its prefix can now end in `oat02-` as well as `oat01-`". self-hosted runner 토큰을 `oat01-`로만 거르는 프록시 규칙과 로그 마스킹을 고친다.
  - OTel: 데스크톱 Code 탭의 `service.name`이 `claude-code-desktop`으로 바뀌었다. `claude-code`로 거르는 대시보드를 고친다.
  - 클라우드 기본값: Foundry 기본 모델이 Sonnet 5.5가 됐다. Vertex는 `VERTEX_REGION_CLAUDE_5_5_SONNET`을 읽는다. 4.5 변수만 설정했다면 요청이 `CLOUD_ML_REGION`이나 `us-east5`로 간다. 데이터 리전 제약이 있으면 꼭 본다. Bedrock Mantle의 `sonnet` 별칭은 원문 "it now means Sonnet 5, not Sonnet 5.5, and an account without Sonnet 5 gets an error, so enable it first"다. 세 클라우드 모두 모드를 정하지 않으면 auto 모드로 시작한다.
  - SDK·`claude -p`: 턴 중간에 넣는 메시지를 사용자 지시로 취급한다. 다른 출처의 글을 중계한다면 원문대로 "mark relayed text `isSynthetic: true`"다. Agent SDK TS 0.3.332는 `setPermissionMode()` 반환형이 바뀌어 컴파일이 깨질 수 있다.
- **새로 보고된 문제(커뮤니티):**
  - **한국 독자 주의, #112183:** 원문 "/ship falls back to review-only for PRs whose branch name has Korean characters." 한글 브랜치 이름의 PR에서 Claude Ship이 리뷰만 한다. 직원 답은 없다.
  - #110979: Read deny 규칙이 mod의 도구 호출을 막지 못한다는 보고가 2.1.332에서도 재현됐다. 원문: "returns the full content of a file covered by a Read deny rule".
- **설치/사용:** `claude update`

[릴리스](https://github.com/anthropics/claude-code/releases/tag/v2.1.332) · [Agent SDK TS 0.3.332](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.332) · [#112183](https://github.com/anthropics/claude-code/issues/112183) · [#110979](https://github.com/anthropics/claude-code/issues/110979)

### 2. (직전 판 6번 후속, 지금 업데이트) LiteLLM 1.111.0: 다른 사용자로 로그인되는 CVE-2026-93355를 고친 첫 안정판 (공식, 01:42 UTC + 직접 확인)

- **확인:** 권고의 수정 버전은 1.111.0이다. 같은 창에 나온 패치판 1.110.4·1.109.5·1.108.18에는 수정이 없다(도구 에이전트가 wheel에서 예외 변수 이름을 확인). **1.111.0으로 올려야 고쳐진다.**
- **깨지는 변경(릴리스 원문):** "fix(cost)!: bill transcriptions by the seconds the provider reports", "fix(proxy)!: return managed file ids from rag ingest and use team credentials on vector store file routes". 음성 전사 비용 집계와 RAG 파일 ID를 쓰는 코드를 확인한다.

[릴리스](https://github.com/BerriAI/litellm/releases/tag/v1.111.0) · [권고](https://github.com/BerriAI/litellm/security/advisories/GHSA-w5h5-479w-p5hx)

### 3. (지금 업데이트, 단 DB 백업 먼저) Open WebUI 0.13.5: 내용을 밝히지 않은 보안 수정과 되돌리기 어려운 DB 변환 (공식, 09:37 UTC)

- **보안(릴리스 원문):** "Update Open WebUI deployments to 0.13.5 promptly to receive this release's security fixes." CVE가 붙으면 권고를 공개한다고 했다. 조회 시점까지 새 권고는 없다.
- **DB 변환(원문):** "Open WebUI 0.13.5 stores each chat message separately, so upgrading converts the history in your existing database." 이어서 원문: "upgrade all workers and servers together because the old and new versions use incompatible formats." 백업 없이 올리면 되돌릴 수 없다.
- **그 밖의 변경:** Terminal 연결이 Machines로 바뀌어 API 경로가 `/api/v1/machines`가 됐다. OpenAI 주소를 넣으면 원문 "Open WebUI now defaults to the Responses API"다. 실험 기능으로 서버 자체를 채팅의 머신으로 쓸 수 있는데, 원문 "Commands run on the same computer that hosts Open WebUI."다.

[릴리스](https://github.com/open-webui/open-webui/releases/tag/v0.13.5)

### 4. (기본값 변경 예고) GitHub Copilot 코드 리뷰 비용을 12-30부터 조직이 낸다. 바꾸지 않으려면 미리 골라야 한다 (공식 변경 로그, 22:18 UTC)

- **원문:** "If you haven't chosen how to bill Copilot code review for members with a Copilot license, those reviews will be billed to your organization starting December 30, 2026. To keep billing unchanged, select Member before then."
- **원문:** "Organization billing requires AI Credits paid usage and can increase your organization's spending."
- **추천 이유:** Business·Enterprise 관리자라면 연말 휴가 전에 설정을 확인한다. 이미 선택을 저장한 조직은 그대로다.

[GitHub 변경 로그](https://github.blog/changelog/2026-11-25-copilot-code-review-billing-will-default-to-the-organization)

### 5. (주의) Cloudflare: 에이전트가 CLI·MCP에서 유료 구독을 바로 켤 수 있다 (공식 변경 로그, 날짜 11-25)

- **원문:** "You and your agents can now activate Cloudflare subscriptions from the Cloudflare CLI (`cf`) or Cloudflare Model Context Protocol (MCP) servers, without switching to the dashboard."
- **확인 절차:** MCP 클라이언트가 elicitation을 지원하면 원문 "Cloudflare displays the plan price and asks for your permission before activating a subscription"이다. CLI 경로에서도 확인을 거치는지는 글에 없다.
- **할 일:** 에이전트에 쓰는 Cloudflare API 토큰에서 결제 권한을 빼 두는 편이 안전하다.
- **같은 날:** "Search for Agents"는 도메인에 `/.well-known/search` 검색 엔드포인트를 연다. 원문 "available on all plans"다.

[구독 활성화](https://developers.cloudflare.com/changelog/post/2026-11-25-activate-pay-as-you-go-cli-mcp/) · [Search for Agents](https://developers.cloudflare.com/changelog/post/2026-11-25-search-for-agents/)

### 6. (깨지는 변경) Vercel Sandbox 5.0.0: `runtime` 옵션이 사라졌고, 프록시가 다른 프로젝트의 샌드박스를 거부한다 (공식, 18:50 UTC)

- **원문:** "`defineSandboxProxy` now accepts only sandboxes of the proxy's own project, read from `VERCEL_PROJECT_ID`." `VERCEL_PROJECT_ID`가 없는 곳(예: Vercel 밖)의 프록시는 원문 "rejects every sandbox until it has an `allow`"다.
- SDK의 `runtime` 옵션과 CLI의 `--runtime`이 없어졌다. 직전 판에서 다룬 관리형 이미지 전환의 다음 단계다.

[릴리스](https://github.com/vercel/sandbox/releases/tag/%40vercel%2Fsandbox%405.0.0)

### 7. (깨지는 변경) Claude Security 플러그인 0.13.1: `claude-security` 명령에 `--provider`가 필수가 됐다 (공식, PR 병합 22:51 UTC)

- **README 원문:** "`--provider` says where your code goes: `anthropic`, `bedrock`, `vertex` or `foundry`, with `--model-base-url` for another host. It is required, except with `--no-sandbox`, where Claude Code chooses."
- 코드가 어느 클라우드로 가는지 명시하게 한 변경이다. 이 명령을 부르는 스크립트는 고친다. 직전 판의 GitHub Action은 바뀌지 않았다.

[PR #7224](https://github.com/anthropics/claude-plugins-official/pull/7224) · [README](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/claude-security/README.md)

### 8. pydantic-ai 2.74.0: 보안 권고 6건과 GPT-6.2 캐시 수정. 2.75.0은 Gemini 동작이 바뀐다 (공식, 2.74.0 11-24 23:22 UTC, 2.75.0 18:21 UTC)

- **권고 6건(직접 셈):** medium 1, low 5다. 4건은 `pydantic-ai-harness`의 `PlaywrightBrowser`가 사설 주소 차단을 우회하는 문제다. 1건은 ElevenLabs 자격 증명이 트레이스에 남는 문제다. 릴리스 본문과 권고 API의 심각도·CVE 번호가 어긋나니 권고 API를 기준으로 본다.
- **GPT-6.2 캐시(직전 판 후속):** 원문 "Send static instructions as leading `input` messages on GPT-6.2, which never caches top-level `instructions`". goose 1.61.0에도 같은 수정이 들어갔다.
- **2.75.0 주의:** `gemini-2.5-flash`에 `thinking=False`를 주면 원문 "now sends `thinking_budget=128` instead of `0`"이다. 12-01 라우팅 변경 뒤에는 0이 400 오류를 낸다고 적었다.

[2.74.0](https://github.com/pydantic/pydantic-ai/releases/tag/v2.74.0) · [2.75.0](https://github.com/pydantic/pydantic-ai/releases/tag/v2.75.0) · [권고](https://github.com/pydantic/pydantic-ai/security/advisories/GHSA-g87v-g79r-whvj)

### 9. VS Code 1.148: 에이전트가 일하는 동안 절전을 막고, 셸 비밀번호 질문을 채팅으로 받는다 (공식, 09:44 UTC)

- **절전 방지:** 기본값이 원문 "Prevent idle sleep while the host is plugged in."이다. 노트북에서 긴 에이전트 작업이 멈추지 않는다.
- **비밀번호 질문:** Copilot 하네스에서 sudo·ssh·git 비밀번호 질문이 채팅에 뜬다. 원문: "Your answer goes only to that program and isn't sent to the model." 끄려면 `chat.agentHost.copilot.shellAuthentication.enabled`를 false로 둔다.
- **Skills over MCP:** Agent Host Protocol 1.1.0에 원문 "Support for representing SEP-2640 Skills over MCP"가 들어갔다.

[VS Code 1.148](https://code.visualstudio.com/updates/v1_148)

### 10. (기본값 변경) Kimi Code 2.8.0: "항상 묻기" 모드에서도 읽기 전용 Bash를 자동 승인한다 (공식, 12:41 UTC)

- **원문:** "Automatically approve statically verified read-only Bash commands in Always Ask mode. Add an `ask` permission rule to keep confirming them."
- **함정:** 직전 판 ParseGap과 오늘 기술 이슈 7번의 DeepSeek-Reasonix 사례처럼 "읽기 전용" 판정은 우회될 수 있다. 모든 명령을 확인받으려면 `ask` 규칙을 추가한다. 직전 판들이 추적한 Plan 모드 우회 수정은 이번에도 보이지 않는다.

[릴리스](https://github.com/MoonshotAI/kimi-code/releases/tag/%40moonshot-ai%2Fkimi-code%402.8.0)

### 짧게

- **(직전 판 4번 후속) T3 Code 0.1.12(07:36 UTC):** 계정 식별자를 보내던 텔레메트리 수정(#42299)이 이 안정판에 들어갔다(태그 조상 관계로 확인). 릴리스 본문에는 적혀 있지 않다. 태그 시점 문서 원문: "Product events do not include prompts, responses, file contents, authentication tokens, conversation IDs, raw provider events, child-agent output, or identifiers from your provider accounts." [릴리스](https://github.com/pingdotgg/t3code/releases/tag/v0.1.12)
- **FastMCP 4.2.6(00:40 UTC):** 원문 "Authlib is no longer installed with fastmcp." `authlib`를 직접 import하는 코드는 의존성을 따로 적는다. [릴리스](https://github.com/PrefectHQ/fastmcp/releases/tag/v4.2.6)
- **Copilot SDK 2.0.3(17:20 UTC):** 원문 "An empty managed sandbox `allowedHosts` list no longer blocks all hosts, even if the previous opt-in was enabled." 빈 목록으로 네트워크를 막아 두었다면 다시 설정한다. [릴리스](https://github.com/github/copilot-sdk/releases/tag/v2.0.3) Copilot CLI 1.0.116도 나왔다. [릴리스](https://github.com/github/copilot-cli/releases/tag/v1.0.116)
- **goose 1.61.0(03:31 UTC):** GPT-6.2 캐시 수정과 원문 "Treat all headers as secret in mcpServers configs over ACP"가 들어갔다. [릴리스](https://github.com/aaif-goose/goose/releases/tag/v1.61.0)
- **Pi 1.6.1(13:41 UTC):** 원문 "Changed the default Mistral model from `devstral-medium-latest` to `mistral-medium-latest`, because Mistral retired Devstral". [릴리스](https://github.com/earendil-works/pi/releases/tag/v1.6.1)
- **Mastra core 1.86.0(10:00 UTC):** 원문 "Fixed read-only filesystems mounted into a natively isolated `LocalSandbox` being writable from shell commands." `wait: true`는 무시되니 `waitSeconds`로 바꾼다. [릴리스](https://github.com/mastra-ai/mastra/releases/tag/%40mastra%2Fcore%401.86.0)
- **TRL 1.19.0(17:28 UTC):** vLLM 0.34와 함께 쓰면 원문 "0.34's new default of 1.0 OOMs the first weight sync"다. 문서 명령에 `--gpu-memory-utilization 0.9`를 붙였다. [릴리스](https://github.com/huggingface/trl/releases/tag/v1.19.0)
- **llama.cpp: KRAFTON Raon-1 mini 지원 병합(18:38 UTC):** 아직 릴리스 빌드에는 없다. PR 원문: "AI usage disclosure: YES. Claude Opus 5.5 and Codex wrote most of the code." [PR #32176](https://github.com/ggml-org/llama.cpp/pull/32176)
- **Zapier MCP(HN 16:07 UTC):** 원문 "MCP tool calls are free through noon Eastern time on Jan. 5, 2027." [Zapier](https://zapier.com/mcp)
- **AWS Kiro Web:** 모델에 따라 동의 없이 명령이 실행되던 CVE는 기술 이슈 7번에 정리했다. 사용자 조치는 필요 없다.
- **1Password를 쓰는 개발자:** 공식 글에 CLI·SDK·Connect·서비스 계정 언급이 없고, 창 안에 새 릴리스도 없다. 사고는 기술 이슈 4번.
- **Show HN:** TokenTV(5달러 시계를 Claude Code·Codex 사용량 표시기로, MIT). [저장소](https://github.com/click6067-ship-it/token-tv) Carry(AI 대화를 로컬 Markdown으로 저장하는 유료 Chrome 확장). [Carry](https://carry.md/)
- **직전 판 후속:**
  - claude-code-action 1.0.290(01:35 UTC)은 Claude Code 2.1.332 반영뿐이고 포크 PR 건너뛰기 기본값은 그대로다.
  - Codex CLI `latest`는 0.172.0이고 0.174.0 알파만 나왔다. #62312는 열려 있다.
  - Inkling 2 Mini: llama.cpp #32778·#32710·#32836·#32910·#32966은 모두 열려 있다. Unsloth 0.1.925-beta가 나왔다.
  - ERNIE 5.5 Flash: SGLang 지원 PR #51236이 열려 있다. Bonsai 2.5는 PrismML 포크 빌드가 나왔다.
  - 새 진전 없음: 아티팩트 데스크톱 앱(릴리스 노트에 새 항목 없음), MCP 레지스트리 #1920, Claude Code #110642·#111167·#111218·#111068, Shirudo #299, OpenAI Agents SDK, GitHub MCP Server, Context7, Gemini CLI, OpenCode, Cline, vLLM, SGLang, transformers, MCP 공식 SDK·Inspector, Ollama. 관리형 마켓플레이스 자동 갱신 기본값(11-30)은 그대로다.

### 참고: 창 경계 항목

- **(원문 11-23 14:13 UTC, The Register 11-25 14:07 UTC) JetBrains Kotlin Benchmark 2.0:** 공개 저장소 과제에서는 에이전트가 이미 나온 수정을 찾아보는 일이 있었다. 원문: "in our preliminary runs, we found agents were doing exactly that". 그래서 비공개 코드 과제를 넣었다. 공개 과제 1위는 86.2%, 비공개 과제 1위는 60%다(The Register 기준). [JetBrains](https://blog.jetbrains.com/kotlin/2026/11/kotlin-benchmark-2-0-how-ai-coding-agents-handle-harder-tasks-and-closed-source-code/) · [The Register](https://www.theregister.com/devops/2026/11/25/jetbrains-finds-agents-may-cheat-with-open-source/5311999)

## 주목할 점

- **증명은 확인되는 속도보다 빨리 쌓인다.** 두 회사가 하루에 열 편이 넘는 "해결"을 올렸다. 그런데 이번에는 기계 검증 기록조차 없다. 리만 가설은 공개부터 외부 검증 완료까지 일주일이 걸렸다. 다음 판에서는 OpenAI 원고의 외부 Lean 빌드와 Google Lean 공개를 먼저 본다. 그 전까지는 "주장"으로 읽는다.
- **"읽기 전용이면 자동 승인"이 퍼진다.** Kimi Code가 기본값으로 넣었고, Kiro Web은 붙인 모델에 따라 동의 없이 명령을 실행했다. DeepSeek-Reasonix는 `git blame` 판정이 뚫렸다. 승인 규칙은 모델이나 분류기를 바꿀 때마다 다시 시험해야 한다.
- **비용·데이터 기본값을 보는 연말이다.** Copilot 코드 리뷰는 12-30부터 조직 과금이 되고, Cloudflare는 에이전트가 구독을 켤 수 있게 했다. Upstage의 싼 Contributor 등급은 데이터 사용과 맞바꾼다.
- **다가오는 일정:**
  - 이번 주: **KEV 조치 기한 11-26(MONAI Label·N-able), 11-27(Drupal 외), 11-28(새로 오른 8건, 추수감사절 연휴)**. Ruby2ßhell Rails PoC는 예고일(11-25)이 지났고, 원문은 "or later"를 허용한다.
  - 다음 주: **Claude Sonnet 4.5 API 은퇴(11-30)**, Claude Code 관리형 마켓플레이스 자동 갱신 기본값(11-30 이후), Gemini 4 Carbon·Argon 도입가 종료(11-30), Huawei openPangu-2.0 종료(11-30), **OpenSSL 4.1.1·4.0.4·3.5.10(12-01)**, Gemini 2.5 Flash 라우팅 변경(12-01), NVIDIA Huang 하원 비공개 인터뷰(12-01)·상원 은행위 청문회(12-03), Puppet Enterprise(12-01)·LiteLLM(12-02) KEV 기한.
  - 12월: **Claude 앱 선택기에서 Sonnet 4.6 제거(12-08)**, Rackspace Spot 종료(12-08), Vercel Audited ZDR 첫 보고서(12-08까지), SWE-2.1 무료 기간 종료(12-08), EU·EFTA·영국 Copilot 테넌트 Claude 기본 켜짐(12-09), Val Town 읽기 전용(12-10), GitHub Copilot 모델 6종 은퇴(12-14), GPT-5.6 Sol·Terra 은퇴(12-23), Inception Mercury API 종료(12-28), **Copilot 코드 리뷰 조직 과금 기본값(12-30)**, FinCEN 연기된 제출 기한(12-30).
  - 그 뒤: Zapier MCP 무료 종료(2027-01-05), BIS 원격 접근 규칙 의견 마감(01-19), Claude Opus 4.5 은퇴(01-22), 다음 MCP 명세(03-30), Microsoft NTLM 서명 2단계(2027-05).

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - reddit("인수분해가 P로 풀렸다" 루머), X 타임라인, The Information(유료), Fortune, SCMP, WSJ, Reuters는 열지 못했다. X 글은 개별 글만 fxtwitter로 읽었다.
  - OpenAI·Google 수학 원고는 PDF 본문을 읽지 않았다. README, Lean 범위 문서, 커밋, PR 본문으로만 확인했다.
  - FinCEN 원문 페이지(SPA), watchTowr·Searchlight Cyber 원문은 2차 보도로만 확인했다. Axios·OpenSec·Aaronson·THN 본문은 r.jina.ai로 읽었다.
- **시각이 날짜만 있거나 불확실한 원문:** 1Password, Anthropic Rust 글, Cloudflare 변경 로그, Jenkins, 스위스 NCSC, Intruder, Defang은 날짜만 있다. 1Password는 HN 첫 게시 시각을 함께 적었다.
- **확인하지 못한 것:**
  - OpenAI 수학 결과를 만든 모델 이름과 결과를 얻은 날짜, Lean 빌드 성공 여부. Google Lean 형식화. 두 회사 직원의 X 반응.
  - 1Password 피해 규모와 GitHub 계정 침투 경로. Private Inference의 단가·대상 모델. ChatGPT 공유 한도 지원 페이지. Upstage·Claude Duet 인도 확대의 공식 발표. Qwen4-Max-Preview 종료 여부.
  - 재현하지 않은 것: 모든 증명, 벤치마크, 취약점 주장. 도구와 패키지는 하나도 설치하거나 실행하지 않았다. GHSA·KEV·MONAI Label 권고 건수, SEC 8-K 문구, OpenRouter 단가는 공개 API와 원문으로 직접 확인했다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 80점 이상 스토리 29개와 20점 초과 스토리 61개의 제목을 모두 봤다. 그 아래는 키워드로 걸렀다.
  - 전역 GHSA는 reviewed 8건, unreviewed 582건, malware 77건을 받아 셌고, unreviewed는 AI 키워드와 critical 제목으로만 걸렀다. 저장소 권고는 136곳을 봤다. 도구 릴리스는 저장소당 최근 릴리스만 봤다.
  - Node.js·Go·Chrome·Rails의 창 안 보안 릴리스는 따로 조회하지 않았다. 급조 저장소 후속과 GitHub 트렌딩, NVIDIA SEC 제출은 보지 못했다. Mistral·Moonshot·Vertex AI 변경 로그와 한국 랩은 얕게 봤다.
  - 창이 끝날 때 열려 있던 것: Claude Microsoft 365 추가 기능 장애, OpenAI 수학 원고의 외부 빌드, Google Lean 공개, Ruby2ßhell PoC, llama.cpp Raon-1 mini 빌드 포함, Inkling 2 Mini 이슈들. 다음 판에서 다룬다.
