---
title: "Obsidian · graphify 도입 검토"
date: 2026-09-14
weight: 1
tags: [claude-code, obsidian, graphify, codegraph]
description: "팀 작업 이력 공유와 Claude Code 세션 비용 절감에 Obsidian과 graphify가 필요한지 검토했다. Obsidian 앱은 도입 안 함, graphify와 codegraph는 보류, docs/ 폴더와 CLAUDE.md 세 줄은 지금 시행."
---

<style>
.prose {
  --rv-obs: #6B4FBB; --rv-obs-soft: #ECE7FA;
  --rv-gfy: #1F7A5C; --rv-gfy-soft: #E1F2EA;
  --rv-acc: #1F5F7A; --rv-acc-soft: #E3EEF4;
  --rv-adopt: #2E7D5B; --rv-adopt-bg: #E2F1EA;
  --rv-hold: #A66B12;  --rv-hold-bg: #F8EBD3;
  --rv-reject: #A3423C; --rv-reject-bg: #F6E1DF;
}
[data-theme="dark"] .prose {
  --rv-obs: #B8A6F5; --rv-obs-soft: #2A2342;
  --rv-gfy: #7FD3AE; --rv-gfy-soft: #163229;
  --rv-acc: #7BBBD8; --rv-acc-soft: #1E3441;
  --rv-adopt: #7FCBA6; --rv-adopt-bg: #17362A;
  --rv-hold: #E3B25C;  --rv-hold-bg: #3C2E14;
  --rv-reject: #E59A95; --rv-reject-bg: #3F2321;
}
.prose .rv-chip { display: inline-block; font-family: var(--font-mono); font-size: 0.75rem; letter-spacing: 0.04em; line-height: 1.6; padding: 0.1em 0.5em; border-radius: 3px; font-weight: 500; white-space: nowrap; }
.prose .rv-adopt  { color: var(--rv-adopt);  background: var(--rv-adopt-bg); }
.prose .rv-hold   { color: var(--rv-hold);   background: var(--rv-hold-bg); }
.prose .rv-reject { color: var(--rv-reject); background: var(--rv-reject-bg); }
.prose .rv-tag { display: inline-block; font-family: var(--font-mono); font-size: 0.75rem; letter-spacing: 0.06em; line-height: 1.6; padding: 0.1em 0.6em; border-radius: 3px; font-weight: 500; color: var(--tc); background: var(--tc-soft); }
.prose .rv-t-obs { --tc: var(--rv-obs); --tc-soft: var(--rv-obs-soft); }
.prose .rv-t-gfy { --tc: var(--rv-gfy); --tc-soft: var(--rv-gfy-soft); }
.prose .rv-t-acc { --tc: var(--rv-acc); --tc-soft: var(--rv-acc-soft); }
.prose .rv-t-hold { --tc: var(--rv-hold); --tc-soft: var(--rv-hold-bg); }
.prose .rv-head { display: flex; align-items: center; gap: var(--space-2); flex-wrap: wrap; }
.prose h2 + .rv-head { margin-top: var(--space-3); }
.prose .rv-verdicts { display: grid; grid-template-columns: 1fr 1fr; gap: var(--space-4) var(--space-6); padding: var(--space-4) var(--space-6); border: 1px solid var(--border-strong); border-radius: var(--radius); background: var(--card); }
.prose .rv-verdict { border-top: 3px solid var(--vc); padding-top: var(--space-2); display: flex; flex-direction: column; gap: var(--space-1); }
.prose .rv-verdict .rv-chip { align-self: flex-start; }
.prose .rv-verdict strong { font-size: 0.9375rem; line-height: 1.3; }
.prose .rv-verdict p { margin: 0; font-size: 0.875rem; line-height: 1.5; color: var(--muted-fg); }
.prose .rv-v-obs { --vc: var(--rv-obs); } .prose .rv-v-gfy { --vc: var(--rv-gfy); } .prose .rv-v-cg { --vc: var(--muted-fg); } .prose .rv-v-acc { --vc: var(--rv-acc); }
@media (max-width: 480px) { .prose .rv-verdicts { grid-template-columns: 1fr; padding-inline: var(--space-4); } }
.prose figure { margin: 0; }
.prose figure svg { width: 100%; height: auto; display: block; }
.prose figcaption { font-size: 0.875rem; line-height: 1.6; color: var(--muted-fg); margin-top: var(--space-2); }
.prose svg text { font-family: var(--font-sans); font-size: 12px; fill: currentColor; }
.prose svg .m { font-family: var(--font-mono); }
.prose svg .s { font-size: 11px; fill: var(--muted-fg); }
.prose svg .b6 { font-weight: 600; }
.prose svg .bx { fill: var(--card); stroke: currentColor; stroke-width: 1.2; }
.prose svg .ln { fill: none; stroke: currentColor; stroke-width: 1.2; }
.prose svg .dash { stroke-dasharray: 4 3; }
.prose svg .dim { stroke: var(--muted-fg); }
.prose svg .dimt { fill: var(--muted-fg); }
.prose svg .obs { stroke: var(--rv-obs); } .prose svg .obst { fill: var(--rv-obs); }
.prose svg .gfy { stroke: var(--rv-gfy); } .prose svg .gfyt { fill: var(--rv-gfy); }
.prose svg .acc { stroke: var(--rv-acc); } .prose svg .acct { fill: var(--rv-acc); }
.prose svg .bad { stroke: var(--rv-reject); } .prose svg .badt { fill: var(--rv-reject); }
.prose svg .w2 { stroke-width: 1.8; }
.prose .rv-skills { display: flex; flex-wrap: wrap; gap: var(--space-2); }
.prose .rv-skill { font-family: var(--font-mono); font-size: 0.8125rem; padding: 0.2em 0.7em; border-radius: 4px; border: 1px solid var(--border-strong); background: var(--card); }
.prose .rv-skill.ok { border-color: var(--rv-adopt); color: var(--rv-adopt); }
.prose .rv-skill.no { border-color: var(--rv-reject); color: var(--rv-reject); background: var(--rv-reject-bg); }
.prose details { border: 1px solid var(--border); border-radius: var(--radius); padding: var(--space-3) var(--space-4); }
.prose details > * + * { margin-top: 1em; }
.prose summary { cursor: pointer; font-weight: 500; color: var(--accent); }
.prose .rv-note { font-size: 0.9375rem; color: var(--muted-fg); }
</style>

