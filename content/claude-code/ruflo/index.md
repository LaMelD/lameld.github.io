---
title: "Ruflo 조사: Claude Code 위에 올리는 멀티 에이전트 하네스, 무엇이 진짜이고 무엇이 아닌가"
date: 2026-10-02
weight: 7
tags: [claude-code, ruflo, claude-flow, multi-agent, mcp, security]
description: "Claude Flow에서 이름을 바꾼 Ruflo가 무엇이고 어떻게 동작하며 어떻게 설치·사용하는지, 그리고 공급망 사고와 독립 감사가 드러낸 위험과 한계까지 2026년 10월 기준으로 정리했다."
---

조사 기준일 2026-10-02. 저장소 [github.com/ruvnet/ruflo](https://github.com/ruvnet/ruflo), npm `ruflo` 3.50.0.

Ruflo는 Claude Code(그리고 Codex) 바깥을 감싸는 **에이전트 하네스**다. 제작자 Reuven Cohen(rUv)의 표현을 빌리면 "Agent = Model + Harness"이고, 모델은 코드를 쓰고 하네스는 도구·메모리·루프·샌드박스·통제를 준다. 2025년 6월 Claude Flow라는 이름으로 시작했고, 2026년 1월 Anthropic 상표 문제를 피해 Ruflo로 바꿨다. 이름은 Rust와 flow, 그리고 rUv를 합친 것이다. npm에는 `ruflo`, `claude-flow`, `@claude-flow/cli` 세 이름이 같은 버전으로 올라간다.

숫자만 보면 큰 프로젝트다. GitHub 스타 7만 3천, 포크 8천 7백, 열린 이슈 1,050개, npm 버전 411개, 주간 다운로드는 `ruflo` 5만 7천에 `claude-flow` 1만 8천을 더해 7만 5천 안팎이다. 그리고 커밋 대부분을 한 사람이 쓴다.

## 무엇을 준다고 하는가

README가 내세우는 것은 다음과 같다.

| 항목 | 주장 |
|---|---|
| 에이전트 | 코딩·테스트·보안·문서·아키텍처용 100개 이상(CLI 설치 기준 98개) |
| 스웜 | hierarchical, mesh, ring, star, adaptive 토폴로지와 Raft·Byzantine·Gossip·CRDT 합의 |
| 하이브마인드 | 여왕 에이전트(strategic, tactical, adaptive) 1개가 워커 8종을 지휘 |
| 메모리 | SQLite 위 AgentDB에 384차원 임베딩(all-MiniLM-L6-v2)을 HNSW로 색인. 세션을 넘어 기억 |
| 자기학습 | SONA, ReasoningBank, 궤적 학습. 성공한 작업 패턴을 저장하고 다음 라우팅에 반영 |
| 라우팅 | 단순 변환은 WASM(토큰 0), 중간은 Haiku/Sonnet, 복잡한 것만 Opus. Thompson sampling 밴딧 |
| 훅 | Claude Code 훅 27종이 자동으로 라우팅·학습·세션 복원 |
| 백그라운드 워커 | audit, optimize, testgaps, consolidate 등 12종이 데몬으로 돈다 |
| 연합(federation) | 다른 머신의 에이전트와 mTLS와 ed25519로 인증하고 PII를 지운 뒤 작업을 주고받음 |
| MCP | 313개 도구를 MCP 서버로 노출 |
| 플러그인 | Claude Code 네이티브 플러그인 35개, npm 플러그인 21개 |

이 표를 그대로 믿으면 안 된다. 아래 "한계" 절에 왜 그런지 적었다.

## 어떻게 동작하는가

큰 그림은 한 줄이다.

```text
사용자 → Ruflo(CLI/MCP) → 라우터 → 스웜 → 에이전트 → 메모리 → LLM 제공자
                       ↑                              ↓
                       └──────── 학습 루프 ←───────────┘
```

**Claude Code에 붙는 방식은 둘이다.** 하나는 MCP 서버다. `claude mcp add claude-flow -- npx ruflo@latest mcp start`로 등록하면 Claude가 `memory_search`, `swarm_init`, `agent_spawn` 같은 도구를 직접 부른다. 다른 하나는 훅이다. `ruflo init`이 `.claude/settings.json`에 PreToolUse, PostToolUse, SessionStart, UserPromptSubmit, PreCompact 훅을 써 넣고, 각 훅은 `.claude/helpers/hook-handler.cjs` 같은 로컬 스크립트를 부른다. 사용자는 평소처럼 Claude Code를 쓰고, 훅이 뒤에서 작업을 라우팅하고 결과를 기록한다.

**학습 루프**는 네 단계다. 작업 전에 `pre-task` 훅이 메모리에서 비슷한 패턴을 찾아 에이전트를 고른다(RETRIEVE). 작업 후 `post-task`가 성공 여부를 기록한다(JUDGE). 성공 패턴은 ReasoningBank에 저장되고(DISTILL), 데몬의 `consolidate` 워커가 낮은 품질을 쳐낸다(CONSOLIDATE). 다음 라우팅은 이 결과를 쓴다(ROUTE). 2026년 5월 저장소 자체 감사에서 이 루프가 프로세스 경계를 넘어 실제로 신뢰도를 바꾸는 것은 측정됐다. 성공과 실패 판정에 따라 패턴 신뢰도가 0.906에서 1.0, 다시 0.952로 움직였고, MoE 라우터는 보상 200회 뒤 coder 확률이 0.081에서 0.994로 갔다.

**스웜과 하이브마인드**는 다르다. 스웜은 토폴로지와 최대 에이전트 수를 정하고 작업을 분배하는 틀이다. 하이브마인드는 그 위에 여왕 에이전트를 두고 워커의 결과를 합의 알고리즘으로 모은다. 문서가 권하는 기본값은 "hierarchical, 최대 8, specialized"다. 이유는 드리프트 방지다. 조정자 하나가 매 출력을 목표와 대조하고, 에이전트가 적을수록 어긋날 면이 작다.

**메모리**는 프로젝트의 `.claude-flow/` 아래 SQLite에 산다. 기본 저장 단위는 네임스페이스로 구분한 키·값이고, 벡터 검색은 HNSW 색인을 탄다. "Context Autopilot"이라는 기능은 Claude Code의 자동 compaction을 PreCompact 훅에서 exit code 2로 막고, 대신 매 프롬프트마다 대화를 SQLite에 보관했다가 SessionStart에서 중요도순으로 되살린다.

**연합**은 v3.6에서 안정화됐다. `federation init`으로 키쌍을 만들고 `federation join wss://…`로 상대 노드에 붙는다. 상대는 처음에 "untrusted"로 시작해 성공률·가동률·위협·무결성 가중 점수로 신뢰가 오르고, 문제가 생기면 즉시 내려간다. 보내는 메시지는 14종 PII 탐지기를 거쳐 차단·마스킹·해시 중 하나로 처리된다.

**3.50.0의 변화**는 "Claude Code mod"다. 훅을 셸 명령 대신 Claude Code 엔진 안의 TypeScript 함수로 돌린다. 라우팅 한 번이 18.2ms에서 0.045ms로 줄었다는 것이 명분이다. 옵트인이고, Claude Code 2.1.287 이상과 Anthropic 쪽 롤아웃 스위치가 켜져 있어야 한다. API는 early access라 바뀔 수 있다고 적혀 있다.

## 설치

세 경로가 있고 표면적이 전혀 다르다.

| | Claude Code 플러그인 | CLI 설치 | MCP만 |
|---|---|---|---|
| 명령 | `/plugin marketplace add ruvnet/ruflo` 후 `/plugin install ruflo-core@ruflo` | `npx ruflo@latest init wizard` | `claude mcp add claude-flow -- npx ruflo@latest mcp start` |
| 작업 공간에 생기는 파일 | 없음 | `.claude/`, `.claude-flow/`, `CLAUDE.md`, 헬퍼 스크립트, settings | 없음 |
| 훅 | 없음 | 설치됨 | 없음 |
| MCP 서버 | ruflo-core만 자체 등록 | 등록됨 | 등록됨 |
| 용도 | 슬래시 명령 몇 개 써 보기 | README가 말하는 "전부" | 도구만 Claude에 노출 |

요구 사항은 Node.js 20 이상이다. 기본 설치는 ML 임베딩을 포함해 약 340MB이고, `npm install -g ruflo@latest --omit=optional`로 깔면 약 45MB다. `curl -fsSL https://cdn.jsdelivr.net/gh/ruvnet/ruflo@main/scripts/install.sh | bash` 한 줄 설치도 있는데, 이 스크립트는 기본값으로 `init`까지 실행하고 `--full`을 주면 전역 설치와 MCP 등록과 진단까지 한다. Windows PowerShell에서는 `npx` 경로만 된다.

설치 뒤 확인은 다음 순서다.

```bash
npx ruflo@latest doctor            # 구성 진단
npx ruflo@latest verify            # 설치된 바이트가 서명된 manifest와 일치하는지
claude mcp list                    # claude-flow 항목이 보여야 한다
```

같은 이유로 `ruflo cleanup --dry-run`이 있다. 삭제 명령은 뒤에서 다시 다룬다.

## 사용 형식

**아무것도 안 해도 된다는 것이 공식 입장이다.** `init` 뒤에는 평소처럼 Claude Code를 쓰면 훅이 알아서 한다. 그래도 명시적으로 쓰는 형식은 셋이다.

자연어로 스웜을 띄우는 것이 가장 흔하다. Claude Code 안에서 이렇게 말하면 MCP 도구를 통해 스웜이 구성된다.

```text
Spawn a Ruflo swarm to review the code in this folder for security and quality issues.
```

CLI로 직접 다루는 형식은 다음과 같다.

```bash
npx ruflo@latest daemon start                                           # 워커 데몬. 스웜 전에 켠다
npx ruflo@latest swarm init --topology hierarchical --max-agents 8 --strategy specialized
npx ruflo@latest agent spawn -t coder --name my-coder
npx ruflo@latest hive-mind spawn "Implement user authentication" --queen-type strategic --consensus byzantine
npx ruflo@latest task orchestrate "Migrate from Express to Fastify" --strategy adaptive
npx ruflo@latest memory store --key auth-pattern --value "..." --namespace patterns
npx ruflo@latest memory search "jwt refresh" --namespace patterns
npx ruflo@latest hooks route "refactor authentication to use JWT" --include-explanation
npx ruflo@latest hooks pretrain --depth deep                            # 코드베이스에서 패턴 사전 학습
npx ruflo@latest session save && npx ruflo@latest session restore latest
```

CLI는 26개 명령군에 140개가 넘는 하위 명령이 있다. `swarm`, `agent`, `task`, `memory`, `agentdb`, `session`, `config`, `hooks`, `mcp`, `plugins`, `workflow`, `neural`, `security`, `performance`, `daemon`, `doctor`, `migrate`, `federation`, `completions`가 주요 묶음이다.

MCP 도구 수를 줄여 토큰과 지연을 아끼는 설정이 있다. 313개를 다 노출하면 Claude Code 세션마다 도구 설명만으로 컨텍스트를 상당히 먹는다.

```bash
export CLAUDE_FLOW_TOOL_MODE=develop          # create, implement, test, fix, memory만
export CLAUDE_FLOW_TOOL_GROUPS=memory,test     # 또는 그룹을 직접 지정
export CLAUDE_FLOW_MAX_AGENTS=5
export CLAUDE_FLOW_CONTEXT_AUTOPILOT=false     # compaction 가로채기를 끈다
```

Codex CLI용은 `npx ruflo@latest init --codex`로 `AGENTS.md`를 만들고, `--dual`이면 Claude Code와 Codex를 함께 쓴다. 이 구성에서는 Ruflo가 상태와 메모리를 들고 Codex가 코드를 쓴다.

## 위험

**공급망 사고가 실제로 있었다.** 3.1.0-alpha.55부터 3.5.2까지 패키지의 `preinstall`에 난독화된 한 줄 스크립트가 들어 있었다. 설치할 때 조용히 `~/.npm/_npx/*/node_modules/` 아래 특정 이름의 디렉토리를 재귀 삭제하고, `~/.npm/_cacache/index-v5/`에서 "claude-flow"나 "ruflo"가 들어간 캐시 색인을 지우며, 모든 에러를 삼켰다. 옛 패키지 캐시를 치우려는 의도였다 해도 문서화 없이 난독화해서 사용자 홈 디렉토리를 건드린 것은 공급망 공격의 전형적 형태다. 이슈 #1261로 외부에서 지적된 뒤 v3.5.40에서 스크립트와 `package.json` 항목이 완전히 제거됐고, 그 뒤 `ruflo verify`와 SECURITY.md가 생겼다. 과거 사고라 지금 설치본에는 없지만, 이런 일이 한 번 있었던 프로젝트라는 사실은 남는다.

**curl | bash 설치**는 기본값으로 프로젝트 초기화까지 하고, `--full`이면 전역 설치와 Claude Code MCP 등록까지 한 번에 한다. 스크립트가 무엇을 하는지 읽지 않고 실행하면 작업 공간과 전역 Claude 설정이 동시에 바뀐다. npx 경로도 같은 코드를 실행하지만 적어도 단계가 보인다.

**훅은 코드 실행 권한이다.** `init`이 `.claude/settings.json`에 넣는 훅은 Claude Code가 매 도구 호출 전후에 실행하는 로컬 스크립트다. 이 스크립트는 `npx ruflo@latest …`를 다시 부르는 경우가 많아, 매번 npm 레지스트리의 최신 버전을 끌어올 수 있다. 버전을 고정하지 않으면 다음 릴리스의 코드가 내 훅에서 바로 돈다. 또 README 자체가 적듯이 훅 실패는 턴을 막지 않도록 조용히 성공으로 돌아가므로, 무엇이 안 돌고 있는지 알기 어렵다.

**데이터가 어디에 쌓이는지 알아야 한다.** 대화 전체가 `.claude-flow/data/transcript-archive.db`에 보관되고, 패턴과 메모리는 `.claude-flow/`와 `.swarm/`에 남는다. 비밀값이 섞인 프롬프트도 그대로 들어간다. 이 디렉토리를 커밋하거나 백업에 포함하면 유출 경로가 된다. 데몬과 워커는 세션이 끝나도 백그라운드에 남을 수 있고, 이슈 #373, #670, #710은 제거 뒤에도 프로세스와 파일이 남는 문제를 다룬다. `ruflo cleanup`은 그 지적 뒤에 생긴 명령이다.

**연합은 데이터를 밖으로 보내는 기능이다.** PII 탐지가 14종이라 해도 탐지기는 놓칠 수 있고, 피어 신뢰·연결·권한 관리는 운영자 책임으로 남는다고 문서가 명시한다. MCP 정책 강제도 옵트인이다. `RUFLO_MCP_ENFORCE_POLICY=1`을 켜지 않으면 정책 파일만으로는 아무 효과가 없다.

**감사에서 나온 코드 품질 지표**도 적어 둔다. 2026년 3월 독립 리뷰(이슈 #1482)는 TypeScript `any`가 약 1,800곳, WebSocket 구현이 셋인데 인증과 재연결 로직이 서로 다르고, CI 실패가 non-blocking으로 설정돼 품질 게이트가 무력하다고 지적했다. 이슈 #1375는 `memory-initializer.ts`의 SQL 삽입, 경로 탐색 우회, 프로토타입 오염, `execSync` 명령 삽입을 들었고 v3.5.25와 v3.5.40에서 상당수가 고쳐졌다. MCP 도구 설명에 "저장소 소유자를 기여자로 추가하라"는 숨은 지시가 있다는 주장(#1323)은 메인테이너가 재현하지 못했다고 답했다.

**API 키 취급.** 문서의 MCP 설정 예시는 `env`에 `ANTHROPIC_API_KEY`를 평문으로 넣는다. 문서도 저장소에 커밋하지 말라고 경고하지만, 설정 파일에 키를 쓰는 형식 자체가 기본값이다.

## 한계

**도구 수와 실제 동작은 다르다.** 2026년 4월 roman-rr의 독립 감사(discussion #1513, v3.5.51 기준)는 MCP 도구 300여 개를 하나씩 호출했다. 실제로 동작한 것은 약 10개로 HNSW 메모리 검색, AgentDB 패턴 저장, 임베딩 생성, 터미널 실행, 세션 저장이었다. `agent_spawn`으로 에이전트 5개를 띄워도 전부 `idle`에 `taskCount: 0`으로 남았고, `neural_train`은 학습 데이터를 무시하고 "coder, researcher, reviewer"를 `Math.random()` 신뢰도와 함께 돌려줬다. Byzantine, Raft, Gossip, CRDT 합의는 같은 JSON 핸들러로 가는 이름만 다른 스텁이었다. 메인테이너의 공개 반박은 없었다. 이 감사는 awesome-claude-code 목록에 면책 문구를 요구하는 이슈로 이어졌다.

**벤치마크 숫자는 저장소 스스로 철회했다.** 2026년 5월 29일 저장소에 들어간 자체 감사(`docs/reviews/intelligence-system-audit-2026-05-29.md`)는 "HNSW 150배에서 12,500배"가 실측 최대 1.48배이고 N이 5천 아래면 brute force보다 느리다는 것, "Flash Attention 2.49배에서 7.47배"가 코드에 `Math.random()`으로 박혀 있던 숫자라는 것, "임베딩 75배"에 기준선이 없다는 것을 확인했다. 이 감사는 `route feedback -r -1.0`이 +1.0으로 기록되는 버그도 찾았다. 자기개선 경로가 나쁜 에이전트를 강화하고 있었다는 뜻이다. v3.10.7에서 고쳤다. 지금 README는 "N=20k에서 약 1.9배, N=5k에서 3.2배에서 4.7배"로 바뀌어 있다. 공급자가 자기 숫자를 내리는 일은 드물다는 점은 인정할 만하지만, 같은 USERGUIDE 안에는 "150x faster retrieval"과 "2-7x speedup (benchmarked)" 같은 옛 문구가 여전히 남아 있다.

**토큰은 줄지 않고 늘 수 있다.** 문서는 "30%에서 50% 토큰 절감"을 말하지만 감사는 그 수치가 하드코딩된 상수라고 밝혔다. 실측은 반대였다. 훅이 매 메시지에 150에서 200토큰의 패턴 주입을 반복해 50메시지 세션에서 약 1만 5천 토큰이 결과와 무관한 잡음으로 들어갔고, 자동 메모리는 5,706건 중 고유한 것이 약 20건이면서 100MB 그래프 파일을 만들었다. 병렬 실행은 걸린 시간을 줄여도 토큰 총량은 늘린다. 저장소의 독립 리서치 요약도 "모델, 도구, 인프라, 재시도, 리뷰는 여전히 돈과 시간이 든다"고 적는다.

**훅은 Claude Code 훅 전부가 아니다.** 이슈 #377은 Ruflo 훅이 Claude Code의 구조화된 결정(JSON으로 allow/deny를 돌려주는 흐름)을 구현하지 않았다고 짚었다. 로깅과 검증은 하지만 런타임에 결정을 돌려주지 않는다. 위험한 Bash 명령을 동적으로 막는 훅은 `pre-command --validate-safety`라는 미리 만든 플래그로만 가능하다. 이슈는 "not planned"로 닫혔다. 3.50.0의 mod 전환은 이 간극을 함수 훅으로 메우려는 시도로 읽힌다.

**설치 경로마다 다른 물건이다.** 플러그인 경로는 슬래시 명령과 에이전트 정의뿐이고 MCP 도구 이름도 `mcp__plugin_ruflo-core_ruflo__memory_store`처럼 다르다. README가 설명하는 기능은 CLI 경로에서만 전부 나온다. 플러그인만 깔고 "스웜이 안 뜬다"고 하면 당연하다.

**문서가 버전을 따라가지 못한다.** 7,683줄짜리 USERGUIDE에는 "16 specialized agent roles"와 "100+ agents", "27 hooks"와 "33 lifecycle hooks", "313 tools"와 "300+ tools"가 섞여 있다. 패키지 이름도 `claude-flow`와 `ruflo`가 번갈아 나온다. 하루에 여러 번 릴리스되는 속도(411개 버전, 8개월)가 원인이다.

**1인 프로젝트다.** 스타가 7만이지만 커밋과 설계 결정은 거의 한 사람이 한다. 보안 PR #1292가 거부되고 일부만 #1298로 따로 들어간 이력, 외부 감사에 대한 공개 답변이 드문 점은 이 구조에서 온다. 메인테이너는 "Dream Cycle"이라는 이름으로 AI 에이전트가 야간에 저장소를 감사하고 이슈를 쓰게 하는데, 그 결과가 `Math.random()` 벤치마크를 잡아낸 것도 사실이고, 이슈 1,050개 중 상당수가 그 자동 생성물인 것도 사실이다.

## 그래서 어디에 쓰나

Claude Code만으로 부족한 지점이 분명할 때만 의미가 있다.

- **세션을 넘는 메모리**가 필요하면 Ruflo에서 실제로 동작하는 부분이 이것이다. 다만 Claude Code 자체의 auto-memory와 `CLAUDE.md`, 그리고 Serena나 Context7 같은 단일 목적 도구로도 같은 효과를 얻는다. 감사자는 Ruflo를 빼고 그쪽을 권했다.
- **여러 머신의 에이전트를 묶는 연합**은 다른 도구에 없다. 그러나 그 기능은 데이터가 밖으로 나가는 기능이고, 운영 책임은 그대로 남는다.
- **"100개 에이전트가 합의한다"는 그림**을 보고 들어오면 실망한다. 그 그림의 대부분은 아직 JSON 상태 파일이다.

혼자 또는 소규모로 개발한다면 쓸 이유가 약하다. Claude Code의 서브에이전트, Agent Teams, 훅, 플러그인만으로 같은 일을 더 적은 표면적으로 할 수 있다. 써 본다면 다음 조건을 지키는 편이 안전하다.

1. 플러그인 경로나 MCP 경로로 먼저 붙인다. 작업 공간에 파일을 쓰는 CLI `init`은 나중에 한다.
2. `npx ruflo@latest` 대신 버전을 고정한다. 훅이 매번 최신을 끌어오게 두지 않는다.
3. `.claude-flow/`, `.swarm/`를 `.gitignore`에 넣고 비밀값이 든 세션은 보관하지 않는다.
4. `CLAUDE_FLOW_TOOL_MODE`로 도구를 줄인다. 313개를 다 켜면 컨텍스트가 그만큼 준다.
5. 토큰 사용량을 설치 전후로 직접 잰다. 절감 수치는 믿지 않는다.
6. 설치 뒤 `ruflo verify`를 돌리고, 제거할 때는 `ruflo cleanup --force`, `claude mcp remove claude-flow`, `daemon stop`을 순서대로 한 뒤 `.claude/settings.json`의 훅 항목을 직접 확인한다.

## 참고

- [ruvnet/ruflo](https://github.com/ruvnet/ruflo) — README, 설치 경로 비교표, 연합 설명
- [docs/USERGUIDE.md](https://github.com/ruvnet/ruflo/blob/main/docs/USERGUIDE.md) — 7,683줄 사용자 가이드. 명령·환경변수·훅 목록
- [v3.50.0 릴리스 노트](https://github.com/ruvnet/ruflo/releases/tag/v3.50.0) — Claude Code mod 전환, 요구 버전
- [v3.5.0 릴리스 개요 (#1240)](https://github.com/ruvnet/ruflo/issues/1240) — 알파 졸업, Rust/WASM 전환, V2→V3 마이그레이션
- [저장소 자체 감사 2026-05-29](https://github.com/ruvnet/ruflo/blob/main/docs/reviews/intelligence-system-audit-2026-05-29.md) — 벤치마크 철회, 음수 보상 버그
- [독립 감사: 300+ MCP 도구 (roman-rr)](https://gist.github.com/roman-rr/ed603b676af019b8740423d2bb8e4bf6) 와 [discussion #1513](https://github.com/ruvnet/ruflo/discussions/1513) — 실제 동작 도구 약 10개, 토큰 낭비 실측
- [보안·신뢰성 독립 리뷰 (#1482)](https://github.com/ruvnet/ruflo/issues/1482), [보안 감사 요약 (#1375)](https://github.com/ruvnet/ruflo/issues/1375), [v3.5.40 보안 수정 (#1384)](https://github.com/ruvnet/ruflo/issues/1384), [난독화 preinstall 공개 (#1261)](https://github.com/ruvnet/ruflo/issues/1261)
- [Claude Code 훅 대비 구현 보고 (#377)](https://github.com/ruvnet/ruflo/issues/377) — 구조화 결정 미구현
- [Claude Flow Is Dead. Long Live Ruflo (dev.to)](https://dev.to/stevengonsalvez/claude-flow-is-dead-long-live-ruflo-5coi) — 개명 배경
- [What the Agent Layer Actually Runs (rywalker)](https://rywalker.com/research/claude-flow) — 마케팅과 실체 대조
- [npm: ruflo](https://www.npmjs.com/package/ruflo) — 버전 이력, 다운로드
