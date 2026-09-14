# lameld.github.io

[LaMelD](https://lameld.github.io/) — Hugo 개발 블로그 소스. 2026-09-14 디자인 리빌딩(Swiss Editorial Grid, 라이트/다크 자동).

- `content/<series>/` 연재별 글, `content/about/` 소개. `posts`·`series`는 목록 전용 섹션.
- `layouts/` 커스텀 템플릿, `assets/css/main.css` 디자인 토큰과 스타일. 결정 근거는 `DESIGN.md`.
- `master`에 push하면 GitHub Actions(`.github/workflows/hugo.yml`)가 Hugo 0.166으로 빌드해 Pages에 배포한다.
- 로컬 미리보기: `hugo server`. 새 글: `hugo new <series>/<slug>/index.md`.
