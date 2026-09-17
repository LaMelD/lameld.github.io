---
title: "MCP 서버 수동 등록: CLI, .mcp.json, --mcp-config"
date: 2026-09-17
weight: 4
tags: [claude-code, mcp]
description: "claude mcp add, .mcp.json 직접 작성, --mcp-config와 shell alias까지 MCP 서버를 등록하는 방법과 스코프별 저장 위치, 우선순위, 확인 커맨드를 정리했다."
---

> **한 줄 요약** — 나만 쓰면 `claude mcp add -s user`, 팀과 공유하면 `.mcp.json` 직접 작성 후 커밋. `~/.claude.json`을 건드리기 싫다면 `~/mcp.json` + shell alias(4번).

## 1. `claude mcp add` — CLI 명령

가장 흔하고 안전한 방법입니다. 스키마 검증과 중복 이름 체크를 해줍니다.

```bash
# stdio (로컬 프로세스)
claude mcp add my-server -e API_KEY=xxx -- npx -y my-mcp-server --flag arg

# HTTP
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp

# HTTP + 헤더
claude mcp add --transport http corridor https://app.corridor.dev/api/mcp \
  --header "Authorization: Bearer ..."
```

| 옵션 | 설명 |
|---|---|
| `-s, --scope` | `local`(기본) / `project` / `user` |
| `-t, --transport` | `stdio`(기본) / `sse` / `http` |
| `-e, --env` | 서버에 넘길 환경변수 (`-e KEY=value`) |
| `-H, --header` | HTTP 헤더 |
| `--client-id` / `--client-secret` / `--callback-port` | OAuth 서버용 |

> `--` (더블 대시) 뒤는 **전부 서버 실행 커맨드로 그대로 전달**됩니다. Claude 자체 옵션과 서버 옵션을 가르는 구분자입니다.

## 2. `claude mcp add-json` — JSON 문자열로 한 번에

서버 문서에 있는 JSON을 그대로 복붙할 때 편합니다.

```bash
claude mcp add-json my-server \
  '{"command":"npx","args":["-y","my-mcp-server"],"env":{"API_KEY":"xxx"}}' \
  -s user
```

## 3. 파일 직접 편집

### 스코프별 저장 위치

| 스코프 | 로드 범위 | 팀 공유 | 저장 위치 |
|---|---|---|---|
| `local` (기본) | 현재 프로젝트만 | ❌ | `~/.claude.json` → `projects["/경로"].mcpServers` |
| `project` | 현재 프로젝트만 | ⭕️ 버전관리 | 프로젝트 루트 `.mcp.json` |
| `user` | 모든 프로젝트 | ❌ | `~/.claude.json` 최상위 `mcpServers` |

> MCP의 "local 스코프"는 `.claude/settings.local.json`과 **아무 관계 없습니다.** 이름만 비슷하고 `~/.claude.json`에 저장됩니다.

### `.mcp.json` 포맷

손으로 편집하는 것을 권장하는 유일한 파일입니다.

```json
{
  "mcpServers": {
    "shared-server": {
      "type": "http",
      "url": "${API_BASE_URL:-https://api.example.com}/mcp",
      "headers": { "Authorization": "Bearer ${API_KEY}" },
      "timeout": 600000
    },
    "db": {
      "command": "npx",
      "args": ["-y", "my-db-server"],
      "env": { "DB_URL": "${DB_URL}" }
    }
  }
}
```

- **환경변수 확장**: `${VAR}`, `${VAR:-기본값}` — `command` / `args` / `env` / `url` / `headers`에서만 동작
- 변수가 없고 기본값도 없으면 에러가 아니라 **경고만 뜨고 `${VAR}` 문자열 그대로** 들어감
- `type`은 `streamable-http`도 `http`의 별칭으로 받음
- `timeout`은 밀리초 단위, 서버별 `MCP_TOOL_TIMEOUT` 오버라이드
- `.mcp.json` 서버는 보안상 **최초 실행 시 승인 프롬프트**가 뜹니다 (`claude mcp reset-project-choices`로 초기화)

### `~/.claude.json` 직접 편집 — 되긴 되지만

최상위 `mcpServers`에 직접 써넣으면 `claude mcp add -s user`와 **결과물이 완전히 같습니다.** 설정 자체는 정상 동작합니다. 다만 감수해야 할 리스크가 있습니다.

1. **덮어쓰기 충돌** — Claude Code가 이 파일에 세션·캐시를 수시로 씁니다. 세션이 떠 있는 상태에서 편집하면 내 수정이 날아갈 수 있습니다
2. **JSON 깨지면 전부 날아감** — OAuth 세션까지 한 파일에 있어서 문법 오류 하나로 재로그인해야 할 수 있습니다 (`~/.claude.json.backup`이 자동 생성되긴 함)
3. **검증 없음** — 중복 이름·스키마 체크를 안 해줍니다

