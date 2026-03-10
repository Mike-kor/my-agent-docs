<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI 에이전트 기반 풀스택 개발 워크플로우</title>
<style>
  :root {
    --bg: #fafafa;
    --surface: #ffffff;
    --border: #e2e2e2;
    --border-strong: #c8c8c8;
    --text: #1a1a1a;
    --text-secondary: #555;
    --text-muted: #888;
    --accent: #2563eb;
    --accent-light: #eff6ff;
    --accent-border: #bfdbfe;
    --code-bg: #f4f4f5;
    --code-border: #e4e4e7;
    --mark-bg: #fefce8;
    --mark-border: #fde68a;
    --font-mono: 'JetBrains Mono', 'Fira Code', 'Cascadia Code', 'Consolas', monospace;
    --font-sans: 'Pretendard', 'Apple SD Gothic Neo', 'Noto Sans KR', system-ui, sans-serif;
    --radius: 6px;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: var(--font-sans);
    background: var(--bg);
    color: var(--text);
    font-size: 15px;
    line-height: 1.7;
    padding: 0 16px;
  }
  .page { max-width: 860px; margin: 0 auto; padding: 48px 0 96px; }
  .doc-header { border-bottom: 2px solid var(--text); padding-bottom: 24px; margin-bottom: 48px; }
  .doc-label { font-size: 11px; font-weight: 600; letter-spacing: 0.12em; text-transform: uppercase; color: var(--text-muted); margin-bottom: 12px; }
  .doc-header h1 { font-size: 28px; font-weight: 700; letter-spacing: -0.02em; line-height: 1.25; margin-bottom: 16px; }
  .doc-header p { font-size: 15px; color: var(--text-secondary); line-height: 1.6; max-width: 640px; }
  .doc-meta { margin-top: 20px; display: flex; gap: 24px; font-size: 12px; color: var(--text-muted); font-family: var(--font-mono); flex-wrap: wrap; }
  .toc { border: 1px solid var(--border); border-radius: var(--radius); padding: 20px 24px; margin-bottom: 48px; background: var(--surface); }
  .toc-title { font-size: 11px; font-weight: 600; letter-spacing: 0.1em; text-transform: uppercase; color: var(--text-muted); margin-bottom: 12px; }
  .toc ol { list-style: none; counter-reset: toc; display: grid; grid-template-columns: 1fr 1fr; gap: 4px 32px; }
  @media (max-width: 600px) { .toc ol { grid-template-columns: 1fr; } }
  .toc li { counter-increment: toc; font-size: 13px; }
  .toc li::before { content: counter(toc, decimal-leading-zero) ". "; font-family: var(--font-mono); color: var(--text-muted); font-size: 11px; }
  .toc a { color: var(--text-secondary); text-decoration: none; }
  .toc a:hover { color: var(--accent); text-decoration: underline; }
  .toc a.active { color: var(--accent); font-weight: 600; }
  section { margin-bottom: 56px; }
  .section-number { font-family: var(--font-mono); font-size: 11px; color: var(--text-muted); letter-spacing: 0.05em; display: block; margin-bottom: 6px; }
  h2 { font-size: 20px; font-weight: 700; letter-spacing: -0.01em; margin-bottom: 4px; padding-bottom: 10px; border-bottom: 1px solid var(--border); }
  h3 { font-size: 15px; font-weight: 600; margin: 28px 0 12px; color: var(--text); }
  h4 { font-size: 13px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.06em; color: var(--text-muted); margin: 24px 0 10px; }
  p { margin-bottom: 14px; color: var(--text-secondary); }
  p:last-child { margin-bottom: 0; }
  strong { color: var(--text); font-weight: 600; }
  a { color: var(--accent); text-decoration: none; }
  a:hover { text-decoration: underline; }
  code { font-family: var(--font-mono); font-size: 12.5px; background: var(--code-bg); border: 1px solid var(--code-border); border-radius: 3px; padding: 1px 5px; color: var(--text); }
  pre { background: #18181b; border-radius: var(--radius); padding: 20px 24px; overflow-x: auto; margin: 16px 0; border: 1px solid #27272a; }
  pre code { background: none; border: none; padding: 0; color: #e4e4e7; font-size: 13px; line-height: 1.6; }
  .table-wrap { overflow-x: auto; margin: 16px 0; border-radius: var(--radius); border: 1px solid var(--border); }
  table { width: 100%; border-collapse: collapse; font-size: 13.5px; }
  thead tr { background: #f4f4f5; }
  th { padding: 10px 14px; text-align: left; font-weight: 600; font-size: 12px; letter-spacing: 0.04em; color: var(--text-muted); text-transform: uppercase; border-bottom: 1px solid var(--border); white-space: nowrap; }
  td { padding: 10px 14px; border-bottom: 1px solid var(--border); vertical-align: top; color: var(--text-secondary); }
  tr:last-child td { border-bottom: none; }
  tbody tr:hover { background: #fafafa; }
  .callout { border-left: 3px solid var(--border-strong); padding: 12px 16px; margin: 16px 0; background: var(--surface); border-radius: 0 var(--radius) var(--radius) 0; }
  .callout.info { border-color: var(--accent); background: var(--accent-light); }
  .callout.warn { border-color: #f59e0b; background: var(--mark-bg); }
  .callout p { color: var(--text-secondary); font-size: 13.5px; }
  .callout-label { font-size: 11px; font-weight: 700; letter-spacing: 0.08em; text-transform: uppercase; color: var(--text-muted); margin-bottom: 6px; }
  .diagram-wrap { border: 1px solid var(--border); border-radius: var(--radius); background: var(--surface); margin: 16px 0; overflow: hidden; }
  .diagram-toolbar { display: flex; align-items: center; justify-content: space-between; padding: 8px 12px; background: #f4f4f5; border-bottom: 1px solid var(--border); font-size: 11px; color: var(--text-muted); font-family: var(--font-mono); letter-spacing: 0.04em; }
  .diagram-controls { display: flex; gap: 4px; }
  .diagram-btn { background: var(--surface); border: 1px solid var(--border); border-radius: 4px; padding: 3px 9px; font-size: 13px; cursor: pointer; color: var(--text-secondary); transition: all 0.1s; line-height: 1; font-family: var(--font-mono); }
  .diagram-btn:hover { background: var(--accent); color: #fff; border-color: var(--accent); }
  .diagram-container { padding: 24px; overflow: auto; }
  .diagram-container pre.mermaid { background: none; border: none; padding: 0; display: flex; justify-content: center; transition: transform 0.15s ease; transform-origin: top center; }
  .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.65); z-index: 1000; align-items: center; justify-content: center; backdrop-filter: blur(2px); }
  .modal-overlay.active { display: flex; }
  .modal-box { background: var(--surface); border-radius: 10px; width: 92vw; max-width: 1100px; max-height: 88vh; overflow: hidden; display: flex; flex-direction: column; box-shadow: 0 20px 60px rgba(0,0,0,0.3); }
  .modal-header { display: flex; align-items: center; justify-content: space-between; padding: 14px 20px; border-bottom: 1px solid var(--border); background: #f4f4f5; }
  .modal-title { font-size: 12px; font-family: var(--font-mono); color: var(--text-muted); letter-spacing: 0.04em; }
  .modal-close { background: none; border: 1px solid var(--border); border-radius: 4px; width: 28px; height: 28px; cursor: pointer; font-size: 16px; color: var(--text-secondary); display: flex; align-items: center; justify-content: center; }
  .modal-close:hover { background: #e4e4e7; }
  .modal-body { overflow: auto; padding: 32px; display: flex; align-items: flex-start; justify-content: center; flex: 1; }
  .modal-body svg { max-width: 100%; height: auto; }
  ul, ol { padding-left: 20px; margin-bottom: 14px; color: var(--text-secondary); }
  li { margin-bottom: 4px; font-size: 14px; }
  .def-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin: 16px 0; }
  @media (max-width: 600px) { .def-grid { grid-template-columns: 1fr; } }
  .def-item { border: 1px solid var(--border); border-radius: var(--radius); padding: 14px 16px; background: var(--surface); }
  .def-key { font-family: var(--font-mono); font-size: 12px; color: var(--text-muted); margin-bottom: 4px; }
  .def-val { font-size: 13.5px; color: var(--text-secondary); }
  hr { border: none; border-top: 1px solid var(--border); margin: 40px 0; }
  .img-wrap { display: flex; gap: 24px; align-items: flex-start; margin: 16px 0; }
  .img-wrap img { max-width: 160px; border: 1px solid var(--border); border-radius: var(--radius); }
  .img-wrap .img-desc { flex: 1; }
  .doc-footer { border-top: 1px solid var(--border); padding-top: 24px; font-size: 12px; color: var(--text-muted); font-family: var(--font-mono); display: flex; justify-content: space-between; }
</style>
</head>
<body>
<div class="page">

  <header class="doc-header">
    <div class="doc-label">Knowledge Sharing / Internal Tech Talk</div>
    <h1>AI 에이전트 기반 풀스택 개발 워크플로우</h1>
    <p>외부에서 AI를 활용한 개인 코딩 방식을 공유하고, 사내 개발팀에 적용 가능한 구조적 인사이트를 함께 제안합니다.</p>
    <div class="doc-meta">
      <span>2026-03-10</span>
      <span>Stack: Next.js 16 / Prisma 7 / Supabase / Vercel</span>
      <span>Tools: OpenSpec / Jules / Cursor Automations</span>
    </div>
  </header>

  <nav class="toc">
    <div class="toc-title">Contents</div>
    <ol>
      <li><a href="#background">배경 — 세 가지 병목</a></li>
      <li><a href="#workflow">전체 워크플로우 구조</a></li>
      <li><a href="#step1">Step 1 — OpenSpec</a></li>
      <li><a href="#step2">Step 2 — Next.js + Prisma + Supabase</a></li>
      <li><a href="#step3">Step 3 — Vercel CLI</a></li>
      <li><a href="#step4">Step 4 — opencode + Copilot Agent</a></li>
      <li><a href="#step5">Step 5 — Google Jules</a></li>
      <li><a href="#inhouse">사내 적용 인사이트</a></li>
      <li><a href="#step6">Step 6 — Cursor Automations</a></li>
      <li><a href="#matrix">마무리 — 도구 매트릭스</a></li>
      <li><a href="#demo">독박게임 시연</a></li>
      <li><a href="#refs">참고 링크</a></li>
    </ol>
  </nav>

  <section id="background">
    <span class="section-number">01</span>
    <h2>배경 — 세 가지 병목</h2>
    <p>업무에서는 프론트엔드만 담당하지만, 개인 프로젝트에서는 DB 설계부터 배포까지 혼자 처리해야 합니다. 이 워크플로우는 그 과정에서 생긴 세 가지 병목을 해결하기 위해 설계되었습니다.</p>
    <div class="table-wrap">
      <table>
        <thead><tr><th>병목</th><th>내용</th><th>해결 도구</th></tr></thead>
        <tbody>
          <tr><td><strong>풀스택 지식 격차</strong></td><td>백엔드 / DB / 인프라는 공부가 아닌 실전에서 막힘</td><td>OpenSpec + Copilot Agent</td></tr>
          <tr><td><strong>AI 비용</strong></td><td>Claude / GPT 계열은 토큰 소비가 빨라 장시간 사용 부담</td><td>Jules (태스크 건당 과금)</td></tr>
          <tr><td><strong>개발 시간 부족</strong></td><td>퇴근 후, 이동 중에는 노트북을 켤 수 없는 상황</td><td>Jules + Cursor Automations</td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <section id="workflow">
    <span class="section-number">02</span>
    <h2>전체 워크플로우 구조</h2>
    <p>각 도구는 독립적으로 사용 가능하지만, 연결할수록 자동화 범위가 넓어집니다.</p>
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
    A[OpenSpec] --> B[Next.js + Prisma + Supabase]
    B --> C[Vercel CLI]
    C --> D{작업 환경}
    D -->|로컬 + 에디터| E[opencode / Copilot]
    D -->|이동 중 / 비동기| F[Jules]
    E --> G[GitHub Push]
    F --> G
    G --> H[Vercel 자동 배포]
    H --> I[Cursor Automations]
        </pre>
      </div>
    </div>
  </section>

  <section id="step1">
    <span class="section-number">03</span>
    <h2>Step 1 — OpenSpec: AI 에이전트용 컨텍스트 설계</h2>
    <h3>왜 필요한가</h3>
    <p>LLM은 맥락 없이 좋은 코드를 생성할 수 없습니다. Cursor, Jules, opencode 등 서로 다른 AI 에이전트가 같은 프로젝트에 투입될 때, <strong>공통 사전 지식(project.md)을 미리 선언해두면 매번 설명을 반복할 필요가 없습니다.</strong></p>
    <div class="def-grid">
      <div class="def-item">
        <div class="def-key">openspec/project.md</div>
        <div class="def-val">스택, 컨벤션, 도메인 용어 기술</div>
      </div>
      <div class="def-item">
        <div class="def-key">openspec/changes/[id]/proposal.md</div>
        <div class="def-val">기능 단위 변경 제안서</div>
      </div>
      <div class="def-item">
        <div class="def-key">openspec/changes/[id]/specs/[cap]/spec.md</div>
        <div class="def-val">SHALL/MUST 기반 요구 명세</div>
      </div>
      <div class="def-item">
        <div class="def-key">openspec validate [id] --strict</div>
        <div class="def-val">명세 검증 및 누락 항목 탐지</div>
      </div>
    </div>
<pre><code>npx openspec init
openspec validate add-user-score --strict</code></pre>

    <h3>project.md 작성 예시</h3>
<pre><code>## Tech Stack
- Framework: Next.js 16 (App Router, Server Components 우선)
- ORM: Prisma (migration 파일로 변경 이력 관리)
- Database: Supabase (PostgreSQL + RLS)
- Auth: Supabase Auth (Google OAuth)
- Deploy: Vercel (main → production, feature/* → preview)

## Conventions
- 컴포넌트: src/components/
- API Route: app/api/ (Route Handlers)
- 환경 변수: vercel env pull로 .env.local 자동 동기화</code></pre>

    <div class="callout info">
      <div class="callout-label">Design Principle</div>
      <p>AI 에이전트는 "좋은 프롬프트"가 아닌 <strong>"좋은 컨텍스트"</strong>에 반응합니다. <code>project.md</code>는 모든 AI 에이전트의 온보딩 문서 역할을 합니다.</p>
    </div>

    <h3>Tip — project.md를 영문 압축형으로 작성하는 이유</h3>
    <p><code>project.md</code>는 매 요청마다 컨텍스트 윈도우에 포함됩니다. 한국어 + 설명체로 작성하면 토큰 소비가 크고, LLM이 해석하는 데도 불필요한 연산이 생깁니다.</p>
    <h4>Before — 설명체</h4>
<pre><code>## 기술 스택
이 프로젝트는 Next.js 16의 App Router를 사용하며,
데이터베이스는 Supabase의 PostgreSQL을 활용합니다.
ORM으로는 Prisma를 사용하고 있으며, 마이그레이션
파일로 스키마 변경 이력을 관리합니다.</code></pre>
    <h4>After — 압축 영문형</h4>
<pre><code>## stack
fw:next16-approuter sc-first; orm:prisma migration-files;
db:supabase-pg rls-enabled; auth:supabase google-oauth;
deploy:vercel main=prod feature/*=preview

## conventions
comp:src/components/ api:app/api/ route-handlers;
env:vercel-env-pull→.env.local; lang:ko-docs en-code</code></pre>

    <div class="table-wrap">
      <table>
        <thead><tr><th>항목</th><th>설명체 (한국어)</th><th>압축 영문형</th></tr></thead>
        <tbody>
          <tr><td>토큰 수 (예시)</td><td>~180 tokens</td><td><strong>~45 tokens</strong></td></tr>
          <tr><td>LLM 파싱 속도</td><td>느림 (자연어 처리)</td><td>빠름 (키-값 구조)</td></tr>
          <tr><td>다국어 에이전트 호환</td><td>Jules 등 영문 에이전트에 불리</td><td>모든 에이전트 동일 해석</td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <section id="step2">
    <span class="section-number">04</span>
    <h2>Step 2 — Next.js + Prisma + Supabase: 풀스택 기반 구축</h2>
    <h3>스택 선택 근거</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>레이어</th><th>선택</th><th>버전</th><th>이유</th></tr></thead>
        <tbody>
          <tr><td>프레임워크</td><td><strong>Next.js App Router</strong></td><td>16.0.8</td><td>Server Component로 API/렌더링 통합, Vercel 네이티브</td></tr>
          <tr><td>ORM</td><td><strong>Prisma</strong></td><td>7.0</td><td>타입 안전 쿼리, migration 파일로 스키마 변경 이력 관리</td></tr>
          <tr><td>DB</td><td><strong>Supabase (PostgreSQL)</strong></td><td>—</td><td>RLS로 행 단위 접근 제어, 무료 플랜에서 실운용 가능</td></tr>
          <tr><td>인증</td><td><strong>next-auth v5</strong></td><td>5.0 beta</td><td>OAuth 제공자 내장, JWT 자동 관리</td></tr>
        </tbody>
      </table>
    </div>

    <h3>Vercel DB 옵션 비교</h3>
    <p>프로젝트 요건에 따라 아래 중 선택할 수 있으며, 모두 <code>vercel env pull</code>로 연결 문자열이 자동 주입됩니다.</p>
    <div class="table-wrap">
      <table>
        <thead><tr><th>DB</th><th>특징</th><th>적합한 경우</th></tr></thead>
        <tbody>
          <tr><td><strong>Supabase</strong></td><td>PostgreSQL + RLS + Auth 내장</td><td>인증·보안 정책이 중요한 앱</td></tr>
          <tr><td><strong>Neon</strong></td><td>서버리스 PostgreSQL, 브랜치별 DB 스냅샷</td><td>Vercel Preview와 DB 브랜치를 1:1 매핑할 때</td></tr>
          <tr><td><strong>PlanetScale</strong></td><td>MySQL 호환, 무중단 스키마 변경</td><td>대규모 트래픽, zero-downtime 마이그레이션</td></tr>
          <tr><td><strong>Upstash</strong></td><td>서버리스 Redis / Kafka</td><td>세션 캐시, 큐, 실시간 카운터</td></tr>
          <tr><td><strong>Vercel Postgres</strong></td><td>Vercel 대시보드 통합 관리</td><td>간단한 프로젝트, 인프라 최소화</td></tr>
        </tbody>
      </table>
    </div>

    <h3>RLS (Row Level Security) 개념</h3>
    <p>RLS는 PostgreSQL의 행 단위 접근 제어 메커니즘입니다. <strong>API 레이어에서 필터를 빠뜨려도 DB 레이어에서 차단</strong>하므로, 보안 계층이 중복됩니다.</p>
    <div class="table-wrap">
      <table>
        <thead><tr><th>구분</th><th>API 레이어 필터</th><th>RLS (DB 레이어)</th></tr></thead>
        <tbody>
          <tr><td>적용 위치</td><td>애플리케이션 코드</td><td>데이터베이스 정책</td></tr>
          <tr><td>우회 가능성</td><td>버그 시 데이터 노출 위험</td><td>우회 불가 (DB 레벨 강제)</td></tr>
          <tr><td>설정 방법</td><td>WHERE 절, 미들웨어</td><td>CREATE POLICY 구문</td></tr>
        </tbody>
      </table>
    </div>

    <h3>Prisma 스키마 및 Connection Pooling</h3>
<pre><code>// prisma/schema.prisma
datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")   // PgBouncer 경유 (Vercel 런타임)
  directUrl = env("DIRECT_URL")     // 직접 연결 (migration 전용)
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
      <p>Vercel 서버리스 환경은 요청마다 새 연결을 생성합니다. PgBouncer가 연결을 재사용하므로 DB 연결 한도를 초과하지 않습니다.</p>
    </div>
<pre><code>npx prisma migrate dev --name add-game-score
npx prisma studio</code></pre>
  </section>

  <section id="step3">
    <span class="section-number">05</span>
    <h2>Step 3 — Vercel CLI: CI/CD 파이프라인 연결</h2>
    <p>Vercel 대시보드 없이도 터미널에서 환경 변수 동기화 → 빌드 → 배포를 처리할 수 있습니다. Jules가 생성한 PR에 Vercel 프리뷰 URL이 자동 첨부되어, 모바일에서 바로 확인 가능합니다.</p>
<pre><code>npm install -g vercel
vercel link                    # 로컬 프로젝트 ↔ Vercel 프로젝트 연결
vercel env pull .env.local     # 프로덕션 환경 변수를 로컬에 동기화
vercel deploy                  # 프리뷰 배포 (브랜치 URL 생성)
vercel --prod                  # 프로덕션 즉시 배포</code></pre>

    <h3>브랜치 전략과 배포 환경 매핑</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>브랜치</th><th>배포 환경</th><th>URL 패턴</th></tr></thead>
        <tbody>
          <tr><td><code>main</code></td><td>Production</td><td><code>myapp.vercel.app</code></td></tr>
          <tr><td><code>feature/*</code></td><td>Preview</td><td><code>myapp-git-feature-xxx.vercel.app</code></td></tr>
          <tr><td>Pull Request</td><td>Preview</td><td>PR 본문에 URL 자동 삽입</td></tr>
        </tbody>
      </table>
    </div>
    <div class="callout info">
      <p>Jules가 PR을 올리면 → Vercel Preview가 자동 생성 → 스마트폰으로 코드 없이 UI 확인 후 Merge 가능. 이 흐름이 이동 중 개발을 가능하게 하는 핵심 구조입니다.</p>
    </div>
  </section>

  <section id="step4">
    <span class="section-number">06</span>
    <h2>Step 4 — 로컬 AI 코딩: opencode + Copilot Agent</h2>
    <div class="table-wrap">
      <table>
        <thead><tr><th>도구</th><th>실행 환경</th><th>강점</th></tr></thead>
        <tbody>
          <tr><td><strong>opencode</strong></td><td>터미널 / CLI</td><td>SSH 환경, 에디터 없는 서버에서도 동작</td></tr>
          <tr><td><strong>Copilot Agent</strong></td><td>VS Code 내</td><td>파일 직접 편집, 멀티 파일 동시 생성</td></tr>
        </tbody>
      </table>
    </div>
    <p>두 도구 모두 <code>openspec/project.md</code>를 컨텍스트로 주면, 프로젝트 컨벤션을 따르는 코드를 생성합니다.</p>

    <h3>Copilot Agent — 멀티 파일 동시 생성 예시</h3>
<pre><code>@workspace openspec/project.md 참고해서 아래 기능 구현해줘:
- src/components/GameResult.tsx (클라이언트 컴포넌트, 점수 저장 UI)
- prisma/migrations/ (game_scores 테이블 + 인덱스)
- supabase/policies/game_scores_rls.sql (RLS 보안 정책)
- app/api/scores/route.ts (POST/GET API Route)</code></pre>

    <div class="callout info">
      <p>하나의 프롬프트로 프론트엔드 컴포넌트 + DB 마이그레이션 + 보안 정책 + API를 동시에 생성합니다.</p>
    </div>

    <h3>Supabase RLS 정책 — 보안 설계 관점</h3>
<pre><code>-- 인증된 사용자만 자신의 점수를 INSERT 가능
CREATE POLICY "insert_own_scores"
  ON public.game_scores FOR INSERT
  WITH CHECK (auth.uid() = user_id);

-- 랭킹 조회는 전체 공개 (SELECT)
CREATE POLICY "read_all_scores"
  ON public.game_scores FOR SELECT
  USING (true);</code></pre>

    <div class="callout">
      <p>"보안을 잊지 말라고 지시"하는 것이 아니라, <strong>컨텍스트 설계로 항상 포함되게 합니다.</strong></p>
    </div>
  </section>

  <section id="step5">
    <span class="section-number">07</span>
    <h2>Step 5 — Google Jules: 클라우드 비동기 AI 코딩 에이전트</h2>
    <p>Jules는 내가 자리를 비운 동안 GitHub 저장소에서 직접 코드를 수정하고 PR을 올립니다.</p>

    <h3>아키텍처 차이</h3>
<pre><code>[기존 도구]
내 PC → AI API 호출 → 코드 생성 → 내 에디터에 반영
내 API 토큰 소비 / 내 PC 켜져 있어야 함

[Jules]
내 지시 → Google 클라우드 샌드박스 → 저장소 클론 → 코드 수정 → PR 생성
Google 인프라 실행 / 내 PC 꺼져 있어도 됨 / 내 API 토큰 소비 없음</code></pre>

    <h3>과금 구조 비교</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>구분</th><th>Copilot / opencode</th><th>Jules</th></tr></thead>
        <tbody>
          <tr><td>API 키</td><td>내 계정 (OpenAI / Anthropic)</td><td>Google 부담</td></tr>
          <tr><td>과금 단위</td><td>입출력 토큰 수</td><td><strong>태스크 건당</strong></td></tr>
          <tr><td>긴 작업 시</td><td>토큰 비용 급증</td><td>비용 고정</td></tr>
          <tr><td>저장소 범위</td><td>에디터에 열린 파일 위주</td><td>저장소 전체</td></tr>
          <tr><td>내 PC 필요</td><td>필요</td><td>불필요</td></tr>
        </tbody>
      </table>
    </div>

    <h3>모델 진화 이력</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>시기</th><th>모델</th><th>체감 수준</th><th>주요 변화</th></tr></thead>
        <tbody>
          <tr><td>2025-08 (베타)</td><td>Gemini 2.5 Pro</td><td>영리한 인턴</td><td>단순 버그 수정, 파일 1~2개 편집</td></tr>
          <tr><td>2025-11</td><td>Gemini 3 Pro</td><td>숙련 인턴</td><td>멀티 파일, 마이그레이션 작성 시작</td></tr>
          <tr><td><strong>2026-01-30</strong></td><td><strong>Gemini 3 Flash</strong></td><td><strong>주니어 개발자</strong></td><td>라우팅 구조 파악, Prisma + RLS 연동 PR</td></tr>
        </tbody>
      </table>
    </div>

    <h3>CLI 주요 명령어</h3>
<pre><code>npm install -g @google/jules
jules login

jules remote list --repo
jules remote new --session "랭킹 페이지 UI 개선:
  1~3위에 금/은/동 배경색 적용
  /app/ranking/page.tsx 수정, Tailwind 사용"

jules remote list --session
jules remote pull --session &lt;id&gt;
jules remote new --session "성능 최적화" --parallel 3</code></pre>

    <h3>이동 중 개발 시나리오</h3>
<pre><code>[09:00 — 출근길 스마트폰]
jules.google.com 접속
→ "닉네임 컬럼 추가: Prisma migration + Profile 컴포넌트 업데이트"

[Jules 클라우드 자동 처리 — 약 15분]
→ schema.prisma 수정 → migration 파일 자동 생성
→ Profile.tsx 업데이트
→ PR #23 생성 + Vercel 프리뷰 URL 첨부

[09:30 — 회사 도착 전]
→ GitHub 모바일에서 PR diff 확인
→ Vercel 프리뷰 URL로 UI 확인
→ Approve + Merge → 프로덕션 자동 배포</code></pre>

    <h3>실제 활용 사례</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>사용 패턴</th><th>예시</th></tr></thead>
        <tbody>
          <tr><td>의존성 업그레이드</td><td>모든 패키지 최신 버전으로 업그레이드, breaking change 수정</td></tr>
          <tr><td>GitHub Issue 자동 처리</td><td>이슈에 <code>jules</code> 라벨 부착 → Jules가 읽고 PR 생성</td></tr>
          <tr><td>테스트 일괄 생성</td><td>src/app/api 하위 route 파일에 대한 테스트 파일 생성</td></tr>
          <tr><td>야간 보안 점검</td><td>Scheduled Task — 매주 월요일 새벽 npm audit fix 자동 실행</td></tr>
        </tbody>
      </table>
    </div>

    <h3>Jules Continuous AI (2025-12~)</h3>
    <div class="def-grid">
      <div class="def-item"><div class="def-key">Scheduled Tasks</div><div class="def-val">Cron 기반 정기 작업 (의존성 검사, lint 수정 등)</div></div>
      <div class="def-item"><div class="def-key">Suggested Tasks</div><div class="def-val">// TODO 주석 감지 후 자동 작업 제안</div></div>
      <div class="def-item"><div class="def-key">CI Fixer</div><div class="def-val">Jules PR의 CI 실패를 스스로 분석하고 재제출</div></div>
      <div class="def-item"><div class="def-key">MCP 연동</div><div class="def-val">Supabase, Linear, Neon 등 외부 서비스 직접 조작</div></div>
    </div>
  </section>

  <section id="inhouse">
    <span class="section-number">08</span>
    <h2>사내 적용 인사이트 — 금융권 개발 환경</h2>
    <p>금융권(전자금융거래법, 망분리 규제)은 외부 AI 서비스에 코드를 직접 전송할 수 없습니다. 그러나 Jules의 <strong>아키텍처 패턴</strong> 자체는 사내에 구현 가능합니다.</p>

    <div class="table-wrap">
      <table>
        <thead><tr><th>Jules (외부)</th><th>사내 구현 가능한 형태</th></tr></thead>
        <tbody>
          <tr><td>Google 클라우드 샌드박스</td><td>사내망 내 격리 에이전트 서버</td></tr>
          <tr><td>Gemini 모델</td><td>온프레미스 LLM (Ollama, vLLM 등)</td></tr>
          <tr><td>GitHub 트리거</td><td>사내 GitLab / Bitbucket 웹훅</td></tr>
          <tr><td>Vercel 프리뷰</td><td>사내 CD 파이프라인 (Jenkins / ArgoCD)</td></tr>
          <tr><td>jules.google.com UI</td><td>사내 에이전트 대시보드</td></tr>
        </tbody>
      </table>
    </div>

    <div class="callout warn">
      <p>규제가 막는 것은 "외부 SaaS 사용"이지, <strong>"비동기 AI 에이전트 패러다임"이 아닙니다.</strong></p>
    </div>

    <h3>사내 AI 코딩 에이전트 아키텍처 (제안)</h3>
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
    A[개발자 지시 - 사내 메신저 / 웹 UI] --> B[에이전트 오케스트레이터 - 사내망 내 서버]
    B --> C[온프레미스 LLM - Ollama / vLLM]
    B --> D[사내 GitLab - 저장소 클론 & PR 생성]
    D --> E[사내 CI/CD - Jenkins / ArgoCD]
    E --> F[개발 / 스테이징 서버 - 격리 환경 배포]
    F --> G[개발자 검토 - PR diff + 프리뷰 URL]
    G -->|Approve| H[운영 반영]
        </pre>
      </div>
    </div>

    <div class="callout info">
      <div class="callout-label">핵심 원칙</div>
      <p>모든 코드와 데이터는 사내망 안에서만 이동합니다. LLM은 외부 API가 아닌 온프레미스 모델로 대체합니다.</p>
    </div>

    <h3>사내 도입 시 기대 효과</h3>
    <div class="table-wrap">
      <table>
        <thead><tr><th>업무 패턴</th><th>현재</th><th>AI 에이전트 적용 후</th></tr></thead>
        <tbody>
          <tr><td>반복 보일러플레이트 생성</td><td>개발자 직접 작성 (수시간)</td><td>에이전트 위임 → PR 검토만 (30분)</td></tr>
          <tr><td>레거시 코드 테스트 작성</td><td>일정에서 우선순위 밀림</td><td>Cron 기반 야간 자동 생성</td></tr>
          <tr><td>의존성 보안 패치</td><td>분기별 수동 작업</td><td>주간 자동 PR + 담당자 알림</td></tr>
          <tr><td>코드 리뷰 1차 검토</td><td>시니어 리뷰어 시간 소비</td><td>에이전트 인라인 코멘트 선제공</td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <section id="step6">
    <span class="section-number">09</span>
    <h2>Step 6 — Cursor Automations: 이벤트 기반 상시 자동화</h2>
    <p>Jules가 "지시받으면 실행하는 에이전트"라면, Cursor Automations는 "트리거가 발생하면 스스로 판단하고 움직이는 에이전트"입니다. 2026년 3월 5일 정식 출시되었습니다.</p>

    <div class="table-wrap">
      <table>
        <thead><tr><th>구분</th><th>Jules</th><th>Cursor Automations</th></tr></thead>
        <tbody>
          <tr><td>작동 방식</td><td>명령형 — 개발자가 지시해야 시작</td><td>이벤트 드리븐 — 트리거 발생 시 자동</td></tr>
          <tr><td>상시 가동</td><td>세션 단위</td><td>항상 켜져 있음</td></tr>
          <tr><td>트리거</td><td>직접 CLI / 웹 입력</td><td>GitHub, Slack, Linear, PagerDuty, Cron</td></tr>
          <tr><td>적합한 작업</td><td>기능 추가, 리팩토링, 대규모 변경</td><td>리뷰 자동화, 모니터링, 핫픽스</td></tr>
          <tr><td>비용</td><td>건당 처리</td><td>Cloud Agent 사용량 과금</td></tr>
        </tbody>
      </table>
    </div>

    <h3>트리거 구조</h3>
    <div class="diagram-wrap">
      <div class="diagram-toolbar">
        <span>diagram / cursor-automations-triggers</span>
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
    A[Schedule - Cron] --> Z[Cursor Cloud Agent]
    B[GitHub - PR / Push / CI 실패] --> Z
    C[Slack 메시지] --> Z
    D[Linear 이슈 생성] --> Z
    E[PagerDuty 인시던트] --> Z
    F[Webhook] --> Z
    Z --> G[핫픽스 PR 생성]
    Z --> H[Slack 알림 발송]
    Z --> I[PR 인라인 코드 리뷰]
    Z --> J[MCP 도구 실행]
        </pre>
      </div>
    </div>

    <h3>실용 시나리오</h3>
<pre><code>[시나리오 1] Vercel 빌드 실패 → 자동 핫픽스
Vercel 빌드 실패 → Webhook 트리거 → Cloud Agent
→ 에러 로그 + diff 분석 → 원인 코드 수정
→ 핫픽스 PR 생성 + Slack 알림 → 개발자 모바일 승인 → Merge

[시나리오 2] PR 오픈 → 자동 보안 리뷰
feature/* PR 오픈 → GitHub 트리거 → Cloud Agent
→ diff 분석 → RLS 누락, SQL Injection 패턴 검사
→ PR 인라인 코멘트 자동 작성

[시나리오 3] 매일 새벽 → 테스트 커버리지 확장
Cron (매일 03:00) → 테스트 없는 함수 탐지
→ 테스트 코드 작성 → PR 생성</code></pre>
  </section>

  <section id="matrix">
    <span class="section-number">10</span>
    <h2>마무리 — 도구 매트릭스</h2>
    <div class="table-wrap">
      <table>
        <thead><tr><th>상황</th><th>도구</th><th>역할</th></tr></thead>
        <tbody>
          <tr><td>초기 설계</td><td>OpenSpec</td><td>아키텍처 + AI 컨텍스트 설계</td></tr>
          <tr><td>풀스택 구축</td><td>Next.js + Prisma + Supabase</td><td>스택 조합 및 RLS 보안 레이어</td></tr>
          <tr><td>CI/CD</td><td>Vercel CLI</td><td>환경 변수 동기화 + 브랜치별 자동 배포</td></tr>
          <tr><td>로컬 코딩</td><td>opencode / Copilot Agent</td><td>동기 멀티 파일 생성</td></tr>
          <tr><td>이동 중 / 비동기</td><td>Jules</td><td>클라우드 위임 실행, 토큰 비용 없음</td></tr>
          <tr><td>상시 자동화</td><td>Cursor Automations</td><td>이벤트 기반 항시 실행</td></tr>
        </tbody>
      </table>
    </div>

    <h3>패러다임 전환</h3>
<pre><code>[이전]
개발자 → 에디터에서 코드 작성 → 커밋 → 배포

[현재]
개발자 → AI 에이전트에게 지시 → 에이전트가 PR 생성 → 개발자가 검토 후 Merge → 자동 배포

[다음 단계]
트리거 발생 → 에이전트가 자동 판단 → PR 생성 → 개발자가 Approve만</code></pre>

    <div class="callout info">
      <p>개발자의 역할이 "코드를 타이핑하는 사람"에서 <strong>"에이전트가 올바르게 동작하도록 컨텍스트와 트리거를 설계하는 사람"</strong>으로 이동하고 있습니다.</p>
    </div>
  </section>

  <section id="demo">
    <span class="section-number">11</span>
    <h2>실시간 시연 — 독박게임</h2>
    <p>이 워크플로우로 실제로 만들고 운영 중인 프로젝트입니다.</p>
    <div class="img-wrap">
      <img src="22kBv.png" alt="독박게임 QR 코드" />
      <div class="img-desc">
        <p><strong><a href="https://dokbakgame.vercel.app" target="_blank">dokbakgame.vercel.app</a></strong></p>
        <ul>
          <li>Next.js App Router + Prisma + Supabase (PostgreSQL + RLS)</li>
          <li>Vercel 브랜치별 자동 배포</li>
          <li>Jules로 이동 중 기능 위임 → PR → Merge 흐름 실사용</li>
          <li>Copilot Agent로 DB 마이그레이션 + RLS 자동화</li>
        </ul>
      </div>
    </div>
    <h3>Jules 실시간 시연</h3>
<pre><code>jules remote new --session "..."</code></pre>
    <div class="table-wrap">
      <table>
        <thead><tr><th>패키지</th><th>용도</th></tr></thead>
        <tbody>
          <tr><td><code>phaser</code></td><td>2D 게임 엔진</td></tr>
          <tr><td><code>three</code> + <code>@react-three/fiber</code></td><td>3D 렌더링</td></tr>
          <tr><td><code>zustand</code></td><td>전역 상태 관리</td></tr>
          <tr><td><code>framer-motion</code></td><td>UI 애니메이션</td></tr>
          <tr><td><code>openai</code></td><td>AI 기능 연동</td></tr>
          <tr><td><code>socket.io-client</code></td><td>실시간 멀티플레이</td></tr>
          <tr><td><code>recharts</code></td><td>랭킹 시각화</td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <section id="refs">
    <span class="section-number">12</span>
    <h2>참고 링크</h2>
    <div class="table-wrap">
      <table>
        <thead><tr><th>도구</th><th>링크</th></tr></thead>
        <tbody>
          <tr><td>Jules 공식 문서</td><td><a href="https://jules.google/docs" target="_blank">jules.google/docs</a></td></tr>
          <tr><td>Jules Changelog</td><td><a href="https://jules.google/docs/changelog" target="_blank">jules.google/docs/changelog</a></td></tr>
          <tr><td>Cursor Automations</td><td><a href="https://cursor.com/docs/cloud-agent/automations" target="_blank">cursor.com/docs/cloud-agent/automations</a></td></tr>
          <tr><td>OpenSpec</td><td><a href="https://openspec.dev" target="_blank">openspec.dev</a></td></tr>
          <tr><td>Supabase RLS 가이드</td><td><a href="https://supabase.com/docs/guides/database/postgres/row-level-security" target="_blank">supabase.com/docs/…/row-level-security</a></td></tr>
          <tr><td>Prisma Connection Pooling</td><td><a href="https://www.prisma.io/docs/guides/performance-and-optimization/connection-management" target="_blank">prisma.io/docs/…/connection-management</a></td></tr>
          <tr><td>Vercel 배포 가이드</td><td><a href="https://vercel.com/docs" target="_blank">vercel.com/docs</a></td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <footer class="doc-footer">
    <span>AI 에이전트 기반 풀스택 개발 워크플로우</span>
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