팀 작업 이력 공유와 Claude Code 세션 비용 절감에 두 도구가 필요한지 검토했습니다.

## 결론

<div class="rv-verdicts">
<div class="rv-verdict rv-v-obs"><span class="rv-chip rv-reject">도입 안 함</span><strong>Obsidian 앱</strong><p>AI와 팀 공유 루프에 앱이 없습니다. 폴더 규칙만 빌려옵니다.</p></div>
<div class="rv-verdict rv-v-gfy"><span class="rv-chip rv-hold">보류</span><strong>graphify</strong><p>코드 지도는 만들지만 기억은 아닙니다. 저장소가 커지면 <code>docs/</code>에만 붙입니다.</p></div>
<div class="rv-verdict rv-v-cg"><span class="rv-chip rv-hold">보류</span><strong>codegraph</strong><p>코드 전용 실시간 검색. 플랫폼 간 호출 경로 추적이 반복될 때만 붙입니다.</p></div>
<div class="rv-verdict rv-v-acc"><span class="rv-chip rv-adopt">지금 시행</span><strong>docs/ + CLAUDE.md 세 줄</strong><p>결정 기록과 작업 로그를 저장소에 두고 Claude가 읽고 쓰게 합니다.</p></div>
</div>

<figure>
<svg viewBox="0 0 720 250" role="img" aria-label="채택한 구성. 팀원 각자의 Claude Code와 브라우저가 git 저장소 안의 docs 폴더를 읽고 쓴다. Obsidian 앱과 graphify는 점선으로 바깥에 있고 필요할 때만 붙인다.">
  <defs>
    <marker id="a0" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0 0L10 5L0 10z" fill="currentColor"/></marker>
    <marker id="a0d" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0 0L10 5L0 10z" style="fill:var(--muted-fg)"/></marker>
  </defs>
  <rect class="bx" x="20" y="30" width="170" height="48" rx="4"/><text x="105" y="59" text-anchor="middle">Claude Code · 팀원 A</text>
  <rect class="bx" x="20" y="100" width="170" height="48" rx="4"/><text x="105" y="129" text-anchor="middle">Claude Code · 팀원 B</text>
  <rect class="bx" x="20" y="170" width="170" height="48" rx="4"/><text x="105" y="199" text-anchor="middle">팀원 · 브라우저</text>
  <rect class="bx acc w2" x="280" y="20" width="200" height="215" rx="6"/>
  <text class="m s" x="292" y="40">git 저장소</text>
  <rect class="bx" x="300" y="52" width="160" height="30" rx="3"/><text class="m" x="380" y="71" text-anchor="middle" style="font-size:11.5px">CLAUDE.md · 규율 3줄</text>
  <rect class="bx" x="300" y="95" width="160" height="125" rx="3"/>
  <text class="m b6" x="312" y="115">docs/</text>
  <text class="m" x="320" y="140" style="font-size:11.5px">README.md</text>
  <text class="m" x="320" y="162" style="font-size:11.5px">decisions/</text>
  <text class="m" x="320" y="184" style="font-size:11.5px">worklog/</text>
  <text class="m" x="320" y="206" style="font-size:11.5px">topics/</text>
  <line class="ln" x1="190" y1="54" x2="278" y2="54" marker-end="url(#a0)"/><text class="s" x="234" y="47" text-anchor="middle">읽고 쓴다</text>
  <line class="ln" x1="190" y1="124" x2="278" y2="124" marker-end="url(#a0)"/><text class="s" x="234" y="117" text-anchor="middle">읽고 쓴다</text>
  <line class="ln" x1="190" y1="194" x2="278" y2="194" marker-end="url(#a0)"/><text class="s" x="234" y="187" text-anchor="middle">GitHub에서 읽는다</text>
  <text class="s" x="630" y="46" text-anchor="middle">필요해질 때만</text>
  <rect class="bx dim dash" x="560" y="60" width="140" height="44" rx="4"/><text class="dimt" x="630" y="87" text-anchor="middle">Obsidian 앱</text>
  <rect class="bx dim dash" x="560" y="150" width="140" height="44" rx="4"/><text class="dimt" x="630" y="177" text-anchor="middle">graphify</text>
  <line class="ln dim dash" x1="558" y1="82" x2="484" y2="82" marker-end="url(#a0d)"/><text class="s" x="521" y="75" text-anchor="middle">본다</text>
  <line class="ln dim dash" x1="558" y1="172" x2="484" y2="172" marker-end="url(#a0d)"/><text class="s" x="521" y="165" text-anchor="middle">지도 생성</text>
