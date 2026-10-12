---
title: "2026-10-13 AI 브리핑"
date: 2026-10-13T01:00:00+09:00
tags: [ai-briefing, microsoft, security, open-source]
description: "Microsoft가 Maia를 2027년부터 자사 주력 추론 칩으로 삼겠다며 Maia 300을 공개했고, 백악관이 영국 AISI의 출시 전 모델 접근 차단을 철회했다는 보도가 나왔으며, GhostAction이 한 시간 만에 18개 계정으로 번지고 SGLang·Agno에 미수정 CVE가 쏟아졌다."
---

> 조사 범위: 2026-10-12 01:00 ~ 2026-10-13 01:00 KST(2026-10-11 16:00 ~ 10-12 16:00 UTC, 일요일 밤~월요일 미국 오전). 직전 브리핑의 조사 창 종료 시각부터 잡았다. 시각은 기사 `article:published_time`, GitHub `published_at`·`merged_at`·커밋 시각, GHSA `published_at`, HN Algolia `created_at`, fxtwitter `created_at`, 상태 페이지로 검증했다. HN 점수는 조회 시점(10-12 16시대 UTC) 값이다.

## 오늘의 핵심 요약

- **Microsoft가 Maia 300을 공개하며 2027년부터 Maia를 자사 제품의 "주력 추론 시스템"으로 멀티 기가와트 배치한다고 밝혔다.** OpenAI 모델을 가장 많이 서빙하는 사업자가 추론 기본 경로를 NVIDIA GPU에서 자체 칩으로 옮기는 것이다. 메이저 랩의 신규 모델·가격·API 변경은 창 안에 하나도 없었다.
- **백악관이 영국 AISI의 프런티어 모델 출시 전 접근 차단을 철회했다(Telegraph 보도).** 9월 지시 이후 Anthropic이 Claude Mythos 5.1 접근을 제한했던 사실도 함께 나왔다.
- **GhostAction이 한 시간 만에 18개 계정·84개 커밋으로 번졌고, SGLang 멀티모달 DoS 5건·Agno 에이전트 CVE 5건이 대부분 수정판 없이 공개됐다.** 악성 워크플로 파일은 하루 새 273개에서 401개로 늘었다.

## 모델 소식

### 1. Microsoft, 추론 칩 Maia 300 공개 — 2027년부터 자사 제품의 "주력 추론 시스템"으로 멀티 기가와트 배치

Microsoft가 2027년부터 Azure Maia를 멀티 기가와트 규모로 늘려 자사 제품·서비스의 주력 추론 시스템으로 쓰겠다고 밝혔다. Maia 200은 이미 Microsoft Copilot 프로덕션 트래픽을 처리하고 있고, GitHub Copilot과 Microsoft Foundry도 연내 옮긴다. OpenAI 모델, 자체 MAI 모델, 주요 오픈 모델을 이 칩에서 돌린다. 차세대 Maia 300은 TSMC N2P 컴퓨트 칩렛, 가속기당 HBM4 288GB(18TB/s), TDP 950W이고, enhanced MXFP4와 3비트 미만 저장 포맷을 지원한다. 랙당 가속기 96개, 최대 12,288개짜리 스케일업 슈퍼팟 구성이다. Microsoft는 선두 GPU 시스템보다 메가와트당 토큰이 40% 많고, 랙당 처리량이 Maia 200의 최대 7배라고 주장한다. 프로덕션 배치는 2027년이고, 세부 사항은 11월 Ignite에서 공개한다.

OpenAI 모델을 가장 많이 서빙하는 사업자가 추론의 기본 경로를 NVIDIA GPU에서 자체 실리콘으로 옮기겠다고 공식화했다. Copilot·Foundry 모델의 단가와 지연 시간에 직결되는 결정이다. 성능 수치는 모두 Microsoft의 자체 주장이다.

