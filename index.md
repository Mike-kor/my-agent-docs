<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI 에이전트 기반 풀스택 개발 워크플로우</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500&family=Inter:wght@400;500;600;700&family=Noto+Sans+KR:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #f8f9fa;
    --surface: #ffffff;
    --surface-raised: #f1f3f5;
    --border: #dee2e6;
    --border-strong: #adb5bd;
    --text: #212529;
    --text-secondary: #495057;
    --text-muted: #868e96;
    --accent: #3b5bdb;
    --accent-hover: #2f4ac0;
    --accent-light: #edf2ff;
    --accent-border: #bac8ff;
    --green: #2f9e44;
    --green-light: #ebfbee;
    --amber: #e67700;
    --amber-light: #fff9db;
    --code-bg: #f1f3f5;
    --code-border: #ced4da;
    --code-text: #c92a2a;
    --font-mono: 'IBM Plex Mono', 'JetBrains Mono', 'Fira Code', monospace;
    --font-sans: 'Inter', 'Noto Sans KR', system-ui, sans-serif;
    --radius-sm: 4px;
    --radius: 8px;
    --radius-lg: 12px;
    --shadow-sm: 0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);
    --shadow: 0 4px 12px rgba(0,0,0,0.08), 0 2px 4px rgba(0,0,0,0.04);
    --shadow-lg: 0 20px 60px rgba(0,0,0,0.15);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: var(--font-sans);
    background: var(--bg);
    color: var(--text);
    font-size: 16px;
    line-height: 1.75;
    -webkit-font-smoothing: antialiased;
  }

  /* ── Layout ── */
  .page { max-width: 780px; margin: 0 auto; padding: 60px 24px 120px; }

  /* ── Header ── */
  .doc-header {
    margin-bottom: 56px;
    padding-bottom: 40px;
    border-bottom: 1px solid var(--border);
  }
  .doc-label {
    display: inline-block;
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--accent);
    background: var(--accent-light);
    border: 1px solid var(--accent-border);
    border-radius: 20px;
    padding: 3px 10px;
    margin-bottom: 20px;
  }
  .doc-header h1 {
    font-size: 32px;
    font-weight: 700;
    letter-spacing: -0.03em;
    line-height: 1.2;
    margin-bottom: 16px;
    color: var(--text);
  }
  .doc-header > p {
    font-size: 17px;
    color: var(--text-secondary);
    line-height: 1.65;
    max-width: 600px;
    margin-bottom: 0;
  }
  .doc-meta {
    margin-top: 24px;
    display: flex;
    gap: 0;
    flex-wrap: wrap;
    font-size: 12px;
    color: var(--text-muted);
    font-family: var(--font-mono);
  }
  .doc-meta span {
    padding: 4px 12px;
    background: var(--surface-raised);
    border: 1px solid var(--border);
    margin-right: -1px;
    margin-bottom: -1px;
  }
  .doc-meta span:first-child { border-radius: var(--radius-sm) 0 0 var(--radius-sm); }
  .doc-meta span:last-child { border-radius: 0 var(--radius-sm) var(--radius-sm) 0; }

  /* ── TOC ── */
  .toc {
    border: 1px solid var(--border);
    border-radius: var(--radius-lg);
    padding: 24px 28px;
    margin-bottom: 56px;
    background: var(--surface);
    box-shadow: var(--shadow-sm);
  }
  .toc-title {
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--text-muted);
    margin-bottom: 16px;
  }
  .toc ol {
    list-style: none;
    counter-reset: toc;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 6px 40px;
  }
  @media (max-width: 600px) { .toc ol { grid-template-columns: 1fr; } }
  .toc li { counter-increment: toc; font-size: 14px; display: flex; align-items: baseline; gap: 8px; }
  .toc li::before {
    content: counter(toc, decimal-leading-zero);
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--accent);
    font-weight: 600;
    flex-shrink: 0;
    min-width: 20px;
  }
  .toc a { color: var(--text-secondary); text-decoration: none; transition: color 0.15s; }
  .toc a:hover { color: var(--accent); }
  .toc a.active { color: var(--accent); font-weight: 600; }

  /* ── Sections ── */
  section { margin-bottom: 72px; }
  .section-number {
    font-family: var(--font-mono);
    font-size: 11px;
    font-weight: 600;
    color: var(--accent);
    letter-spacing: 0.08em;
    display: block;
    margin-bottom: 8px;
    text-transform: uppercase;
  }
  h2 {
    font-size: 22px;
    font-weight: 700;
    letter-spacing: -0.02em;
    margin-bottom: 6px;
    padding-bottom: 14px;
    border-bottom: 2px solid var(--border);
    color: var(--text);
  }
  h3 {
    font-size: 17px;
    font-weight: 600;
    margin: 36px 0 14px;
    color: var(--text);
    display: flex;
    align-items: center;
    gap: 8px;
  }
  h3::before {
    content: '';
    display: inline-block;
    width: 3px;
    height: 16px;
    background: var(--accent);
    border-radius: 2px;
    flex-shrink: 0;
  }
  h4 {
    font-size: 12px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--text-muted);
    margin: 28px 0 10px;
  }

  /* ── Typography ── */
  p { margin-bottom: 16px; color: var(--text-secondary); line-height: 1.75; }
  p:last-child { margin-bottom: 0; }
  strong { color: var(--text); font-weight: 600; }
  a { color: var(--accent); text-decoration: none; font-weight: 500; }
  a:hover { text-decoration: underline; color: var(--accent-hover); }
  ul, ol { padding-left: 22px; margin-bottom: 16px; color: var(--text-secondary); }
  li { margin-bottom: 6px; font-size: 15px; line-height: 1.65; }

  /* ── Inline code ── */
  code {
    font-family: var(--font-mono);
    font-size: 13px;
    background: #fff0f0;
    border: 1px solid #ffc9c9;
    border-radius: var(--radius-sm);
    padding: 2px 7px;
    color: #c0392b;
    font-weight: 600;
  }

  /* ── Code blocks ── */
  pre {
    background: #0d1117;
    border-radius: var(--radius);
    padding: 24px 28px;
    overflow-x: auto;
    margin: 20px 0;
    border: 1px solid #30363d;
    box-shadow: 0 4px 16px rgba(0,0,0,0.2);
    position: relative;
  }
  pre::before {
    content: '';
    display: block;
    height: 3px;
    background: linear-gradient(90deg, #3b5bdb, #7950f2, #e64980);
    border-radius: var(--radius) var(--radius) 0 0;
    position: absolute;
    top: 0; left: 0; right: 0;
  }
  pre code {
    background: none;
    border: none;
    padding: 0;
    color: #e6edf3;
    font-size: 14.5px;
    line-height: 1.85;
    font-weight: 400;
    text-shadow: 0 0 1px rgba(255,255,255,0.05);
  }

  /* ── Tables ── */
  .table-wrap {
    overflow-x: auto;
    margin: 20px 0;
    border-radius: var(--radius);
    border: 1px solid var(--border);
    box-shadow: var(--shadow-sm);
  }
  table { width: 100%; border-collapse: collapse; font-size: 14px; }
  thead tr { background: var(--surface-raised); }
  th {
    padding: 12px 16px;
    text-align: left;
    font-weight: 600;
    font-size: 12px;
    letter-spacing: 0.05em;
    color: var(--text-muted);
    text-transform: uppercase;
    border-bottom: 2px solid var(--border);
    white-space: nowrap;
  }
  td {
    padding: 12px 16px;
    border-bottom: 1px solid var(--border);
    vertical-align: top;
    color: var(--text-secondary);
    line-height: 1.6;
  }
  tr:last-child td { border-bottom: none; }
  tbody tr:hover { background: var(--accent-light); transition: background 0.1s; }

  /* ── Callouts ── */
  .callout {
    border-radius: var(--radius);
    padding: 16px 20px;
    margin: 20px 0;
    border: 1px solid var(--border);
    background: var(--surface);
    box-shadow: var(--shadow-sm);
    border-left: 4px solid var(--border-strong);
  }
  .callout.info {
    border-color: var(--accent-border);
    border-left-color: var(--accent);
    background: var(--accent-light);
  }
  .callout.warn {
    border-color: #ffd43b;
    border-left-color: var(--amber);
    background: var(--amber-light);
  }
  .callout p { color: var(--text-secondary); font-size: 14.5px; line-height: 1.7; }
  .callout-label {
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 8px;
  }

  /* ── Diagram ── */
  .diagram-wrap {
    border: 1px solid var(--border);
    border-radius: var(--radius);
    background: var(--surface);
    margin: 20px 0;
    overflow: hidden;
    box-shadow: var(--shadow-sm);
  }
  .diagram-toolbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 14px;
    background: var(--surface-raised);
    border-bottom: 1px solid var(--border);
    font-size: 11px;
    color: var(--text-muted);
    font-family: var(--font-mono);
    letter-spacing: 0.04em;
  }
  .diagram-controls { display: flex; gap: 4px; }
  .diagram-btn {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 4px 10px;
    font-size: 13px;
    cursor: pointer;
    color: var(--text-secondary);
    transition: all 0.15s;
    font-family: var(--font-mono);
    font-weight: 500;
  }
  .diagram-btn:hover { background: var(--accent); color: #fff; border-color: var(--accent); }
  .diagram-container { padding: 28px; overflow: auto; }
  .diagram-container pre.mermaid {
    background: none;
    border: none;
    padding: 0;
    display: flex;
    justify-content: center;
    transition: transform 0.15s ease;
    transform-origin: top center;
    box-shadow: none;
  }

  /* ── Modal ── */
  .modal-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.7);
    z-index: 1000;
    align-items: center;
    justify-content: center;
    backdrop-filter: blur(4px);
  }
  .modal-overlay.active { display: flex; }
  .modal-box {
    background: var(--surface);
    border-radius: var(--radius-lg);
    width: 92vw;
    max-width: 1100px;
    max-height: 88vh;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    box-shadow: var(--shadow-lg);
  }
  .modal-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 16px 22px;
    border-bottom: 1px solid var(--border);
    background: var(--surface-raised);
  }
  .modal-title { font-size: 12px; font-family: var(--font-mono); color: var(--text-muted); }
  .modal-close {
    background: none;
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    width: 30px;
    height: 30px;
    cursor: pointer;
    font-size: 18px;
    color: var(--text-secondary);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.1s;
  }
  .modal-close:hover { background: var(--surface-raised); }
  .modal-body {
    overflow: auto;
    padding: 36px;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    flex: 1;
  }
  .modal-body svg { max-width: 100%; height: auto; }

  /* ── Def grid ── */
  .def-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin: 20px 0; }
  @media (max-width: 600px) { .def-grid { grid-template-columns: 1fr; } }
  .def-item {
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 16px 18px;
    background: var(--surface);
    box-shadow: var(--shadow-sm);
    transition: box-shadow 0.15s, border-color 0.15s;
  }
  .def-item:hover { box-shadow: var(--shadow); border-color: var(--accent-border); }
  .def-key {
    font-family: var(--font-mono);
    font-size: 12px;
    color: var(--accent);
    margin-bottom: 6px;
    font-weight: 600;
  }
  .def-val { font-size: 14px; color: var(--text-secondary); line-height: 1.5; }

  /* ── Image wrap ── */
  .img-wrap { display: flex; gap: 28px; align-items: flex-start; margin: 20px 0; }
  .img-wrap img {
    max-width: 160px;
    border: 1px solid var(--border);
    border-radius: var(--radius);
    box-shadow: var(--shadow-sm);
  }
  .img-wrap .img-desc { flex: 1; }

  /* ── Footer ── */
  .doc-footer {
    border-top: 1px solid var(--border);
    padding-top: 28px;
    margin-top: 80px;
    font-size: 12px;
    color: var(--text-muted);
    font-family: var(--font-mono);
    display: flex;
    justify-content: space-between;
  }

  hr { border: none; border-top: 1px solid var(--border); margin: 48px 0; }