</svg>
<figcaption>채택한 구성. 공유는 git이 하고, Claude는 파일을 읽고 씁니다. 두 도구는 루프 바깥에 있으며 붙이는 시점은 맨 아래 재검토 트리거에 있습니다.</figcaption>
</figure>

## Obsidian: 앱은 안 씁니다. 폴더 규칙만 가져옵니다.

<p class="rv-head"><span class="rv-tag rv-t-obs">Obsidian</span><span class="rv-chip rv-reject">도입 안 함</span></p>

<figure>
<svg viewBox="0 0 720 260" role="img" aria-label="vault의 실체. docs 폴더 안에 마크다운 파일들이 있고 .obsidian 폴더는 선택이다. 왼쪽에서 Claude Code가 파일로 읽고 쓰고, 오른쪽에서 GitHub와 Quartz 웹이 앱 없이 보여준다. Obsidian 앱은 설치한 사람만 쓰는 점선 경로다.">
  <defs>
    <marker id="a1" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0 0L10 5L0 10z" fill="currentColor"/></marker>
    <marker id="a1o" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0 0L10 5L0 10z" style="fill:var(--rv-obs)"/></marker>
  </defs>
  <rect class="bx" x="20" y="105" width="160" height="50" rx="4"/><text x="100" y="135" text-anchor="middle">Claude Code</text>
  <rect class="bx obs w2" x="270" y="20" width="200" height="225" rx="6"/>
  <text class="m b6 obst" x="284" y="43">docs/  = vault</text>
  <text class="m" x="292" y="72" style="font-size:11.5px">README.md</text>
  <text class="m" x="292" y="98" style="font-size:11.5px">decisions/*.md</text>
  <text class="m" x="292" y="124" style="font-size:11.5px">worklog/*.md</text>
  <text class="m" x="292" y="150" style="font-size:11.5px">topics/*.md</text>
  <rect class="bx dim dash" x="284" y="172" width="172" height="56" rx="3"/>
  <text class="m dimt" x="296" y="193" style="font-size:11.5px">.obsidian/</text>
  <text class="s" x="296" y="213">앱이 열 때 자동 생성 · 선택</text>
  <line class="ln" x1="180" y1="130" x2="268" y2="130" marker-end="url(#a1)"/><text class="s" x="224" y="122" text-anchor="middle">파일로 읽고 쓴다</text>
  <rect class="bx" x="560" y="30" width="140" height="44" rx="4"/><text x="630" y="57" text-anchor="middle">GitHub</text>
  <rect class="bx" x="560" y="108" width="140" height="44" rx="4"/><text x="630" y="135" text-anchor="middle">Quartz 웹</text>
  <rect class="bx obs dash" x="560" y="186" width="140" height="44" rx="4"/><text class="obst" x="630" y="213" text-anchor="middle">Obsidian 앱</text>
  <line class="ln" x1="470" y1="52" x2="558" y2="52" marker-end="url(#a1)"/><text class="s" x="514" y="44" text-anchor="middle">링크 클릭</text>
  <line class="ln" x1="470" y1="130" x2="558" y2="130" marker-end="url(#a1)"/><text class="s" x="514" y="122" text-anchor="middle">그래프 뷰 · 백링크</text>
  <line class="ln obs dash" x1="470" y1="208" x2="558" y2="208" marker-end="url(#a1o)"/><text class="s" x="514" y="200" text-anchor="middle">설치한 사람만</text>
</svg>
<figcaption>vault는 폴더입니다. 앱이 vault로 인식하는 조건은 .obsidian/ 하나뿐이고 그마저 앱이 열 때 자동으로 생깁니다. 앱은 세 가지 보는 방법 중 하나이며, 사람이 그래프 뷰를 원하면 Quartz로 웹 빌드해도 됩니다.</figcaption>
</figure>

### obsidian-skills 5개 중 앱이 필요한 것

<div class="rv-skills">
<span class="rv-skill ok">obsidian-markdown</span>
<span class="rv-skill ok">obsidian-bases</span>
<span class="rv-skill ok">json-canvas</span>
<span class="rv-skill ok">defuddle</span>
<span class="rv-skill no">obsidian-cli · 앱 실행 필요</span>
</div>

#### 앱 없이 잃는 것

- 이름 변경 시 링크 자동 갱신. 대신 파일명을 안정적으로 짓고 `aliases`로 옛 이름을 남깁니다.
- 렌더 검증. 콜아웃 문법이 틀려도 알려주지 않지만 내용은 깨지지 않습니다.
- `.base`, `.canvas`를 볼 도구. 팀 공유에는 마크다운만 의미가 있습니다.

#### 빌려오는 규칙

- 노트 상단 YAML 프로퍼티: `status`, `date`, `scope`.
- 개념 하나에 파일 하나, 파일명은 폴더 전체에서 유일.
- 링크는 위키링크 대신 일반 마크다운 링크. GitHub에서 클릭되고 Claude에겐 차이가 없습니다.

## graphify: 지도는 만들지만 기억은 아닙니다.

<p class="rv-head"><span class="rv-tag rv-t-gfy">graphify</span><span class="rv-chip rv-hold">보류</span></p>

<figure>
<svg viewBox="0 0 720 280" role="img" aria-label="graphify가 하는 일. 코드는 tree-sitter AST로 LLM 비용 없이, 마크다운·PDF·이미지는 Claude로 토큰을 써서 추출한다. 출력은 graph.html, graph.json, obsidian과 wiki 폴더, GRAPH_REPORT.md이며 Claude Code는 세션 시작에 GRAPH_REPORT.md를 읽는다.">
  <defs>
    <marker id="a2" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0 0L10 5L0 10z" fill="currentColor"/></marker>
  </defs>
  <rect class="bx" x="20" y="40" width="130" height="34" rx="4"/><text x="85" y="62" text-anchor="middle">코드</text>
  <rect class="bx" x="20" y="90" width="130" height="34" rx="4"/><text x="85" y="112" text-anchor="middle">마크다운</text>
  <rect class="bx" x="20" y="140" width="130" height="34" rx="4"/><text x="85" y="162" text-anchor="middle">PDF</text>
  <rect class="bx" x="20" y="190" width="130" height="34" rx="4"/><text x="85" y="212" text-anchor="middle">이미지</text>
  <rect class="bx gfy w2" x="230" y="40" width="220" height="190" rx="6"/>
  <text class="b6 gfyt" x="340" y="63" text-anchor="middle" style="font-size:14px">graphify</text>
  <rect class="bx" x="245" y="78" width="190" height="48" rx="3"/>
  <text x="340" y="98" text-anchor="middle">tree-sitter AST</text><text class="s" x="340" y="116" text-anchor="middle">LLM 비용 없음</text>
  <rect class="bx" x="245" y="146" width="190" height="66" rx="3"/>
  <text x="340" y="172" text-anchor="middle">Claude 추출 · vision 포함</text><text class="s" x="340" y="192" text-anchor="middle">토큰 소모</text>
  <polyline class="ln" points="150,57 200,57 200,102 243,102" marker-end="url(#a2)"/>
  <line class="ln" x1="150" y1="107" x2="195" y2="107"/>
  <line class="ln" x1="150" y1="157" x2="195" y2="157"/>
  <line class="ln" x1="150" y1="207" x2="195" y2="207"/>
  <line class="ln" x1="195" y1="107" x2="195" y2="207"/>
  <line class="ln" x1="195" y1="179" x2="243" y2="179" marker-end="url(#a2)"/>
  <line class="ln" x1="450" y1="135" x2="490" y2="135"/>
  <line class="ln" x1="490" y1="56" x2="490" y2="188"/>
  <line class="ln" x1="490" y1="56" x2="528" y2="56" marker-end="url(#a2)"/>
  <line class="ln" x1="490" y1="100" x2="528" y2="100" marker-end="url(#a2)"/>
  <line class="ln" x1="490" y1="144" x2="528" y2="144" marker-end="url(#a2)"/>
  <line class="ln" x1="490" y1="188" x2="528" y2="188" marker-end="url(#a2)"/>
  <rect class="bx" x="530" y="40" width="170" height="32" rx="3"/><text class="m" x="615" y="61" text-anchor="middle" style="font-size:11.5px">graph.html</text>
  <rect class="bx" x="530" y="84" width="170" height="32" rx="3"/><text class="m" x="615" y="105" text-anchor="middle" style="font-size:11.5px">graph.json</text>
  <rect class="bx" x="530" y="128" width="170" height="32" rx="3"/><text class="m" x="615" y="149" text-anchor="middle" style="font-size:11.5px">obsidian/ · wiki/</text>
  <rect class="bx gfy w2" x="530" y="172" width="170" height="32" rx="3"/><text class="m b6 gfyt" x="615" y="193" text-anchor="middle" style="font-size:11.5px">GRAPH_REPORT.md</text>
  <line class="ln" x1="615" y1="204" x2="615" y2="226" marker-end="url(#a2)"/>
  <rect class="bx" x="530" y="228" width="170" height="36" rx="4"/><text x="615" y="251" text-anchor="middle" style="font-size:11.5px">Claude Code · 세션 시작에 읽음</text>
</svg>
<figcaption>graphify가 하는 일. 코드는 무료로, 문서와 이미지는 Claude 토큰을 써서 그래프를 만들고, Claude는 세션 시작에 리포트 한 장을 읽어 구조를 파악합니다. 파일 6개 규모에서는 절감이 약 1배이고, 규모가 커질수록 효과가 커집니다.</figcaption>
</figure>

<figure>
<svg viewBox="0 0 720 235" role="img" aria-label="영구 기억이 아닌 이유. 세션마다 코드가 바뀌면 graphify 스냅샷은 이전 것을 덮어쓴다. 결정 기록은 누적되지만 그 줄은 사람이나 Claude가 써야 하며 graphify는 결정 기록 줄에 아무것도 쓰지 않는다.">
  <defs>
    <marker id="a3" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0 0L10 5L0 10z" fill="currentColor"/></marker>
    <marker id="a3r" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0 0L10 5L0 10z" style="fill:var(--rv-reject)"/></marker>
  </defs>
  <text class="s" x="215" y="16" text-anchor="middle">세션 1</text>
  <text class="s" x="415" y="16" text-anchor="middle">세션 2</text>
  <text class="s" x="615" y="16" text-anchor="middle">세션 3</text>
  <text class="b6" x="20" y="52">코드</text>
  <rect class="bx" x="140" y="28" width="150" height="38" rx="4"/><text x="215" y="52" text-anchor="middle">코드 v1</text>
  <rect class="bx" x="340" y="28" width="150" height="38" rx="4"/><text x="415" y="52" text-anchor="middle">코드 v2</text>
  <rect class="bx" x="540" y="28" width="150" height="38" rx="4"/><text x="615" y="52" text-anchor="middle">코드 v3</text>
  <line class="ln" x1="215" y1="66" x2="215" y2="101" marker-end="url(#a3)"/>
  <line class="ln" x1="415" y1="66" x2="415" y2="101" marker-end="url(#a3)"/>
  <line class="ln" x1="615" y1="66" x2="615" y2="101" marker-end="url(#a3)"/>
  <text class="s" x="224" y="88">생성</text><text class="s" x="424" y="88">생성</text><text class="s" x="624" y="88">생성</text>
  <text class="b6 gfyt" x="20" y="127">graphify</text>
  <rect class="bx gfy" x="140" y="103" width="150" height="38" rx="4"/><text x="215" y="127" text-anchor="middle">스냅샷 1</text>
  <rect class="bx gfy" x="340" y="103" width="150" height="38" rx="4"/><text x="415" y="127" text-anchor="middle">스냅샷 2</text>
  <rect class="bx gfy" x="540" y="103" width="150" height="38" rx="4"/><text x="615" y="127" text-anchor="middle">스냅샷 3</text>
  <line class="ln" x1="290" y1="122" x2="338" y2="122" marker-end="url(#a3)"/><text class="s" x="314" y="115" text-anchor="middle">덮어씀</text>
  <line class="ln" x1="490" y1="122" x2="538" y2="122" marker-end="url(#a3)"/><text class="s" x="514" y="115" text-anchor="middle">덮어씀</text>
  <line class="ln bad dash" x1="415" y1="141" x2="415" y2="176" marker-end="url(#a3r)"/>
  <text class="badt" x="424" y="163" style="font-size:11px">graphify는 여기에 쓰지 않음</text>
  <text class="b6 acct" x="20" y="197">결정 기록</text>
  <text class="s" x="20" y="213">사람 · Claude가 씀</text>
  <rect class="bx acc" x="140" y="178" width="150" height="38" rx="4"/><text x="215" y="202" text-anchor="middle">결정 1</text>
  <rect class="bx acc" x="340" y="178" width="150" height="38" rx="4"/><text x="415" y="202" text-anchor="middle">결정 1 · 2</text>
  <rect class="bx acc" x="540" y="178" width="150" height="38" rx="4"/><text x="615" y="202" text-anchor="middle">결정 1 · 2 · 3</text>
  <line class="ln" x1="290" y1="197" x2="338" y2="197" marker-end="url(#a3)"/><text class="s" x="314" y="190" text-anchor="middle">누적</text>
  <line class="ln" x1="490" y1="197" x2="538" y2="197" marker-end="url(#a3)"/><text class="s" x="514" y="190" text-anchor="middle">누적</text>
</svg>
<figcaption>"영구 기억"이 아닌 이유. graphify가 기억하는 건 지금 코드가 어떻게 생겼나이고, 매번 다시 생성되는 스냅샷입니다. 왜 그렇게 결정했는지는 아래 줄에 누군가 써야 생기며, 그 규율은 graphify와 무관합니다.</figcaption>
</figure>

#### 얻는 것

- 세션 시작 구조 파악 비용 절감. 큰 저장소일수록 큽니다.
- 사람이 볼 리포트와 그래프. 처음 보는 코드베이스 조망에 유용합니다.
- 코드와 문서를 한 그래프로. 도구 하나로 둘 다 다룹니다.

#### 못 얻는 것

- 결정과 이력. 코드 품질도 그래프와 무관합니다.
- 낡은 그래프는 해롭습니다. 재생성 훅 없이는 틀린 지도를 믿습니다.
- 리포트가 수만 토큰이면 절감 목적이 뒤집힙니다.

## graphify vs codegraph: 비슷해 보이지만 역할이 다릅니다.

<p class="rv-head"><span class="rv-tag rv-t-hold">graphify vs codegraph</span><span class="rv-chip rv-hold">둘 다 보류</span></p>

<figure>
<svg viewBox="0 0 720 285" role="img" aria-label="세션 흐름에서 graphify와 codegraph가 작동하는 위치 비교. graphify는 세션 시작에 리포트를 읽고 작업 중에는 여전히 grep과 Read로 파일을 탐색하며 범위는 코드와 문서다. codegraph는 세션 시작에는 아무것도 없고 작업 중 MCP로 인덱스에 질의해 소스 조각을 받으며, 인덱스는 저장 시 자동 동기화되고 범위는 코드만이다.">
  <defs>
    <marker id="a4" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0 0L10 5L0 10z" fill="currentColor"/></marker>
  </defs>
  <line class="ln dim dash" x1="360" y1="14" x2="360" y2="275"/>
  <text class="b6 gfyt" x="20" y="26" style="font-size:14px">graphify</text>
  <line class="ln dim" x1="30" y1="62" x2="330" y2="62"/>
  <line class="ln dim" x1="75" y1="57" x2="75" y2="67"/><line class="ln dim" x1="195" y1="57" x2="195" y2="67"/><line class="ln dim" x1="300" y1="57" x2="300" y2="67"/>
  <text class="s" x="75" y="50" text-anchor="middle">세션 시작</text><text class="s" x="195" y="50" text-anchor="middle">작업 중</text><text class="s" x="300" y="50" text-anchor="middle">끝</text>
  <rect class="bx gfy" x="20" y="84" width="110" height="44" rx="4"/>
  <text class="m" x="75" y="102" text-anchor="middle" style="font-size:10.5px">GRAPH_REPORT.md</text><text x="75" y="119" text-anchor="middle" style="font-size:11.5px">읽음</text>
  <rect class="bx" x="140" y="84" width="110" height="44" rx="4"/>
  <text x="195" y="102" text-anchor="middle" style="font-size:11.5px">grep · Read</text><text class="s" x="195" y="119" text-anchor="middle">파일 직접 탐색</text>
  <rect class="bx" x="20" y="170" width="250" height="40" rx="4"/>
  <text x="145" y="195" text-anchor="middle" style="font-size:11.5px">저장소 파일: 코드 + 문서 + PDF + 이미지</text>
  <line class="ln" x1="75" y1="170" x2="75" y2="130" marker-end="url(#a4)"/><text class="s" x="82" y="152" style="font-size:10.5px">훅으로 생성</text>
  <line class="ln" x1="195" y1="128" x2="195" y2="168" marker-end="url(#a4)"/><text class="s" x="202" y="152" style="font-size:10.5px">매번 읽음</text>
  <text class="s" x="20" y="245">언어 경계 연결 없음</text>
  <text class="s" x="20" y="263">앱·데몬 없음, 스킬 하나</text>
  <text class="b6" x="380" y="26" style="font-size:14px">codegraph</text>
  <line class="ln dim" x1="390" y1="62" x2="690" y2="62"/>
  <line class="ln dim" x1="435" y1="57" x2="435" y2="67"/><line class="ln dim" x1="575" y1="57" x2="575" y2="67"/><line class="ln dim" x1="660" y1="57" x2="660" y2="67"/>
  <text class="s" x="435" y="50" text-anchor="middle">세션 시작</text><text class="s" x="575" y="50" text-anchor="middle">작업 중</text><text class="s" x="660" y="50" text-anchor="middle">끝</text>
  <rect class="bx dim dash" x="380" y="84" width="110" height="44" rx="4"/><text class="dimt" x="435" y="111" text-anchor="middle" style="font-size:11.5px">없음</text>
  <rect class="bx" x="500" y="84" width="150" height="44" rx="4"/>
  <text x="575" y="102" text-anchor="middle" style="font-size:11.5px">MCP 질의</text><text class="s" x="575" y="119" text-anchor="middle">소스 조각 반환</text>
  <rect class="bx" x="380" y="170" width="140" height="40" rx="4"/><text class="m" x="450" y="195" text-anchor="middle" style="font-size:11px">인덱스 (.codegraph/)</text>
  <rect class="bx" x="540" y="170" width="160" height="40" rx="4"/><text x="620" y="195" text-anchor="middle" style="font-size:11.5px">저장소 파일: 코드만</text>
  <line class="ln" x1="540" y1="190" x2="522" y2="190" marker-end="url(#a4)"/><text class="s" x="530" y="225" text-anchor="middle" style="font-size:10.5px">저장 시 자동 동기화</text>
  <polyline class="ln" points="520,128 520,150 450,150 450,168" marker-end="url(#a4)"/><text class="s" x="485" y="145" text-anchor="middle" style="font-size:10.5px">질의</text>
  <text class="s" x="380" y="245">Swift↔ObjC, RN 브리지 연결</text>
  <text class="s" x="380" y="263">응답이 컨텍스트에 남음 (+80%)</text>
</svg>
<figcaption>같은 세션에서 두 도구가 작동하는 위치. graphify는 시작에 지도를 주고 작업 중에는 물러나며, codegraph는 작업 중 grep을 대체합니다. 둘 다 붙이면 코드에 대한 진실이 둘이 되고 지시가 충돌하므로, 함께 쓴다면 코드는 codegraph, 문서는 graphify로 역할을 못 박습니다.</figcaption>
</figure>

| | graphify | codegraph |
|---|---|---|
| 대상 | 코드 + 문서 + PDF + 이미지 | 코드만 |
| 단위 | 개념과 관계, 커뮤니티 | 심볼, 호출 엣지, 의존성 |
| 생성 | 코드는 AST, 문서·이미지는 Claude | Rust 파서. LLM 없음, API 키 없음 |
| 최신성 | `--update` / `--watch` / 커밋 훅 | 파일 저장 시 자동 동기화 |
| 사용 방식 | 세션 시작에 리포트 읽기 | 작업 중 MCP 질의, 소스 조각 반환 |
| 언어 경계 | 없음 | Swift↔ObjC, RN 브리지, 17개 프레임워크 라우트 |
| 설치 | Python 스킬 하나 | 바이너리 + MCP 데몬 |
| 비용 주의 | 문서·이미지 재처리마다 토큰 | 세션 끝 잔류 컨텍스트 약 +80% |
| 사람용 화면 | graph.html, 리포트, 위키 | 심볼 뷰어. 호출자 · 소스 · 피호출자 |
| 라이선스 | MIT | MIT. 호스팅 제품 준비 중 |

### 둘 다 붙이면

#### 장점

- 역할이 나뉘면 보완됩니다. 코드 흐름은 codegraph, 문서와 결정의 연결은 graphify.
- graphify 리포트는 처음 보는 코드베이스를 조망하는 데 여전히 쓸모 있습니다.

#### 단점

- 같은 코드에 진실이 둘. 갱신 시점이 어긋나면 조정에 토큰을 씁니다.
- 컨텍스트가 겹쳐 쌓입니다. codegraph 잔류분 위에 리포트까지 얹힙니다.
- 지시가 충돌합니다. "grep 대신 MCP"와 "리포트를 읽어라".
- 유지 대상이 둘. 스킬과 바이너리, 훅 둘, 캐시 폴더 둘.

<figure>
<svg viewBox="0 0 720 200" role="img" aria-label="선택 기준. 지금 아픈 곳이 흩어진 문서와 암묵지면 graphify를 docs에만, 코드 탐색 비용과 언어 경계면 codegraph를 코드에, 둘 다면 코드는 codegraph 문서는 graphify로 역할을 CLAUDE.md에 못 박는다.">
  <defs>
    <marker id="a5" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto"><path d="M0 0L10 5L0 10z" fill="currentColor"/></marker>
  </defs>
  <rect class="bx w2" x="20" y="75" width="150" height="50" rx="4"/><text class="b6" x="95" y="105" text-anchor="middle">지금 아픈 곳은?</text>
  <line class="ln" x1="170" y1="100" x2="200" y2="100"/>
  <line class="ln" x1="200" y1="40" x2="200" y2="160"/>
  <line class="ln" x1="200" y1="40" x2="358" y2="40" marker-end="url(#a5)"/><text class="s" x="279" y="33" text-anchor="middle">흩어진 문서 · 암묵지</text>
  <line class="ln" x1="200" y1="100" x2="358" y2="100" marker-end="url(#a5)"/><text class="s" x="279" y="93" text-anchor="middle">코드 탐색 비용 · 언어 경계</text>
  <line class="ln" x1="200" y1="160" x2="358" y2="160" marker-end="url(#a5)"/><text class="s" x="279" y="153" text-anchor="middle">둘 다</text>
  <rect class="bx gfy" x="360" y="18" width="340" height="44" rx="4"/>
  <text class="b6 gfyt" x="372" y="37">graphify를 docs/에만</text><text class="s" x="372" y="54">코드는 CLAUDE.md 색인으로 충분</text>
  <rect class="bx" x="360" y="78" width="340" height="44" rx="4"/>
  <text class="b6" x="372" y="97">codegraph를 코드에</text><text class="s" x="372" y="114">MCP 데몬과 컨텍스트 잔류를 감수</text>
  <rect class="bx acc" x="360" y="138" width="340" height="44" rx="4"/>
  <text class="b6 acct" x="372" y="157">코드는 codegraph, 문서는 graphify</text><text class="s" x="372" y="174">역할을 CLAUDE.md 한 줄로 못 박기</text>
</svg>
<figcaption>선택 기준. 순서는 codegraph를 먼저 붙이고 측정한 뒤 문서 더미가 커지면 graphify를 docs/에 얹는 것입니다. 반대로 가면 나중에 코드 쪽 graphify를 걷어내야 합니다.</figcaption>
</figure>

## 지금 시행: docs/ 폴더와 CLAUDE.md 세 줄

<p class="rv-head"><span class="rv-tag rv-t-acc">지금</span><span class="rv-chip rv-adopt">시행</span></p>

```text
docs/
├── README.md        # 시작점. 구조 요약 10줄 + 아래 폴더 링크
├── decisions/       # YYYY-MM-DD-제목.md   프로퍼티: status, date, scope
└── worklog/         # YYYY-MM-DD.md        다음 세션에 넘길 미완 상태 한 단락
```

- 작업 시작 전에 `docs/README.md`를 읽어라.
- 설계 선택이 생기면 `docs/decisions/`에 기록해라. 뒤집히면 새 항목을 쓰고 옛 항목의 `status`만 바꿔라.
- 세션 끝에 `docs/worklog/`에 한 단락 남겨라.

<p class="rv-note">구조 요약만 썩는 문서입니다. 모듈 목록과 진입점 몇 줄로 짧게 유지하고, 이력은 Linear와 PR에 맡깁니다.</p>

<details>
<summary>웹 · iOS · Android · Windows · Mac으로 커질 때</summary>

원칙은 하나입니다. Claude가 세션 시작에 읽는 양이 프로젝트 크기와 무관하게 일정해야 합니다. Claude Code는 하위 디렉터리의 CLAUDE.md를 그 안에서 작업할 때만 불러오므로 도구 없이 지킬 수 있습니다.

```text
repo/
├── CLAUDE.md                  # 공통 규칙 + "docs/README.md 읽어라"
├── docs/
│   ├── README.md              # 전체 지도 20줄, 플랫폼별 링크
│   ├── shared/decisions/      # API 계약, 인증, 데이터 모델. 한 번만 쓰고 링크
│   └── web/ ios/ android/ windows/ mac/
│       ├── README.md          # 그 플랫폼 구조 요약
│       └── decisions/
└── apps/
    ├── ios/CLAUDE.md          # "docs/ios/README.md 읽어라" + iOS 규칙
    └── android/CLAUDE.md
```

- 결정 프로퍼티 `scope: [shared]` 또는 `scope: [ios, android]`로 범위별 조회.
- 공통 결정은 `shared/`에 한 번만 쓰고 PR 리뷰. 복사하면 다섯 곳이 각자 낡습니다.
- 저장소가 여러 개면 공통 결정은 한 곳에 두고 URL로 링크.

</details>

## 재검토: 이 조건이 보이면 그때 붙입니다

| 관찰되는 조건 | 조치 |
|---|---|
| `docs/` 문서가 수십 개를 넘어 README 색인으로 못 찾음 | <span class="rv-chip rv-hold">graphify</span> `docs/`에만. `--wiki`와 post-commit 훅 |
| Claude가 세션마다 구조 파악에 도구 호출 수십 번을 씀 | <span class="rv-chip rv-hold">graphify</span> 코드에 적용. 리포트는 README에서 링크만 |
| 플랫폼 간 호출 경로(Swift↔ObjC, RN) 추적이 반복됨 | <span class="rv-chip rv-hold">codegraph</span> 코드 쪽에. graphify는 문서로 한정 |
| 사람이 수백 개 노트를 백링크·그래프로 탐색해야 함 | <span class="rv-chip rv-reject">Obsidian 앱</span> 대신 Quartz 웹 빌드. 원하는 사람만 앱, `workspace.json`은 .gitignore |

## 참고

- [safishamsi/graphify](https://github.com/safishamsi/graphify) — 출력 구조, 갱신 방식, 토큰 벤치마크
- [colbymchenry/codegraph](https://github.com/colbymchenry/codegraph) — 벤치마크와 잔류 컨텍스트 주의 문구
- [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) — obsidian-cli만 앱 필요
- [Graphify + Obsidian: A Second Brain for Claude Code](https://www.chaseai.io/blog/graphify-obsidian-claude-code-second-brain) — 유행하는 1인 세팅
- [CodeGraph vs Graphify](https://www.besthub.dev/articles/codegraph-vs-graphify-comparing-two-open-source-code-knowledge-graph-tools-172d025b944f) — 제3자 비교
