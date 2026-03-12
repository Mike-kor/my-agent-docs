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
    <p>혼자서 기획부터 배포까지 운영하는 사이드 프로젝트에 AI 에이전트를 도입한 경험을 정리했습니다. 외부 도구 기반이지만, 사내 규제 환경에서도 동일한 구조를 구현할 수 있습니다.</p>
    <div class="doc-meta">
      <span>2026-03-10</span>
      <span>작성: 프론트엔드 개발팀</span>
      <span>대상: 개발 관리자 / 기술 의사결정자</span>
    </div>
  </header>

  <nav class="toc">
    <div class="toc-title">Contents</div>
    <ol>
      <li><a href="#background">문제 인식 — 개발자 1인의 병목</a></li>
      <li><a href="#paradigm">패러다임 전환 — 무엇이 달라지나</a></li>
      <li><a href="#workflow">전체 흐름 한눈에 보기</a></li>
      <li><a href="#context">핵심 원리 — 컨텍스트 설계</a></li>
      <li><a href="#async">비동기 AI 에이전트 — Google Jules</a></li>
      <li><a href="#inhouse">사내 적용 가능성 — 금융권 관점</a></li>
      <li><a href="#demo">실증 사례 — 독박게임</a></li>
      <li><a href="#matrix">도구 선택 가이드</a></li>
      <li><a href="#refs">참고 자료</a></li>
    </ol>
  </nav>

  <!-- ── 01 ── -->
  <section id="background">
    <span class="section-number">01</span>
    <h2>문제 인식 — 개발자 1인의 병목</h2>
    <p>업무에서는 프론트엔드만 담당하지만, 사이드 프로젝트는 DB 설계부터 배포까지 혼자 처리해야 합니다. 반복적으로 막히는 지점 세 가지가 있었고, 각각 다른 방식으로 해소했습니다.</p>

    <div class="table-wrap">
      <table>
        <thead><tr><th>병목</th><th>실제 상황</th><th>해소 방법</th></tr></thead>
        <tbody>
          <tr>
            <td><strong>전문 지식 부재</strong></td>
            <td>백엔드·DB·인프라 영역은 아는 만큼만 설계됨. 모르는 부분은 구글링과 시행착오에 시간이 소요</td>
            <td>AI가 프로젝트 맥락을 이미 알고 있는 상태에서 코드 생성 → 리뷰만 하면 됨</td>
          </tr>
          <tr>
            <td><strong>AI 비용 부담</strong></td>
            <td>Claude·GPT는 긴 작업일수록 토큰 비용이 선형 증가. 복잡한 기능 하나에 예상보다 많은 비용 발생</td>
            <td>Google Jules는 태스크 건당 과금 → 작업 길이에 관계없이 비용 고정</td>
          </tr>
          <tr>
            <td><strong>가용 시간 부족</strong></td>
            <td>퇴근 후·이동 중에는 노트북을 열기 어려움. 개발 세션이 단절되면 컨텍스트 복구에도 시간이 듦</td>
            <td>AI가 백그라운드에서 작업 후 PR 생성 → 스마트폰으로 검토·승인만 하면 배포까지 완료</td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>

  <!-- ── 02 ── -->
  <section id="paradigm">
    <span class="section-number">02</span>
    <h2>패러다임 전환 — 무엇이 달라지나</h2>
    <p>AI 코딩 도구의 진화는 단순한 자동완성 수준을 넘어, <strong>개발자가 코드를 직접 작성하는 방식 자체</strong>를 바꾸고 있습니다.</p>

    <div class="def-grid">
      <div class="def-item">
        <div class="def-key">이전 — 동기적 개발</div>
        <div class="def-val">개발자가 에디터에서 직접 코드를 작성하고, 커밋하고, 배포까지 모든 단계를 순차적으로 처리</div>
      </div>
      <div class="def-item">
        <div class="def-key">현재 — 에이전트 위임</div>
        <div class="def-val">개발자가 요구사항을 지시하면 AI 에이전트가 코드 작성·PR 생성까지 처리. 개발자는 검토 후 Merge만</div>
      </div>
      <div class="def-item">
        <div class="def-key">다음 단계 — 이벤트 드리븐</div>
        <div class="def-val">지시 없이도 빌드 실패·보안 취약점·TODO 주석 등 트리거에 반응해 에이전트가 자동으로 PR 생성</div>
      </div>
      <div class="def-item">
        <div class="def-key">개발자 역할 변화</div>
        <div class="def-val">"코드를 타이핑하는 사람"에서 "에이전트가 올바르게 움직이도록 맥락과 구조를 설계하는 사람"으로</div>
      </div>
    </div>

    <div class="callout info">
      <div class="callout-label">핵심 인사이트</div>
      <p>생산성 향상의 크기는 <strong>얼마나 좋은 프롬프트를 쓰느냐</strong>가 아니라, <strong>얼마나 잘 정의된 맥락을 AI에게 제공하느냐</strong>에 비례합니다. 이 점이 팀 단위 도입 시 가장 중요한 설계 원칙입니다.</p>
    </div>
  </section>

  <!-- ── 03 ── -->
  <section id="workflow">
    <span class="section-number">03</span>
    <h2>전체 흐름 한눈에 보기</h2>
    <p>각 도구는 독립적으로 사용 가능하지만, 연결할수록 사람의 개입이 줄고 자동화 범위가 넓어집니다.</p>

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
    A[컨텍스트 설계\nOpenSpec] --> B[프로젝트 기반\nNext.js + Supabase]
    B --> C[자동 배포\nVercel CI/CD]
    C --> D{작업 환경}
    D -->|로컬 + 에디터| E[즉시 코딩\nCopilot Agent]
    D -->|이동 중 / 비동기| F[클라우드 위임\nJules]
    E --> G[GitHub PR]
    F --> G
    G --> H[자동 배포\nVercel]
        </pre>
      </div>
    </div>

    <div class="callout">
      <p>이 흐름의 핵심은 <strong>개발자가 자리를 비워도 개발 사이클이 멈추지 않는다</strong>는 점입니다. 퇴근 후 Jules에게 작업을 위임하면, 출근 전에 PR이 생성되고 배포 미리보기까지 준비되어 있습니다.</p>
    </div>
  </section>

  <!-- ── 04 ── -->
  <section id="context">
    <span class="section-number">04</span>
    <h2>핵심 원리 — 컨텍스트 설계 (OpenSpec)</h2>
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
        <div class="def-val">SHALL/MUST 형식의 요구 명세. AI가 구현 범위를 벗어나지 않도록 경계를 설정</div>
      </div>
      <div class="def-item">
        <div class="def-key">보안 정책 포함</div>
        <div class="def-val">RLS 정책·인증 규칙을 컨텍스트에 넣으면, 매번 "보안 신경 써"라고 지시하지 않아도 에이전트가 항상 포함시킴</div>
      </div>
    </div>

    <div class="callout warn">
      <div class="callout-label">실무 팁</div>
      <p>컨텍스트 문서는 <strong>영문 압축형</strong>으로 작성하는 게 효과적입니다. 같은 내용을 한국어 설명체로 쓰면 토큰이 약 4배 소비되고, LLM이 의도를 잘못 해석하는 경우도 생깁니다. "Noto Sans KR을 기본 폰트로 사용합니다"보다 <code>font:noto-sans-kr</code>이 더 정확하고 빠릅니다.</p>
    </div>
  </section>

  <!-- ── 05 ── -->
  <section id="async">
    <span class="section-number">05</span>
    <h2>비동기 AI 에이전트 — Google Jules</h2>
    <p>기존 AI 코딩 도구는 개발자가 에디터 앞에 앉아 있어야 작동합니다. Jules는 다릅니다. 지시만 내리면 Google 클라우드에서 저장소를 직접 클론해 코드를 수정하고, PR을 생성해 결과를 보고합니다.</p>

    <h3>기존 방식 vs. Jules</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>항목</th><th>기존 AI 도구 (Copilot 등)</th><th>Jules</th></tr></thead>
        <tbody>
          <tr><td>실행 위치</td><td>개발자 PC</td><td>Google 클라우드 샌드박스</td></tr>
          <tr><td>개발자 PC 필요 여부</td><td>필수</td><td><strong>불필요</strong></td></tr>
          <tr><td>비용 구조</td><td>입출력 토큰 수 비례 — 작업이 길수록 비용 증가</td><td><strong>태스크 건당 고정</strong></td></tr>
          <tr><td>작업 범위</td><td>현재 열려있는 파일 위주</td><td>저장소 전체</td></tr>
          <tr><td>결과 전달</td><td>에디터에 직접 반영</td><td>GitHub PR + Vercel 프리뷰 URL</td></tr>
        </tbody>
      </table>
    </div>

    <h3>실제 사용 시나리오 — 출근길 30분</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>시간</th><th>행동</th><th>처리 주체</th></tr></thead>
        <tbody>
          <tr><td>09:00 출근길</td><td>스마트폰에서 Jules에 작업 지시: "닉네임 컬럼 추가, Prisma 마이그레이션 포함"</td><td>개발자 (30초)</td></tr>
          <tr><td>09:00~09:20</td><td>DB 스키마 수정 → 마이그레이션 파일 생성 → 컴포넌트 업데이트 → PR 생성</td><td><strong>Jules 자동</strong></td></tr>
          <tr><td>09:20 도착 전</td><td>GitHub 모바일로 diff 확인, Vercel 프리뷰 URL로 UI 검토, Approve → 자동 배포</td><td>개발자 (5분)</td></tr>
        </tbody>
      </table>
    </div>

    <h3>Jules가 자동으로 처리하는 작업들</h3>
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
        <div class="def-val">본인이 올린 PR의 CI 테스트가 실패하면 스스로 원인 분석 후 재제출</div>
      </div>
      <div class="def-item">
        <div class="def-key">모델 수준 (2026.01)</div>
        <div class="def-val">Gemini 3 Flash 기준 — 라우팅 구조 파악, DB 연동 PR까지 스스로 처리. 체감상 주니어 개발자 수준</div>
      </div>
    </div>

    <div class="callout info">
      <div class="callout-label">다음 단계 — Cursor Automations (2026.03.05 출시)</div>
      <p>Jules가 "지시를 받아야 움직이는" 에이전트라면, <strong>Cursor Automations</strong>는 지시 없이도 이벤트(빌드 실패, PR 오픈, Cron 등)에 반응해 스스로 PR을 생성합니다. 개발자가 아무것도 하지 않아도 빌드가 복구되고, 보안 리뷰 코멘트가 달리며, 테스트가 채워집니다. Jules와 함께 쓰면 개발자는 Approve만으로 개발 사이클을 완결할 수 있습니다.</p>
    </div>
  </section>

  <!-- ── 06 ── -->
  <section id="inhouse">
    <span class="section-number">06</span>
    <h2>사내 적용 가능성 — 금융권 관점</h2>
    <p>전자금융거래법·망분리 규제로 외부 AI 서비스에 코드를 직접 전송할 수 없는 환경입니다. 그러나 규제가 막는 것은 <strong>"외부 SaaS 사용"이지, 비동기 AI 에이전트 구조 자체가 아닙니다.</strong> Jules의 아키텍처는 사내망 안에서 동일하게 구현 가능합니다.</p>

    <h3>외부 → 사내 매핑</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>외부 서비스 (Jules)</th><th>사내 대체 구현</th><th>기술적 난이도</th></tr></thead>
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