> **결론:** 이미 직접 넣어둔 설정은 그대로 두면 됩니다. 앞으로 추가할 때만 `claude mcp add -s user`를 쓰고, 굳이 편집해야 한다면 Claude Code를 종료한 뒤에 하세요. 이 파일을 아예 안 건드리고 싶다면 **4번의 shell alias 방식**을 보세요.

## 4. `--mcp-config` — 내 파일로 관리하기

```bash
# 파일 또는 JSON 문자열, 공백 구분으로 여러 개
claude --mcp-config ./my-mcp.json '{"mcpServers":{}}'

# 다른 설정 전부 무시하고 이것만 사용
claude --mcp-config ./my-mcp.json --strict-mcp-config
```

CI나 일회성 테스트에 적합합니다. 설정 파일에 흔적을 남기지 않습니다.

### 응용: shell alias로 상시 적용

일회성 옵션으로 끝낼 것이 아니라 alias에 물려두면, `~/.claude.json`을 **아예 건드리지 않고** MCP 설정을 내가 관리하는 파일 하나로 유지할 수 있습니다.

먼저 설정 파일을 따로 만듭니다 — `~/mcp.json`

```json
{
  "mcpServers": {
    "notion": { "type": "http", "url": "https://mcp.notion.com/mcp" },
    "playwright-remote": { "type": "http", "url": "http://127.0.0.1:8931/mcp" }
  }
}
```

`~/.zshrc` (또는 `~/.bashrc`)에 alias로 물립니다.

```bash
alias claude='claude --mcp-config ~/mcp.json'
```

**장점**

- `~/.claude.json`을 읽기 전용처럼 취급 가능 — 손편집 리스크 없음
- 작은 파일 하나라 dotfiles 저장소에 넣어 버전관리·머신 간 동기화 가능
- 서버 추가·제거가 텍스트 편집 한 번

**주의**

- alias를 안 거치고 `claude`를 직접 실행하면 서버가 안 붙습니다. 스크립트·CI·다른 셸에서 주의
- 기존에 `~/.claude.json`에 넣어둔 같은 서버가 있으면 중복됩니다 → `claude mcp remove <name> -s user`로 정리
- 다른 설정을 전부 무시하고 이 파일만 쓰려면 `--strict-mcp-config`를 같이 붙입니다

## 5. 우선순위

같은 이름의 서버가 여러 곳에 있을 때, 위에서부터 이깁니다.

1. Local 스코프
2. Project 스코프
3. User 스코프
4. 플러그인 제공 서버
5. claude.ai 커넥터

> **병합이 아닙니다.** 우선순위가 높은 쪽의 **서버 엔트리 전체**가 통째로 사용되고 나머지는 무시됩니다. 1~3은 **이름**으로, 4~5는 **엔드포인트(URL/커맨드)**로 중복을 판정합니다.

플러그인이 제공하는 서버는 `plugin:<플러그인명>:<서버명>` 형태로 등록됩니다.

## 6. 확인·관리 커맨드

```bash
claude mcp list             # 목록 + health check (⏸ = 승인 대기)
claude mcp get <name>       # 상세
claude mcp login <name>     # OAuth 인증
claude mcp logout <name>    # 저장된 OAuth 자격증명 삭제
claude mcp remove <name> -s <scope>
claude mcp reset-project-choices    # .mcp.json 승인/거부 초기화
claude mcp add-from-claude-desktop  # Claude Desktop에서 가져오기 (Mac/WSL)
```

세션 안에서는 `/mcp` 슬래시 커맨드로 연결 상태를 보고 개별 서버를 켜고 끌 수 있습니다. 이 토글 기록은 `~/.claude.json`에 프로젝트별로 `enabledMcpServers` / `disabledMcpServers`에 저장됩니다.

> `enabledMcpServers` / `disabledMcpServers`는 settings.json의 `enabledMcpjsonServers` / `disabledMcpjsonServers`와 **다른 것**입니다. 후자는 `.mcp.json` 서버의 승인 여부를 다룹니다.

## 7. 어떤 방법을 쓸까

| 상황 | 방법 |
|---|---|
| 내가 모든 프로젝트에서 쓸 서버 | `claude mcp add -s user` |
| 팀 전체가 공유할 서버 | `.mcp.json` 직접 작성 후 커밋 |
| 이 프로젝트에서 나만 실험 | `claude mcp add` (기본 local) |
| 서버 문서의 JSON 복붙 | `claude mcp add-json` |
| CI / 일회성 테스트 | `--mcp-config` (+ `--strict-mcp-config`) |
| `~/.claude.json`을 안 건드리고 싶을 때 | `~/mcp.json` • shell alias에 `--mcp-config` 물리기 |
| MCP 설정을 dotfiles로 버전관리 | 위와 동일 (`~/mcp.json`을 저장소에 포함) |
| 조직 전체 강제 배포 | `managed-mcp.json` • `allowedMcpServers` / `deniedMcpServers` |

---

*기준: Claude Code v2.1.220 / 2026-07-31 확인*
