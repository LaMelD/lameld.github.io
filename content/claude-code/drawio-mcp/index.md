---
title: "draw.io MCP: 공식 서버와 커뮤니티 서버 조사"
date: 2026-10-16
weight: 9
tags: [claude-code, drawio, mcp, diagram]
description: "draw.io를 AI 에이전트에 붙이는 MCP 서버를 조사했다. JGraph 공식 jgraph/drawio-mcp와 커뮤니티 lgazo/drawio-mcp-server를 중심으로, 각 저장소의 README와 npm·PyPI·GitHub 메타데이터에 적힌 사실만 정리했다. 실측은 하지 않았다."
---

> 이 글은 각 프로젝트의 README, 공식 문서, npm·PyPI 레지스트리, GitHub API에서 확인한 내용만 적은 **조사 글**입니다. 직접 설치하거나 실행하지 않았습니다. 어느 쪽이 더 낫다는 판단이나 사용 장면 추정은 넣지 않았습니다. 버전, 날짜, 스타 수는 2026-10-16에 확인한 값입니다.

## 조사 대상

"draw.io MCP"로 검색하면 여러 프로젝트가 나옵니다. 이름이 비슷해서 먼저 구분합니다.

| 프로젝트 | 패키지 | 최신 버전 | 라이선스 | 저장소 생성 | 스타 |
|---|---|---|---|---|---|
| [jgraph/drawio-mcp](https://github.com/jgraph/drawio-mcp) | npm `@drawio/mcp` | 1.7.0 (2026-10-13) | Apache-2.0 | 2026-02-02 | 5,649 |
| [lgazo/drawio-mcp-server](https://github.com/lgazo/drawio-mcp-server) | npm `drawio-mcp-server` | 2.3.0 (2026-09-15) | MIT | 2025-04-21 | 1,486 |
| [simonkurtz-MSFT/drawio-mcp-server](https://github.com/simonkurtz-MSFT/drawio-mcp-server) | 소스 또는 Docker | — | MIT | 2026-02-17 | 82 |
| [yohasacura/drawio-mcp](https://github.com/yohasacura/drawio-mcp) | PyPI `drawio-mcp` | 1.0.2 | MIT | 2026-02-09 | 5 |
| [techdeveloper-org/mcp-drawio-diagram](https://github.com/techdeveloper-org/mcp-drawio-diagram) | 소스 | — | MIT (README) | 2026-03-31 | 1 |

주의할 이름이 둘 있습니다. npm의 `drawio-mcp`는 Sujimoshi/drawio-mcp(1.6.0, 2025-09-22)이고, PyPI의 `drawio-mcp`는 yohasacura/drawio-mcp입니다. 서로 다른 프로젝트입니다. 공식 패키지는 스코프가 붙은 `@drawio/mcp`입니다.

## jgraph/drawio-mcp (공식)

출처: [저장소 README](https://github.com/jgraph/drawio-mcp), [mcp-tool-server/README.md](https://github.com/jgraph/drawio-mcp/blob/main/mcp-tool-server/README.md), [plugins/claude-code/README.md](https://github.com/jgraph/drawio-mcp/blob/main/plugins/claude-code/README.md), [draw.io 공식 문서](https://www.drawio.com/docs/manual/generate/drawio-mcp-server/)

README는 "The official draw.io MCP server"라고 소개합니다. 저장소 하나에 네 가지 연동 방식이 들어 있고, README가 직접 비교표를 제공합니다. 아래는 그 표를 옮긴 것입니다.

| | MCP App Server | MCP Tool Server | 어시스턴트 플러그인 | 프로젝트 지침 |
|---|---|---|---|---|
| 동작 | 채팅 안에 인라인 렌더링 | 브라우저에서 draw.io를 연다 | `.drawio` 파일 생성, PNG/SVG/PDF 내보내기 또는 브라우저 URL | Claude가 Python으로 draw.io URL 생성 |
| 설치 | 불필요 (`mcp.draw.io` 호스팅) | npm 패키지 | 플러그인 한 줄 (PNG/SVG/PDF는 draw.io Desktop 필요) | 불필요, 지침 붙여넣기 |
| 입력 형식 | XML, Mermaid | XML, CSV, Mermaid | XML, Mermaid(Mermaid는 Desktop 필요) | XML, CSV, Mermaid |
| 대상 | Claude.ai, ChatGPT, VS Code, Cursor, GitHub Copilot CLI, MCP Apps 호스트 | Claude Desktop, Cursor, OpenCode, 모든 MCP 클라이언트 | Claude Code, Codex CLI, GitHub Copilot, Cursor, OpenCode | Claude.ai (Projects) |

### MCP App Server

MCP Apps 확장을 써서 다이어그램을 채팅 안에 iframe으로 렌더링합니다. 호스팅 엔드포인트는 `https://mcp.draw.io/mcp`이고, Claude.ai에서는 [커넥터 디렉터리](https://claude.ai/directory/draw-io)에 등록되어 있습니다. GitHub Copilot CLI처럼 인라인 렌더링이 안 되는 호스트에서는 draw.io로 여는 링크를 돌려준다고 적혀 있습니다. Node.js 로컬 실행, `jgraph/drawio-mcp` Docker 이미지, Cloudflare Workers 배포를 지원합니다.

도구는 셋입니다.

- `create_diagram`: draw.io XML 또는 Mermaid(28종)를 인라인 다이어그램으로 렌더링
- `search_shapes`: AWS, Azure, GCP, P&ID, electrical, Cisco, Kubernetes, UML, BPMN 등 라이브러리의 1만여 도형을 키워드로 검색해 XML에 쓸 스타일 문자열을 반환. 내장 라이브러리에 맞는 것이 없으면 draw.io 아이콘 서비스(`icons.diagrams.net`)로 보충
- `get_drawio_reference`: XML 또는 Mermaid 생성 참조 문서를 반환

### MCP Tool Server

npm `@drawio/mcp`입니다. `npx @drawio/mcp`로 실행하고, Claude Code 등록 명령은 README에 이렇게 적혀 있습니다.

```sh
claude mcp add drawio -- npx -y @drawio/mcp
```

도구 목록입니다.

| 도구 | README 설명 |
|---|---|
| `open_drawio_xml` | XML을 draw.io 에디터로 연다. 옵션 `lightbox`(읽기 전용), `dark`, `postLayout: "elk"`, `direction`, `routing: "libavoid"` |
| `open_drawio_csv` | CSV를 다이어그램으로 변환해 연다 |
| `open_drawio_mermaid` | Mermaid를 draw.io 다이어그램으로 연다. flowchart에 한해 `postLayout: "elk"` |
| `get_drawio_reference` | `format`이 `xml` 또는 `mermaid`인 참조 문서 반환 |
| `search_shapes` | App Server와 같은 도형 검색. 기본 10개, 최대 50개 |
| `list_pages` / `get_page` / `set_page` | 로컬 다중 페이지 `.drawio` 파일을 페이지 단위로 읽고 교체. 경로는 `.drawio` 또는 `.xml` |

동작 원리는 README의 "How It Works"에 있습니다. 내용을 pako deflateRaw로 압축해 base64로 인코딩하고, XML은 `#R`, Mermaid와 CSV는 `#create=` 프래그먼트를 붙인 draw.io URL을 만들어 LLM에 돌려줍니다.

`open_drawio_xml`은 받은 XML을 먼저 수리한다고 적혀 있습니다. 앞뒤에 섞인 텍스트(코드 펜스, 도구 호출 태그)를 떼고, 속성값의 `<`와 `&`를 이스케이프하고, 레이어가 자기 자신을 부모로 쓴 경우를 고칩니다. README는 이 실수들이 테스트에서 LLM이 쓴 다이어그램의 3%를 깨뜨렸다고 씁니다. 그 뒤 정규화를 합니다. 엣지를 양 끝 셀의 공통 조상 아래로 옮기고, 지오메트리 없는 엣지에 지오메트리를 붙이고, 자식을 잘라 먹는 컨테이너를 키웁니다. draw.io Desktop CLI의 `--normalize`와 같은 처리라고 합니다.

### 레이아웃 후처리

README의 "Diagram layouts" 표를 옮긴 것입니다.

| 패스 | 하는 일 | App Server | Tool Server | 플러그인 | 프로젝트 지침 |
|---|---|---|---|---|---|
| ELK (`postLayout: "elk"`) | 노드를 계층 레이아웃으로 재배치하고 엣지도 라우팅 | ✅ | — | Desktop CLI `--layout` | — |
| libavoid (`routing: "libavoid"`) | 노드 위치는 두고 커넥터만 도형을 피해 직교 라우팅 | ✅ | 1.3.0부터 | Desktop CLI `--layout libavoid` | — |

README는 둘 중 하나만 쓰라고 적습니다. ELK가 엣지까지 그리므로 libavoid를 겹치면 중복이라는 설명입니다. Mermaid는 이미 자동 배치라 해당 없습니다.

### Claude Code 플러그인

```text
/plugin marketplace add jgraph/drawio-mcp
/plugin install drawio@drawio
```

플러그인은 `drawio` 스킬을 담고 있습니다. 호출은 `/drawio:drawio`이고, "draw a flowchart for user login"처럼 요청만 해도 스킬 설명으로 트리거된다고 적혀 있습니다. README가 설명하는 동작 순서입니다.

1. draw.io Desktop이 설치되어 있고 표준 유형(flowchart, sequence, class, state, ER, gantt, mindmap)이면 Mermaid로 쓰고 Desktop CLI로 변환한다. 커스텀 스타일, 정밀 배치, 특정 도형 라이브러리가 필요하거나 Desktop이 없으면 draw.io XML을 직접 쓴다.
2. `.drawio` 파일을 만든다. XML이면 선택적으로 ELK `--layout`을 돌린다.
3. 요청에 `png`, `svg`, `pdf`가 있으면 Desktop CLI로 내보낸다. XML을 임베드하므로 draw.io에서 다시 열 수 있다. `url`이면 Node 내장 `zlib`으로 압축해 `https://app.diagrams.net/#R...`을 브라우저에서 연다.
4. 결과를 연다.

| 형식 | 산출물 | 의존성 |
|---|---|---|
| 기본 | `.drawio` 파일 | 없음 |
| `png` / `svg` / `pdf` | `.drawio.png` 등, XML 임베드 | draw.io Desktop |
| `url` | `app.diagrams.net` 브라우저 탭 + 로컬 `.drawio` 보관 | Node.js |

1.2.3부터 플러그인의 `.mcp.json`에 호스팅 App Server(`https://mcp.draw.io/mcp`, `type: http`)가 같이 들어 있습니다. 앱을 보여 주는 호스트에서는 인라인 렌더링이 되고, Claude Code 터미널에서는 파일을 쓴다고 적혀 있습니다. 전부 로컬에 두려면 `/mcp`에서 그 서버를 끄거나 `.mcp.json`을 셀프 호스팅 App Server로 바꾸라고 안내합니다.

Codex CLI와 GitHub Copilot CLI용 플러그인, Cursor 마켓플레이스 플러그인도 같은 저장소에 있습니다.

### 데이터가 어디로 가는가

README "Data Residency & Offline Use"의 표입니다.

| 방식 | 다이어그램이 기기 밖으로 나가는가 |
|---|---|
| App Server 호스팅 (`mcp.draw.io`) | 나간다. MCP 요청으로 draw.io 서버에 전송 |
| App Server 셀프 호스팅 | 안 나간다 |
| Tool Server (`@drawio/mcp`) | 안 나간다. URL 프래그먼트에 실리고 브라우저는 프래그먼트를 서버에 보내지 않는다 |
| 어시스턴트 플러그인 | 안 나간다. 로컬 파일과 로컬 Desktop CLI만 사용. 단, 함께 묶인 호스팅 App Server로 그린 것은 위 첫 줄과 같다 |

README는 어느 구성 요소도 클라우드 래스터라이저(`convert.diagrams.net`)를 호출하지 않는다고 명시합니다. 다만 뷰어 코드는 기본적으로 `app.diagrams.net`과 `viewer.diagrams.net`에서 받아오므로, 이를 막으려면 App Server는 `VIEWER_PATH`, Tool Server는 `DRAWIO_BASE_URL`로 설정하라고 안내합니다.

## lgazo/drawio-mcp-server (커뮤니티)

출처: [저장소 README](https://github.com/lgazo/drawio-mcp-server), [TOOLS.md](https://github.com/lgazo/drawio-mcp-server/blob/main/TOOLS.md), npm 레지스트리

README 첫 줄은 "Let's do some Vibe Diagramming"입니다. 공식 서버와 달리 LLM이 XML을 통째로 쓰는 방식이 아닙니다. 서버가 실행 중인 draw.io에 연결되어, 도구 호출로 셀을 추가하고 고치고 지웁니다.

연결 방식은 둘입니다.

- `--editor` 플래그: 서버가 draw.io를 직접 호스팅하고 `http://localhost:3000/`에서 연다.
- 브라우저 확장: [Chrome](https://chrome.google.com/webstore/detail/drawio-mcp-extension/okdbbjbbccdhhfaefmcmekalmmdjjide)과 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/drawio-mcp-extension/)에 있고, `app.diagrams.net` 탭에 연결한다.

draw.io Desktop(Electron) 연동은 실험 단계이고, Desktop의 `connect-src 'self'` CSP가 WebSocket 연결을 거부해 현재는 동작하지 않는다고 적혀 있습니다.

요구 사항은 Node.js 22 이상(README 기준, npm 패키지의 `engines`는 20 이상)입니다. Claude Code 설치는 플러그인입니다.

```text
/plugin marketplace add lgazo/drawio-mcp-server
/plugin install drawio
```

`/drawio-open`과 `/drawio-status` 슬래시 명령이 함께 옵니다. 다른 호스트는 `npx drawio-mcp-server install <host>`가 설정을 써 주고, `<host>`는 `claude-code`, `claude-desktop`, `codex`, `zed`, `opencode`, `all`입니다.

TOOLS.md의 도구를 분류별로 옮긴 것입니다.

| 분류 | 도구 |
|---|---|
| 문서 | `list-documents`, `set-document-title`, `save-document` |
| 조회 | `get-selected-cell`, `list-paged-model`, `get-shape-categories`, `get-shapes-in-category`, `get-shape-by-name` |
| 수정 | `add-rectangle`, `add-edge`, `add-cell-of-shape`, `edit-cell`, `edit-edge`, `set-cell-shape`, `set-cell-data`, `set-cell-parent`, `delete-cell-by-id` |
| 페이지 | `list-pages`, `get-current-page`, `create-page`, `copy-page`, `rename-page` |
| 레이어 | `list-layers`, `create-layer`, `set-active-layer`, `get-active-layer`, `move-cell-to-layer` |
| 입출력 | `export-diagram`(SVG/PNG/XML, `embed_xml` 옵션), `import-diagram`(XML, XML 임베드 SVG/PNG), `import-mermaid` |

TOOLS.md에 적힌 규칙입니다.

- 연결된 draw.io 탭이 하나면 `target_document`를 생략할 수 있고, 여럿이면 모든 호출에 `list-documents`가 준 id를 넣어야 한다.
- 페이지 단위 도구는 `target_page`로 `{ "index": 0 }` 또는 `{ "id": "..." }` 중 하나를 요구한다.
- 호출은 연결된 문서별로 FIFO 직렬화되어 여러 에이전트가 같은 탭의 다른 페이지를 동시에 다룰 수 있다.
- 서버는 열려 있는 탭에만 신호를 보내며, 파일을 열거나 탭을 열거나 전환하지 않는다.
- 벤더 도형(AWS `mxgraph.aws4.*`, GCP `mxgraph.gcp2.*`, Azure `mxgraph.azure2.*`, Cisco19, CiscoSafe)은 에디터 사이드바에서 런타임에 발견한다.

TOOLS.md에 자동 레이아웃 도구는 없습니다. `add-rectangle`과 `add-cell-of-shape`는 `x`, `y`, `width`, `height`를 파라미터로 받습니다. `import-mermaid`는 draw.io 에디터에 내장된 Mermaid 파이프라인으로 변환한다고 적혀 있습니다.

## 그 밖의 서버

### simonkurtz-MSFT/drawio-mcp-server

README에 lgazo/drawio-mcp-server의 포크라고 적혀 있습니다. 원본이 브라우저 확장으로 살아 있는 draw.io에 연결하는 것과 달리, 이 포크는 브라우저나 draw.io 인스턴스 없이 메모리에서 XML을 생성합니다. Deno 2.3 이상이 필요하고, stdio와 streamable HTTP 전송을 지원하며, Docker Hub에 distroless 이미지가 있습니다. 700개 이상의 Azure 아키텍처 아이콘을 base64 SVG로 내장합니다. 도구는 `add-cells`, `edit-cells` 등 배열을 받는 배치 형태이고, README는 "Stateless contract"라며 매 호출에 전체 `diagram_xml`을 넘기고 반환값을 다음 호출에 넘기라고 안내합니다. PNG/SVG/PDF 변환은 지원하지 않고 jgraph 쪽 CLI를 쓰라고 적혀 있습니다.

### yohasacura/drawio-mcp

Python 3.11 이상, PyPI `drawio-mcp`, `uvx drawio-mcp`로 실행합니다. `.drawio` XML 파일을 생성하는 서버입니다. 도구는 `diagram`, `draw`, `style`, `layout`, `inspect` 다섯 개이고 각각 `action` 파라미터로 세부 동작을 고릅니다. `layout` 도구에 `sugiyama`, `tree`, `grid`, `flowchart`, `polish`, `reroute_edges`, `resolve_overlaps` 등 자동 배치 액션이 있습니다. README가 지원 클라이언트로 적은 것은 GitHub Copilot 에이전트 모드, Claude Desktop, Cursor입니다.

### techdeveloper-org/mcp-drawio-diagram

Python으로 UML 12종(class, sequence, activity, state, component, package, deployment, usecase, object, communication, composite, interaction)의 `.drawio` 파일을 생성합니다. 프로젝트 소스를 AST와 호출 그래프로 분석해 그린다고 적혀 있고, 같은 조직의 `claude-workflow-engine` 저장소를 `PYTHONPATH`에 넣어야 동작합니다. 그 파이프라인의 13개 MCP 서버 중 하나입니다.

## 실측에서 확인할 것

다음 글에서 직접 돌려 볼 항목입니다. 위 문서 내용이 실제로 그런지 확인하는 것이 목적입니다.

1. 공식 Tool Server를 Claude Code에 붙여 같은 프롬프트로 XML 직접 생성, `postLayout: "elk"`, Mermaid 세 가지 결과 비교
2. 공식 플러그인이 draw.io Desktop 유무에 따라 Mermaid와 XML 중 무엇을 고르는지, PNG에 XML이 임베드되는지
3. 커뮤니티 서버 `--editor` 모드에서 기존 그림에 노드를 추가시켰을 때의 좌표
4. `search_shapes`의 AWS 아이콘 검색 정확도
5. 일부러 깨뜨린 XML을 `open_drawio_xml`에 넣었을 때 수리되는지
6. 호스팅 App Server를 Claude Code 터미널에서 썼을 때 돌아오는 응답 형태

## 참고

- [draw.io MCP server 공식 문서](https://www.drawio.com/docs/manual/generate/drawio-mcp-server/)
- [jgraph/drawio-mcp](https://github.com/jgraph/drawio-mcp) — README, `mcp-tool-server/README.md`, `plugins/claude-code/README.md`, `plugins/claude-code/.mcp.json`
- [@drawio/mcp on npm](https://www.npmjs.com/package/@drawio/mcp)
- [GitHub MCP Registry: io.draw/mcp](https://github.com/mcp/io.draw/mcp)
- [lgazo/drawio-mcp-server](https://github.com/lgazo/drawio-mcp-server) — README, `TOOLS.md`
- [simonkurtz-MSFT/drawio-mcp-server](https://github.com/simonkurtz-MSFT/drawio-mcp-server) — README
- [yohasacura/drawio-mcp](https://github.com/yohasacura/drawio-mcp) — README, [PyPI](https://pypi.org/project/drawio-mcp/)
- [techdeveloper-org/mcp-drawio-diagram](https://github.com/techdeveloper-org/mcp-drawio-diagram) — README
- [MCP Apps 확장 사양](https://modelcontextprotocol.io/docs/extensions/apps)
