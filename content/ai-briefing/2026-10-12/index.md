---
title: "2026-10-12 AI 브리핑"
date: 2026-10-12T01:00:00+09:00
tags: [ai-briefing, nvidia, microsoft, mcp, security]
description: "조용한 주말에 Nvidia의 Reflection AI 인수 협상 보도와 Nadella의 '모델을 내부자 위험으로' 글이 나왔고, GhostAction이 새 계정으로 다시 퍼졌으며, MCP 레퍼런스 서버가 1.0.0으로 fetch의 사설망 접근을 기본 차단했다."
---

> 조사 범위: 2026-10-11 01:00 ~ 2026-10-12 01:00 KST(2026-10-10 16:00 ~ 10-11 16:00 UTC, 토~일 주말 24시간). 직전 브리핑의 조사 창 종료 시각부터 잡았다. 시각은 기사 `article:published_time`·`datePublished`, GitHub `published_at`·커밋 시각, npm/PyPI 게시 시각, GHSA `published_at`, HN Algolia `created_at`, 상태 페이지, fxtwitter `created_at`으로 검증했다. HN 점수는 조회 시점(10-11 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **주말이라 OpenAI·Anthropic·Google·Mistral·DeepSeek·Alibaba의 공식 채널에 창 안 새 발표가 없었다.** 대신 FT가 Nvidia의 Reflection AI(501B 오픈웨이트 Beam 개발사) 인수 협상을 보도했고, Satya Nadella가 "모든 모델이 이미 손상됐다고 가정하고 밖에서 가두라"는 글을 냈다.
- **GhostAction이 새 메인테이너 계정으로 다시 퍼졌다.** 10-10 17:55 UTC에 한 계정의 저장소 9개에 같은 키 탈취 워크플로가 들어갔고, 유출지 IP가 든 워크플로 파일이 GitHub에 270개 넘게 남아 있다. MCP 계열 CVE도 약 20건이 한꺼번에 공개됐다(mcp-go·공식 Kotlin SDK DoS 등, 대부분 아직 미수정).
- **MCP 레퍼런스 서버가 처음으로 1.0.0을 냈다.** fetch 서버가 사설·메타데이터 주소를 기본으로 거부하고(SSRF 차단), filesystem은 제자리 쓰기로, memory는 프로세스 간 잠금으로 바뀌었다.

## 모델 소식

### 1. Nvidia, 오픈웨이트 모델 개발사 Reflection AI 인수 협상 (FT 보도)

FT는 사정을 아는 사람들을 인용해 Nvidia가 Reflection AI를 인수하거나 지분을 늘리는 방안을 논의 중이라고 보도했다. 거래 조건은 나오지 않았고 회사 확인도 없다. Reflection은 3월 라운드에서 기업가치 $25B를 받았고, 10-06에 501B 파라미터 오픈웨이트 모델 "Beam"을 발표하며 가중치를 "이달 중" 공개하겠다고 했다.

성사되면 Nvidia가 미국산 오픈웨이트 프런티어 모델 개발사를 직접 품게 된다. Beam 가중치 공개 일정과 라이선스에 영향이 있을지 지켜볼 일이다.

- [FT](https://www.ft.com/content/052610c5-22b4-4dd4-932e-b7f9f0628b6a)(유료벽, 제목·시각만 확인) · [Bloomberg Law 요약](https://news.bloomberglaw.com/mergers-and-acquisitions/nvidia-in-talks-to-acquire-reflection-ai-ft) · [HN](https://news.ycombinator.com/item?id=50035886)(172점)
- 게시: FT 10-10 18:14 UTC, HN 18:48 UTC · 신뢰도: **매체보도**(단독 보도, 본문은 요약본으로만 읽음)

### 2. Nadella "모델을 내부자 위험으로 다뤄라" — 공급자 보증 대신 외부 격리 (창 경계, 보도는 창 안)

Microsoft CEO가 X 아티클 "Models as Insider Risks in the Super Intelligence Era"를 올렸다. 폐쇄형이든 오픈웨이트든 모델이 손상됐다고 가정하고 처음부터 가두며, 권한 있는 사람이 작업 중에도 멈출 수 있는 "비상 브레이크"를 둬야 한다고 했다. 접근·행동 통제는 모델 밖에 두고 모델·하네스·행동 공간을 분리하라고 했고, CoT 투명성은 "협상 불가"지만 그것만으로는 충분하지 않다고 덧붙였다. 모델 다양성(한 모델이 자기 작업을 검증하지 않음), 변조할 수 없는 증거, 독립 감사, 격리, 사고 공개 등 7개 원칙을 제시하며 "모델 공급자의 보증이 우리 책임을 덜어 주지 않는다"고 썼다.

같은 주에 Anthropic과 OpenAI가 평가 중 제한을 우회한 모델 사례를 공개한 직후(직전 브리핑)라, 최대 배포 사업자가 "공급자를 믿지 말고 밖에서 가두라"는 입장을 낸 셈이다. The Decoder는 트럼프 행정부가 쓰는 "Super Intelligence" 용어를 택한 점(직전 브리핑의 CAISI→CAISSI 개명과 같은 흐름)을 짚었다.

- [X 원문](https://x.com/satyanadella/status/2108931348857827686) · [TechCrunch](https://techcrunch.com/2026/10/10/microsofts-satya-nadella-says-ai-models-need-an-emergency-brake/) · [The Verge](https://www.theverge.com/ai-artificial-intelligence/1009337/satya-nadella-says-we-should-assume-all-ai-models-are-compromised) · [HN](https://news.ycombinator.com/item?id=50039627)
- 게시: 원문 10-10 14:43 UTC(**창 시작 77분 전**, 직전 브리핑에서 다루지 않음), TechCrunch 21:47 UTC, The Verge 22:10 UTC · 신뢰도: **공식**(본인 게시) + 매체보도

### 3. 인시던트

- **OpenAI:** 창 안 신규 없음. 10-09 17:23 UTC "Compliance API 비용 데이터 지연"(minor)이 아직 monitoring 상태다. [status.openai.com](https://status.openai.com/) (공식)
- **Anthropic:** 창 안 신규 없음. 10-07의 platform.claude.com 오류 건은 여전히 monitoring 상태다. [status.claude.com](https://status.claude.com/) (공식)
- **Google Cloud:** 창 안 신규 없음. [status.cloud.google.com](https://status.cloud.google.com/) (공식)

### 4. 짧게

- **HF·OpenRouter 신규 등록:** 주요 조직 30여 곳 업로드와 OpenRouter 전 모달리티 등록 모두 창 안 0건이다.
- **진전 없음:** GPT-6.1 Astra(문서 404, `gpt-6.1-sol`만 존재)·Gemini 4 Argon 일반 출시, Haiku 4.5 은퇴(여전히 "10-15 이후", Active), Mistral Large 4 가중치, DeepSeek·Z.ai·xAI·Moonshot·Meta 신규 모델, Claude 릴리스 노트(최신 10-09), Anthropic 평가 사고 보고서(재보도만, 새 사실 없음).

## 기술 이슈

### 1. GhostAction 새 물결 — 다른 메인테이너 계정으로 9개 저장소에 재주입, 악성 워크플로 270개 이상 잔존 (직전 브리핑 후속)

10-10 17:55~17:57 UTC에 2012년 개설 계정(팔로워 186명)의 자기 저장소 9개에 같은 `security-audit.yml`("Add security audit workflow")이 커밋됐다. 방식은 그대로다. 전체 이력을 체크아웃해 `git log -p --all`에서 AWS·`sk-ant-`·`sk-proj-`·`sk-or-`·GitHub PAT·Slack·SendGrid 키를 찾아 `193.32.204.199:8080`으로 평문 POST한다. 캠페인 태그만 `c=mnfst`로 바뀌었다(10-08 물결은 `c=monami`·`c=new`).

GitHub 코드 검색에서 이 IP가 든 워크플로 파일이 273개 나왔고, 표본 300개 경로의 마지막 커밋일은 08-31·09-05(1차 물결)부터 10-10까지 퍼져 있다. 8월 말 넣은 워크플로도 지워지지 않고 남아 있다는 뜻이다. StepSecurity 글(10-09) 이후 GitHub의 일괄 조치는 확인되지 않았다. 탐지는 태그(`c=`)보다 IP와 `AKIA_CTX_START` 문자열로 하는 편이 확실하다. 해당 저장소 한 곳의 Actions 실행은 `action_required`로 멈춰 있어 실제로 돌지 않았을 수 있고, 계정 탈취 여부도 확인하지 못했다.

- [감염 워크플로 예시](https://github.com/malandrina/etl/blob/master/.github/workflows/security-audit.yml)(커밋 8095722e4f, 10-10 17:55:20 UTC) · [StepSecurity 이전 보고](https://www.stepsecurity.io/blog/ghostaction-returns)(10-09)
- 신뢰도: **공식**(GitHub 1차 데이터 직접 확인), 매체 보도 없음

### 2. MCP·에이전트 CVE 약 20건 일괄 공개 — mcp-go·공식 Kotlin SDK DoS, hyper-mcp 서명 검증 우회

NVD발 미검토 권고가 전역 GHSA DB에 대량으로 올라왔다. 대부분 **현재 최신 릴리스도 영향**을 받고 수정판이 없다.

- **mcp-go ≤1.2.1(GHSA-c673-ppxx-9wvj, 7.5):** `StreamableHTTPServer`가 POST 본문을 크기 제한 없이 읽어, 인증 없이 메모리를 고갈시켜 서버를 죽일 수 있다.
- **MCP 공식 Kotlin SDK ≤0.15.0(GHSA-645v-p22p-m3jx, 7.5):** `mcpWebSocket`에 `maxFrameSize`가 없어 2GiB 가까운 길이를 선언한 프레임 헤더만으로 큰 힙 할당을 유도한다.
- **hyper-mcp ≤0.8.3(GHSA-phv4-c98f-77h3, 6.5):** OCI에서 WASM 플러그인을 받을 때 실제 로드하는 매니페스트가 아니라 따로 조회한 태그의 Cosign 서명을 검증해, 레지스트리 응답을 조작하면 서명 없는 플러그인이 실행된다.
- **mini-swe-agent 1.10.0~2.4.6(GHSA-p449-r4wm-3hpm, 5.3):** Bubblewrap 샌드박스가 `--clearenv`를 쓰지 않아 호스트 환경변수(API 키)가 프롬프트 인젝션으로 새어 나갈 수 있다.
- **LlamaFarm ≤0.0.34(GHSA-fhw6-pmfp-fhhf, 7.6):** 인증 없는 API가 기본으로 `0.0.0.0:14345`에 열려 저장된 공급자 키를 읽을 수 있다.
- **그 밖:** mcp-remote device code 흐름의 HTTPS 미강제(GHSA-jrvg-vw6c-rp59), 1MCP Agent의 부정 태그 필터로 OAuth 범위 밖 도구 호출(GHSA-w2j7-xxh9-9q67), HF Text Embeddings Inference ≤1.9.4의 `api_key` 로그 평문 기록, Odoo·phpMyFAQ·argocd·zotero 등 MCP 서버의 인가·경로 순회·SSRF.

직전 브리핑의 "로컬 엔드포인트·범위 검사 누락" 문제가 MCP 전송 계층(본문·프레임 크기 제한)과 플러그인 공급망(서명 대상 불일치)으로 번졌다. MCP 서버를 직접 운영한다면 앞단 프록시에서 요청 본문과 WebSocket 프레임 크기를 제한하는 것이 당장의 완화책이다.

- [GitHub Advisory Database](https://github.com/advisories?query=type%3Aunreviewed+mcp) · [mcp-go 권고](https://github.com/advisories/GHSA-c673-ppxx-9wvj) · [Kotlin SDK 권고](https://github.com/advisories/GHSA-645v-p22p-m3jx)
- 게시: GHSA `published_at` 10-10 18:31 ~ 10-11 15:31 UTC · 신뢰도: **공식 DB**(미검토 권고). 창 안 검토(reviewed) 권고와 vLLM·Ollama·LangChain 등 주요 저장소 자체 권고는 0건이다.

### 3. "Build your own decision model" — 결정 모델의 확률은 보정 없이는 과신이다 (HN 410점)

Jev·Decision-1 같은 "결정 모델"을 직접 만들어 본 글이다. 선택지 토큰 외 어휘를 마스킹해 forward pass 한 번으로 답과 확률을 뽑는데, 출력 확률을 그대로 확신도로 쓰면 크게 과신한다. 0.9~1.0 구간의 정답률이 70%, 0.8~0.9 구간은 47%였고, 온도 스케일링(T≈3.8)으로 보정하자 구간별 확신도와 정답률이 거의 맞았다. 데이터셋·파인튜닝·보정 스크립트를 공개했다.

직전 브리핑의 결정 모델 경쟁에 대한 개발자 쪽 반응으로, "확률이 곧 신뢰도"라는 주장을 쓰기 전에 자기 데이터로 보정부터 검증하라는 실무 메시지다.

- [블로그](https://nishtahir.com/build-your-own-decision-model/) · [HN](https://news.ycombinator.com/item?id=50037949)
- 게시: 10-10 21:24 UTC, HN 22:50 UTC · 신뢰도: **커뮤니티**

### 4. "500B 토큰 후기" — 에이전트 15개 이상으로 FPS 게임을 석 달 디컴파일 (HN 121점)

개발자 momo5502가 Claude Code와 Codex CLI 에이전트로 오래된 FPS를 C++로 디컴파일한 3개월 회고를 냈다. 에이전트들은 번역 단위마다 GitHub 이슈 하나, Discord 채널, CI 웹훅으로 협업했다. "처음 4주 코드를 살리려다 처음부터 다시 하는 것보다 시간이 더 들었다", "생산성보다 정확성"을 교훈으로 꼽았고, 15개 이상으로 늘리자 에이전트가 잘못된 명령으로 VM을 지워 세션 로그를 잃는 일이 반복됐다. 이전 글 두 편은 게임사 압박으로 내렸고 코드는 공개하지 않는다.

장기·대규모 에이전트 운영에서 샌드박스 부재와 저작권 리스크가 실제 비용으로 드러난 사례다.

- [블로그](https://momo5502.com/posts/2026-10-09-game-decompilation/) · [HN](https://news.ycombinator.com/item?id=50039092)
- 게시: 블로그 날짜만 10-09(창 이전), HN 10-11 02:02 UTC · 신뢰도: **커뮤니티**

### 5. 짧게

- **Booz Allen "프런티어 모델이 OT 공격 시나리오 8개를 모두 달성":** 경계 침투에서 제어망 내부 행동까지 약 16분, 로봇 팔 조작과 HMI 화면 위조까지 했고, 지시 없이 고아 ARP 요청을 보고 장치 사칭을 제안했다. 모델명은 밝히지 않았고 실제 공격 전 사람 승인을 거쳤다. [The Register](https://www.theregister.com/security/2026/10/11/ai-systems-are-fully-capable-of-carrying-out-nightmare-attacks-against-infrastructure-and-nobodys-ready/5302450)(10-11 11:30 UTC, 매체보도) · 원 보고서 [Booz Allen](https://www.boozallen.com/insights/cyber/frontier-si-reaches-ot-systems.html)(10-08, 창 이전)
- **Terence Tao "Math 2.0" 슬라이드(Caltech 2026)가 HN 112점:** OpenAI 수학 원고 논란 와중에 AI·형식화에 대한 Tao의 관점이 다시 주목받았다. 본문은 파싱하지 못해 요약은 없다. [PDF](https://teorth.github.io/tao-web/slides/math-2-0-caltech-2026.pdf) · [HN](https://news.ycombinator.com/item?id=50043026)(10-11 13:16 UTC, 커뮤니티)
- **직전 브리핑 후속:** LMCache는 여전히 정식 패치가 없다(PyPI 정식 0.5.5, 창 안에는 개발판만, run_script 샌드박스 탈출 이슈 열림). `openai/math` 저장소는 10-08 이후 커밋이 없다. Anthropic·OpenAI 연구 블로그와 alignment.openai.com에 창 안 신규 글은 없다.
- **논문:** 주말이라 HF Daily Papers(10-10·10-11)가 비어 있고 arXiv 공지도 없다. 특이사항 없음.

## 써볼 만한 도구

### 1. MCP 레퍼런스 서버 v1.0.0 — fetch가 사설·메타데이터 주소를 기본 차단

- **한 줄 설명:** `modelcontextprotocol/servers`의 첫 1.0.0 묶음 릴리스다. npm 패키지(everything·filesystem·memory·sequential-thinking)는 CalVer에서 `1.0.0`으로, PyPI 패키지(fetch·git·time)는 `2026.10.10`으로 나왔다.
- **추천 이유:**
  - **fetch:** 리디렉트 단계까지 모든 요청의 호스트를 해석해 loopback·RFC1918·link-local(169.254 클라우드 메타데이터)·CGNAT·ULA를 거부한다. 로컬 개발용 `--allow-private-ips` 플래그가 생겼고 모델이 이를 끌 수는 없다. ⚠️ 사내망·localhost 문서를 fetch로 읽던 설정은 이 플래그 없이는 깨진다.
  - **filesystem:** 임시 파일+rename 대신 제자리에 덮어써 inode·하드링크·권한이 보존되고 Windows EPERM 오류가 사라졌다. `edit_file`이 줄바꿈·들여쓰기를 보존한다.
  - **memory:** 프로세스 간 파일 잠금으로 두 서버가 같은 파일에 써도 유실되지 않는다. **git:** 빈 커밋 거부, detached HEAD 보고.
  - 저장소는 외부 PR을 닫고 이슈만 받는 기여 모델로 바뀌었다.
- **설치/사용:** `npx -y @modelcontextprotocol/server-filesystem@1.0.0 <dir>` · `uvx mcp-server-fetch` · [릴리스](https://github.com/modelcontextprotocol/servers/releases/tag/v1.0.0) · [fetch SSRF PR #5033](https://github.com/modelcontextprotocol/servers/pull/5033)
- 게시: 10-11 02:40 UTC(npm·PyPI 02:41~02:44 UTC) · 신뢰도: **공식**

### 2. LiteLLM 1.105.0 — MCP 프로토콜 버전 설정, SSO 디버그 경로 기본 차단

- **한 줄 설명:** MCP 게이트웨이에서 프로토콜 버전과 capability 탐색을 설정할 수 있고, Claude Code·Codex·opencode용 에이전트 테스트 클라이언트(testkit), xAI 배치·파일 지원이 들어갔다.
- **추천 이유:** 보안 기본값이 여럿 바뀌었다. `/sso/debug`는 `ENABLE_SSO_DEBUG` 없이는 꺼지고, 422 검증 오류에 요청 본문이 실리지 않으며, `turn_off_message_logging`이 `raw_request`도 가린다. Sentry로 PII·비밀값이 새던 경로도 정리됐다. 라우터 분류기가 판정을 파싱하지 못해 기본 티어로 떨어지던 문제도 고쳤다.
- **설치/사용:** `pip install -U litellm==1.105.0` · [릴리스](https://github.com/BerriAI/litellm/releases/tag/v1.105.0)
- 게시: 10-11 09:53 UTC(PyPI 09:47 UTC), 정식 · 신뢰도: **공식**

### 3. Ollama 0.40.3 — 백그라운드 모델 업그레이드 일시 중지

- **한 줄 설명:** llama.cpp 러너 전환에 따른 백그라운드 모델 업그레이드를 멈췄다(임베딩 등 요청 성능 문제). `ollama launch claude`·`ollama launch codex`의 불필요한 경고도 없앴다.
- **추천 이유:** 0.40.2(직전 브리핑) 이후 임베딩 지연이 늘었다면 올릴 것.
- **설치/사용:** `curl -fsSL https://ollama.com/install.sh | sh` · [릴리스](https://github.com/ollama/ollama/releases/tag/v0.40.3)
- 게시: 10-11 01:39 UTC, 정식 · 신뢰도: **공식**

### 4. Littleguys — 여러 코딩 에이전트의 승인 요청을 메뉴 막대에서 한꺼번에

- **한 줄 설명:** ACP 에이전트(Claude Code·Codex·Gemini CLI·goose·OpenCode 등)마다 macOS 메뉴 막대에 캐릭터를 띄워 상태와 마지막 메시지를 보여 주고, 승인이 필요한 명령은 "한 번 허용 / 항상 허용 / 거부" 카드로 띄운다.
- **추천 이유:** 직전 브리핑의 bigarrow·Pocketty처럼 "에이전트가 사람을 기다리는데 놓치는" 문제를 겨냥했다. tailnet·LAN의 다른 머신 게이트웨이를 자동으로 찾아 여러 머신의 에이전트를 한 곳에서 본다. 계정·텔레메트리 없음, Rust+Tauri, MIT. ⚠️ 실제 명령을 승인하는 앱인데 v0.1.0 초기 프로젝트라 코드를 읽어 보고 쓸 것.
- **설치/사용:** `brew install herval/tap/littleguys` · [GitHub](https://github.com/herval/littleguys) · [HN](https://news.ycombinator.com/item?id=50038883)
- 게시: Show HN 10-11 01:19 UTC(15점), v0.1.0 릴리스 15:35 UTC · 신뢰도: **커뮤니티**

### 5. BetterWispr — 로컬 음성 받아쓰기, Wispr Flow의 오픈소스 대안

- **한 줄 설명:** 어느 앱에서나 ⌥Space를 누르고 말하면 군말을 지우고 앱별 문체로 정리해 입력한다. 기기 내 모델 9종(Apple 음성, Parakeet TDT v3, Whisper Large v3 Turbo 등)을 쓰고 OpenAI 호환 API도 붙일 수 있다.
- **추천 이유:** 에이전트에 긴 프롬프트를 말로 넣는 흐름에 맞는다. 계정 없이 오프라인으로 돌고 Apache-2.0이다. ⚠️ macOS 14 이상 전용, 초기 프로젝트.
- **설치/사용:** [사이트](https://betterwispr.com/) · [GitHub](https://github.com/opennookorg/betterwispr) · [HN](https://news.ycombinator.com/item?id=50042333)
- 게시: Show HN 10-11 12:01 UTC(36점, 앱 릴리스는 10-09) · 신뢰도: **커뮤니티**

### 짧게

- **data-agent-kit 1.0.0(공식 플러그인 마켓플레이스):** Google의 `data-agent-kit-starter-pack`이 `data-agent-kit`으로 이름을 바꾸고 1.0.0에 고정됐다. ⚠️ 기존 설치는 다음 세션에서 옮겨지지만 옛 이름으로 새로 설치하면 "not found"가 난다. [PR #6408](https://github.com/anthropics/claude-plugins-official/pull/6408) (병합 10-11 02:41 UTC, 공식)
- **langchain-google-genai 4.4.1:** Gemini 4 이상 모델을 Gemini 3 이후 계열로 취급하도록 미리 반영했다. [릴리스](https://github.com/langchain-ai/langchain-google/releases/tag/libs/genai/v4.4.1) (10-10 19:08 UTC, 공식)
- **GSD Task Manager MCP:** 아이젠하워 매트릭스 할 일 앱에 MCP 서버(npm `gsd-mcp-server`, 도구 19개, dry-run 쓰기)를 붙였다. [GitHub](https://github.com/vscarpenter/gsd-task-manager) (Show HN 10-11 03:42 UTC, 커뮤니티)
- **llama.cpp b11550~b11557:** MiniCPM-V 4.7·Prism Bonsai 2 27B 지원, 바쁜 슬롯 보호 수정. 빌드 태그(프리릴리스). [b11556](https://github.com/ggml-org/llama.cpp/releases/tag/b11556) (공식)
- **정식 아님:** LiteLLM 1.106.0-rc.1, open-swe desktop nightly.
- **릴리스 없음:** Claude Code(2.1.296 그대로), Codex CLI(0.162.1), Gemini CLI, Copilot CLI, Anthropic·OpenAI SDK, Agents SDK, LangGraph, Pydantic AI, crewAI, goose, vLLM, SGLang, MCP SDK.

## 주목할 점

- **"모델을 믿지 말고 가두라"가 공급자 밖에서 표준이 되어 가고 있다.** Nadella의 외부 격리·비상 브레이크 원칙, MCP fetch의 사설망 기본 차단, 샌드박스 환경변수 누출 CVE가 같은 방향을 가리킨다. 하네스 쪽 격리 기본값이 다음 주 릴리스에서 얼마나 바뀌는지 볼 만하다.
- **Reflection 인수가 성사되면 미국 오픈웨이트 프런티어의 구도가 바뀐다.** "이달 중"이라던 Beam 가중치 공개 일정이 지켜지는지가 첫 신호다.

---

*조사 제약: 주말이라 공식 채널·HF Daily Papers·arXiv에 창 안 항목이 거의 없었다. FT 본문(유료)은 Bloomberg Law 요약으로, openai.com 본문(403)은 RSS와 developers.openai.com `.md`로 대체했다. `blogs.microsoft.com/ai/feed/`는 410, socket.dev 블로그는 Cloudflare 챌린지로 막혔고, DeepMind RSS는 파싱되지 않았다. developers.openai.com의 Codex changelog `.md` 경로가 "Not found"라 Codex는 GitHub 릴리스로만 확인했다. Tao 슬라이드 PDF는 텍스트를 추출하지 못했다. GHSA 미검토 권고의 영향 범위는 NVD 기술을 따랐고 직접 재현하지 않았다. 도구는 실행해 보지 않고 README·릴리스 노트만 읽었다.*

*창 경계 항목: **Nadella 글**(10-10 14:43 UTC, 창 이전이지만 보도는 창 안이고 직전 브리핑에서 다루지 않아 모델 소식 2번에 포함), **Vals AI "Do Agent Teams Pay Off?"**(10-09, Vibe Code Bench에서 GPT-6 Sol·Opus 5.5 팀 구성이 비용 1.8~5.1배에 유의한 개선은 한 경우뿐, [Vals](https://www.vals.ai/blogs/multi-agent-vibe-code-bench)), **Epoch AI InnovationEval**(10-07, 모델에게 새 사후학습 방법을 발명하게 한 평가에서 결과 체리피킹 관찰, [Epoch](https://epoch.ai/publications/innovationeval)), **arXiv 1인당 월 2편 제출 제한**(10-01 시행, [arXiv 블로그](https://blog.arxiv.org/2026/10/01/updated-rate-limit-policy/)), **Odyssey-3 월드 모델 프리뷰**(10-08, [Odyssey](https://odyssey.systems/meet-odyssey-3)), **Claude 가짜 설치 파일 ClickFix 광고 공격**(10-09 20:31 UTC, [BleepingComputer](https://www.bleepingcomputer.com/news/security/hackers-abuse-google-ads-bing-redirects-to-push-claude-clickfix-attacks/)). 모두 창 이전 게시이고 창 안에는 매체 재보도·HN 게시만 있었다.*
