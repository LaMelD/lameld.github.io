---
title: "Claude Code 설정 파일 정리: settings.json과 .claude.json"
date: 2026-09-17
weight: 3
tags: [claude-code, settings]
description: "settings.json 계열은 사람이 편집하는 설정 파일이고 .claude.json은 Claude Code가 관리하는 상태 파일이다. 네 파일의 스코프와 우선순위, 무엇을 어디에 넣을지를 정리했다."
---

> **한 줄 요약** — `settings.json` 계열은 **사람이 편집하는 설정 파일**, `.claude.json`은 **Claude Code가 관리하는 상태·캐시 파일**입니다. 서로 역할이 완전히 다릅니다.

## 1. 전체 지도

| 파일 | 스코프 | git 커밋 | 손으로 편집 | 용도 |
|---|---|---|---|---|
| `~/.claude/settings.json` | 유저 (모든 프로젝트) | 해당 없음 | ⭕️ | 내 개인 기본 설정 |
| `.claude/settings.json` | 프로젝트 (팀 공유) | ⭕️ 커밋함 | ⭕️ | 팀 공통 훅·권한·환경변수 |
| `.claude/settings.local.json` | 프로젝트 (나만) | ❌ 자동 gitignore | ⭕️ | 이 저장소에서 나만의 오버라이드 |
| `~/.claude.json` | 유저 전역 상태 | 해당 없음 | ❌ 건드리지 말 것 | OAuth 세션, MCP 설정, 프로젝트별 상태, 캐시 |

## 2. 우선순위

같은 키가 여러 파일에 있으면 **아래로 갈수록 이깁니다.**

1. `~/.claude/settings.json` — 유저
2. `.claude/settings.json` — 프로젝트 공유
3. `.claude/settings.local.json` — 프로젝트 개인
4. CLI 인자 (그 세션에만 적용되는 일시적 오버라이드)
5. **managed 설정** — 조직 정책, 어떤 것으로도 덮을 수 없음

> **예외:** `permissions` 규칙은 덮어쓰기가 아니라 **스코프 간에 병합(merge)** 됩니다. 유저 설정의 allow 목록과 프로젝트 설정의 allow 목록이 합쳐집니다.

### managed 설정 경로

- macOS: `/Library/Application Support/ClaudeCode/managed-settings.json`
- Linux / WSL: `/etc/claude-code/managed-settings.json`
- Windows: `C:\Program Files\ClaudeCode\managed-settings.json`
- 같은 디렉토리의 `managed-settings.d/` 드롭인도 읽습니다

## 3. settings.json

사람이 직접 쓰는 설정 파일. 유저 레벨(`~/.claude/`)과 프로젝트 레벨(`.claude/`) 둘 다 같은 스키마입니다.

```json
{
  "model": "opus",
  "env": {
    "NODE_ENV": "development"
  },
  "permissions": {
    "allow": ["Bash(npm test)", "Bash(npm run build)"],
    "deny": ["Bash(rm -rf *)"]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{ "type": "command", "command": "npx prettier --write $CLAUDE_FILE_PATHS" }]
      }
    ]
  }
}
```

**프로젝트 `.claude/settings.json`에 넣을 것** — 팀 전체가 공유해야 의미 있는 것

- 포맷터·린터 훅
- 공통 허용 명령 (`Bash(npm test)` 등)
- 프로젝트 전용 모델·환경변수

## 4. settings.local.json

**프로젝트 스코프에만 존재하는 개념**입니다. `.claude/settings.local.json`.

- Claude Code가 `.gitignore`에 자동으로 추가합니다
- 권한 프롬프트에서 "항상 허용"을 누르면 여기에 쌓입니다
- 내 로컬 경로, 개인 API 키, 실험용 설정 등을 넣는 곳

### `~/.claude/settings.local.json`은 레거시입니다

공식 문서의 계층에는 **유저 레벨 local 파일이 없습니다.** 그런데 예전에 Claude Code를 쓰던 환경에는 이 파일이 남아있는 경우가 있습니다.

CLI 바이너리(v2.1.220)를 뜯어보면 이 파일은 `legacy`로 명시돼 있고, **읽기와 권한 회수(revoke) 경로만** 남아있습니다.

```text
localSettings: legacy settings.local.json could not be evaluated
Failed to read legacy settings.local.json at ...
Transform failed against legacy settings.local.json at ...
Failed to revoke from legacy settings.local.json at ...
```

초기 버전이 전역 "항상 허용"을 여기에 기록했고, 지금은 하위 호환용으로만 유지되는 상태입니다.

> **정리:** 새로 넣을 설정은 전부 `~/.claude/settings.json`으로. 기존 `~/.claude/settings.local.json`은 내용을 옮기고 지워도 됩니다.

## 5. .claude.json

`~/.claude.json` — 위 세 파일과 성격이 완전히 다릅니다. **설정이 아니라 상태 저장소**입니다.

공식 문서 설명:

> Other configuration is stored in `~/.claude.json`. This file contains your OAuth session, MCP server configurations for user and local scopes, per-project state (allowed tools, trust settings), and various caches.

### 실제로 들어있는 것

- **인증** — `oauthAccount`, `userID`, `machineID`
- **MCP 서버** — 유저·로컬 스코프의 `mcpServers`
- **프로젝트별 상태** — `projects` 아래에 절대경로를 키로 프로젝트마다 한 덩어리씩
- **온보딩·통계·실험 플래그** — `numStartups`, `tipsHistory`, `skillUsage`, `cachedStatsigGates` 등 잡다한 캐시

`projects` 하위의 프로젝트별 키:

```text
allowedTools
hasTrustDialogAccepted
mcpServers
enabledMcpjsonServers
disabledMcpjsonServers
mcpContextUris
exampleFiles
projectOnboardingSeenCount
hasClaudeMdExternalIncludesApproved
hasClaudeMdExternalIncludesWarningShown
```

> **손으로 편집하지 마세요.** OAuth 세션이 들어있고 Claude Code가 수시로 덮어씁니다. 프로젝트가 쌓이면 파일이 계속 커집니다 (프로젝트 31개 기준 약 100KB). Claude Code가 `~/.claude.json.backup`을 자동으로 만들어 둡니다.

### 관련: `.mcp.json`

**프로젝트 스코프** MCP 서버는 `.claude.json`이 아니라 프로젝트 루트의 `.mcp.json`에 따로 저장됩니다. 이건 커밋해서 팀과 공유하는 파일입니다.

## 6. 실전 배치 가이드

| 넣고 싶은 것 | 어디에 |
|---|---|
| 내 기본 모델, 전역 단축 설정 | `~/.claude/settings.json` |
| 팀 공통 포맷터 훅, 공통 허용 명령 | `.claude/settings.json` (커밋) |
| 내 로컬 경로, 개인 키, "항상 허용" 누적분 | `.claude/settings.local.json` |
| 팀과 공유할 MCP 서버 | `.mcp.json` (커밋) |
| 나만 쓰는 MCP 서버 | `claude mcp add` 명령 (→ `~/.claude.json`에 기록됨) |
| 조직 전체 강제 정책 | managed-settings.json |

## 7. 확인 커맨드

```bash
# 어떤 설정이 최종 적용됐는지
claude config list

# 현재 권한 규칙과 출처 확인 (세션 안에서)
/permissions

# 훅 등록 상태 확인 (세션 안에서)
/hooks

# 파일 직접 확인
ls -la ~/.claude/settings*.json .claude/settings*.json
```

---

*기준: Claude Code v2.1.220 / 2026-07-31 확인*