</style>
</head>
<body>
<div class="page">

  <header class="doc-header">
    <div class="doc-label">Tech Insight / AI 개발 생산성</div>
    <h1>AI 에이전트가 바꾸는 개발 방식</h1>
    <p>혼자서 기획부터 배포까지 운영하는 사이드 프로젝트에 AI 에이전트를 도입한 경험을 정리합니다. 외부 도구 기반이지만, 사내 규제 환경에서도 동일한 구조를 구현할 수 있습니다.</p>
    <div class="doc-meta">
      <span>2026-03-10</span>
      <span>작성: 프론트엔드 개발팀</span>
      <span>대상: 개발 관리자 / 기술 의사결정자</span>
    </div>
  </header>

  <nav class="toc">
    <div class="toc-title">Contents</div>
    <ol>
      <li><a href="#background">배경 — 개발자 1인의 세 가지 병목</a></li>
      <li><a href="#context">AI 컨텍스트 설계 — OpenSpec</a></li>
      <li><a href="#stack">풀스택 구현 — Next.js + Prisma + Supabase</a></li>
      <li><a href="#deploy">배포 파이프라인 — Vercel</a></li>
      <li><a href="#async">비동기 AI 에이전트 — Google Jules 시연</a></li>
      <li><a href="#inhouse">사내 적용 가능성 — 금융권 관점</a></li>
      <li><a href="#demo">실증 사례 — 독박게임</a></li>
      <li><a href="#matrix">도구 선택 가이드</a></li>
      <li><a href="#refs">참고 자료</a></li>
    </ol>
  </nav>

  <!-- ── 01 ── -->
  <section id="background">
    <span class="section-number">01</span>
    <h2>배경 — 개발자 1인의 세 가지 병목</h2>
    <p>업무에서는 프론트엔드만 담당하지만, 사이드 프로젝트는 DB 설계부터 배포까지 혼자 처리해야 합니다. 반복적으로 막히는 지점 세 가지가 있었고, 각각 다른 방식으로 해소했습니다.</p>

    <div class="table-wrap">
      <table>
        <thead><tr><th>병목</th><th>실제 상황</th><th>해소 방법</th></tr></thead>
        <tbody>
          <tr>
            <td><strong>전문 지식 부재</strong></td>
            <td>프론트엔드 전공이라 백엔드·DB·인프라는 아는 만큼만 설계됨. 모르는 부분은 구글링과 시행착오에 시간 소요</td>
            <td>AI가 프로젝트 맥락을 이미 아는 상태에서 코드 생성 → 리뷰만 하면 됨</td>
          </tr>
          <tr>
            <td><strong>AI 비용 부담</strong></td>
            <td>Claude·GPT는 긴 작업일수록 토큰 비용이 선형 증가. 복잡한 기능 하나에 예상보다 많은 비용 발생</td>
            <td>Google Jules는 태스크 건당 과금 → 작업 길이에 관계없이 비용 고정</td>
          </tr>
          <tr>
            <td><strong>가용 시간 부족</strong></td>
            <td>퇴근 후·이동 중에는 노트북을 열기 어려움. 개발 세션이 단절되면 컨텍스트 복구에도 시간이 듦</td>
            <td>AI가 백그라운드에서 작업 후 PR 생성 → 스마트폰으로 검토·승인만 하면 배포 완료</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="callout info">
      <div class="callout-label">이 글의 목적</div>
      <p>이 세 가지를 해결하기 위해 선택한 도구와 구조를 순서대로 설명합니다. 그리고 마지막에 <strong>"이 방식이 사내에서도 가능한가?"</strong>를 함께 생각해보려 합니다.</p>
    </div>
  </section>

  <!-- ── 02 ── -->
  <section id="context">
    <span class="section-number">02</span>
    <h2>AI 컨텍스트 설계 — OpenSpec</h2>
    <p>여러 AI 도구를 같은 프로젝트에서 쓸 때 가장 큰 낭비는 <strong>매번 "이 프로젝트가 어떤 프로젝트인지" 설명을 반복하는 것</strong>입니다. OpenSpec은 프로젝트의 스택·컨벤션·도메인 용어를 구조화된 문서로 미리 정의해두고, 모든 AI 에이전트가 공통으로 참조하게 합니다.</p>

    <div class="def-grid">
      <div class="def-item">
        <div class="def-key">project.md</div>
        <div class="def-val">스택, 폴더 구조, 코딩 컨벤션, 도메인 용어 정의. 모든 AI 에이전트의 온보딩 문서</div>
      </div>
      <div class="def-item">
        <div class="def-key">proposal.md</div>
        <div class="def-val">기능 단위 변경 제안서. 무엇을 왜 만드는지 사람과 AI가 같은 기준으로 이해</div>
      </div>
      <div class="def-item">
        <div class="def-key">spec.md</div>
        <div class="def-val">SHALL/MUST 형식의 요구 명세. AI가 구현 범위를 벗어나지 않도록 경계 설정</div>
      </div>
      <div class="def-item">
        <div class="def-key">보안 정책 포함</div>
        <div class="def-val">RLS 정책·인증 규칙을 컨텍스트에 넣으면, 매번 "보안 신경 써"라고 지시하지 않아도 에이전트가 항상 포함시킴</div>
      </div>
    </div>

    <h3>project.md 작성 예시 — 압축 영문형 권장</h3>
    <p><code>project.md</code>는 매 요청마다 컨텍스트 윈도우에 통째로 들어갑니다. 한국어 설명체로 쓰면 토큰이 약 4배 소비되고, LLM 파싱 품질도 오히려 떨어집니다.</p>

