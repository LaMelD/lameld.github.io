---
title: "draw.io MCP: 파일을 써 주는 공식 서버와 에디터를 조종하는 커뮤니티 서버"
date: 2026-10-16
weight: 9
tags: [claude-code, drawio, mcp, diagram]
description: "draw.io를 AI 에이전트에 붙이는 MCP 서버는 크게 둘이다. JGraph 공식 @drawio/mcp는 모델이 쓴 XML·Mermaid를 draw.io로 열어 주고, 커뮤니티 lgazo/drawio-mcp-server는 열려 있는 에디터의 셀을 하나씩 읽고 고친다. 문서와 저장소를 읽고 정리한 조사 글이며 실측은 다음 글에서 한다."
---

> 이 글은 공식 문서, README, npm 레지스트리, GitHub 메타데이터를 읽고 정리한 **조사 글**입니다. 직접 설치해 돌려 보지 않았으므로 "문서에 그렇게 적혀 있다" 수준입니다. 설치와 실측은 다음 글에서 합니다. 수치는 2026-10-16 기준입니다.

[artcraft 글](/claude-code/artcraft/)에서 "숫자와 규칙으로 다 적을 수 있는 것은 에이전트가 처음부터 만들 수 있다"고 썼습니다. 다이어그램이 정확히 그런 산출물입니다. 노드와 엣지와 좌표는 전부 텍스트로 적히고, draw.io 파일은 XML입니다. 그래서 draw.io MCP는 artcraft와 달리 에이전트가 **내용까지** 만드는 도구입니다.

찾아보니 "draw.io MCP"라는 이름 아래 설계가 전혀 다른 두 계열이 있습니다.

| | 공식 `jgraph/drawio-mcp` | 커뮤니티 `lgazo/drawio-mcp-server` |
|---|---|---|
| 만든 곳 | JGraph(draw.io 개발사) | 개인 메인테이너 lgazo |
| 첫 공개 | 2026-02 | 2025-04 |
| 최신 버전 | 1.7.0 (2026-10-13) | 2.3.0 (2026-09-15) |
| 라이선스 | Apache-2.0 | MIT |
| GitHub 스타 | 약 5,600 | 약 1,500 |
| Node 요구 | 18 이상 | 22 이상 |
| 기본 발상 | 모델이 다이어그램 **전체를 텍스트로 쓰고** 서버가 draw.io로 열어 준다 | 서버가 **살아 있는 에디터**에 붙어 셀 단위로 읽고 쓰고 지운다 |
| 입력 | draw.io XML, Mermaid, CSV | 도구 호출(add-rectangle, add-edge, edit-cell…) |
| 결과물 | `.drawio` 파일, 브라우저 탭, 채팅 안 인라인 뷰어 | 브라우저 탭의 에디터 상태, 내보낸 SVG/PNG/XML |

공식 쪽은 "문서 생성기"이고 커뮤니티 쪽은 "에디터 리모컨"입니다. 같은 이름을 달고 있지만 쓰는 장면이 다릅니다.

## 공식: jgraph/drawio-mcp

