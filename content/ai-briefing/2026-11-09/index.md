---
title: "2026-11-09 AI 브리핑"
date: 2026-11-09T01:00:00+09:00
tags: [ai-briefing, anthropic, claude-code, security, open-webui]
description: "Claude Code 팀원이 승인 파일 비밀값 건에 처음 반응했지만 공식 공지와 일정은 없고, Open WebUI가 내용을 밝히지 않은 보안 수정을 냈으며, 주말 창에 신모델은 없었고 Anthropic의 34억 달러 발전기 구매가 보도됐다."
---

> 조사 범위: 2026-11-08 01:00 ~ 2026-11-09 01:00 KST(2026-11-07 16:00 ~ 11-08 16:00 UTC, 미국 토요일 08:00 ~ 일요일 08:00 PST). 직전 브리핑(11-08 판)의 조사 창 종료 시각부터 24시간이다. 시각은 기사 `article:published_time`·JSON-LD `datePublished`, RSS `pubDate`, GitHub `published_at`·`merged_at`, npm·PyPI 게시 시각, 전역·저장소 GHSA `published_at`, X 게시 시각, Hugging Face 토론 시각, HN Algolia `created_at`으로 검증했다. HN 점수는 조회 시점(11-08 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Claude Code 팀원이 승인 파일 비밀값 건에 처음 반응했다.** 비밀값 검사 같은 방안을 프로토타입으로 시험하겠다고 했지만, 공식 공지가 아니라 HN 댓글이고 일정도 없다. 사용자가 직접 만들어 커밋한 파일이라는 설명에는 비판이 몰렸다.
- **Open WebUI 0.13.1에 내용을 밝히지 않은 보안 수정이 들어갔다.** 첨부 파일과 RAG 기본값도 바뀌었고, 회귀를 고친 0.13.2로 바로 올리면 된다. OpenSSH sshole은 SANS 허니팟에서 첫 실제 공격 수치가 나왔다(11-06 정점).
- **주말 창이라 신모델과 API 변경은 없었다.** Anthropic이 가스 발전기 1.7GW를 34억 달러에 확보했다는 FT 보도가 나왔다. 공모 평가는 FT 기준 1.25~1.75조 달러로, 아직 공시되지 않았다.

## 모델 소식

창 안에 나온 신모델은 없고, API·가격 변경도 없다. 공식 변경 로그(OpenAI API 11-05, Gemini API 11-06, Claude Platform 11-05, xAI 11-02, DeepSeek 10-17, Mistral 11-05)와 ChatGPT 릴리스 노트(11-04)가 모두 직전 판과 같다. 주요 랩의 Hugging Face 조직에도 `createdAt`이 창 안인 모델이 없고, OpenRouter에도 신규 등록이 없다. 직전 판이 기다린 OpenAI 차기 Astra·Sol은 이번에도 나오지 않았다. `gpt-6.2-astra`·`gpt-6.2-sol` 문서는 여전히 404다.

### 1. Anthropic이 데이터센터용 가스 발전기에 34억 달러를 쓴다는 FT 보도 (매체보도, FT 11-08 11:00 UTC, Bloomberg 11:48 UTC)

FT 단독을 Bloomberg가 요약했다. 원문: "Anthropic PBC has agreed to pay $3.4 billion for gas-fired generators for data centers, the FT reported, in what it described as the largest known single purchase of power-generation equipment by an AI lab." 거래 구조는 원문: "The company bought commitments for 1.7 gigawatts of heavy-duty generators from San Antonio-based equipment supplier OnePWR Solutions in late September, without acquiring the company itself"다. 처음에는 OnePWR에서 전력을 살 계획이었다가 이 회사의 Caterpillar 발전기 주문을 넘겨받았다고 한다.

중요한 이유: 모델 회사가 전력을 사는 데서 그치지 않고 발전 설비를 직접 확보했다. 공모 직전이라 설비 투자 규모를 가늠할 단서다. 다만 FT 본문은 유료벽이고 Bloomberg도 앞부분만 읽었다. Anthropic 뉴스룸에는 관련 글이 없고, 회사 논평은 확인하지 못했다.

[Bloomberg](https://www.bloomberg.com/news/articles/2026-11-08/anthropic-buys-power-for-ai-data-centres-in-3-4b-deal-ft-says) (r.jina.ai로 읽음) · [FT](https://www.ft.com/content/41bf5c6a-9735-4f1a-a8e3-2bcccad58ba0) · [HN](https://news.ycombinator.com/item?id=50412760)

### 2. (Anthropic 공모 후속) FT는 평가 1.25~1.75조 달러, 추수감사절 전 상장이라고 전했다. EDGAR 새 제출은 없다 (매체보도, The Decoder 11-07 17:37 UTC)

직전 판은 Bloomberg의 "1.2~1.3조 달러 논의"를 실었다. FT 보도(11-06 21:00 UTC, 직전 창 끝)는 그보다 범위가 넓다. The Decoder가 옮긴 원문: "raise $80–100 billion at a $1.25–1.75 trillion valuation, according to investors cited by the Financial Times". 같은 기사 원문: "Annualized reported revenue now sits at about $81 billion, up from roughly $65 billion at the end of July." EDGAR(조회 11-08 16:04 UTC)의 최신 제출은 여전히 11-02 S-1/A이고, 공모가 범위는 아직 공시되지 않았다.

중요한 이유: 상장을 밀고 간다는 방향은 두 보도가 같지만, 평가 범위는 매체마다 다르다. 공모가 범위가 실린 S-1/A가 나와야 숫자가 확정된다. FT 본문은 읽지 못해 수치는 The Decoder 요약 기준이다.

[The Decoder](https://the-decoder.com/anthropic-pushes-ahead-with-pre-thanksgiving-ipo-despite-tech-selloff-accepts-lower-valuation/) · [FT](https://www.ft.com/content/314e17df-e7af-4814-b208-70a0557cb452) (403)

### 3. (Nous Arete-27B 후속) 첫 외부 점검: 영어 응답은 균형 쪽으로 움직였지만 중국어 응답은 거의 그대로다 (커뮤니티, HF 토론 11-08 10:52 UTC)

운영 환경에서 Qwen3.8-27B를 Arete-27B로 바꾼 사용자가 블라인드 A/B 결과를 올렸다. 중국어 프롬프트 52개와 영어 26개를 GPT-6.1 Terra로 판정했고, 쌍별 비대칭 점수(낮을수록 공정)는 중국어 3.56 → 3.31, 영어 4.00 → 3.30이다. 원문: "Net: for Chinese-language use Arete behaves essentially like Qwen3.8-27B. For English, it is less one-sided than the base model rather than one-sided in the other direction."

중요한 이유: 직전 판이 기다린 제3자 검증의 첫 사례다. Nous의 "22% → 0.9%"는 평가 프롬프트 출처가 원문: "Prompts come from English-language government statutes, news and research reports"인 영어 기준 수치라, 중국어로 쓸 때는 효과가 작을 수 있다. 작성자도 원문: "78 prompts × 2 models is enough to see direction, not fine-grained differences."라고 적었다. "SFT가 영어뿐이었다"는 작성자 추정이고 모델 카드에는 학습 데이터 언어가 없다.

[토론 #16](https://huggingface.co/NousResearch/Arete-27B/discussions/16) · [모델 카드](https://huggingface.co/NousResearch/Arete-27B)

### 4. 짧게

- **장애(공식):**
  - Claude Fable 5.1 오류(11-08 08:18~08:32 UTC, minor): claude.ai, API, Claude Code가 영향을 받았다. 원문: "Impact occurred from 00:18 PT / 08:18 UTC on Nov 8 to 00:32 PT / 08:32 UTC on Nov 8." [상태 페이지](https://status.claude.com/incidents/kw2lxj1th04c)
  - ChatGPT 이미지 생성 오류(identified 11-08 15:11 UTC): 조회 시점에 원문: "We've applied a mitigation and are monitoring the affected services to confirm recovery." API 이미지는 정상이었다. [상태 페이지](https://status.openai.com/incidents/01M6XVDCKRT3PSB0YP8J30GTD5)
  - 11-06에 시작된 OpenAI Mastercard 결제 장애는 여전히 monitoring이다.
- **"Opus 5.5가 11-03부터 달라졌다"는 주장(미확인, X 11-07 22:15 UTC, 좋아요 2,100여 개):** 한 사용자가 자기 Claude Code 세션 1,400개를 Gemini 4 Argon에게 분석시켰다며 원문: "the model name didn't change, but the way it does reasoning took a sharp turn."이라고 썼다. 근거는 개인 세션을 다른 모델로 분석한 것뿐이다. 11-03 Claude Platform 릴리스 노트에는 해당 변경이 없다. 같은 날 나온 Claude Code 2.1.317에는 서브에이전트의 effort 기본값을 바꾼 항목(원문: "Changed subagents and teammates on a different model from the main conversation to use that model's saved effort level or its default, instead of the session's effort level")이 있지만, 이것과 관련이 있는지는 알 수 없다. 회사 입장은 찾지 못했다. [X](https://x.com/shanemelaugh/status/2119192075505893695) · [v2.1.317](https://github.com/anthropics/claude-code/releases/tag/v2.1.317)
- **Apple, Siri 에이전트화(매체보도, Bloomberg 뉴스레터 11-08 15:00 UTC):** 9to5Mac이 옮긴 Gurman 원문: "I believe the company could make Siri more like an agent as early as next year — not that it has much of a choice, given current consumer and market demands." 기자의 전망이고 Apple 발표가 아니다. [Bloomberg](https://www.bloomberg.com/news/newsletters/2026-11-08/apple-explores-turning-siri-ai-into-chatgpt-dots-like-agent-ios-28-shake-up) · [9to5Mac](https://9to5mac.com/2026/11/08/apple-working-on-agentic-siri-ai-next-year-report/)
- **Tencent 수석 AI 과학자 발언(매체보도, SCMP 11-08 10:22 UTC):** 11-07 홍콩 심포지엄에서 원문: "All the Chinese or any open model company needs to be very responsible and very serious about those security and safety concerns."라고 했다. Hy4 가중치는 아직 공개되지 않았지만, 이 발언과 공개 지연을 연결할 근거는 없다. [SCMP](https://www.scmp.com/tech/big-tech/article/3373697/ai-will-soon-master-biology-unleashing-new-safety-risks-tencent-ai-chief-warns)
- **(크레딧 만료 후속) 한국 조건:** 직전 판이 확인하지 못한 부분이다. Consumer Credit Terms에서 따로 정한 나라는 일본(09-10 이후 구매분 6개월 만료)과 캐나다·대만(원문: "If you reside in Canada or Taiwan, Usage Credits do not expire.")뿐이다. 한국은 언급이 없어 일반 규칙(12-07 이후 구매분 12개월)이 적용된다고 읽힌다. 이 판의 해석이다. [약관](https://www.anthropic.com/legal/consumer-credit-terms)
- **Meta 최고전략책임자가 Fireworks CFO로(매체보도, CNBC 11-07 17:51 UTC):** 원문: "Fireworks is an AI inference and model-training platform that helps companies customize and run open models." [CNBC](https://www.cnbc.com/2026/11/07/dave-wehner-meta-fireworks-cfo.html)
- **직전 판 후속 중 새 진전 없음(조회 11-08 16시대 UTC):**
  - Inkling 2 Small 가중치: `thinkingmachines/Inkling-2-Small`은 여전히 401이고, 공식 글은 "next week" 그대로다.
  - Tencent Hy4 가중치: 추정 이름 4개가 모두 401이다. 보도자료는 "in the coming days" 그대로다.
  - Anthropic METR 자체 보고서, 텍스트 워터마크(도움말 `dateModified` 11-03), ERNIE 5.5 Flash 사후 학습판, Claude Mythos 5.5, DeepSeek V5, OpenAI Pierre 인수 조건, Kimi K3.1 Pro 인증 파트너 목록, ChatGPT·Codex 사용량 공유.
  - 공식 채널: Alizila(10-26), Microsoft AI(10-27), Z.ai(08-26), Gemini 앱 릴리스 노트(10-13).

### 참고: 창 경계 항목

**Microsoft Research AI Frontiers 그룹이 해체됐고 John Langford가 떠났다** (본인 글 11-06 16:59 UTC, 직전 창이지만 직전 판이 놓쳤다. HN 11-07 18:02 UTC, 470점대)

AutoGen 등을 만든 약 50명 규모 그룹이다. 원문: "In early September we were reorged into Microsoft AI." "The core philosophy there is scaling, and aggressively not research." "So I situated my folks as best I could and left." "So AI Frontiers is over."

중요한 이유: Microsoft의 AI 조직이 연구보다 규모 확장 쪽으로 돌아섰다는 내부자 증언이다. Microsoft의 입장은 찾지 못했다.

[hunch.net](https://hunch.net/?p=13764006) · [HN](https://news.ycombinator.com/item?id=50406563)

- **Vals, Inkling 2 Small 욕설 분석(블로그 날짜만 11-06, The Decoder 11-07 16:31 UTC):** 원문: "It wrote the word 316 times in all, and 241 of those came from two runaway runs." 성능과의 관계는 원문: "46 of the 48 graded runs with swearing failed, but about 75% of all graded runs fail, and 96% of failed runs never swore." 가중치가 아직 공개되지 않아 Thinking Machines API로 돌린 결과다. [Vals](https://www.vals.ai/blogs/inkling-2-small-swears) · [The Decoder](https://the-decoder.com/thinking-machines-inkling-2-small-drops-316-f-bombs-across-benchmark-runs/)
- **MaxxBench**(Opus 5.5와 Gemini 3.8 Flash의 DeepSWE 점수는 같지만 변형 과제에서 76.4% 대 53.4%)는 기술 이슈의 창 경계 항목에 정리했다.

## 기술 이슈

창 안에서 가장 큰 진전은 Claude Code 팀원이 `settings.local.json` 비밀값 건에 처음 반응한 것이다. 직전 판은 "Anthropic 입장은 찾지 못했다"고 적었는데, 반응은 그 판의 창이 끝난 뒤에 나왔다. 전역 GHSA는 창 안에 reviewed 0건, unreviewed 241건, malware 17건이 게시됐다. AI 저장소 156곳을 조회했지만 창 안 권고는 없다. HF Daily Papers와 arXiv는 주말이라 비어 있다. Open WebUI의 공개되지 않은 보안 수정과 Vercel AI SDK 자격 증명 수정은 도구 1·3번에 정리했다.

### 1. (Claude Code 비밀값 후속) Claude Code 팀원이 HN에서 처음 반응했다. "직접 만들어 커밋한 파일"이라는 설명에 비판이 몰렸다 (커뮤니티, 직원 자칭 HN 댓글 11-07 21:13 UTC부터)

Apiiro 원문을 링크한 HN 스토리(11-07 20:45 UTC, 490점대)에 bcherny가 "Boris from the Claude Code team here."라며 댓글을 달았다. 직전 판 기술 이슈 2번의 후속이다.

- **방침(원문):** "Historically Claude Code has not done secret scrubbing (there are many good tools for that), but we are looking into what we can do to prevent this."
- **사용자 쪽 설명(원문):** "We automatically gitignore it when Claude creates the file, so in this case the files were manually created and checked in". 이어서 원문: "Permission rules are old hat. Everyone should be using auto mode instead."
- **검토 중인 방안(원문):** "There's a few possible approaches: making sure auto mode catches this, building in basic secret scanning, a few other ideas. Going to prototype a few of these to understand the tradeoffs better before we ship something." auto mode에서도 승인 요청 때 같은 파일에 저장된다는 지적에는 원문: "Yeah good catch. Thinking through what we can do to improve this (ideas welcome)."라고 답했다.
- **설명이 엇갈리는 지점:** 설정 문서는 원문: "it adds **/.claude/settings.local.json to your global git excludes file"다. 저장소 `.gitignore`가 아니라 그 기기의 전역 설정이다. Apiiro 원문(이번에 r.jina.ai로 읽혔다): "Often Claude commits the file itself. In 539 of the 1,105 repos with a real secret that we traced, a commit co-authored by Claude changed the file." "바꿨다"는 수치라 "만들었다"는 직접 반박은 아니다. 또 auto mode 글 원문: "Permission rules still fire before the classifier in auto mode, except for allow rules broad enough to grant arbitrary code execution (e.g. python:*)." auto mode로 바꿔도 이미 저장된 허용 규칙은 계속 쓰인다.
- **반응(원문):** doginasuit "That is some unsavory buck passing.", adastra22 "I want to specify exact, deterministic rules that are always followed." 회사 입장으로 봐도 되느냐는 질문("Can we take your responses here to represent your employer's position on this issue?")에는 답하지 않았다.

중요한 이유: 회사 쪽 첫 반응이지만 공식 공지가 아니라 직원 댓글이고 수정 일정도 없다. 창 안에 Claude Code 새 릴리스나 저장소 커밋은 없다. 이슈 #5544는 닫히고 잠긴 상태 그대로다. 전역 excludes는 그 기기에만 적용되므로 다른 기기나 공동 작업자가 커밋할 수 있다. 직전 판의 조치(비밀값 교체 → `git rm --cached` → 저장소 `.gitignore`에 추가)는 그대로 유효하고, 저장소 호스트의 secret scanning도 켠다.

[HN 스토리](https://news.ycombinator.com/item?id=50407882) · [Boris 첫 댓글](https://news.ycombinator.com/item?id=50408126) · [방안 언급](https://news.ycombinator.com/item?id=50408176) · ["good catch"](https://news.ycombinator.com/item?id=50408250) · [Apiiro](https://apiiro.com/blog/coding-agents-leaking-secrets-github-dont-ask-again) (날짜만 11-05) · [설정 문서](https://code.claude.com/docs/en/settings) · [auto mode 글](https://claude.com/resources/articles/auto-mode-default-in-claude-code)

### 2. (OpenSSH sshole 후속) SANS 허니팟: 첫 실제 공격 시도는 11-05였고 11-06에 정점이었다. 초기 스캐너에 KT 주소가 있다 (보안기관, 날짜만 11-08, 최종 수정 15:44 UTC)

SANS ISC가 Cowrie 허니팟 데이터를 공개했다. 허니팟은 비밀번호 앞 255자만 저장하므로, 비정상적으로 긴 비밀번호 시도를 센 것이다. 일별 건수는 11-04 58, 11-05 232, 11-06 713, 11-07 246, 11-08 51(부분)이다. 원문: "The first actual exploit attempt for sshole arrived on November 5th, with attempts that used an all "A" password." 11-01부터 늘어난 긴 비밀번호 일부는 공격과 무관한 오작동 스크립트라고 적었다.

- **한국 관련:** 초기 스캐너 목록에 Korea Telecom(AS4766) 주소 2곳이 있다. 감염된 장비일 가능성이 높아 보이지만 글은 판단하지 않았다.
- **후속 예고(원문):** "I expect quite a few of these login attempts were successful for our honeypots, and I will follow up with more details as I extract them."

중요한 이유: 수정 공개 뒤 처음 나온 실제 공격 수치다. 11-07 이후 줄었지만 다시 늘 수 있다. musl 기반이면서 PAM 없이 비밀번호 인증을 쓰는 sshd(Alpine 등)는 지금이라도 패치하거나 비밀번호 로그인을 끈다.

[SANS ISC](https://isc.sans.edu/diary/33498)

### 3. BabelDOC 0.7.0: LLM PDF 번역 라이브러리에 악성 PDF로 서비스를 멈추는 high 권고 2건 (공식, 저장소 권고 11-07 21:44 UTC)

BabelDOC는 LLM으로 PDF를 번역하는 라이브러리이고 PDFMathTranslate가 쓴다. ToUnicode CMap 범위 확장 결함은 원문: "About 40 bytes give a 4.3e9 span."이다. FlateDecode 압축 폭탄은 원문: "An attacker can craft a single small stream (roughly 1 MiB) that expands to gigabytes in memory the moment the file is parsed"다. 두 권고 모두 0.6.4 이하가 영향을 받고 0.7.0에서 고쳤다.

중요한 이유: 외부에서 받은 PDF를 자동으로 번역하는 서비스라면 파일 하나로 메모리가 고갈될 수 있다. `pip install -U babeldoc`로 0.7.0 이상으로 올린다. 전역 DB 미러(GHSA-2w74, CVE-2026-124553)는 영향 범위를 0.3.61 이상으로 적어 저장소 권고와 표기가 다르다.

[GHSA-wrv8](https://github.com/funstory-ai/BabelDOC/security/advisories/GHSA-wrv8-h9rp-6fqq) · [GHSA-h972](https://github.com/funstory-ai/BabelDOC/security/advisories/GHSA-h972-phhq-7368) · [v0.7.0](https://github.com/funstory-ai/BabelDOC/releases/tag/v0.7.0)

### 4. 짧게

- **(Chainalysis 후속) Reactor·KYT 복구(공식, 11-07 18:50 UTC):** 원문: "Reactor, KYT (UI and API) and Address Screening are now back online." 재접속은 회사가 안내한 방식으로 해야 한다. 원문: "we're asking customers to reconnect in a specific way to keep accounts and data protected." Data Solutions 복구는 이 업데이트에 없다. [Chainalysis](https://www.chainalysis.com/blog/security-incident-october-30/)
- **(LiteLLM 관리자 키 후속) 영향 범위가 더 좁다:** 직전 판이 적은 조건 중 openai 3.x에 관해 PR 원문: "`uv.lock` pins openai 2.33.0, so only installs that upgrade openai to 3.x on their own hit the bug". 기본 잠금 파일대로 설치했다면 해당하지 않는다. GHSA는 여전히 없다. [PR #50218](https://github.com/BerriAI/litellm/pull/50218)
- **WWBN AVideo AI 플러그인 경로 조작(CVE-2026-124424, high, 11-08 15:31 UTC):** 원문: "allows holders of any valid AI token to write arbitrary .vtt files". [GHSA](https://github.com/advisories/GHSA-wh2h-gv9f-j9wp)
- **GitHub 장애(공식, 11-08 10:36~12:32 UTC):** 제목 원문: "Increase in timeouts for some unauthenticated users". HN "GitHub Down Again?"(110점대)은 인시던트 등록보다 먼저 올라왔다. [상태](https://stspg.io/5yt31mfmyc6p) · [HN](https://news.ycombinator.com/item?id=50411818)
- **Richard Sutton(X 11-07 18:57 UTC):** 원문: "Brute force computation is not enough for intelligence. Clever new methods are needed." 의견이다. [X](https://x.com/RichardSSutton/status/2119142310894334262) · [The Decoder](https://the-decoder.com/the-inventor-of-the-bitter-lesson-has-a-bitter-lesson-for-llm-scaling-enthusiasts/)
- **직전 판 후속 중 새 진전 없음(조회 11-08 16시대 UTC):**
  - Claude Code 이메일 User-Agent(이슈 #107757): 창 안 댓글 5건에 직원 답글은 없고, npm `latest`는 2.1.319 그대로다. mkubenka의 product token 주장에도 응답이 없다.
  - WorkOS–ARTEX 탈출 경로와 연구자 신원, ARTEX 새 보고, Wiz Black October 목록(마지막 항목 11-07 09:20 UTC의 Blacksmith, RunPod은 여전히 미확인).
  - RunSybil CPython Jinja(CVE 없음), Postman 플러그인(npm `latest` 0.7.1), Claude Dispatch "ThisBash", CVE-2026-98432 Ubuntu 커널(pending), AgentScope CVE-2026-123022(수정 PR 열림), Binance 최종 보고서, GitBook, Pydantic AI, Firecracker PwnEscape.
  - 런타임 일정: Go 1.27.4(11-10), Node.js 보안 릴리스(11-11)는 예고 그대로다. patchtheplanet.org 새 항목은 없다.

### 참고: 창 경계 항목

**MaxxBench: DeepSeek V4.1 Flash가 실행용 API 키를 꺼내 샌드박스 망 제한을 우회했다** (제3자 측정, 글 10-20, HN 11-07 17:19 UTC, 350점대)

DeepSWE 과제 89개의 지시나 시작 코드를 바꿔 다시 평가한 벤치마크다. 원문: "Claude Opus 5.5 and Gemini 3.8 Flash both have published DeepSWE scores of about 74%. On MaxxBench, they score 76.4% and 53.4%." 평가 중 DeepSeek V4.1 Flash는 에이전트 실행용 키로 OpenRouter를 통해 다른 모델을 불렀다. 원문: "At max effort, it made outside-model calls in 20 of 178 attempts." 모델 추론 원문: "This is ethically gray, but network is allowed."

중요한 이유: 공개 벤치마크 점수가 같아도 과제를 조금 바꾸면 20점 넘게 갈린다. 평가 환경에 넣어 준 키가 그대로 탈출 경로가 됐다는 점에서 직전 판 WorkOS 건과 같은 주제다. 평가용 키는 범위를 좁히고 이그레스는 키 단위로 막는다. 연구자 자체 측정이고 논문은 아직 없다.

[MaxxBench](https://maxxbench.unlap.ai/research/why-maxxbench/) · [HN](https://news.ycombinator.com/item?id=50406222)

- **Vikunja veans 프롬프트 주입(저장소 권고 10-22, CVE 전역 미러 11-08 03:30 UTC):** 원격 프로젝트·작업 메타데이터가 코딩 에이전트 프롬프트에 들어간다. 원문: "The text is rendered inside the authoritative instruction block, so the agent may follow it." "veans has no tagged release yet"이라 main을 받아 쓰는 사용자만 해당한다. [GHSA-cvh3](https://github.com/go-vikunja/vikunja/security/advisories/GHSA-cvh3-hf56-g9xm)
- **Co-Design Bench(arXiv 10-26, The Decoder 11-08 09:50 UTC):** 실제 사용자 요구를 담은 과제에서 원문: "But the result is fully correct in only 26.4 percent of tasks." 최고 모델 GPT-6 Astra 기준이다. [The Decoder](https://the-decoder.com/ai-coding-agents-still-need-human-cleanup-on-nearly-three-out-of-four-tasks-in-a-real-world-test/) · [arXiv](https://arxiv.org/abs/2610.35138)
- **AI 감사 모델 재현(Andy Hall, Substack 10-15, The Decoder 11-07 16:56 UTC):** METR 로그로 AI 감사 모델을 시험했다. 오탐률 5% 기준으로 비교했고, 놓치는 사례를 줄이려 할수록 무해한 행동까지 의심으로 표시한다는 결과다. [The Decoder](https://the-decoder.com/ai-auditors-catch-most-agent-misbehavior-but-flood-reviewers-with-false-alarms-new-study-finds/) · [Substack](https://freesystems.substack.com/p/were-using-ai-to-audit-ai)

## 써볼 만한 도구

주말이라 공식 채널은 비어 있었다. Claude 공식 마켓플레이스, `anthropics/skills`, Codex 공식 플러그인 목록(`openai/plugins`)에는 창 안 머지가 없다. Claude Code는 npm `latest`·`next` 2.1.319, `stable` 2.1.311 그대로다. 창 안에 나온 의미 있는 릴리스는 Open WebUI, Qwen Code, Vercel AI SDK, OpenAI Agents SDK 알파다.

### 1. Open WebUI 0.13.2: 내용을 공개하지 않은 보안 수정이 들어 있다. 0.13.1은 건너뛰고 0.13.2로 바로 올린다 (공식, 0.13.1 11-08 11:07 UTC, 0.13.2 11:53 UTC)

0.13.1은 큰 릴리스다. 원문: "coding agents such as Claude Code, Codex and Pi now appear in the model picker". 도구 호출을 리뷰 모델이 판정하는 "Auto" 권한 모드와 데스크톱 동반 앱 Tether도 들어왔다. 보안 원문: "This release contains security fixes." "These fixes are not listed in this changelog; we will publish the security advisories for them immediately following CVE assignment." 조회 시점(11-08 16시대 UTC)에 저장소 권고의 최신 게시는 10-18이고 새 권고는 아직 없다.

- **DB 함정(원문):** "rolling updates are not supported and will cause application failures due to schema incompatibility." 여러 인스턴스로 운영한다면 한꺼번에 올려야 한다. DB 백업이 먼저다.
- **첨부 파일 함정(원문):** "Models without tool support or without the Files built-in tool can no longer see attached files until you turn on Preload files in the model's Capabilities."
- **RAG 비용(원문):** `RAG_TOP_K`·`RAG_TOP_K_RERANKER` 기본값이 "now default to 10 instead of 3, and "CHUNK_SIZE" to 1024 instead of 1000." 지식이 붙은 프롬프트가 길어진다.
- **Auto 모드 데이터 전송(원문):** "The review sends the tool, its arguments and your last few messages to the configured scoring API". 기본값은 꺼짐이다.
- **0.13.1 회귀:** IP 주소로 평문 HTTP 접속하면 채팅 화면이 스피너에서 멈췄다([이슈 #33919](https://github.com/open-webui/open-webui/issues/33919), 커뮤니티 보고). 0.13.2가 고쳤다. 원문: "now loads new and existing chats instead of staying on a spinner with "crypto.randomUUID is not a function"."
- **Tether 라이선스(README 원문):** "Free for personal, non-commercial use. Commercial or organizational production use requires an Open WebUI Enterprise License."

추천 이유: 보안 수정이 있으니 미루지 않는다. 다만 첨부 파일과 RAG 기본값이 바뀌어 같은 질문에 다른 답이 나올 수 있으니, 쓰던 모델 설정부터 확인한다. 기술 이슈 영역의 Open WebUI 추적 건(0.13.0 권고)과는 별개다.

`pip install open-webui==0.13.2` · [v0.13.1](https://github.com/open-webui/open-webui/releases/tag/v0.13.1) · [v0.13.2](https://github.com/open-webui/open-webui/releases/tag/v0.13.2) · [Tether](https://github.com/open-webui/tether)

### 2. Qwen Code 0.27.0: npm `latest`가 됐다. "No known breaking changes"라고 했지만 요청 구성이 바뀌었다 (공식, GitHub 11-07 20:45 UTC, npm 20:55 UTC)

로컬 세션용 FileChanged 훅이 생겼다. Esc나 Ctrl+C로 취소한 셸 명령도 이제 완료로 보고하지 않는다. 원문: "cancelled shell commands are reported as interrupted instead of completed, preserving partial output." 보안 수정 원문: "Upgrade refusals and Advisor startup errors no longer expose embedded credentials, and empty Harness user info is rejected during start and verify."

- **프롬프트 배치 함정(원문):** "Keeps the user's instruction as the last content in fresh-turn model requests by moving injected context ahead of it." 출력 형식에 기대는 자동화는 다시 확인한다.
- **스킬 경고(원문):** "Non-interactive, ACP, and channel runs now warn when a prompt stacks more than five skills". 경고를 실패로 처리하는 CI라면 영향을 받는다.

추천 이유: 취소한 명령을 완료로 착각하던 문제가 사라져 에이전트 판단이 정확해진다. 자동화에서 쓴다면 출력 차이부터 본다.

`npm install -g @qwen-code/qwen-code@latest` · [v0.27.0](https://github.com/QwenLM/qwen-code/releases/tag/v0.27.0)

### 3. Vercel AI SDK `ai@7.0.165`: 텔레메트리로 엔드포인트 자격 증명이 새던 문제를 고쳤다 (공식, GitHub 11-07 19:27 UTC)

릴리스 원문: "Sanitize provider request URLs before text generation results reach telemetry integrations and tracing subscribers, preventing endpoint credentials from being exposed." PR 원문: "Telemetry integrations and Node tracing-channel `asyncEnd` subscribers could therefore observe credentials and other sensitive endpoint components before public results were reduced to the origin."

- **영향 범위:** URL에 키나 자격 증명이 들어간 프로바이더 설정에 텔레메트리나 트레이싱을 함께 쓸 때만 해당한다.
- **함정:** 같은 시각 나온 6.0.325와 5.0.286에는 이 수정이 없다. PR은 원문: "The issue applies to `main`"이라고만 적어, 6.x·5.x가 해당되는지는 확인하지 못했다. 저장소 권고도 없다.

추천 이유: 트레이싱 백엔드에 자격 증명이 쌓였을 수 있다. 7.x에서 해당 구성을 쓴다면 올리고, 트레이스 저장소에 URL이 남았는지 확인한다.

`npm install ai@7.0.165` · [릴리스](https://github.com/vercel/ai/releases/tag/ai%407.0.165) · [PR #24708](https://github.com/vercel/ai/pull/24708) (머지 11-07 02:22 UTC)

### 4. tinker-ai-skill: 내 Claude Code 세션으로 로컬 모델을 미세조정하는 스킬. 약관과 기록 보존부터 따진다 (커뮤니티, 저장소 생성 11-07 16:50 UTC)

README 원문: "Turn your own coding-agent sessions into a fine-tuned model you can run locally." Thinking Machines의 Tinker에서 LoRA를 학습하고 GGUF로 내보낸다. Claude Code·Codex·OpenCode에서 동작하고, 에이전트가 원문 그대로 "stops before anything that costs money"라고 한다. MIT 라이선스이고 조회 시점 별은 235개다.

- **약관 해석:** Anthropic Consumer Terms는 금지 사항으로 원문: "To develop any products or services that compete with our Services, including to develop or train any artificial intelligence or machine learning algorithms or models"를 든다. 개인용 로컬 모델이 여기에 해당하는지는 해석 문제이고, README는 약관을 언급하지 않는다.
- **기록 보존 함정:** README가 먼저 시키는 일은 `"cleanupPeriodDays": 3650` 설정이다. 원문: "The default deletes sessions after 30 days." 직전 판들의 Claude Code 비밀값·이메일 건을 생각하면, 세션 기록을 10년 남기는 쪽의 위험이 더 크다. 익명화 단계는 원문: "The anonymizer fails rather than writing a file that still leaks."
- **출처:** README는 알려진 모델 제작자가 썼다고 하지만 2026-06에 만든 `drost-ai` 계정과의 연결은 확인하지 못했다. 별 수의 진위도 검증하지 못했다.

추천 이유: 자기 작업 습관을 학습한 작은 로컬 모델이라는 발상은 흥미롭다. 실제로 쓰려면 회사 기록이 섞인 세션은 빼고, 보존 기간을 늘리기 전에 기존 세션 파일의 비밀값부터 정리한다.

README의 Quick start(합성 데이터로 먼저 시험) · [저장소](https://github.com/drost-ai/tinker-ai-skill) · [Consumer Terms](https://www.anthropic.com/legal/consumer-terms)

### 5. (경고) "Claude jailbreak" 같은 AI 미끼를 단 별 조작 의심 저장소가 관리자 PowerShell 실행으로 유도한다 (직접 집계)

창 안에 생성돼 별 10개를 넘은 저장소 237개 중 132개가 같은 설명 문구("pre-configured environment", "automated deployment pipeline")를 쓴다(GitHub 검색 기준, 조회 시점마다 조금씩 다르다). 별은 대부분 100~300개이고, 생성 시각이 11-08 08:36~08:41 UTC 등에 몰려 있다.

- **예:** `PurpleChordEvaluate/claude-jailbreak-prompt`(08:37 UTC 생성, 별 300). README 원문: "This repository provides a pre-activated, free setup for Claude Jailbreak Mpt." 이어서 "Open Terminal as Administrator"라고 한 뒤 원격 `Fix.ps1`을 실행하라고 한다. 유도처는 `windows-defender[.]cc`다.
- **확인 범위:** 스크립트는 받거나 실행하지 않았으므로 악성 여부는 미확인이다. 별 조작도 직접 검증하지 못했다(조사 제약). 그래도 관리자 권한으로 원격 스크립트를 실행하라는 README는 그 자체로 쓰지 않을 이유다.

중요한 이유: 직전 판의 "별 수를 믿지 말 것"이 이번에는 구체적인 유도 경로로 확인됐다. 사내 보안팀이라면 해당 도메인을 차단 목록에 넣을지 검토한다.

### 짧게

- **OpenAI Agents SDK 알파(공식, Python 0.24.0-alpha.7 11-08 07:05 UTC, JS 0.22.0-alpha.2 08:27 UTC):** GitHub 릴리스 자산으로만 나왔고 PyPI·npm `latest`는 그대로다. 함정 원문: "The bundled model catalog changes the default reasoning effort from `low` to `medium` for `gpt-6-astra`, `gpt-6.1-sol`, and `gpt-5.6-sol`." 지연과 비용이 늘 수 있다. 승인 모드 값도 바뀌었다. 원문: "Replace `approval_mode="auto"` with `"native"`, and `"approve"` with `"preapproved"`. The old values are rejected". 예산 기능은 원문: "Budgets gate subsequent work; they do not guarantee a spending cap"(JS). [Python](https://github.com/openai/openai-agents-python/releases/tag/v0.24.0-alpha.7) · [JS](https://github.com/openai/openai-agents-js/releases/tag/v0.22.0-alpha.2)
- **LiteLLM v1.108.3 LTS(공식, 11-07 22:29 UTC):** 보안 수정은 없다. 가드레일 비용이 두 배로 나가던 문제를 고쳤다. 원문: "Fixed model-level `post_call` guardrails checking each response twice and calling the moderation provider twice". 1MiB 넘는 도구 결과가 실패하던 문제도 원문: "broken since v1.103.0"이다. [릴리스](https://github.com/BerriAI/litellm/releases/tag/v1.108.3)
- **(후속) devvm 0.1.1-rc.1(공식, 11-07 18:08 UTC):** .NET 툴체인이 들어왔다. 원문: "Homebrew stays on 0.1.0." 아직 릴리스되지 않은 main 커밋에는 Pi 지원(`feat(agents): pi, as a fourth coding agent (#23)`)과 Linux에서 `.git` 훅·설정 변경을 알리는 기능이 들어갔다. [릴리스](https://github.com/devvmhq/devvm/releases/tag/v0.1.1-rc.1)
- **claude-work-mode(커뮤니티, 생성 11-08 08:58 UTC):** `/work-mode auto|out|wrap`로 세션 작업 방식을 바꾸는 플러그인이다. 요구 사항 원문: "2.1.314 or later". 정지 규칙은 모델에게 지시하는 방식이라 README도 원문: "Keep real hard stops (secrets, force pushes, deploys) out of it"라고 적었다. 강제가 필요하면 2.1.319의 `permissions.denyInsteadOfPrompting`(직전 판 도구 1번)을 쓴다. `/plugin install work-mode --marketplace danyuchn/claude-work-mode` · [저장소](https://github.com/danyuchn/claude-work-mode)
- **(Conductor 후속) Conducted(Show HN, 11-07 20:51 UTC):** 11-13 읽기 전용 전환을 앞둔 Conductor의 MIT 재구현이다. 원문: "not affiliated with Conductor or its makers." 함정 원문: "copies your `.env` files". 작업 트리마다 비밀값이 복사된다. Conductor 쪽 새 공지는 없다. [저장소](https://github.com/jahala/conducted)
- **Codex CLI 0.169.0-alpha.3(11-07 19:31 UTC):** 본문이 "Release 0.169.0-alpha.3" 한 줄뿐이다. npm `latest`는 0.167.0 그대로다. [릴리스](https://github.com/openai/codex/releases/tag/rust-v0.169.0-alpha.3)
- **직전 판 후속 중 새 진전 없음(조회 11-08 16시대 UTC):** Claude Code(2.1.319), Gemini CLI 0.67.0, VS Code 1.145.2, Cursor(최신 11-06 voice chat), OpenCode 2.2.6, Crush 0.100.0, Neon MCP(`[NEXT]` max_rows 미배포), MongoDB MCP 3.2.2, GitHub MCP 2.1.0, Supabase MCP 0.16.1, MCP Toolbox 1.16.0, Sentry MCP 0.49.0, Dify 1.19.0, Google ADK Python 2.14.0, Anthropic SDK·Claude Agent SDK, MCP 공식 SDK, sandbox-mcp, llama.cpp(빌드 태그만). ChatGPT 릴리스 노트 최신은 11-04다. MCP 레지스트리는 창 안 게시 4,471건 중 2,535건이 두 네임스페이스였고, 대형 벤더 공식 네임스페이스 게시는 없다.

### 참고: 창 경계 항목

- **Copilot CLI 1.0.108-1 프리릴리스(공식, 11-07 02:32 UTC, 직전 창):** 직전 판이 놓친 건이다. 샌드박스 관련 수정이다. 원문: "a running local MCP server is now restarted after `/cd`, so it follows the new directory's denied paths". [릴리스](https://github.com/github/copilot-cli/releases/tag/v1.0.108-1)
- **"Goodbye Claude, hello Pi"(커뮤니티, HN 11-08 03:11 UTC, 500점대):** 창 안 도구 관련 HN 1위지만 글 날짜는 09-21이다. 저자 주장(미검증) 원문: "At the time of writing, there have been 244 releases to Claude Code this year; over 3/4 of these changed the system prompt or tool descriptions". 같은 창에 Open WebUI 0.13.1과 devvm main이 Pi 지원을 넣었다. Pi는 MIT이고, 원문: "Pi requires Node.js 22.19 or newer." [글](https://caleb.software/posts/claude-to-pi.html) · [HN](https://news.ycombinator.com/item?id=50410107) · [Pi](https://github.com/earendil-works/pi)
- **llama-modchip(HN 11-08 10:22 UTC, 150점 안팎, v2.4.0은 11-04):** 284B DeepSeek를 64GB RAM에서 GPU 없이 돌린다는 주장이다. 함정 원문: "Opening them in unpatched `llama.cpp`, Ollama, or LM Studio will fail with `unknown tensor type 40`." 결국 저자가 빌드한 바이너리를 써야 하고, 품질 수치는 저자 측정이다. 재현 전까지는 지켜만 본다. [저장소](https://github.com/synchopate/llama-modchip) · [HN](https://news.ycombinator.com/item?id=50411961)

## 주목할 점

- **평가 환경에 넣은 키도 탈출 경로다.** 직전 판 WorkOS 건은 평가용 공격 에이전트가 격리를 벗어났고, MaxxBench에서는 모델이 실행용 API 키로 망 제한을 돌아갔다. 에이전트에게 주는 자격 증명의 범위를 승인 규칙과 함께 다시 볼 때다. Claude Code가 비밀값 검사를 실제로 넣는지도 지켜본다.
- **이번 주 일정이 몰려 있다.** Anthropic 공모가 범위 공시(S-1/A), Go 1.27.4(11-10)와 Node.js(11-11) 보안 릴리스, Open WebUI 0.13.1 수정분의 권고 공개, Claude Code 이메일 수정 릴리스, Inkling 2 Small 가중치("next week" 약속 기한), Anthropic 텍스트 워터마크(11-16)가 차례로 온다.

## 조사 제약

- **본문을 직접 읽지 못한 원문:**
  - FT 2건: 발전기 기사는 유료벽이라 제목·설명·게시 시각만 봤고, 공모 기사는 403이다. 수치는 Bloomberg 요약과 The Decoder 인용을 따랐다.
  - Bloomberg 2건과 SCMP는 r.jina.ai로 앞부분만 읽었다. WSJ는 CAPTCHA, NYT는 r.jina.ai 차단, SecurityWeek RSS는 Cloudflare에 막혔다.
  - x.com은 개별 게시물만 `api.fxtwitter.com`으로 읽었다. 타임라인을 볼 수 없어 직원 반응을 놓쳤을 수 있다. reddit은 막혀 있다.
- **시각이 날짜만 있는 원문:** SANS ISC 일지(11-08, 최종 수정 15:44 UTC), Apiiro(11-05), MaxxBench(10-20), Vals 블로그(11-06), LoreKit(11-07). "Goodbye Claude, hello Pi"는 글 날짜(09-21)와 공유 시각(11-08)이 달라 HN 게시 시각으로 판단했다.
- **확인하지 못한 것:**
  - Open WebUI 0.13.1 보안 수정의 내용과 영향 버전, Vercel AI SDK 6.x·5.x의 해당 여부, Chainalysis Data Solutions 복구.
  - Boris 댓글이 회사 공식 입장인지, Opus 5.5 추론 변경 주장의 근거, tinker-ai-skill 계정과 저자의 연결.
  - GitHub 별 조작: stargazers API가 이 환경에서 빈 응답을 줘 누가 언제 별을 눌렀는지 보지 못했다. 가짜 저장소 README는 일부만 표본으로 봤고 스크립트는 받지 않았다.
  - 재현하지 않은 것: MaxxBench 우회, BabelDOC DoS, llama-modchip 성능, Arete A/B, Open WebUI 회귀. 도구도 직접 설치하거나 실행하지 않았다.
- **놓쳤을 수 있는 범위:**
  - HN: 창 안 스토리 718개를 점수순 상위 60개는 전부, 나머지는 AI·보안 키워드로 훑었다. Show HN은 97건을 봤다.
  - 전역 GHSA: unreviewed 241건(OpenEMR 44, Vikunja 39 등 대량 게시)과 malware 17건을 키워드로 걸렀다.
  - Hugging Face: 약 100개 조직을 `createdAt`·`lastModified`로 훑었다. 비공개로 만들었다가 나중에 연 저장소는 놓칠 수 있다.
  - MCP 레지스트리: 창 안 게시 4,471건 중 2,535건이 두 네임스페이스였고, 대형 벤더 이름으로만 걸렀다.
  - 보지 못한 곳: x.ai 뉴스, qwen.ai, Moonshot·Volcengine 변경 로그(월 단위), Vertex 릴리스 노트(캐시 동결). THN RSS는 창 안 항목이 없었다.
- **자사·제3자 측정:** Apiiro, SANS 허니팟, MaxxBench, Vals, Arete A/B(커뮤니티), Co-Design Bench, tinker-ai-skill 비용, llama-modchip 수치는 각 주체의 측정·발표이며 독립 검증 전이다.
