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
- 글: 1024px 이상에서 본문 + 14rem 목차 사이드바(sticky). 목차는 `## `~`###` 헤딩이 있을 때만.
- 글 목록은 카드가 아니라 헤어라인 행: 날짜(mono) · 제목 · 시리즈. 시리즈 안에서는 번호를 붙인다.
- 스위스 규칙선: 섹션 헤드와 연도 라벨 아래 1px `--fg` 선, 나머지는 `--border`.

## Motion

hover 색·테두리 150ms만. 스크롤 애니메이션 없음. `prefers-reduced-motion`이면 전환도 끈다.

## Content model

- 시리즈 = Hugo 섹션(`content/<name>/_index.md`). `description`이 카드 설명, `cascade.tags`가 하위 글 태그 기본값.
- 글 순서: 시리즈 안에서는 `weight` 오름차순(장 순서), Posts·홈 Latest는 날짜 내림차순.
- `posts`, `series`, `about`은 유틸리티 섹션(`params.utilitySections`)이라 목록·카드에서 제외한다.
- 새 글: `hugo new <series>/<slug>/index.md` → archetype이 `tags: []`를 넣어 준다.

## Templates

`_default/single.html`(글), `list.html`(시리즈·태그 목록), `posts.html`, `series.html`, `terms.html`(태그 색인),
`plain.html`(About 같은 정적 페이지 — `layout: plain`). Hugo 0.146+에서는 `page.html`이 단일 글의
정식 템플릿 이름이라 정적 페이지 레이아웃에 그 이름을 쓰면 안 된다.
