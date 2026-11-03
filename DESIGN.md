# Design — Swiss Editorial Grid

2026-09-14 리빌딩. 방향: 스위스 모더니즘 그리드, 글 읽기 우선, 단일 액센트, 라이트/다크 자동.
토큰과 규칙의 원본은 `assets/css/main.css` 상단 `:root` 블록이다. 이 문서는 그 결정의 이유를 남긴다.

## Tokens

| 토큰 | Light | Dark | 용도 |
|---|---|---|---|
| `--bg` / `--fg` | #FAFAFA / #09090B | #0F172A / #F1F5F9 | 바탕 / 본문 글자 |
| `--card` | #FFFFFF | #1B2336 | 시리즈 카드 |
| `--muted` / `--muted-fg` | #EEF1F4 / #52606D | #1E293B / #94A3B8 | 보조 배경 / 보조 글자 (대비 6:1 이상) |
| `--border` | #E4E4E7 | #283449 | 헤어라인 |
| `--accent` | #15803D | #4ADE80 | 링크 hover, 활성 메뉴, 번호, 포커스 링 (대비 5:1 / 10:1) |
| `--code-bg` | #F3F4F6 | #0D1117 | 인라인 코드. 코드 블록은 Chroma github / github-dark |

테마 전환: `<html data-theme="light|dark">`. `head.html`의 인라인 스크립트가 첫 페인트 전에
localStorage → `prefers-color-scheme` 순으로 결정한다. JS가 없으면 라이트.

## Typography

- 본문: Pretendard Variable (jsDelivr CDN), 16px, line-height 1.7, 글 본문은 17px / 1.8. `word-break: keep-all`.
- 라벨·날짜·코드: JetBrains Mono (Google Fonts). `.label`은 12px 대문자 자간 0.12em — 섹션 제목("Latest", "Series", "On this page")에 쓴다.
- 제목: 자간 -0.02em, 홈 h1은 -0.04em.

## Layout

- 컨테이너 최대 72rem, 좌우 여백 `clamp(1rem, 4vw, 2rem)`. 본문 최대 폭 42rem.
- 홈: 7:5 비대칭 그리드(이름 / 메타 표). 768px 미만은 1열.
- 헤더: 상단 고정(sticky). 높이는 `--header-h`(4rem + 테두리 1px)이고 목차 `top`과 앵커 `scroll-padding-top`이 이 값을 쓴다. 헤더 높이를 바꾸면 토큰도 같이 바꾼다.
- 글: 1024px 이상에서 본문 + 14rem 목차 사이드바(sticky). 목차는 `## `~`###` 헤딩이 있을 때만.
  - 지금 읽는 항목은 목차 규칙선 위 2px 액센트 막대와 `--fg` 글자로 표시한다(`aria-current="location"`).
  - 목차가 화면을 넘치면 지금 읽는 장의 하위 항목만 펼치고, 접힌 장은 제목 옆에 항목 수를 mono로 붙인다. 하위 항목은 두 줄까지, 전체 제목은 `title`.
  - 그래도 넘치면 목록만 스크롤한다. 스크롤바 대신 항목이 더 있는 쪽 가장자리를 흐린다(스크롤 연동 애니메이션 미지원·reduced-motion이면 얇은 스크롤바).
  - 스크립트는 `single.html` 인라인. 스크립트가 없으면 접지 않고 전체 목록이 스크롤된다.
- 글 목록은 카드가 아니라 헤어라인 행: 날짜(mono) · 제목 · 시리즈. 시리즈 안에서는 번호를 붙인다.
- 스위스 규칙선: 섹션 헤드와 연도 라벨 아래 1px `--fg` 선, 나머지는 `--border`.

## Motion

hover 색·테두리 150ms만. 스크롤 애니메이션 없음. 예외는 목차가 읽는 위치를 따라갈 때의 목차 내부 스크롤 하나다. `prefers-reduced-motion`이면 전환도 끄고 목차도 즉시 이동한다.

## Content model

- 시리즈 = Hugo 섹션(`content/<name>/_index.md`). `description`이 카드 설명, `cascade.tags`가 하위 글 태그 기본값.
- 글 순서: 시리즈 안에서는 `weight` 오름차순(장 순서), Posts·홈 Latest는 날짜 내림차순.
- `posts`, `series`, `about`은 유틸리티 섹션(`params.utilitySections`)이라 목록·카드에서 제외한다.
- 새 글: `hugo new <series>/<slug>/index.md` → archetype이 `tags: []`를 넣어 준다.

## Templates

`_default/single.html`(글), `list.html`(시리즈·태그 목록), `posts.html`, `series.html`, `terms.html`(태그 색인),
`plain.html`(About 같은 정적 페이지 — `layout: plain`). Hugo 0.146+에서는 `page.html`이 단일 글의
정식 템플릿 이름이라 정적 페이지 레이아웃에 그 이름을 쓰면 안 된다.