<pre><code>## stack
fw:next16-approuter sc-first; orm:prisma migration-files;
db:supabase-pg rls-enabled; auth:supabase google-oauth;
deploy:vercel main=prod feature/*=preview

## conventions
comp:src/components/ api:app/api/ route-handlers;
env:vercel-env-pull→.env.local; lang:ko-docs en-code

## domain
game=dokbakgame; score-table=game_scores(userId,score,gameName);
rls:insert=own select=public</code></pre>

    <div class="callout warn">
      <div class="callout-label">토큰 비교</div>
      <p>같은 내용을 한국어 설명체로 쓰면 <strong>~180 tokens</strong>, 위 압축 영문형은 <strong>~45 tokens</strong>입니다. 압축할수록 같은 컨텍스트 윈도우에 더 많은 코드 맥락을 넣을 수 있습니다. <code>project.md</code>는 사람이 아니라 LLM이 읽는 문서입니다.</p>
    </div>
  </section>

  <!-- ── 03 ── -->
  <section id="stack">
    <span class="section-number">03</span>
    <h2>풀스택 구현 — Next.js + Prisma + Supabase</h2>
    <p>프론트엔드 개발자가 백엔드·DB까지 혼자 운영하기 위한 스택입니다. 각 레이어가 서로 타입을 공유하고, AI 에이전트가 전체 구조를 한 번에 파악할 수 있도록 단순하게 구성했습니다.</p>

    <h3>스택 선택 근거</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>레이어</th><th>선택</th><th>이유</th></tr></thead>
        <tbody>
          <tr><td>프레임워크</td><td><strong>Next.js 16 App Router</strong></td><td>Server Component로 프론트·API를 같은 코드베이스에서 처리. Vercel 네이티브 배포</td></tr>
          <tr><td>ORM</td><td><strong>Prisma</strong></td><td>schema.prisma 파일 하나로 DB 구조·타입·마이그레이션 이력을 통합 관리. AI가 스키마만 보고 전체 DB 구조를 파악 가능</td></tr>
          <tr><td>DB</td><td><strong>Supabase (PostgreSQL)</strong></td><td>RLS(Row Level Security)로 DB 레이어에서 행 단위 접근 제어. API에서 실수로 필터를 빠뜨려도 DB가 차단</td></tr>
          <tr><td>인증</td><td><strong>Supabase Auth</strong></td><td>Google OAuth 내장, JWT 자동 관리. auth.uid()가 RLS 정책과 직접 연결</td></tr>
        </tbody>
      </table>
    </div>

    <h3>Vercel이 지원하는 DB 옵션</h3>
    <p>모두 <code>vercel env pull</code> 한 줄로 연결 문자열이 로컬에 자동 동기화됩니다. 이 프로젝트는 RLS 기반 보안 정책이 필요해 Supabase를 선택했습니다.</p>
    <div class="table-wrap">
      <table>
        <thead><tr><th>DB</th><th>특징</th><th>적합한 경우</th></tr></thead>
        <tbody>
          <tr><td><strong>Supabase</strong></td><td>PostgreSQL + RLS + Auth 내장</td><td>인증·보안 정책이 중요한 앱</td></tr>
          <tr><td><strong>Neon</strong></td><td>서버리스 PostgreSQL, 브랜치별 DB 스냅샷</td><td>PR 환경마다 독립 DB를 쓰고 싶을 때</td></tr>
          <tr><td><strong>PlanetScale</strong></td><td>MySQL 호환, 무중단 스키마 변경</td><td>대규모 트래픽, zero-downtime 마이그레이션</td></tr>
          <tr><td><strong>Upstash</strong></td><td>서버리스 Redis / Kafka</td><td>세션 캐시, 큐, 실시간 카운터</td></tr>
        </tbody>
      </table>
    </div>

    <h3>Prisma 스키마 — AI가 DB 구조를 읽는 단일 소스</h3>
    <p>AI 에이전트는 <code>schema.prisma</code>를 컨텍스트로 받으면 DB 전체 구조를 즉시 파악합니다. 별도로 "테이블 구조가 이렇습니다"를 설명할 필요가 없습니다.</p>

<pre><code>// prisma/schema.prisma
datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")   // PgBouncer 경유 (Vercel 서버리스용)
  directUrl = env("DIRECT_URL")     // 직접 연결 (migration 실행 전용)
}

model GameScore {
  id        String   @id @default(cuid())
  userId    String
  gameName  String
  score     Int
  createdAt DateTime @default(now())

  user User @relation(fields: [userId], references: [id])
  @@index([gameName, score(sort: Desc)])
}</code></pre>

    <div class="callout">
      <div class="callout-label">Connection Pooling 분리 이유</div>
      <p>Vercel 서버리스는 요청마다 새 DB 커넥션을 만듭니다. PgBouncer(Supabase 내장)가 커넥션을 재사용해 DB 연결 한도 초과를 방지합니다. <code>DATABASE_URL</code>은 PgBouncer 경유, <code>DIRECT_URL</code>은 migration 전용으로 분리합니다.</p>
    </div>

    <h3>Copilot Agent — 멀티 파일 한 번에 생성</h3>
    <p>에디터에서 직접 코딩할 때는 Copilot Agent를 씁니다. <code>project.md</code>를 컨텍스트로 주면, 프롬프트 하나로 프론트·DB·보안·API를 동시에 뽑아냅니다.</p>

<pre><code>@workspace openspec/project.md 참고해서 아래 기능 구현해줘:
- src/components/GameResult.tsx  (점수 저장 UI, 클라이언트 컴포넌트)
- prisma/migrations/             (game_scores 테이블 + 인덱스)
- supabase/policies/rls.sql      (본인 점수만 INSERT, 랭킹은 전체 SELECT)
- app/api/scores/route.ts        (POST / GET API Route)</code></pre>

    <div class="callout info">
      <p>이 방식의 핵심은 <strong>"보안 신경 써"라고 매번 지시하지 않아도 된다</strong>는 점입니다. <code>project.md</code>에 RLS 정책이 명시되어 있으면 에이전트가 항상 포함시킵니다.</p>
    </div>
  </section>

  <!-- ── 04 ── -->
  <section id="deploy">
    <span class="section-number">04</span>
    <h2>배포 파이프라인 — Vercel</h2>
    <p>Vercel은 GitHub 저장소와 연결하면 브랜치 전략이 곧 배포 전략이 됩니다. 별도 CI 설정 없이 PR 하나만 올려도 프리뷰 환경이 자동으로 만들어집니다.</p>

    <h3>브랜치 → 배포 환경 자동 매핑</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>브랜치 / 이벤트</th><th>배포 환경</th><th>URL</th><th>활용</th></tr></thead>
        <tbody>
          <tr><td><code>main</code></td><td>Production</td><td><code>dokbakgame.vercel.app</code></td><td>실사용자 서비스</td></tr>
          <tr><td><code>feature/*</code> push</td><td>Preview</td><td><code>dokbakgame-git-feature-xxx.vercel.app</code></td><td>개발 중 확인</td></tr>
          <tr><td>Pull Request 생성</td><td>Preview</td><td>PR 본문에 URL 자동 첨부</td><td><strong>모바일에서 UI 검토 후 Merge</strong></td></tr>
        </tbody>
      </table>
    </div>

    <div class="callout info">
      <div class="callout-label">이 구조가 핵심입니다</div>
      <p>Jules가 PR을 올리면 → Vercel이 프리뷰 URL을 PR 본문에 자동 삽입 → 스마트폰으로 UI 확인 → Approve → 프로덕션 자동 배포. <strong>노트북 없이도 완전한 개발·검토·배포 사이클이 돌아갑니다.</strong></p>
    </div>

    <h3>환경 변수 관리</h3>
    <p>Vercel 대시보드에 등록한 환경 변수는 CLI 한 줄로 로컬에 동기화됩니다. 팀원이 합류해도 별도 설정 파일 공유가 불필요합니다.</p>

<pre><code>vercel link                  # 로컬 프로젝트 ↔ Vercel 프로젝트 연결
vercel env pull .env.local   # 프로덕션 환경 변수 로컬 동기화
npx prisma migrate dev       # DB 스키마 변경 → migration 파일 생성 + 적용</code></pre>

    <h3>전체 흐름 한눈에 보기</h3>
    <div class="diagram-wrap">
      <div class="diagram-toolbar">
        <span>diagram / workflow-overview</span>
        <div class="diagram-controls">
          <button class="diagram-btn" onclick="zoomDiagram(this,-0.2)">&#8722;</button>
          <button class="diagram-btn" onclick="zoomDiagram(this,0)">reset</button>
          <button class="diagram-btn" onclick="zoomDiagram(this,0.2)">&#43;</button>
          <button class="diagram-btn" onclick="expandDiagram(this)">expand</button>
        </div>
      </div>
      <div class="diagram-container" data-scale="1">
        <pre class="mermaid">
flowchart LR
    A[OpenSpec\nproject.md] --> B[Next.js 16\nApp Router]
    B --> C[Prisma ORM\nschema.prisma]
    C --> D[Supabase\nPostgreSQL + RLS]
    B --> E[Vercel\nCI/CD]
    E --> F{작업 환경}
    F -->|로컬 + 에디터| G[Copilot Agent\n즉시 코딩]
    F -->|이동 중 / 비동기| H[Jules\n클라우드 위임]
    G --> I[GitHub PR]
    H --> I
    I --> J[Vercel\n프리뷰 자동 생성]
    J -->|모바일 Approve| K[Production\n자동 배포]
        </pre>
      </div>
    </div>
  </section>

  <!-- ── 05 ── -->
  <section id="async">
    <span class="section-number">05</span>
    <h2>비동기 AI 에이전트 — Google Jules 시연</h2>
    <p>지금까지 설명한 스택 위에서, <strong>노트북 없이 출근길에 기능을 배포하는 흐름</strong>을 실제로 보여드립니다.</p>

    <h3>기존 AI 도구와 Jules의 차이</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>항목</th><th>기존 AI 도구 (Copilot 등)</th><th>Jules</th></tr></thead>
        <tbody>
          <tr><td>실행 위치</td><td>개발자 PC</td><td>Google 클라우드 샌드박스</td></tr>
          <tr><td>개발자 PC 필요</td><td>필수</td><td><strong>불필요</strong></td></tr>
          <tr><td>비용 구조</td><td>입출력 토큰 수 비례 — 작업이 길수록 증가</td><td><strong>태스크 건당 고정</strong></td></tr>
          <tr><td>작업 범위</td><td>열려있는 파일 위주</td><td>저장소 전체</td></tr>
          <tr><td>결과 전달</td><td>에디터에 직접 반영</td><td>GitHub PR + Vercel 프리뷰 URL</td></tr>
        </tbody>
      </table>
    </div>

    <h3>시연 — 이슈 발생부터 배포까지</h3>
    <p>지금 GitHub에 이슈를 하나 만들고, Jules에 위임합니다. Jules가 코드를 수정해 PR을 올리고, Vercel이 프리뷰 URL을 생성하면 스마트폰으로 확인 후 Merge합니다.</p>

    <div class="table-wrap">
      <table>
        <thead><tr><th>단계</th><th>행동</th><th>처리 주체</th></tr></thead>
        <tbody>
          <tr><td>① 이슈 생성</td><td>GitHub에 이슈 작성 + <code>jules</code> 라벨 부착 (또는 jules.google.com에서 직접 지시)</td><td>개발자 (1분)</td></tr>
          <tr><td>② 클라우드 처리</td><td>저장소 클론 → Prisma 스키마 수정 → 마이그레이션 생성 → 컴포넌트 업데이트</td><td><strong>Jules 자동 (~15분)</strong></td></tr>
          <tr><td>③ PR + 프리뷰</td><td>PR 생성 + Vercel 프리뷰 URL 자동 첨부</td><td>Jules + Vercel 자동</td></tr>
          <tr><td>④ 모바일 검토</td><td>GitHub 모바일로 diff 확인 → 프리뷰 URL로 UI 검토 → Approve</td><td>개발자 (5분)</td></tr>
          <tr><td>⑤ 배포</td><td>main 브랜치 Merge → 프로덕션 자동 배포</td><td>Vercel 자동</td></tr>
        </tbody>
      </table>
    </div>

    <h3>Jules가 처리하는 작업 범위</h3>
    <div class="def-grid">
      <div class="def-item">
        <div class="def-key">Scheduled Tasks</div>
        <div class="def-val">매주 월요일 새벽 npm 보안 패치 자동 실행 후 PR 생성</div>
      </div>
      <div class="def-item">
        <div class="def-key">Issue → PR</div>
        <div class="def-val">GitHub 이슈에 <code>jules</code> 라벨만 붙이면 Jules가 읽고 구현 PR 생성</div>
      </div>
      <div class="def-item">
        <div class="def-key">CI Fixer</div>
        <div class="def-val">본인이 올린 PR의 CI가 실패하면 스스로 원인 분석 후 수정·재제출</div>
      </div>
      <div class="def-item">
        <div class="def-key">모델 수준 (2026.01)</div>
        <div class="def-val">Gemini 3 Flash 기준 — 라우팅 구조 파악, DB 연동 PR까지 처리. 체감상 주니어 개발자 수준. 24시간 일하고, 월급이 없으며, 내 토큰을 쓰지 않습니다</div>
      </div>
    </div>

    <div class="callout info">
      <div class="callout-label">다음 단계 — Cursor Automations (2026.03.05 출시)</div>
      <p>Jules가 "지시를 받아야 움직이는" 에이전트라면, <strong>Cursor Automations</strong>는 지시 없이도 이벤트(빌드 실패, PR 오픈, Cron 등)에 반응해 스스로 PR을 생성합니다. 빌드가 자동 복구되고, 보안 리뷰 코멘트가 자동으로 달리며, 테스트가 채워집니다. Jules와 함께 쓰면 개발자는 Approve만으로 개발 사이클을 완결할 수 있습니다.</p>
    </div>
  </section>

  <!-- ── 06 ── -->
  <section id="inhouse">
    <span class="section-number">06</span>
    <h2>사내 적용 가능성 — 금융권 관점</h2>
    <p>지금까지 보여드린 방식은 모두 외부 SaaS 기반입니다. 그렇다면 전자금융거래법·망분리 규제가 있는 금융권에서는 어떻게 할 수 있을까요?</p>
    <p>규제가 막는 것은 <strong>"외부 서비스에 코드를 전송하는 행위"</strong>이지, 비동기 AI 에이전트 구조 자체가 아닙니다. Jules의 아키텍처는 사내망 안에서 동일하게 구현할 수 있습니다.</p>

    <h3>외부 → 사내 대체 매핑</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>외부 서비스 (Jules)</th><th>사내 대체 구현</th><th>난이도</th></tr></thead>
        <tbody>
          <tr><td>Google 클라우드 샌드박스</td><td>사내망 격리 에이전트 서버</td><td>Medium</td></tr>
          <tr><td>Gemini 모델</td><td>온프레미스 LLM (Ollama, vLLM, 사내 GPU 서버)</td><td>Medium~High</td></tr>
          <tr><td>GitHub 저장소 연동</td><td>사내 Bitbucket 웹훅</td><td>Low</td></tr>
          <tr><td>Vercel 프리뷰 배포</td><td>사내 Jenkins / Jarvis 파이프라인</td><td>Low (기존 인프라 활용)</td></tr>
          <tr><td>jules.google.com 대시보드</td><td>사내 에이전트 관제 UI</td><td>Medium</td></tr>
        </tbody>
      </table>
    </div>

    <h3>사내 구현 구조</h3>
    <div class="diagram-wrap">
      <div class="diagram-toolbar">
        <span>diagram / inhouse-agent-architecture</span>
        <div class="diagram-controls">
          <button class="diagram-btn" onclick="zoomDiagram(this,-0.2)">&#8722;</button>
          <button class="diagram-btn" onclick="zoomDiagram(this,0)">reset</button>
          <button class="diagram-btn" onclick="zoomDiagram(this,0.2)">&#43;</button>
          <button class="diagram-btn" onclick="expandDiagram(this)">expand</button>
        </div>
      </div>
      <div class="diagram-container" data-scale="1">
        <pre class="mermaid">
flowchart TD
    A[개발자 지시\n메신저 / 웹 UI] --> B[에이전트 오케스트레이터\n사내망 서버]
    B --> C[온프레미스 LLM\nOllama / vLLM]
    B --> D[사내 Bitbucket\nPR 생성]
    D --> E[사내 CI/CD\nJenkins / Jarvis]
    E --> F[스테이징 서버\n격리 환경 배포]
    F --> G[개발자 검토\nPR + 프리뷰 URL]
    G -->|Approve| H[운영 반영]
        </pre>
      </div>
    </div>

    <h3>도입 시 기대 효과</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>업무 영역</th><th>현재 방식</th><th>AI 에이전트 적용 후</th><th>예상 절감</th></tr></thead>
        <tbody>
          <tr><td>반복 코드 생성</td><td>개발자 직접 작성 (수시간)</td><td>에이전트 위임 → 검토만 (30분)</td><td>공수 70~80% 절감</td></tr>
          <tr><td>레거시 테스트 커버리지</td><td>일정 우선순위에 밀려 미진행</td><td>야간 Cron으로 자동 생성·PR</td><td>별도 공수 불필요</td></tr>
          <tr><td>보안 패치 (npm audit)</td><td>분기별 수동 작업</td><td>주간 자동 PR + 담당자 알림</td><td>보안 대응 시간 단축</td></tr>
          <tr><td>코드 리뷰 1차 검토</td><td>시니어 개발자 시간 소모</td><td>에이전트 인라인 코멘트 선제공</td><td>리뷰 사이클 단축</td></tr>
        </tbody>
      </table>
    </div>

    <div class="callout warn">
      <div class="callout-label">도입 시 고려사항</div>
      <p>온프레미스 LLM은 외부 상용 모델 대비 코딩 품질이 낮을 수 있습니다. 초기에는 <strong>반복성이 높고 검증이 쉬운 작업</strong>(보일러플레이트, 테스트, 의존성 업데이트)부터 파일럿을 진행하고, 모델 품질과 ROI를 함께 검증하는 접근이 현실적입니다.</p>
    </div>

    <div class="callout info">
      <p><strong>전금법 준수 + AI 효율화는 상충 관계가 아닙니다.</strong> "외부 SaaS를 쓰지 않으면서 같은 패러다임을 구현"하는 것이 핵심 설계 과제입니다.</p>
    </div>
  </section>

  <!-- ── 07 ── -->
  <section id="demo">
    <span class="section-number">07</span>
    <h2>실증 사례 — 독박게임</h2>
    <p>이 워크플로우를 실제로 적용해 운영 중인 프로젝트입니다. 기획·개발·배포를 1인이 처리하면서 AI 에이전트를 통해 사실상 팀 개발 수준의 속도를 유지하고 있습니다.</p>

    <div class="img-wrap">
      <img src="22kBv.png" alt="독박게임 QR 코드" />
      <div class="img-desc">
        <p><strong><a href="https://dokbakgame.vercel.app" target="_blank">dokbakgame.vercel.app</a></strong></p>
        <ul>
          <li>실시간 멀티플레이 보드게임 — 1인 개발·운영</li>
          <li>Next.js App Router + Prisma + Supabase (PostgreSQL + RLS)</li>
          <li>Jules로 기능 위임 → PR → 모바일 Approve → 자동 배포 흐름 실사용 중</li>
          <li>Copilot Agent로 DB 스키마 변경·보안 정책을 한 프롬프트로 생성</li>
          <li>Cursor Automations(2026.03 출시)로 빌드 실패 자동 감지·수정 가능성 확인 중</li>
        </ul>
      </div>
    </div>

    <div class="callout info">
      <p>이 문서 자체도 동일한 방식으로 작성되었습니다. 초안 작성·구조 편집·스타일 조정 모두 AI 에이전트와 협업하여 진행했습니다.</p>
    </div>
  </section>

  <!-- ── 08 ── -->
  <section id="matrix">
    <span class="section-number">08</span>
    <h2>도구 선택 가이드</h2>
    <p>상황과 목적에 따라 적합한 도구가 다릅니다.</p>

    <div class="table-wrap">
      <table>
        <thead><tr><th>상황</th><th>적합한 도구</th><th>핵심 가치</th></tr></thead>
        <tbody>
          <tr><td>프로젝트 착수 / AI 컨텍스트 구성</td><td>OpenSpec</td><td>모든 AI 도구의 공통 지식 베이스 확립</td></tr>
          <tr><td>에디터에서 즉각적인 코딩 지원</td><td>GitHub Copilot Agent</td><td>멀티 파일 동시 생성, 즉각 피드백</td></tr>
          <tr><td>자리를 비운 상태에서 기능 개발 위임</td><td>Google Jules</td><td>비동기 실행, 태스크 건당 고정 비용</td></tr>
          <tr><td>이벤트 기반 상시 자동화 (Next Step)</td><td>Cursor Automations <span style="font-size:11px;color:var(--text-muted);">2026.03 출시</span></td><td>빌드 실패·PR 오픈 등 트리거에 반응해 에이전트가 자동 실행 — 별도 지시 불필요</td></tr>
          <tr><td>사내망 환경 (망분리)</td><td>사내 에이전트 서버 + 온프레미스 LLM</td><td>Jules 아키텍처를 내부 인프라로 구현</td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <!-- ── 09 ── -->
  <section id="refs">
    <span class="section-number">09</span>
    <h2>참고 자료</h2>
    <div class="table-wrap">
      <table>
        <thead><tr><th>항목</th><th>링크</th></tr></thead>
        <tbody>
          <tr><td>Google Jules 공식 문서</td><td><a href="https://jules.google/docs" target="_blank">jules.google/docs</a></td></tr>
          <tr><td>Jules 업데이트 내역</td><td><a href="https://jules.google/docs/changelog" target="_blank">jules.google/docs/changelog</a></td></tr>
          <tr><td>Cursor Automations 문서</td><td><a href="https://cursor.com/docs/cloud-agent/automations" target="_blank">cursor.com/docs/cloud-agent/automations</a></td></tr>
          <tr><td>OpenSpec</td><td><a href="https://openspec.dev" target="_blank">openspec.dev</a></td></tr>
          <tr><td>Supabase RLS 가이드</td><td><a href="https://supabase.com/docs/guides/database/postgres/row-level-security" target="_blank">supabase.com — Row Level Security</a></td></tr>
          <tr><td>Prisma 공식 문서</td><td><a href="https://www.prisma.io/docs" target="_blank">prisma.io/docs</a></td></tr>
          <tr><td>Vercel 배포 가이드</td><td><a href="https://vercel.com/docs" target="_blank">vercel.com/docs</a></td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <footer class="doc-footer">
    <span>AI 에이전트가 바꾸는 개발 방식</span>
    <span>2026-03-10</span>
  </footer>

</div>

<!-- Modal -->
<div class="modal-overlay" id="diagramModal">
  <div class="modal-box">
    <div class="modal-header">
      <span class="modal-title" id="modalTitle">diagram</span>
      <button class="modal-close" onclick="closeModal()">&#215;</button>
    </div>
    <div class="modal-body" id="modalBody"></div>
  </div>
</div>

<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
  mermaid.initialize({
    startOnLoad: true,
    theme: 'neutral',
    themeVariables: {
      fontFamily: 'JetBrains Mono, Fira Code, Consolas, monospace',
      fontSize: '13px',
      primaryColor: '#f4f4f5',
      primaryBorderColor: '#c8c8c8',
      primaryTextColor: '#1a1a1a',
      lineColor: '#888',
      edgeLabelBackground: '#fafafa'
    }
  });
</script>

<script>
  function zoomDiagram(btn, delta) {
    const container = btn.closest('.diagram-wrap').querySelector('.diagram-container');
    const pre = container.querySelector('pre.mermaid');
    let scale = parseFloat(container.dataset.scale) || 1;
    scale = delta === 0 ? 1 : Math.min(3, Math.max(0.4, scale + delta));
    container.dataset.scale = scale;
    pre.style.transform = 'scale(' + scale + ')';
    pre.style.transformOrigin = 'top center';
  }

  function expandDiagram(btn) {
    const wrap = btn.closest('.diagram-wrap');
    const label = wrap.querySelector('.diagram-toolbar span').textContent;
    const svgEl = wrap.querySelector('pre.mermaid svg');
    if (!svgEl) return;
    document.getElementById('modalTitle').textContent = label;
    const body = document.getElementById('modalBody');
    body.innerHTML = '';
    const clone = svgEl.cloneNode(true);
    clone.removeAttribute('width');
    clone.removeAttribute('height');
    clone.style.width = '100%';
    clone.style.height = 'auto';
    body.appendChild(clone);
    document.getElementById('diagramModal').classList.add('active');
    document.body.style.overflow = 'hidden';
  }

  function closeModal() {
    document.getElementById('diagramModal').classList.remove('active');
    document.body.style.overflow = '';
  }

  document.getElementById('diagramModal').addEventListener('click', function(e) {
    if (e.target === this) closeModal();
  });

  document.addEventListener('keydown', function(e) {
    if (e.key === 'Escape') closeModal();
  });

  const sections = document.querySelectorAll('section[id]');
  const tocLinks = document.querySelectorAll('.toc a');
  const observer = new IntersectionObserver(function(entries) {
    entries.forEach(function(entry) {
      if (entry.isIntersecting) {
        tocLinks.forEach(function(a) {
          a.classList.toggle('active', a.getAttribute('href') === '#' + entry.target.id);
        });
      }
    });
  }, { rootMargin: '-20% 0px -70% 0px' });
  sections.forEach(function(s) { observer.observe(s); });
</script>
</body>
</html>
