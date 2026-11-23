---
title: "AI 시대, git push 앞뒤에서 바뀌는 것들: Scion·Pando·Aspen·Delta를 원 출처로 읽기"
date: 2026-11-23T12:00:00+09:00
weight: 7
tags: [git, github, coding-agent, monorepo, merge-queue]
description: "코딩 에이전트가 늘면서 Git의 서버·병합·리뷰가 바뀌고 있다. Anthropic Scion, OpenAI Pando, GitHub·Cursor·Entire·GitLab·Aspen·Zed Delta를 다룬 GeekNews 아티클을 원 출처 9곳과 대조했다. 초당 푸시 수치의 측정 조건, 아직 프리뷰인 것들, 세션 기록이 Git 원격에 올라가는 문제까지 정리했다."
---

> 이 글은 GeekNews가 직접 쓴 아티클 [「AI 시대, git push 앞뒤에서 바뀌는 것들」](https://news.hada.io/article/git-push-in-the-agent-era)(2026-11-12, GeekNews Weekly 202646)을 읽고 정리한 글이다. 아티클은 원 발표문 대신 GeekNews 토픽을 링크한다. 그래서 사례 8건과 병합 큐 설명을 **원 발표문과 공식 문서**까지 따라가 대조했다. 날짜는 원 발표문 기준이다. Scion 출처 논란은 [10-16](/ai-briefing/2026-10-16/)·[10-17 브리핑](/ai-briefing/2026-10-17/), Pando는 [11-04 브리핑](/ai-briefing/2026-11-04/)에서 짧게 다뤘다. 2026-11-23에 확인했다.

## 한눈에 보기

- **"Git을 쓴다"는 세 층이다.** 클라이언트·프로토콜(`clone`·`push`), 서버·저장소(객체 보관, 브랜치 갱신), 협업 흐름(리뷰, CI, main 병합). 회사마다 바꾼 층이 다르다.
- **대부분은 명령을 그대로 두고 서버를 다시 만든다.** OpenAI Pando, GitHub, Cursor, Entire, GitLab, Aspen이 여기에 든다. `git clone`과 `git push`는 그대로다.
- **Anthropic만 Git 자체를 바꿨다.** 사내 모노레포를 Scion으로 옮겼다. 다만 Anthropic도 대부분의 저장소에는 Git 설계가 맞다고 썼다.
- **푸시가 빨라져도 main 병합은 남는다.** Aspen은 브랜치별 푸시가 초당 567회인데, main에 하나씩 얹으면 초당 약 4회다. 둘 다 CI는 뺀 수치다.
- **리뷰 기록이 Git 밖으로 나간다.** Zed Delta는 대화와 편집 이력을 커밋과 따로 저장한다. Entire는 세션 기록을 별도 Git ref에 담아 원격에 함께 올린다.

## 에이전트가 늘면 Git의 어디가 바빠지나

한 컴퓨터에서 에이전트 셋을 돌릴 때는 `git worktree`로 충분하다. 작업 폴더는 나뉘지만 객체 저장소는 공유한다. 문제는 작업마다 클라우드 샌드박스를 띄울 때다. 환경마다 코드를 새로 받고, 푸시마다 CI가 돌고, CI는 같은 커밋을 또 받는다.

Anthropic의 [공개 수치](https://www.anthropic.com/engineering/redesigning-source-control)(2026-10-13)가 규모를 보여 준다.

| 항목 | 값 | 초당 환산 |
|---|---|---|
| 하루 병합 | 5월 약 2,500건 → 9월 3만 건 이상 (약 12배) | 9월 기준 약 0.35건 |
| 하루 CI 체크아웃 | 2,500만 회 안팎 | 약 290회 |

초당 환산은 하루 86,400초로 나눈 값이다. 쓰기보다 **읽기**가 훨씬 많다. main 이력만 약 100GB, 압축한 작업 사본은 25GB가 넘는다. CI마다 전부 내려받으면 원문: "over 600 PB of git traffic per day"가 된다.

GitHub도 [4월 글](https://github.blog/news-insights/company-news/an-update-on-github-availability/)(2026-04-28)에서 PR 하나가 Git 저장소, 병합 가능 검사, 브랜치 보호, Actions, 검색, 알림을 건드릴 수 있다고 적었다. 원문: "we needed to design for a future that requires 30X today's scale."

## 명령은 그대로, 서버를 다시 만든다

| 사례 | 원 발표일 | 달라진 것 | 지금 상태 |
|---|---|---|---|
| [OpenAI Pando](https://openai.com/index/scaling-git-at-openai/) | 2026-11-02 | 관련 없는 브랜치는 병렬로 갱신한다. 팩파일을 여러 요청에 재사용한다 | 사내 운영(4월부터). 백엔드는 비공개 |
| [GitHub 새 저장 구조](https://github.blog/engineering/architecture-optimization/inside-githubs-new-git-storage-architecture/) | 2026-11-05 | 팩 데이터는 Azure Blob, 브랜치 참조와 팩 목록은 복제 로그 Ref-CAS | 제한된 비공개 프리뷰. 내년 초 확대 예정 |
| [Cursor Continuity](https://cursor.com/blog/git-at-any-scale) | 2026-08-18 | 푸시를 S3의 WAL에 남긴다. 디스크 사본은 다시 만들 수 있는 캐시다 | Cursor의 Git 호스팅 Origin의 저장 계층 |
| [Entire](https://entire.com/news/entire-launches-distributed-git-network-for-the-agent-era) | 2026-07-08 | GitHub를 원본으로 두고, 에이전트는 가까운 지역 미러에서 받는다 | 프리뷰. 대기자 명단 |
| [GitLab 차세대 SCM](https://about.gitlab.com/blog/scaling-git-for-the-agentic-software-factory/) | 2026-10-13 | Git 프로토콜은 유지하고 서버를 재설계. 필요한 파일·이력만 요청하는 인터페이스 | 비공개 베타 |
| [Aspen](https://reasonmachines.com/blog/aspen) | 2026-11-05 | Pando 설계를 독립적으로 구현한 오픈소스 서버(Rust, Apache-2.0) | pre-alpha |

**Pando가 내준 것.** 여러 ref를 함께 읽을 때 저장소 전체의 한 시점을 보장하지 않는다. 원문: "cannot assume those reads describe a single point in time". 보장하는 것은 ref 하나하나의 순서다. 여러 브랜치를 같이 읽는 도구라면 이 차이를 알아야 한다. 대신 읽기의 99% 이상을 로컬 디스크 캐시에서 내주고, 푸시 지연 중앙값은 약 80% 줄었다고 한다(자사 보고).

**GitLab의 다음 병목.** GitLab은 소스 관리가 첫 병목일 뿐이라고 했다. 그다음은 검증(CI), 보안 검토, 배포다.

### 초당 푸시 수치는 나란히 놓으면 안 된다

발표마다 초당 푸시 수를 내세우지만 조건이 다 다르다. 모두 자사 측정이다.

| 출처 | 초당 푸시 | 조건 |
|---|---|---|
| GitHub | 7,200 | **내부 테스트.** 동시 에이전트 5,000개, 저장소 하나. 지금 가장 바쁜 저장소도 실제로는 원문: "at most dozens of writes in a single second" |
| Entire | 586 | 모의 에이전트 128개, 각자 다른 브랜치. **미러로 동기화하지 않는** 직접 경로(`entire/unmirrored/`) |
| Aspen | 567 | 8코어 서버 1대, S3 Standard, `git.git`(파일 약 4,500개), 브랜치 전용 URL, 5분 |
| Cursor | 120 / 300 초과 | 합성 테스트. S3 Standard / S3 Express One Zone |
| OpenAI Pando | 공개 안 함 | 1,000 클라이언트 비교는 원문: "Synthetic teaching models, not production benchmarks." |

- 아티클은 GitHub의 새 저장 구조가 초당 7,200회 푸시를 처리한다고 소개한다. 운영 트래픽이 아니라 내부 테스트 결과다.
- Aspen README는 자기 수치에 더 강한 단서를 달았다. 원문: "treat both as indicative, not as performance claims." 데이터에 대해서는 원문: "Do not put data you cannot afford to lose in it yet."

## Anthropic은 왜 Git 자체를 바꿨나

Anthropic은 얕은 복제, sparse checkout, 사내 미러 수백 대로 버텼다. 원문: "We've used all of them". 병합이 빨라지자 미러 운영이 어려워졌고, 미리 만들어 둔 사본은 main 끝에서 점점 뒤처졌다.

Scion의 출발점은 **클라이언트가 늘 온라인**이라는 가정이다. 원문: "nearly all of our clients were agents and jobs that are online by definition".

- 클라이언트는 필요한 데이터를 필요할 때 받는다.
- 커밋·리베이스·병합이 모두 서버에서 일어난다. 원문: "so there's nothing to push"
- 클라이언트(`scn`)와 프로토콜이 Git과 다르다. 코드 리뷰(Antique), 코드 검색, 권한 관리도 새로 만들었다.
- 7명이 Scion과 Antique를 만들고 모노레포를 옮기는 데 5개월이 걸렸다. CI에서만 원문: "a 25% CPU capacity win"이 났다고 한다(자사 측정).

내려놓은 것은 Git의 분산·오프라인 성격이다. Scion에서 Git으로 내보내거나 미러링하는 방법은 발표 글에 없다. 공개 계획도 없어 보인다. 원문: "it's difficult to offer Scion in a form that would be broadly useful to other organizations". 일반 용도로는 맞지 않는다고 스스로 못 박았다. 원문: "Scion is by no means a suitable replacement for git for general use."

**아티클에 없는 이야기.** 발표 직후 Meta Mononoke 개발자가 설계 출처를 문제 삼았다. Anthropic은 10월 15일에 각주를 달아 Sapling의 서버 Mononoke를 밝혔다. 원문: "It pioneered many of the techniques we use in Scion, in particular the Bonsai data model."

## 푸시가 빨라져도 main 병합은 남는다

에이전트 열 개가 **각자 브랜치에** 푸시하는 일은 나눠서 처리할 수 있다. 열 개를 **main에** 얹는 일은 다르다. 다음 변경이 직전 결과에 기대기 때문에 줄을 선다. Aspen의 567회와 약 4회가 그 차이다. 4회는 부하가 아니라 왕복 횟수에 막힌 값이다. 원문: "limited by round trips rather than load."

합쳐지는 것과 동작하는 것도 다르다. 한 에이전트가 API 응답 형식을 바꾸고, 다른 에이전트가 옛 형식에 맞춰 화면을 고쳤다고 하자. 파일이 다르면 Git은 충돌 없이 합친다. 깨진 것은 합친 뒤에 돌려 봐야 안다.

**GitHub 병합 큐**([문서](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-a-merge-queue))가 이 검증을 맡는다.

- 최신 base + 큐 앞쪽 PR의 변경 + 이번 PR을 합친 임시 브랜치를 만들고, 필수 검사를 통과해야 병합한다.
- **묶어서 병합해도 CI는 줄지 않는다.** 원문: "Merge limits do not combine merge_group builds." merge limits는 한 번에 병합할 PR 수이고, 동시에 도는 CI 수는 build concurrency로 조절한다.
- 맨 앞으로 끼어들면 진행 중이던 PR이 모두 다시 빌드된다.

**Aspen의 병합 큐**는 PR마다 커밋을 남기면서 main은 한 번만 갱신한다. 기본은 꺼져 있고 처리량은 아직 측정하지 않았다. 원문: "we'll publish its number once it's measured."

## 리뷰 기록이 커밋 밖으로 나간다

에이전트와 일하면 제약 조건, 버린 시도 같은 기록이 PR을 열기 전에 쌓인다. 이것을 어디에 두는지가 갈린다.

| | Zed Delta | Entire CLI |
|---|---|---|
| 무엇 | 동료를 에이전트 스레드에 초대해 코드와 대화를 함께 리뷰 | 커밋마다 프롬프트·대화 기록·수정 파일을 체크포인트로 남김 |
| 저장 위치 | 커밋과 따로 **DeltaDB**에. 내 기기와 Delta 서버 | 내 Git 저장소의 **별도 ref** `refs/entire/checkpoints/…` |
| Git과의 관계 | 푸시·빌드 기준은 여전히 커밋 | 커밋의 `Entire-Checkpoint` 트레일러로 연결 |
| 저장소를 옮기면 | 따라오지 않는다 | 해당 ref까지 옮겨야 따라온다 |
| 상태 | 공개 베타(2026-09-16), 베타 동안 무료 | 오픈소스(MIT) |

- **Delta 팀은 자기 저장소에서 PR을 껐다.** 원문: "33 of us have landed 570 changes to main". [저장 문서](https://delta.dev/docs/privacy-and-security/data-storage)에 따르면 스레드에 붙은 저장소의 Git 객체(커밋과 파일 내용)도 Delta 서버에 저장된다. 로컬에서 지워도 서버 사본이 확실히 지워지지는 않는다고 적혀 있다.
- **Entire는 아티클 설명보다 한 걸음 더 나간다.** 아티클은 `main`만 푸시하면 세션 기록이 따라오지 않는다고 했다. Git 동작만 보면 맞다. 하지만 [README](https://github.com/entireio/cli)와 [저장 문서](https://docs.entire.com/guides/checkpoints/checkpoint-storage)를 보면, `entire enable`이 설치한 훅이 **코드를 푸시할 때 체크포인트도 같이 올린다.** 이것이 기본값이다.
- **공개 저장소라면 주의한다.** README 원문: "Session transcripts are pushed to your Git remote". 저장소가 공개면 누구나 대화 기록을 읽을 수 있다. 비밀값 가림은 최선의 노력 수준이다. 올라갈 내용은 아래 명령으로 본다.
- **저장 방식이 바뀌었다.** CLI 0.10.0부터 ref 방식이 기본이다. 그 전에 켠 저장소는 `entire/checkpoints/v1` 브랜치 방식으로 남아 있다.

```bash
entire checkpoint list --outgoing                  # 다음 푸시에 올라갈 체크포인트
entire configure --local --skip-push-sessions      # 세션을 올리지 않기
git for-each-ref refs/entire/checkpoints/          # 저장된 체크포인트 ref 보기
```

## 우리 팀에서 지금 할 수 있는 것

아티클에는 없는, 보통 규모 팀을 위한 정리다. 새 서버 없이 Git과 GitHub 기능만 쓴다.

```bash
git worktree add ../app-issue-42 -b issue-42 origin/main   # 에이전트마다 작업 폴더 분리
git clone --filter=blob:none <url>                         # 이력은 받고 파일 내용은 필요할 때
git clone --mirror <url> && git -C <repo>.git push --mirror <new-url>   # 모든 ref까지 이전
```

- **한 컴퓨터면 worktree부터 쓴다.** 객체를 공유하므로 다시 받을 것이 없다.
- **CI가 많으면 읽기부터 줄인다.** 빌드에는 얕은 복제, 이력이 필요한 작업에는 blob 없는 partial clone을 쓴다.
- **병합이 막히면 병합 큐를 켠다.** GitHub Actions는 워크플로에 `merge_group` 이벤트가 있어야 큐에서 검사가 돈다. 외부 CI는 `gh-readonly-queue/{base_branch}` 접두사 브랜치에 트리거를 건다.
- **저장소를 옮길 때는 ref를 센다.** 일반 `clone`은 `refs/heads`와 태그만 가져온다. 에이전트 기록이 다른 ref에 있으면 `--mirror`로 옮겨야 한다. GitHub끼리 옮기면 `refs/pull/*`만 거부되는데, GitHub가 직접 관리하는 ref라 정상이다.
- 브랜치·리베이스 규칙은 [팀 단위 Git 협업 전략](/gitstudy/team-workflow/)에 정리해 뒀다.

## 원 출처와 대조한 결과

- **맞았던 것:** Anthropic의 병합·체크아웃 수치, Scion의 구조와 한계, Pando의 일관성 보장, Aspen의 567회·4회와 "CI 제외", Cursor의 S3 로그, 병합 큐가 CI를 합치지 않는다는 설명, Delta의 저장 위치.
- **좁혀 읽을 것:** GitHub 7,200회는 내부 테스트이고 아직 비공개 프리뷰다. GitLab도 비공개 베타, Aspen은 pre-alpha다. 아티클의 표에는 출시 상태 칸이 없다.
- **빠진 것:** Scion의 Mononoke 출처 논란, Entire 수치가 미러를 거치지 않은 경로라는 점, Entire의 기본 푸시 동작과 공개 저장소 노출 위험.
- **날짜:** GeekNews 토픽 날짜는 원 발표일보다 늦다. Zed Delta는 11-08로 올라왔지만 원 글은 9월 16일이다.

## 읽고 나서

- **층을 나눠 보면 유행어가 정리된다.** "Git을 대체한다"에 해당하는 것은 Scion 하나뿐이다. 나머지는 Git을 유지하려고 서버를 고치는 쪽이다.
- **보통 팀의 병목은 초당 푸시가 아니다.** 하루 병합 수십 건인 팀에게 수백 회/초는 의미가 없다. 먼저 닿는 벽은 CI 시간과 병합 검증이다. 아티클이 4절에서 한 말이 가장 실용적이다.
- **기록의 위치는 지금 정해야 한다.** 에이전트 대화에는 내부 경로, 고객 이름, 실수로 붙인 토큰이 섞인다. 도구를 켜기 전에 어디에 저장되고 어디로 푸시되는지부터 확인한다.

## 참고

- GeekNews 아티클: [AI 시대, git push 앞뒤에서 바뀌는 것들](https://news.hada.io/article/git-push-in-the-agent-era)
- 원 발표: [Anthropic Scion](https://www.anthropic.com/engineering/redesigning-source-control) · [OpenAI Pando](https://openai.com/index/scaling-git-at-openai/) · [GitHub 저장 구조](https://github.blog/engineering/architecture-optimization/inside-githubs-new-git-storage-architecture/) · [GitHub 가용성](https://github.blog/news-insights/company-news/an-update-on-github-availability/) · [Cursor Continuity](https://cursor.com/blog/git-at-any-scale) · [Entire](https://entire.com/blog/an-entirely-new-git-hosting-network) · [GitLab](https://about.gitlab.com/blog/scaling-git-for-the-agentic-software-factory/) · [Aspen](https://reasonmachines.com/blog/aspen) · [Zed Delta](https://zed.dev/blog/delta-public-beta)
- 저장소·문서: [reason-machines/aspen](https://github.com/reason-machines/aspen) · [entireio/cli](https://github.com/entireio/cli) · [Delta 데이터 저장](https://delta.dev/docs/privacy-and-security/data-storage) · [GitHub 병합 큐](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-a-merge-queue)