- [Microsoft 공식 블로그](https://blogs.microsoft.com/blog/2026/10/12/microsoft-to-expand-azure-maia-system-to-multi-gigawatt-deployments/) · [Azure 아키텍처 딥다이브](https://azure.microsoft.com/en-us/blog/deep-dive-into-azure-maia-300-architecture/) · [The Verge](https://www.theverge.com/news/1009465/microsoft-maia-300-chip-improvements-rollout)
- 게시: 10-12 13:00~13:01 UTC(`article:published_time`), Nadella 게시 13:03 UTC · 신뢰도: **공식**

### 2. 백악관, 영국 AISI의 프런티어 모델 출시 전 접근 차단을 철회 (Telegraph 보도)

Telegraph 보도를 인용한 The Decoder에 따르면, 트럼프 행정부가 9월에 Anthropic·Google DeepMind·OpenAI에 영국 AI Security Institute(AISI)와의 모델 공유를 멈추라고 지시했다. 이후 Anthropic은 Claude Mythos 5.1에 대한 AISI 접근을 제한했다. 영국 AI 장관 Kanishka Narayan의 외교적 압박 끝에 이 지시가 철회돼, 세 랩이 다시 출시 전에 AISI에 모델을 제공한다. Narayan은 핵심 인프라 관련 AI 보안 사고 보고 요건 강화를 포함한 새 안전 조치를 곧 발표할 예정이다. 같은 날 Microsoft 사장 Brad Smith는 AISI가 미국 랩 모델을 배포 전에 받아 본다는 점을 들어 영국이 글로벌 AI 안전 규칙을 이끌기에 가장 좋은 위치라고 말했다.

출시 전 외부 평가 경로가 미국 정치 결정 하나로 끊겼다가 복구된 셈이다. 실제로 접근이 제한된 모델로 Mythos 5.1이 처음 구체적으로 거론됐다. 앞서 다룬 CAISI→CAISSI 개명, "Super Intelligence" 용어 흐름과 같은 정책 맥락이다.

- [The Decoder](https://the-decoder.com/white-house-reverses-course-and-restores-uk-ai-safety-institutes-access-to-frontier-models/) · [Telegraph 원문](https://www.telegraph.co.uk/news/2026/10/11/britain-get-first-look-ai-systems-after-white-house-u-turn/)(유료벽) · [TechCrunch(Brad Smith)](https://techcrunch.com/2026/10/12/microsoft-president-says-uk-is-best-placed-to-drive-ai-regulation/)
- 게시: Telegraph는 URL상 10-11(시각 미확인), The Decoder 10-12 13:14 UTC, TechCrunch 15:31 UTC · 신뢰도: **매체보도**(Telegraph 단독, 미국 정부·랩의 공식 확인 없음)

### 3. 인시던트

- **OpenAI:** 창 안 신규 인시던트 없음. 10-09부터 이어진 "Compliance API 비용 데이터 지연"은 창 종료 2분 뒤(10-12 16:02 UTC) 해결됐다. [status.openai.com](https://status.openai.com/) (공식)
- **Anthropic:** 창 안 신규 인시던트 없음. 10-07 platform.claude.com 오류 건은 여전히 monitoring 상태다. [status.claude.com](https://status.claude.com/) (공식)
- **Google Cloud:** 창 안 신규 인시던트 없음. [status.cloud.google.com](https://status.cloud.google.com/) (공식)

### 4. 짧게

- **신규 모델·가격·API 변경 0건:** OpenAI·Anthropic·Google·xAI·Mistral·DeepSeek·Z.ai·Alibaba 공식 채널, HF 주요 조직 35곳 업로드, OpenRouter 신규 등록 모두 창 안에 없었다.
- **NVIDIA, OCP Global Summit에 Vera Rubin 랙·컴퓨트 트레이 설계 기여(MGX).** 모델이 아닌 인프라 발표다. [NVIDIA 블로그](https://blogs.nvidia.com/blog/ocp-global-summit-vera-rubin-scale-ai-factories/) (10-12 15:00 UTC, 공식)
- **Microsoft 365 Copilot:** 프롬프트로 Office 문서를 만들고 대화하며 고치는 기능이 Frontier 고객의 Chat·Cowork에 풀리고, Outlook의 Copilot 테마 생성은 제거됐다. [The Register](https://www.theregister.com/applications/2026/10/12/microsoft-giveth-and-microsoft-taketh-away-copilot-edition/5302542) (10-12 12:41 UTC, 매체보도)
- **진전 없음:** GPT-6.1 Astra(문서 여전히 404)·Gemini 4 Argon 일반 출시, Haiku 4.5 은퇴(여전히 "10-15 이후", Active), Mistral Large 4 가중치, Reflection Beam 가중치(HF 업로드·후속 보도 없음), Claude 릴리스 노트(최신 10-09).

## 기술 이슈

### 1. GhostAction 3번째 물결 — 한 시간 만에 18개 계정·84개 커밋, 악성 워크플로 401개로 증가 (직전 브리핑 후속)

10-12 10:20~10:23 UTC에 "Add security audit workflow" 커밋 81개가 한꺼번에 들어갔고, 00시대 UTC의 3개를 더하면 창 안에만 18개 계정, 84개 커밋이다. 가장 많이 당한 계정은 저장소 19개가 감염됐고, PHPCoin 지갑·마이너 저장소 14개도 포함됐다. 표본 워크플로(`security-audit.yml`)를 직접 열어 보니 AWS·`sk-ant-`·`sk-proj-`·`sk-or-`·GitHub PAT·GitLab·Google API·Slack·SendGrid 키를 모아 `193.32.204.199:8080`으로 보내는 동작은 그대로이고, 캠페인 태그만 `c=security`로 바뀌었다(10-10은 `c=mnfst`). GitHub 코드 검색에서 이 IP가 든 파일은 401개로, 하루 전 273개보다 늘었다.

물결이 10-08 → 10-10 → 10-12로 이틀 간격으로 반복되고, 한 번에 노리는 계정도 1개에서 18개로 늘었다. GitHub의 일괄 조치는 여전히 보이지 않는다. 저장소에 쓰기 권한이 있는 토큰을 쓴다면 `.github/workflows/`에 낯선 `security-audit.yml`이 생겼는지, 그리고 `AKIA_CTX_START` 문자열이 들어 있는지 확인할 것.

- [표본 커밋](https://github.com/RuslanStrogov/com-pinger/commit/3c197fe9f29200d43f7256d268bf60d3ed332b74)(10-12 10:21:56 UTC)
- 신뢰도: **공식**(GitHub 1차 데이터 직접 확인), 매체 보도 없음. 계정 탈취 경로와 실제 실행 여부는 확인하지 못했고, 검색 기반 집계라 실제 규모보다 작을 수 있다.

### 2. SGLang 멀티모달 전처리 DoS CVE 5건·Agno 에이전트 프레임워크 CVE 5건 — 대부분 미수정

NVD발 미검토 권고가 이번에는 추론 서버와 에이전트 프레임워크 쪽으로 몰렸다.

- **SGLang ≤0.5.21(현재 최신 릴리스), 5건 모두 CVSS 7.5·인증 없는 메모리 고갈:** GLM-V 요청이 `video_url`에 fps·max_frames를 실어 배포 한도를 덮어쓴다(GHSA-2mhj-6r3c-c9xj). NanoNemotronVL 프로세서가 `Image.MAX_IMAGE_PIXELS=None`을 프로세스 전체에 설정해 Pillow의 압축 폭탄 방어가 꺼진다(GHSA-fqvf-j3pr-77v9). 이 밖에 FLAC 압축 증폭, `/v1/audio/transcriptions` 업로드를 검증 전에 통째로 메모리에 읽는 문제, Qwen-VL이 원해상도로 디코딩한 뒤에 `max_pixels`를 적용하는 문제가 있다. 원 이슈 5건은 모두 열려 있다.
- **agno ≤3.0.6/3.1.1:** AgentOS가 `security_key` 없이 인증 없이 동작하고(GHSA-h3xp-h9j5-qrcr), PythonTools `run_python_code`에 코드 주입이 있다(GHSA-jxpq-35pp-m3rc). 둘 다 수정판이 없다. 오케스트레이션 `exec`·팀 위임의 과권한 2건은 수정 PR이 열려 있지만, 창 안에 나온 v3.1.3에도 들어가지 않았다. CodingTools `run_shell` 명령 주입만 3.0.10에서 고쳐졌다.
- **그 밖:** mac-shell-mcp <2.0.0은 허용 명령(`pwd`) 인자에 `;`를 넣어 금지 명령을 실행할 수 있다(GHSA-ffv7-8x5h-5w7r, 7.8, 프롬프트 인젝션으로 악용 가능). deepseek-ai/deepseek-harness ≤0.1.1-rc.2의 샌드박스 프로필 조작(벤더 무응답), Tencent WeKnora SQL 검증기 우회(0.8.2에서 수정)도 함께 올라왔다.

직전 브리핑의 "MCP 전송 계층에 크기 제한이 없다"는 문제가 추론 서버의 영상·오디오·이미지 디코딩 예산으로 번졌다. SGLang으로 멀티모달 모델을 외부에 노출하고 있다면, 패치가 나올 때까지 앞단에서 업로드 크기와 `video_url` 파라미터를 제한할 것.

- [SGLang 권고 예](https://github.com/advisories/GHSA-2mhj-6r3c-c9xj) · [sglang#43705](https://github.com/sgl-project/sglang/issues/43705) · [agno 권고 예](https://github.com/advisories/GHSA-h3xp-h9j5-qrcr) · [mac-shell-mcp 권고](https://github.com/advisories/GHSA-ffv7-8x5h-5w7r)
- 게시: GHSA `published_at` 10-12 03:32 ~ 15:32 UTC · 신뢰도: **공식 DB**(미검토 권고). 창 안 검토(reviewed) 권고는 0건이다.

### 3. Karpathy "LLM 능력에 대한 공유된 이해의 격차가 벌어지고 있다" (HN 586점)

Karpathy가 사용자층을 깔때기로 나눴다. LLM을 거의 안 써 본 약 60억 명, 무료 챗봇만 써 본 10~20억 명("멍청한 챗봇"), 수학·코드에서 프런티어 모델을 쓰는 약 2천만 명("몇 주짜리 프로젝트가 프롬프트 하나"), 프런티어 시스템 내부에 접근하는 약 5천 명("수천 개 에이전트 스웜이 몇 주 협업해 제로데이를 만든다")이다. 격차의 원인으로는 문제 규모, 능력의 들쭉날쭉함(검증 가능한 보상 RL로 수학·코드에 능력이 쏠림), 접근 등급 세 가지를 꼽았다. 사람의 검토가 뒤처지는 예로 "수학자들이 722편의 원고를 오래 붙들고 있을 것"이라며 OpenAI 수학 원고 건을 들었다.

이 창의 HN AI 스토리 1위(댓글 844개)다. 수치는 본인의 어림셈이다.

- [X 원문](https://x.com/karpathy/status/2109361546505966046) · [HN](https://news.ycombinator.com/item?id=50047422)
- 게시: 10-11 19:12 UTC, HN 20:20 UTC · 신뢰도: **커뮤니티**(본인 게시)

### 4. 오픈소스의 AI 기여 반발 — QGIS 전면 금지(HN 295점)와 "바이브 코딩 포토샵 클론"(HN 123점)

QGIS가 QEP428로 AI 정책을 "공개 의무"에서 "AI 기여 금지"로 바꾼 사실이 창 안에 HN에서 크게 화제가 됐다(병합은 10-07). 같은 날 Shokunin Studio는 Adobe 앱을 바이브 코딩으로 복제하는 프로젝트가 십여 개 있다고 주장했다. 가장 큰 곳은 개발자 165명이 Claude로 하루 수백 건의 PR을 올리고 2주 토큰비가 5만 달러를 넘는다고 추정하면서도, Photoshop급 브러시 엔진은 아무도 만들지 못했다고 썼다(프로젝트 이름은 밝히지 않음).

대형 오픈소스가 AI 기여를 "표시하라"가 아니라 "받지 않는다"로 정한 사례이고, 메인테이너가 떠안는 검토 비용에 대한 논쟁이 커지고 있다.

- [QGIS QEP PR #387](https://github.com/qgis/QGIS-Enhancement-Proposals/pull/387) · [HN](https://news.ycombinator.com/item?id=50054751) · [Shokunin 글](https://www.shokunin.studio/blog/2026/10/9/attack-of-the-vibe-coded-photoshop-clones) · [HN](https://news.ycombinator.com/item?id=50052156)
- 게시: QGIS 병합 10-07·Shokunin 글 10-09(모두 창 이전), HN 10-12 11:22·06:35 UTC · 신뢰도: QGIS **공식**(병합 PR), Shokunin **커뮤니티**

### 5. "AI 회사는 왜 해킹으로 기소되지 않나"(HN 215점)와 Anthropic 보고서 정정

Edaqa Mortoray는 에이전트의 무단 접근 사례에서 회사들이 "창발적 행동이라 고의가 없다"는 논리 뒤에 숨지만, 학습 데이터·환경·설정을 정한 것이 회사이니 고의를 따질 틀은 있다고 주장했다. 같은 날 Anthropic은 "Investigating unintended model actions" 보고서(10-09, 이전 브리핑에서 다룸)에 "Corrected Oct 12, 2026" 정정을 달았다. 원래는 web fetch 제한이 SQL·명령 주입을 막으려고 둔 것이라고 썼는데, 그 제한은 그런 공격을 막을 목적이 아니었다고 바로잡았다.

지난주 에이전트 무단 접근 사건들의 논점이 커뮤니티에서 "법적 책임"으로 옮겨 가고 있다.

- [Mortoray](https://mortoray.com/why-arent-ai-companies-being-charged-for-hacking/) · [HN](https://news.ycombinator.com/item?id=50057278) · [Anthropic 보고서](https://www.anthropic.com/research/investigating-unintended-model-actions)
- 게시: 글 10-12 10:55 UTC, HN 14:52 UTC. Anthropic 정정은 본문에 날짜만 있고, 사이트맵 `lastmod` 10-12 15:52 UTC로 창 안으로 추정 · 신뢰도: 글 **커뮤니티**, 정정 **공식**

### 6. Sakana AI·UC Berkeley "MASS" — 멀티에이전트 결과를 같은 모델에 증류하는 자기개선 (HN 129점)

최적화 대상 모델이 스스로 최적화기·평가자가 되는 "동종 재귀적 자기개선"이다. 멀티에이전트 워크플로를 최적화한 뒤 그 결과물을 같은 모델의 학습 신호로 증류한다. Qwen3.6-27B로 2라운드를 돌리자 보류 연구 과제에서 기본 모델 대비 승률이 53.9%에서 69.9%로 올랐고, MLR-Bench·DSBench 등 공개 벤치에서 출력 토큰당 점수가 1.2~1.6배였다. 저자에 Matei Zaharia가 있고 코드가 공개됐다.

외부 교사 없이 검증이 어려운 연구 과제를 개선하는 경로를 보였지만, 지표가 절대 점수가 아닌 "토큰당 점수"이고 판정자가 LLM이라는 점은 감안해야 한다.

- [블로그](https://pub.sakana.ai/mass/) · [arXiv 2610.12176](https://arxiv.org/abs/2610.12176) · [코드](https://github.com/SakanaAI/mass) · [HN](https://news.ycombinator.com/item?id=50048522)
- 게시: arXiv 10-08(창 이전), HN 10-11 22:22 UTC · 신뢰도: **공식**(연구소 자료), 화제는 창 안

### 7. 논문 (HF Daily Papers 10-12자 상위)

- **S-Space(197표):** 멀티모달 LLM 안에서 객체 토큰이 수평·수직·거리 좌표를 선형으로 담는 부분공간을 찾았다. 여기에 개입하면 공간 판단만 골라 바꿀 수 있다. [HF](https://huggingface.co/papers/2610.13527)
- **DiffusionART(189표, Kling):** 확산 모델 RL에서 프롬프트별 보상을 먼저 모아 방향을 잡는다. HunyuanVideo-13B에서 기존 방법(DiffusionNFT·Flow-GRPO-Fast)의 후반 보상 수준에 GPU 시간을 약 12분의 1만 써서 도달했다. [HF](https://huggingface.co/papers/2610.12572)
- **SoftwareGym(80표):** GUI 앱 182개를 자동 합성해 컴퓨터 사용 에이전트를 학습시키자 도메인 밖 OSWorld Pass@3가 41.4%에서 57.0%로 올랐다. [HF](https://huggingface.co/papers/2610.12611)
- **ProgramGym(46표):** C·C++·Go·Rust 저장소에서 검증 가능한 학습 환경 1,648개를 만들어 Qwen3.8-27B의 ProgramBench 점수를 34.9%에서 42.8%(에이전트 워크플로를 더하면 56.3%)로 올렸다. [HF](https://huggingface.co/papers/2610.13395)
- 게시: HF Daily Papers 10-12 00:00 UTC(arXiv 원 게시 10-07~10-09) · 신뢰도: **공식**(초록 기준)

### 8. 짧게

- **"Zod 4.6.6 악성코드 감염?"(HN 168점)은 오경보다.** 문제 커밋은 09-21에 유출된 git 자격증명으로 main에 직접 푸시됐고(이더리움 컨트랙트에서 페이로드를 받는 로더), 같은 날 되돌려져 npm에는 배포되지 않았다고 창 안의 v4.6.6 릴리스 노트(10-11 16:19 UTC)가 밝혔다. Zod는 MCP TypeScript SDK의 핵심 의존성이라 반응이 컸다. [릴리스](https://github.com/colinhacks/zod/releases/tag/v4.6.6) · [HN](https://news.ycombinator.com/item?id=50052814) (공식+커뮤니티)
- **"Fable took my jorb"(HN 208점):** 회사의 마지막 엔지니어가 "Fable이 내 보수보다 싸서" 해고됐다는 회고다. "AI가 일을 더 잘해서가 아니라 경영진에게 버틸 자신감을 줬다"는 대목이 공감을 얻었다. [글](https://cendyne.dev/posts/2026-10-11-fable-took-my-jorb.html) · [HN](https://news.ycombinator.com/item?id=50053216) (10-11 19:00 UTC, 커뮤니티)
- **Tao 블로그 게스트 글 "Illustration beyond Proof"(HN 92점):** AI가 정리 생산의 희소성을 깨뜨린 뒤 수학의 의미를 묻는 글로, 수학 원고 논란의 연장선이다. [글](https://terrytao.wordpress.com/2026/10/11/illustration-beyond-proof/) · [HN](https://news.ycombinator.com/item?id=50049133) (10-11 23:21 UTC, 커뮤니티)
- **직전 브리핑 후속:** LMCache는 여전히 정식 패치가 없다(PyPI 정식 0.5.5, 창 안에는 nightly만). mcp-go·MCP Kotlin SDK·mini-swe-agent도 창 안 수정 릴리스가 없다. `openai/math`는 10-09 이후 커밋이 없다. alignment.openai.com·DeepMind·Google Research·METR 블로그에는 창 안 신규 글이 없다.

## 써볼 만한 도구

### 1. SkillTotal 0.63/0.64 — 에이전트가 깔려는 패키지를 설치 직전에 정적 스캔하는 Claude Code 플러그인

- **한 줄 설명:** Claude Code가 `npx`·`npm install`·`pip install`·`uvx`·`claude mcp add`를 실행하기 직전에 hook으로 끼어들어 대상 패키지·MCP 서버·스킬을 로컬에서 정적 분석하고, 악성 징후가 있으면 설치를 막는다. 코드를 실행하지 않고 LLM도 부르지 않으며 근거마다 file:line을 붙인다.
- **추천 이유:** GhostAction과 MCP 공급망 CVE가 이어지는 지금, "에이전트가 알아서 패키지를 까는" 지점에 바로 걸 수 있는 방어책이다. 창 안의 0.63은 셸 구문을 제대로 해석해 따옴표 속 `;`·if·here-doc에 숨긴 설치 명령도 잡고, 읽지 못한 코드를 "clean"으로 판정하지 않는다. 0.64는 `.clinerules`·`.cursor/rules` 지시 주입, hex로 디코드해 실행하는 코드 등 우회 구문 5종을 더 잡는다. ⚠️ 개인 프로젝트(스타 12개)이고, 탐지율(악성 73/73, 오탐 0)은 자기 보고다. 설치 명령마다 최대 60초를 기다릴 수 있다.
- **설치/사용:** `pipx install skilltotal` 후 Claude Code에서 `/plugin marketplace add pezhik/skilltotal` → `/plugin install skilltotal@skilltotal` · [GitHub](https://github.com/pezhik/skilltotal)
- 게시: v0.63.0 10-11 17:59 UTC, v0.64.0 10-12 14:28 UTC, 정식 · 신뢰도: **커뮤니티**

### 2. OpenHands 1.27.0 — OpenCode·Pi를 내장 ACP 에이전트로

- **한 줄 설명:** OpenCode와 Pi가 내장 ACP 에이전트(로컬 백엔드)로 들어갔고, ACP 모델 목록을 에이전트에서 받아온다. Automations 설정(초안 저장, 에이전트가 폼을 대신 채움)이 정리됐고, Agent Canvas를 DigitalOcean Managed Agents에 연결할 수 있다.
- **추천 이유:** UI 하나에서 코딩 에이전트 백엔드를 바꿔 끼울 수 있어 에이전트끼리 비교·평가하기 좋다. MCP·hooks UI 버그 수정도 많다.
- **설치/사용:** [릴리스](https://github.com/OpenHands/OpenHands/releases/tag/v1.27.0)
- 게시: 10-12 15:06 UTC, 정식 · 신뢰도: **공식**

### 3. LLMxRay 0.7 — Ollama 앞단 프록시로 프롬프트 캐시가 깨지는 지점을 보여 줌

- **한 줄 설명:** 0.7.2의 Live 기능이 11435 포트에 Ollama처럼 동작하는 프록시를 연다. VS Code Copilot·Open WebUI·Continue·n8n을 여기에 붙이면 매 턴 프롬프트 구성(시스템·도구 스키마·히스토리), 캐시 적중량, 캐시가 깨진 지점, CPU로 일부 넘어갔는지가 보인다.
- **추천 이유:** 로컬 모델이 "왜 느린가"를 바로 짚는다. 작성자는 46K 토큰짜리 Copilot 프롬프트에서 컨텍스트를 64K로 올리자 캐시 적중이 99%가 됐다고 보고했다. ⚠️ Ollama 전용, 스타 22개의 소형 프로젝트. 프롬프트 원문 저장은 opt-in이다.
- **설치/사용:** npm `llmxray` 또는 Docker(`-p 11435:11435`) · [릴리스](https://github.com/LogneBudo/llmxray/releases/tag/v0.7.2)
- 게시: v0.7.0 10-11 20:19 UTC ~ v0.7.2 10-12 11:53 UTC, 정식 · 신뢰도: **커뮤니티**

### 4. Plimsoll — 실행 결과에 "격리 강도"를 찍어 주는 에이전트 코드 샌드박스

- **한 줄 설명:** Go로 만든 셀프호스트 샌드박스다. 실행 결과마다 격리 등급(process < container < kernel < vm)을 돌려주고, 요청에 최소 등급을 걸면 실제 샌드박스가 그보다 약할 때 아예 실행하지 않는다. 요청·결과의 SHA-256 기록과 서명 도구도 들어 있다.
- **추천 이유:** 직전 브리핑의 "모델 밖에서 가둬라" 원칙을 "얼마나 강하게 가뒀는지 증거로 남긴다"는 형태로 구현했다. Apache-2.0. ⚠️ 0.x 단계, 최신 릴리스 v0.21.0은 10-09(창 이전)다.
- **설치/사용:** [GitHub](https://github.com/plimsollmark/plimsoll) · [HN](https://news.ycombinator.com/item?id=50049388)
- 게시: Show HN 10-11 23:47 UTC(15점) · 신뢰도: **커뮤니티**

### 5. Atticus — Claude 구독으로 도는 macOS 음성 비서

- **한 줄 설명:** ⌃⌥Space를 누른 채 말하면 창 배치·파일 찾기·화면 읽기·텍스트 입력을 한다. 앱 열기 같은 단순 명령은 로컬에서 처리하고, 복잡한 요청만 `claude -p`(Claude Code 헤드리스)로 보내 API 키가 필요 없다. 한 번 시킨 동작은 학습해 다음부터 AI 없이 실행한다.
- **추천 이유:** Claude Code를 개인 자동화의 백엔드로 쓰는 짧고 읽기 쉬운 예시다(MIT). ⚠️ macOS 26 이상, 손쉬운 사용·화면 기록 권한을 요구하고 자동 업데이트되는 만든 지 이틀 된 개인 프로젝트이며 설치가 curl|bash다. 구독 플랜을 이렇게 쓰는 것이 이용 조건에 맞는지 직접 확인할 것.
- **설치/사용:** [GitHub](https://github.com/SaarthurR/atticus-mac) · [HN](https://news.ycombinator.com/item?id=50049588)
- 게시: Show HN 10-12 00:15 UTC(34점) · 신뢰도: **커뮤니티**

### 짧게

- **MCP 레퍼런스 서버 v2(미배포):** `v2/main` 브랜치에 2026-07-28 스펙(세션 없는 `server/discover`)과 2025-11-25 스펙을 한 프로세스에서 함께 지원하는 변경이 병합됐다. 신규 세대에서 filesystem은 Roots를 못 써 명령줄 디렉터리가 필수가 되고, SSE는 2025 세대 전용으로 남아 폐기 예고를 띄운다. 10-11의 1.0.0 배포판은 2025 세대만 지원한다. [PR #5126](https://github.com/modelcontextprotocol/servers/pull/5126) · [PR #5131](https://github.com/modelcontextprotocol/servers/pull/5131) (병합 10-12 02:48·05:33 UTC, 공식)
- **Pensieve(HN 106점):** macOS 로컬 의미+정확 검색 앱이다. MCP로 코딩 에이전트에 관련 구절 몇 개만 돌려줘 grep·read 호출을 줄인다. 무료 오픈소스. [사이트](https://tedhaley.ca/pensieve/) · [HN](https://news.ycombinator.com/item?id=50049508) (10-12 00:03 UTC, 커뮤니티)
- **openai-python 3.29.0:** `billing_not_active` 오류 코드, 새 음성 "shida", vault 메타데이터 필터 등 API 동기화. [릴리스](https://github.com/openai/openai-python/releases/tag/v3.29.0) (10-11 17:38 UTC, 공식)
- **llama.cpp b11558~b11574:** b11571·b11572에서 pplx-decider 계열 결정 모델을 지원한다. 빌드 태그(프리릴리스). [b11572](https://github.com/ggml-org/llama.cpp/releases/tag/b11572) (공식)
- **langchain-core 1.6.10 / langchain-anthropic 1.7.6 / langchain-openai 1.7.1:** 모델 프로필에 대화 중간 지원 여부 선언 추가. (10-12 14시대 UTC, 공식)
- **릴리스 없음:** Claude Code, Codex CLI, Copilot CLI, Anthropic SDK, OpenAI Agents SDK, MCP SDK 전체, LiteLLM, Ollama, vLLM, SGLang, LangGraph, Pydantic AI, crewAI, goose, Cline, Continue, Zed. claude-plugins-official·anthropics/skills도 창 안 커밋이 없다.

## 주목할 점

- **추론 단가 경쟁이 칩 단계로 내려가고 있다.** Microsoft가 OpenAI 모델까지 Maia로 돌리겠다고 한 만큼, 11월 Ignite에서 Foundry·Copilot의 가격이나 지연 시간 변화가 함께 나오는지 볼 만하다.
- **방어의 초점이 "모델"에서 "에이전트가 실행하는 경로"로 옮겨 가고 있다.** 이틀 간격의 GhostAction 물결, 추론 서버·에이전트 프레임워크의 미수정 CVE, 설치 직전 스캔(SkillTotal)·격리 등급 증명(Plimsoll) 같은 도구가 같은 문제를 가리킨다. 다음 GhostAction 물결이 10-14 전후에 오는지, GitHub가 일괄 조치에 나서는지가 다음 신호다.

---

*조사 제약: Telegraph 원문은 유료벽(402)이라 The Decoder·TechCrunch 보도로 대체했고 게시 시각을 확정하지 못했다. AP 본문은 403(r.jina.ai 경유도 일시 차단)이라 "OpenAI 안전 연구원 3인 해고" 재보도(10-11 브리핑에서 다룸)에 새 사실이 있는지 확인하지 못했다. blogs.microsoft.com은 자동화 접근이 403이라 r.jina.ai로 본문을 읽었다. Anthropic 보고서 정정 시각은 본문에 날짜만 있어 사이트맵 `lastmod`로 추정했다. Microsoft Research 피드는 빈 응답, Epoch AI 피드는 파싱 실패, phoronix는 Cloudflare에 막혀 "Linus Torvalds의 Claude Code 사용" 건(HN 24점)은 제외했다. MCP 레지스트리 API는 응답이 느려 첫 페이지만 확인했다. GhostAction 집계는 GitHub 검색 기반이라 실제보다 작을 수 있다. GHSA 미검토 권고의 영향 범위는 NVD 기술을 따랐고 재현하지 않았다. 도구는 실행해 보지 않고 README·릴리스 노트만 읽었다.*

*창 경계 항목: **AlphaProof Nexus Science 논문**(Gemini 3.1 Pro + AlphaProof로 Erdős 미해결 문제 9개 등, 게재 10-08, [DOI](https://doi.org/10.1126/science.aej2213)), **Cloudflare Clef-omni**(Qwen3-Omni 기반 멀티모달 결정 모델, HF 10-09, [블로그](https://blog.cloudflare.com/clef-faster-cheaper-multimodal/)), **vLLM Semantic Router Decision 3.0 모델**(HF 10-10, [d3-flash](https://huggingface.co/vllm-sr/d3-flash)), **TII Falcon-ASR**(1.6B 아랍어 중심 음성 인식, 10-10, [HF 블로그](https://huggingface.co/blog/tiiuae/falcon-asr)), **Skills Hub v1.0.0**(스킬을 48개 에이전트 도구로 동기화하는 데스크톱 앱, 10-11 07:44 UTC, [릴리스](https://github.com/qufei1993/skills-hub/releases/tag/v1.0.0)). 모두 창 이전 게시이고 창 안에는 매체 재보도나 HN 게시만 있었다.*
