---
title: "2048: React Native로 만들어 Google Play에 올리는 2048"
date: 2026-09-22
weight: 5
tags: [project, react-native, expo, typescript, android, game]
description: "Expo와 순수 TypeScript로 만든 2048. 추가 의존성은 AsyncStorage 하나, 테스트는 Node 내장 러너, 빌드는 로컬 gradle. 설계부터 실기기 검증까지 하루에 끝낸 기록."
---

저장소: [github.com/LaMelD/2048](https://github.com/LaMelD/2048)

React Native(Expo)로 만든 2048이다. Google Play 출시용이고 안드로이드 단독이다. 광고 없음, 과금 없음, 네트워크 없음, 수집하는 데이터 없음. 점수와 진행 중인 판은 기기 안에만 남는다.

<p align="center"><img src="screenshot.png" width="270" alt="Galaxy Z Flip7에서 실행 중인 화면"></p>

- 4×4 격자, 스와이프 병합, 점수, 게임오버 판정
- 타일 이동·생성·병합 애니메이션
- 최고 점수 저장, 앱을 껐다 켜도 진행 중이던 판 복원
- 되돌리기 1회
- 화면 어디서든 스와이프

## 의존성은 하나만

가장 먼저 정한 것은 "무엇을 안 쓸 것인가"다. 2048은 스와이프를 놓으면 타일이 한 번 움직이는 게임이라 드래그를 따라가는 연출이 없다. Reanimated와 gesture-handler는 쓸 자리가 없고, 의존성 2개와 babel 설정만 늘어난다.

| | 선택 | 이유 |
|---|---|---|
| 프레임워크 | Expo SDK 57 | `expo prebuild`로 android/를 만들고 로컬 gradle로 빌드. 계정도 클라우드 빌드도 필요 없다 |
| 게임 로직 | 순수 TypeScript | RN에 의존하지 않아 `node --test`만으로 검증 |
| 애니메이션 | RN 내장 `Animated` | 타일 16개의 transform엔 충분하다 |
| 제스처 | RN 내장 `PanResponder` | 손을 뗀 시점의 dx, dy를 비교하는 한 줄 |
| 영속화 | AsyncStorage | 유일한 추가 의존성 |
| 테스트 | Node 24 내장 test runner | 테스트 프레임워크를 설치하지 않는다 |

Node 24는 TypeScript를 설정 없이 실행한다. 착수 전에 한 줄짜리 테스트로 직접 확인했다. 조건이 하나 있는데, import할 때 `.ts` 확장자를 명시해야 한다. Metro 번들러는 반대로 확장자를 붙이지 않는 것이 관례라, 테스트가 import하는 모듈은 RN 의존성을 가지면 안 된다. 저장 데이터 파싱 로직을 `storage-parse.ts`로 따로 뗀 이유가 이것이다.

## 타일은 격자가 아니라 id를 가진 객체

설계의 핵심은 자료 구조 하나다. 타일을 4×4 배열이 아니라 id를 가진 객체의 배열로 다룬다.

```ts
type Tile = { id: number; value: number; row: number; col: number };
type GameState = { tiles: Tile[]; score: number; nextId: number };
```

격자 배열만으로는 이동 전후를 연결할 수 없다. "이 타일이 어디서 어디로 갔는지"를 화면이 알아야 이동 애니메이션을 그릴 수 있고, 그러려면 id가 있어야 한다. 병합된 타일은 이동 방향 앞쪽 타일의 id를 물려받고, 사라지는 타일의 id는 버린다. 화면은 사라진 id를 언마운트한다.

`move`는 순수 함수다. 한 줄씩 끝으로 밀고, 같은 값이 만나면 병합해 점수에 더한다. 한 수에서 이미 병합된 타일은 다시 병합되지 않아서 `2 2 4 4`를 왼쪽으로 밀면 `8`이 아니라 `4 8`이다. 아무것도 움직이지 않았으면 입력 상태를 그대로 돌려준다. 새 타일도 놓지 않는다. "막힌 방향으로 스와이프해도 판이 진행되지 않는다"는 규칙이 별도 분기 없이 따라온다.

2048 타일이 나와도 게임을 멈추지 않는다. 승리 화면은 없고, 게임은 빈 칸이 없고 인접한 같은 값도 없을 때만 끝난다.

## 병합 애니메이션: 고스트 타일

실기기에서 처음 돌렸을 때 병합이 애니메이션 없이 숫자만 바뀌었다. `slideLine`이 병합된 뒤쪽 타일을 결과에서 빼 버려 즉시 언마운트됐기 때문이다.

원작 2048 방식으로 고쳤다. 로직이 사라진 타일에 목표 좌표를 붙여 `ghosts`로 함께 돌려준다. 화면은 고스트를 타일보다 아래에 먼저 그리고, 같은 key라 기존 컴포넌트가 spring으로 목표 칸까지 미끄러진다. 150ms 뒤에 앱이 고스트를 비우고, 앞쪽 타일은 값 변화를 120ms 미뤄 표시한 뒤 팝한다. `ghosts`는 옵셔널 필드라 저장 데이터에서 자동으로 빠지고, 기존 테스트 33건은 한 줄도 고치지 않았다.

같은 시기에 고친 다른 하나는 스와이프 영역이다. `PanResponder`가 보드 View에만 붙어 있어 보드 밖 터치를 아무도 받지 않았다. 감지를 `useSwipe` 훅으로 빼서 화면 전체 View에 붙였다. 헤더 버튼 위 드래그는 Pressable이 먼저 잡는데, 그건 의도한 동작이다.

## android/는 커밋한다

Expo 템플릿의 `.gitignore`는 `/android`를 무시한다. 그대로 두면 안 된다. 릴리즈 서명 설정이 `android/app/build.gradle`에 들어가야 하는데, 이 디렉토리를 무시하면 `expo prebuild`가 재생성할 때마다 서명 설정이 사라져 출시 빌드가 깨진다. 한 번 prebuild해서 커밋한 뒤로는 `app.json`과 `android/`를 함께 고친다.

키스토어 비밀번호는 저장소 안의 `gradle.properties`가 아니라 `~/.gradle/gradle.properties`에 둔다. `.jks`도 저장소 밖이다. 둘 다 저장소 안에 존재하지 않으므로 실수로 커밋될 수 없다. 서명 설정이 없으면 debug 키로 서명된다.

## 실기기 검증

Galaxy Z Flip7(Android 16)에 release APK를 설치해 adb로 조작하고 스크린샷으로 판정했다. 계획은 debug 빌드였지만 Metro 번들러를 계속 띄워야 해서 release로 바꿨다. JS가 내장돼 독립 실행되고, 어차피 출시 전에 release를 다시 확인한다.

| 항목 | 방법 | 결과 |
|---|---|---|
| 병합 점수 | 우 스와이프 `4 2 2 _` → `_ _ 4 4` | +4 정확 |
| 새 타일 | 스와이프 후 | 정확히 1개 추가 |
| 이어하기 | `am force-stop` → `am start` | 판과 점수 동일 |
| 되돌리기 | 탭 | 직전 판 복원, 최고점 유지 |
| 세로 고정 | `user_rotation 1` | 세로 유지 |
| 보드 밖 스와이프 | 보드 위·아래·헤더 옆 3곳 | 모두 동작 |
| 콜드 스타트 | `am start -W` | 252ms |

앱 아이콘 5장은 디자인 도구 없이 Pillow로 그렸다. 1024 캔버스에 둥근 타일과 흰 "2048"이다.

## 실행

```bash
npm install
npx tsc --noEmit          # 타입 검사
node --test               # 테스트 39건
npx expo run:android      # 연결된 기기에 디버그 빌드 설치
cd android && ./gradlew :app:bundleRelease   # 출시용 AAB
```

## 만들면서 배운 것

1. 계획에 적은 검증 커맨드는 계획을 쓸 때 한 번 실제로 돌려봐야 한다. `node --test src/`는 Node가 `src`를 모듈로 해석해 실패했고, 테스트 파일의 `node:test` 타입과 `.ts` import는 tsc를 깨뜨렸다. 둘 다 구현 중에 드러났지만 계획 단계에서 잡을 수 있었다. 후자는 `tsconfig.json`에서 테스트 파일을 제외하는 것으로 해결했다. `@types/node`와 옵션 추가는 devDependency와 설정 2개가 늘어 기각했다.
2. `setState` updater 안에서 다른 `setState`를 부르면 안 된다. updater는 순수해야 하고 StrictMode에서 두 번 호출될 수 있어 되돌리기 상태와 최고 점수가 어긋난다. 계획 검토에서 잡아 updater 밖으로 뺐다.
3. `assembleRelease`가 7초 만에 끝나면 의심해야 한다. JS 번들 태스크가 캐시를 탔을 수 있다. Hermes 바이트코드는 `strings`로 속성명을 못 찾으므로 반영 여부는 grep이 아니라 동작으로 판정한다.
4. Z Flip7은 디스플레이가 2개라 `adb exec-out screencap -p`의 stdout에 경고가 섞여 PNG가 깨진다. `/sdcard`에 찍고 `adb pull`로 가져온다.
5. 구현 8개 태스크 중 7개를 Codex CLI에 넘겼다. 재시도 0회, 범위 밖 수정 0건. 실패 2건은 모두 계획 쪽 결함이었고 Codex는 두 번 다 원인을 정확히 진단해 보고했다. 위임이 잘 되려면 코드보다 계획의 검증 커맨드가 정확해야 한다.

Play Console 등록과 서명된 AAB 제출은 진행 중이다. 개인 개발자 계정으로 새로 여는 경우 프로덕션 출시 전에 테스터 12명이 14일간 비공개 테스트를 해야 하므로, 코드와 무관하게 달력상 2주가 더 걸린다.