저장소: [github.com/jgraph/drawio-mcp](https://github.com/jgraph/drawio-mcp) · 문서: [drawio.com/docs/manual/generate/drawio-mcp-server](https://www.drawio.com/docs/manual/generate/drawio-mcp-server/) · npm: [@drawio/mcp](https://www.npmjs.com/package/@drawio/mcp)

저장소 하나에 네 가지 연동 방식이 들어 있습니다. README가 직접 비교표를 제공합니다.

| | MCP App Server | MCP Tool Server | 어시스턴트 플러그인 | 프로젝트 지침 |
|---|---|---|---|---|
| 동작 | 채팅 안에 인라인 렌더링 | 브라우저에서 draw.io를 연다 | `.drawio` 파일 생성, PNG/SVG/PDF 내보내기 | Claude가 Python으로 URL 생성 |
| 설치 | 없음 (`mcp.draw.io` 호스팅) | npm 패키지 | 플러그인 한 줄 | 없음, 지침 붙여넣기 |
| 입력 형식 | XML, Mermaid | XML, CSV, Mermaid | XML, Mermaid(Desktop 필요) | XML, CSV, Mermaid |
| 대상 | Claude.ai, ChatGPT, VS Code, Cursor, Copilot CLI | Claude Desktop, Cursor, OpenCode 등 MCP 클라이언트 | Claude Code, Codex CLI, Copilot CLI, Cursor, OpenCode | Claude.ai Projects |

### MCP App Server: 채팅 안에서 바로 보기

MCP Apps 확장을 쓰는 원격 HTTP 서버입니다. `https://mcp.draw.io/mcp`를 원격 MCP로 등록하면 끝이고, Claude.ai에서는 [커넥터 디렉터리](https://claude.ai/directory/draw-io)에서 한 번에 붙입니다. 도구는 셋입니다.

- `create_diagram`: draw.io XML 또는 Mermaid(28종)를 받아 대화 안에 인터랙티브 iframe으로 렌더링
- `search_shapes`: AWS, Azure, GCP, Cisco, Kubernetes, UML, BPMN 등 라이브러리의 1만여 도형을 키워드로 검색해 XML에 바로 넣을 스타일 문자열을 반환. 내장 라이브러리에 없으면 draw.io 아이콘 서비스에서 브랜드 로고와 개념 아이콘을 보충
- `get_drawio_reference`: XML 또는 Mermaid 생성 규칙 문서를 반환. 모델이 형식마다 한 번 읽고 시작

터미널 호스트처럼 인라인 렌더링이 안 되는 곳에서는 draw.io로 여는 링크를 돌려줍니다. Docker 이미지와 Cloudflare Workers 셀프 호스팅도 지원합니다.

### MCP Tool Server: 로컬 stdio 서버

원래의 MCP 서버이고 `npx @drawio/mcp`로 뜹니다. Claude Code 등록은 한 줄입니다.

```sh
claude mcp add drawio -- npx -y @drawio/mcp
```

도구 목록입니다.

| 도구 | 하는 일 |
|---|---|
| `open_drawio_xml` | XML을 draw.io 에디터로 연다. `lightbox`(읽기 전용), `dark`, `postLayout: "elk"`, `routing: "libavoid"` 옵션 |
| `open_drawio_csv` | CSV를 조직도나 흐름도로 변환해 연다 |
| `open_drawio_mermaid` | Mermaid를 편집 가능한 draw.io 다이어그램으로 연다 |
| `get_drawio_reference` | XML 또는 Mermaid 참조 문서 반환 |
| `search_shapes` | App Server와 같은 도형 검색 |
| `list_pages` / `get_page` / `set_page` | 로컬 다중 페이지 `.drawio` 파일을 페이지 단위로 읽고 교체 |

동작 원리는 단순합니다. 내용을 deflateRaw로 압축해 base64로 만들고, `https://app.diagrams.net/#R...` 형태의 URL을 돌려줍니다. 다이어그램은 URL 프래그먼트에 실리므로 서버로 전송되지 않습니다.

README에서 눈에 띄는 대목이 하나 있습니다. `open_drawio_xml`은 받은 XML을 먼저 **수리**합니다. 앞뒤에 섞인 코드 펜스나 도구 호출 태그를 떼고, 속성값의 `<`와 `&`를 이스케이프하고, 레이어가 자기 자신을 부모로 쓴 경우를 고칩니다. 테스트에서 LLM이 쓴 다이어그램의 3%가 이런 실수로 깨졌다고 적혀 있습니다. 그다음 정규화를 합니다. 엣지를 양 끝 셀의 공통 조상에 넣고, 지오메트리 없는 엣지에 지오메트리를 붙이고, 자식을 잘라 먹는 컨테이너를 키웁니다. draw.io Desktop CLI의 `--normalize`와 같은 처리입니다. 모델이 쓰는 XML이 완벽하지 않다는 전제를 서버가 흡수하는 설계입니다.

### 레이아웃: ELK와 libavoid

모델이 좌표를 손으로 찍지 않아도 되게 두 가지 후처리를 제공합니다.

| 패스 | 하는 일 | App Server | Tool Server | 플러그인 |
|---|---|---|---|---|
| ELK (`postLayout: "elk"`) | 노드를 계층 레이아웃으로 재배치하고 엣지도 다시 그림 | ✅ | — | Desktop CLI `--layout` |
| libavoid (`routing: "libavoid"`) | 노드 위치는 두고 커넥터만 도형을 피해 직교 라우팅 | ✅ | 1.3.0부터 | Desktop CLI `--layout libavoid` |

둘은 양자택일입니다. ELK가 엣지까지 그리므로 둘을 겹치면 중복입니다. Mermaid는 이미 자동 배치라 해당 없습니다. artcraft 글에서 좌표를 눈대중으로 찍다가 망한 경험을 생각하면, 이 후처리가 있고 없고가 결과물 품질을 가를 가능성이 큽니다. 실측에서 가장 먼저 확인할 항목입니다.

### Claude Code 플러그인

```text
/plugin marketplace add jgraph/drawio-mcp
/plugin install drawio@drawio
```

플러그인은 MCP 서버가 아니라 **스킬**입니다. `/drawio:drawio`로 부르거나 "로그인 흐름도 그려 줘"라고만 해도 트리거됩니다. 동작은 이렇습니다.

1. 표준 다이어그램(흐름도, 시퀀스, 클래스, 상태, ER, 간트, 마인드맵)이고 draw.io Desktop이 깔려 있으면 Mermaid로 쓰고 Desktop CLI로 변환. 커스텀 스타일, 정밀 배치, 특정 도형 라이브러리가 필요하거나 Desktop이 없으면 XML을 직접 쓴다.
2. `.drawio` 파일을 만든다. XML이면 선택적으로 ELK `--layout`을 돌린다.
3. 요청에 `png`, `svg`, `pdf`가 있으면 Desktop CLI로 내보내되 XML을 임베드해 draw.io에서 다시 열 수 있게 한다. `url`이면 Node 내장 `zlib`으로 압축해 `app.diagrams.net`을 브라우저에서 연다.

```text
/drawio:drawio png flowchart for user login   → login-flow.drawio.png
/drawio:drawio url architecture overview       → 브라우저에서 열고 .drawio는 로컬에 보관
```

1.2.3부터는 플러그인이 호스팅 App Server(`https://mcp.draw.io/mcp`)도 `.mcp.json`에 같이 묶습니다. Claude.ai처럼 앱을 보여 주는 호스트에서는 인라인 렌더링이 되고, 터미널에서는 그대로 파일을 씁니다. 전부 로컬에 두고 싶으면 `/mcp`에서 그 서버만 끄면 됩니다.

### 데이터가 어디로 가는가

README에 방식별로 표가 있습니다. 요약하면 호스팅 App Server만 다이어그램이 `mcp.draw.io`로 전송되고, 나머지는 로컬에 머뭅니다. Tool Server는 URL 프래그먼트에 실리므로 브라우저가 서버로 보내지 않고, 플러그인은 로컬 파일과 로컬 Desktop CLI만 씁니다. 클라우드 래스터라이저(`convert.diagrams.net`)는 어디서도 호출하지 않는다고 명시되어 있습니다. 다만 뷰어 **코드**는 `app.diagrams.net`과 `viewer.diagrams.net`에서 받아오므로 완전 오프라인은 `VIEWER_PATH`나 `DRAWIO_BASE_URL`로 따로 막아야 합니다.

## 커뮤니티: lgazo/drawio-mcp-server

저장소: [github.com/lgazo/drawio-mcp-server](https://github.com/lgazo/drawio-mcp-server) · npm: [drawio-mcp-server](https://www.npmjs.com/package/drawio-mcp-server)

공식 서버보다 열 달 먼저 나왔고 "Vibe Diagramming"을 표방합니다. 설계가 반대입니다. 모델이 XML을 통째로 쓰는 게 아니라, 열려 있는 draw.io 탭에 WebSocket으로 붙어 **셀 하나하나를 조작**합니다.

연결 방식은 둘입니다.

- `--editor` 플래그로 서버가 draw.io를 직접 호스팅. `http://localhost:3000/`을 열면 된다.
- 브라우저 확장([Chrome](https://chrome.google.com/webstore/detail/drawio-mcp-extension/okdbbjbbccdhhfaefmcmekalmmdjjide), [Firefox](https://addons.mozilla.org/en-US/firefox/addon/drawio-mcp-extension/))으로 `app.diagrams.net` 탭에 연결.

draw.io Desktop 연동은 실험 단계이고, Desktop의 `connect-src 'self'` CSP가 WebSocket을 막아 현재는 동작하지 않는다고 적혀 있습니다.

```text
/plugin marketplace add lgazo/drawio-mcp-server
/plugin install drawio
```

또는 `npx drawio-mcp-server install claude-code`가 설정을 써 줍니다. `claude-desktop`, `codex`, `zed`, `opencode`, `all`도 받습니다.

도구는 공식 서버보다 훨씬 잘게 쪼개져 있습니다.

| 분류 | 도구 |
|---|---|
| 문서 | `list-documents`, `set-document-title`, `save-document` |
| 조회 | `get-selected-cell`, `list-paged-model`, `get-shape-categories`, `get-shapes-in-category`, `get-shape-by-name` |
| 수정 | `add-rectangle`, `add-edge`, `add-cell-of-shape`, `edit-cell`, `edit-edge`, `set-cell-shape`, `set-cell-data`, `set-cell-parent`, `delete-cell-by-id` |
| 페이지 | `list-pages`, `get-current-page`, `create-page`, `copy-page`, `rename-page` |
| 레이어 | `list-layers`, `create-layer`, `set-active-layer`, `get-active-layer`, `move-cell-to-layer` |
| 입출력 | `export-diagram`(SVG/PNG/XML, XML 임베드 옵션), `import-diagram`, `import-mermaid` |

artcraft의 photocraft 도구와 같은 모양입니다. 명령 id와 JSON 파라미터로 에디터를 원격 조종하고, 상태를 다시 읽어 확인합니다.

설계에서 눈에 띄는 점이 둘 있습니다. 첫째, 탭이 여러 개면 모든 호출에 `target_document`를, 페이지 단위 도구에는 `target_page`를 요구합니다. 둘째, 문서별로 호출을 FIFO로 직렬화합니다. README의 예시 프롬프트가 "에이전트 셋을 병렬로 띄워 각자 다른 페이지를 맡겨라"인 걸 보면 멀티 에이전트 작업을 염두에 둔 설계입니다. 벤더 도형(AWS, GCP, Azure, Cisco)은 에디터 사이드바에서 런타임에 발견하므로 `mxgraph.gcp2.cloud_run` 같은 이름을 바로 쓸 수 있다고 합니다.

## 두 설계가 갈리는 지점

공식 서버의 모델은 "모델이 한 번에 전체를 쓴다"입니다. 장점은 컨텍스트에 다이어그램이 통째로 들어 있어 일관성이 좋고, 레이아웃 후처리가 좌표 문제를 받아 준다는 것입니다. 약점은 수정입니다. 노드 하나를 옮기려면 XML 전체를 다시 쓰거나 `get_page`로 읽어 `set_page`로 교체해야 합니다.

커뮤니티 서버의 모델은 "에디터를 조금씩 고친다"입니다. 장점은 사람이 draw.io에서 그린 그림에 에이전트가 이어서 작업할 수 있고, 수정이 셀 단위라 국소적이라는 것입니다. 약점은 좌표입니다. `add-rectangle`에 `x`, `y`를 넘기는 건 모델 몫이고, ELK 같은 자동 배치 도구가 목록에 없습니다. artcraft에서 타원 좌표를 찍던 문제가 여기서 그대로 나올 수 있습니다.

추정하자면 이렇게 나뉠 것 같습니다.

- 처음부터 새로 그리는 아키텍처 도식, 흐름도, 시퀀스: 공식 서버 + ELK
- 사람이 그려 둔 도면에 노드를 추가하거나 라벨을 고치는 일: 커뮤니티 서버
- 코드베이스를 읽고 `.drawio` 파일을 저장소에 커밋하는 일: 공식 플러그인

이 추정이 맞는지가 실측의 핵심입니다.

## 그 밖의 서버

검색에 같이 잡힌 것들입니다. 전부 읽어 보지는 않았고 README 요약만 적습니다.

- [simonkurtz-MSFT/drawio-mcp-server](https://github.com/simonkurtz-MSFT/drawio-mcp-server): Deno 기반, XML을 직접 생성. 브라우저 확장도 draw.io 인스턴스도 필요 없음
- [yohasacura/drawio-mcp](https://github.com/yohasacura/drawio-mcp): Python, PyPI `drawio-mcp`. 자동 레이아웃과 다중 페이지 지원
- [techdeveloper-org/mcp-drawio-diagram](https://github.com/techdeveloper-org/mcp-drawio-diagram): UML과 SDLC 다이어그램 12종에 특화

npm의 `drawio-mcp`(mxgraph 기반)와 PyPI의 `drawio-mcp`는 이름이 같지만 다른 프로젝트입니다. 공식 패키지는 스코프가 붙은 `@drawio/mcp`이고, 커뮤니티 lgazo 쪽은 `drawio-mcp-server`입니다. 설치할 때 이름을 정확히 봐야 합니다.

## 실측에서 확인할 것

다음 글에서 돌려 볼 항목입니다.

1. 공식 Tool Server를 Claude Code에 붙여 같은 프롬프트로 XML 직접 생성, `postLayout: "elk"`, Mermaid 세 가지 결과를 비교
2. 공식 플러그인이 draw.io Desktop 유무에 따라 Mermaid와 XML 중 무엇을 고르는지, PNG 내보내기에 XML이 정말 임베드되는지
3. 커뮤니티 서버 `--editor` 모드에서 사람이 그린 그림에 노드를 추가시켰을 때 좌표를 어디에 찍는지
4. `search_shapes`가 AWS 아이콘을 얼마나 정확히 찾는지, 한국어 키워드는 되는지
5. 서버가 XML을 수리한다는 3% 주장이 실제로 체감되는지. 일부러 깨진 XML을 넣어 본다
6. 호스팅 App Server를 Claude Code 터미널에서 썼을 때 링크가 어떻게 돌아오는지

## 참고

- [draw.io MCP server 공식 문서](https://www.drawio.com/docs/manual/generate/drawio-mcp-server/)
- [jgraph/drawio-mcp](https://github.com/jgraph/drawio-mcp) — README, `mcp-tool-server/README.md`, `plugins/claude-code/README.md`
- [@drawio/mcp on npm](https://www.npmjs.com/package/@drawio/mcp)
- [GitHub MCP Registry: io.draw/mcp](https://github.com/mcp/io.draw/mcp)
- [lgazo/drawio-mcp-server](https://github.com/lgazo/drawio-mcp-server) — README, `TOOLS.md`
- [MCP Apps 확장 사양](https://modelcontextprotocol.io/docs/extensions/apps)
