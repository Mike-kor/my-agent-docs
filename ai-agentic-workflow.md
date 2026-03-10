---
marp: true
theme: default
class: lead
backgroundColor: #ffffff
color: #333333
paginate: true
---

# 🚀 프론트엔드 개발자의 AI 에이전트 풀스택 워크플로우

> **"타자 속도가 아니라, 에이전트가 쉬지 않고 일할 수 있도록 시스템을 설계하는 오케스트레이션 능력이 1인 개발자의 경쟁력이다."**

---

## 🧐 들어가며

- 🏢 회사에서는 프론트엔드만, 백엔드·DB는 "그쪽 팀에 요청"
- 🚧 사이드 프로젝트를 시작해도 풀스택의 벽에서 멈춤
- 💸 비싼 Claude Opus / GPT Codex를 쓰면 토큰이 빠르게 소진

**이 발표는 그 세 가지 문제를 동시에 해결하는 이야기입니다.**

---

## 🗺️ 전체 흐름 한눈에

```mermaid
flowchart LR
    A[OpenSpec\n아키텍처 설계] --> B[Next.js + Prisma\n+ Supabase 구축]
    B --> C[Vercel CLI\nCI/CD 연결]
    C --> D{개발 상황}
    D --> |로컬 PC| E[opencode / Copilot\nLLM 풀스택 코딩]
    D --> |이동 중 / 모바일| F[Jules\n비동기 병렬 작업]
    E --> G[GitHub Push]
    F --> G
    G --> H[🚀 Vercel 자동 배포]
```

---

## 🏗️ Step 1 — OpenSpec으로 아키텍처 먼저 선언하기

> **"코드보다 구조가 먼저다. AI에게 컨텍스트를 주면 AI가 코드를 채운다."**

코드를 짜기 전에 **OpenSpec**으로 프로젝트 아키텍처를 선언적으로 정의합니다.  
`openspec/project.md` 하나가 이후 투입되는 모든 AI 에이전트의 **공통 사전 교육 자료**가 됩니다.

```bash
# 초기화
npx openspec init

# 기능 추가 제안서 생성 (proposal.md + tasks.md + spec 델타 자동 생성)
openspec validate add-user-score --strict
```

---

### 📝 openspec/project.md 예시

```markdown
## Tech Stack
- Framework: Next.js 14 (App Router)
- ORM: Prisma
- Database: Supabase (PostgreSQL)
- Auth: Supabase Auth (Google OAuth)
- Deploy: Vercel

## Conventions
- 컴포넌트: src/components/, Server Components 우선
- API: app/api/ (Route Handlers)
- DB 변경: Prisma migration 파일로 관리
```

> 💡 **Key Takeaway**: OpenSpec `project.md`는 AI 에이전트의 "온보딩 문서"다.  
> 잘 쓸수록 AI의 결과물 품질이 올라간다.

---

## 🔨 Step 2 — Next.js + Prisma + Supabase 풀스택 구축

```bash
# 프로젝트 생성
npx create-next-app@latest my-app --typescript --tailwind --app --src-dir

# Prisma + Supabase 연결
npm install prisma @prisma/client
npx prisma init --datasource-provider postgresql
```

**Prisma 스키마 정의 (`prisma/schema.prisma`)**

```prisma
model GameScore {
  id        String   @id @default(cuid())
  userId    String
  gameName  String
  score     Int
  createdAt DateTime @default(now())

  user User @relation(fields: [userId], references: [id])

  @@index([gameName, score(sort: Desc)])
}
```

---

### 🔗 Supabase Connection Pooling

```bash
# .env
DATABASE_URL="postgresql://postgres.[ref]:[pw]@pooler.supabase.com:6543/postgres?pgbouncer=true"
DIRECT_URL="postgresql://postgres.[ref]:[pw]@pooler.supabase.com:5432/postgres"
```

```prisma
datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")
  directUrl = env("DIRECT_URL")
}
```

```bash
# 마이그레이션 생성 & 적용
npx prisma migrate dev --name add-game-score
```

---

## 🚀 Step 3 — Vercel CLI로 CI/CD 연결

```bash
# Vercel CLI 설치 및 프로젝트 연결
npm install -g vercel
vercel link

# 프로덕션 환경 변수 로컬 동기화
vercel env pull .env.local

# 프리뷰 배포
vercel deploy

# 프로덕션 즉시 배포
vercel --prod
```

---

### 🌐 브랜치별 자동 배포

| 브랜치 | 환경 | URL |
|--------|------|-----|
| `main` | Production | `myapp.vercel.app` |
| `feature/*` | Preview | `myapp-git-feature-xxx.vercel.app` |
| PR | Preview | PR 댓글에 URL 자동 생성 |

> Jules가 PR을 올리면 → Vercel이 **프리뷰 URL 자동 생성** → 모바일에서 바로 확인 가능.

---

## 💻 Step 4 — 로컬 LLM 코딩: opencode + Copilot Agent

> **"로컬에서는 에디터 안의 AI가 풀스택을 커버한다."**

### opencode — 터미널 기반 AI 코딩 에이전트

```bash
# 설치
npm install -g opencode

# 현재 프로젝트 기반으로 작업 지시
opencode "Prisma 스키마에 nickname 컬럼 추가하고
          마이그레이션 파일과 API route까지 만들어줘"
```

- 터미널 중심 / SSH 환경에서도 동작
- `openspec/project.md`를 컨텍스트로 주면 프로젝트 규칙을 따르는 코드 생성

---

### 🤖 Copilot Agent — 에디터 안의 풀스택 에이전트

VS Code에서 단 한 번의 프롬프트:

```
@workspace openspec/project.md 참고해서
게임 점수 저장 기능 구현해줘.
- src/components/GameResult.tsx (클라이언트 컴포넌트)
- Prisma 마이그레이션 (game_scores 테이블)
- Supabase RLS 정책 SQL
- app/api/scores/route.ts
```

**Copilot Agent가 한 번에 생성하는 것들:**

1. `GameResult.tsx` — 점수 저장 UI 컴포넌트
2. `prisma/migrations/xxx_add_game_scores.sql` — 테이블 + 인덱스
3. `supabase/policies/game_scores_rls.sql` — RLS 보안 정책
4. `app/api/scores/route.ts` — POST / GET API

---

### 🔐 Supabase RLS 정책 (Copilot이 자동 생성)

```sql
-- 인증된 사용자만 자신의 점수 INSERT 가능
CREATE POLICY "insert_own_scores"
  ON public.game_scores FOR INSERT
  WITH CHECK (auth.uid() = user_id);

-- 랭킹은 전체 공개
CREATE POLICY "read_all_scores"
  ON public.game_scores FOR SELECT
  USING (true);
```

> 💡 **Key Takeaway**: OpenSpec 컨텍스트 + Copilot Agent = 프론트엔드 + DB 마이그레이션 + RLS를 **한 번에**.  
> "공부"가 아니라 "좋은 컨텍스트 설계"가 풀스택을 가능하게 한다.

---

## 📱 Step 5 — 이동 중에도 개발: Google Jules

> **"퇴근 후 지하철 안에서도 PR이 쌓인다."**

---

### Jules란?

Google이 만든 **클라우드 기반 비동기 AI 코딩 에이전트**.

```
내가 잠자는 동안  ──┐
내가 이동하는 동안 ──┼──▶ Jules는 클라우드에서 코드를 고치고 PR을 올린다
내가 회의하는 동안 ──┘
```

**핵심 차이점 — 토큰이 내 지갑에서 나가지 않는다**

| 구분 | Copilot / opencode | Jules |
|------|-------------------|-------|
| 실행 위치 | 내 로컬 / 내 API 키 | Google 클라우드 |
| 과금 방식 | 토큰 소비 (내 비용) | **건당 처리 (태스크 단위)** |
| 컨텍스트 한계 | 에디터 열린 파일 위주 | **저장소 전체** |
| 작업 중 내 PC 필요 | ✅ 필요 | ❌ 불필요 |

---

### 📈 Jules 모델 진화 — 인턴에서 주니어로

| 시기 | 모델 | 체감 수준 |
|------|------|-----------|
| 2025년 8월 (베타) | Gemini 2.5 Pro | 🧑‍💼 영리한 인턴 |
| 2025년 11월 | Gemini 3 Pro | 🧑‍💻 숙련 인턴 |
| **2026년 1월 30일** | **Gemini 3 Flash** | 🚀 **주니어 개발자** |

---

### 💬 Gemini 3 Flash 이후 체감 변화

> **2026년 1월 30일**, Gemini 3 Flash가 **전체 사용자**에게 적용됐습니다.

Claude Opus 4.6이나 GPT Codex 5.3 수준은 아닙니다.  
하지만 **"Next.js 라우팅과 Prisma 스키마 전체를 파악하고 혼자서 PR을 올리는 주니어"** 정도는 됩니다.

그리고 이 주니어는:
- 🕐 24시간 일하고
- 💰 월급이 없으며
- 🔁 토큰을 내 계정에서 빼가지 않습니다

---

### 🛠️ Jules CLI 설치 & 사용법

```bash
# 설치
npm install -g @google/jules

# Google 계정 인증
jules login

# 연결된 저장소 확인
jules remote list --repo

# 작업 위임 (저장소 자동 감지)
jules remote new --session "랭킹 페이지 UI 개선:
  1~3위에 금/은/동 배경색 적용
  /app/ranking/page.tsx 수정, Tailwind 사용"

# 진행 중 세션 확인
jules remote list --session

# 완료된 코드 로컬에 가져오기
jules remote pull --session <session_id>

# 병렬 작업 (최대 5개 동시)
jules remote new --session "성능 최적화" --parallel 3
```

---

### 🖥️ Jules TUI 대시보드

```bash
# 인터랙티브 대시보드 실행
jules
```

- 세션 목록 / 상태 실시간 확인
- Side-by-Side diff 뷰어
- PR 링크 바로 열기

---

### 📱 실제 모바일 사용 시나리오

```
[출근길 지하철 — 스마트폰]
jules.google.com 접속
→ "닉네임 컬럼 추가: Prisma 마이그레이션 + Profile 컴포넌트 업데이트"
→ 작업 위임 완료, 폰 닫음

[회의 중 — Jules가 클라우드에서 자동 처리]
→ Prisma schema 수정 → migration 파일 생성
→ Profile.tsx 업데이트 → PR #23 생성
→ Vercel 프리뷰 자동 배포

[점심시간 — 모바일 GitHub]
→ PR diff 확인 → 프리뷰 URL로 UI 확인 → Merge
→ Vercel 프로덕션 자동 배포 🚀
```

---

### ⚙️ Jules 개인 활용 사례

실제로 이렇게 써봤습니다:

- 📦 **의존성 업그레이드**: 아침에 일어나면 PR이 올라와 있음
  ```bash
  jules remote new --session "모든 패키지 최신 버전으로 업그레이드하고 breaking change 수정"
  ```
- 🐛 **GitHub Issue → 자동 수정 PR**: 이슈에 `jules` 라벨만 붙이면 Jules가 읽고 PR 생성
- 🧪 **테스트 일괄 생성**: 새로 만든 API route에 대한 테스트 파일 자동 생성
- 🌙 **야간 스케줄**: Scheduled Task로 매주 월요일 새벽 `npm audit fix` 자동 실행

---

### 🔄 Jules Continuous AI

Jules는 2025년 12월부터 **상시 가동** 패러다임을 도입했습니다:

| 기능 | 설명 |
|------|------|
| **Scheduled Tasks** | Cron 기반 정기 작업 (의존성 검사, 테스트 등) |
| **Suggested Tasks** | `// TODO` 감지 → 자동 제안 |
| **CI Fixer** | Jules PR의 CI 실패를 스스로 수정 후 재제출 |
| **MCP 연동** | Supabase, Linear, Neon 직접 조작 |

> 💡 **Key Takeaway**: Jules는 **"위임받은 작업을 끝까지 책임지는 주니어 개발자"**다.  
> 토큰 비용 없이, 내가 자는 동안에도 PR을 올린다.

---

## ⚡ Step 6 — 넥스트 레벨: Cursor Automations (2026년 3월)

> **"Jules가 시키면 하는 주니어라면, Cursor Automations는 알아서 움직이는 시니어다."**

### Jules vs Cursor Automations

| 구분 | Jules | Cursor Automations |
|------|-------|-------------------|
| 작동 방식 | 명령형 — 내가 지시해야 시작 | 이벤트 드리븐 — 트리거 발생 시 자동 |
| 항상 켜져 있나? | ❌ 세션 단위 | ✅ 상시 가동 |
| 비용 | 건당 처리 | Cloud Agent 사용량 |
| 적합한 작업 | 기능 추가, 리팩토링 | 리뷰, 모니터링, 핫픽스 |

---

### 🔗 Cursor Automations 트리거 종류

```mermaid
flowchart LR
    A[⏰ Schedule] --> Z[Cursor Cloud Agent]
    B[🐙 GitHub PR/Push] --> Z
    C[💬 Slack 메시지] --> Z
    D[📋 Linear 이슈] --> Z
    E[🚨 PagerDuty 인시던트] --> Z
    F[🔗 Webhook] --> Z
    Z --> G[핫픽스 PR 생성]
    Z --> H[Slack 알림]
    Z --> I[PR 코드 리뷰]
```

---

### 🎬 활용 시나리오

**① Vercel 빌드 에러 자동 수정**
```
Vercel 빌드 실패 → Webhook 트리거
→ Cloud Agent: 에러 로그 분석 → 핫픽스 코드 작성
→ PR 생성 + Slack 알림 → 모바일로 리뷰 → Merge ✅
```

**② PR 자동 보안 리뷰**
```
feature/* PR 오픈 → GitHub 트리거
→ Agent: diff 분석 → RLS 누락, SQL Injection 검사
→ PR 인라인 코멘트 자동 작성
```

**③ 매일 새벽 테스트 커버리지**
```
Cron (매일 03:00) → 테스트 없는 함수 탐지
→ 테스트 코드 작성 → PR 생성
→ 아침에 일어나면 테스트 PR이 기다림 ☕
```

---

## 🎯 마무리 — 도구 매트릭스

| 상황 | 도구 | 나의 역할 |
|------|------|-----------|
| 초기 설계 | **OpenSpec** | 아키텍처 설계자 |
| 풀스택 구축 | **Next.js + Prisma + Supabase** | 스택 조합자 |
| CI/CD | **Vercel CLI** | 파이프라인 설계자 |
| 로컬 코딩 | **opencode / Copilot Agent** | 리뷰어 |
| 이동 중 | **Jules** | 작업 위임자 |
| 상시 자동화 | **Cursor Automations** | 시스템 설계자 |

---

### 💡 1인 개발자의 새로운 정의

- **과거**: 코드를 직접 타이핑하는 사람
- **현재**: AI 에이전트에게 올바른 지시를 내리는 사람
- **미래**: 에이전트들이 24시간 돌아가는 **시스템을 설계**하는 사람

> **"미래의 1인 개발은 타자 속도가 아니라,**  
> **에이전트가 쉬지 않고 일할 수 있도록**  
> **트리거와 PR 파이프라인을 설계하는 오케스트레이션 능력에 달려 있다."**

---

## 🎮 실시간 시연 — 독박게임

이 모든 워크플로우로 실제로 만들고 운영 중인 프로젝트입니다.

**👉 [dokbakgame.vercel.app](https://dokbakgame.vercel.app)**

- Next.js App Router + Prisma + Supabase
- Vercel 자동 배포
- Jules로 이동 중 기능 추가
- Copilot Agent로 DB 마이그레이션 자동화

### 지금 바로 Jules로 실시간 수정을 시연합니다 🚀

```bash
jules remote new --session "..."
```

---

## 📎 참고 링크

| 도구 | 링크 |
|------|------|
| Jules 공식 문서 | [jules.google/docs](https://jules.google/docs) |
| Jules CLI 레퍼런스 | [jules.google/docs/cli/reference](https://jules.google/docs/cli/reference) |
| Jules Changelog | [jules.google/docs/changelog](https://jules.google/docs/changelog) |
| Cursor Automations | [cursor.com/docs/cloud-agent/automations](https://cursor.com/docs/cloud-agent/automations) |
| OpenSpec | [openspec.dev](https://openspec.dev) |
| Vercel 배포 가이드 | [vercel.com/docs](https://vercel.com/docs) |
| Supabase RLS 문서 | [supabase.com/docs/guides/database/postgres/row-level-security](https://supabase.com/docs/guides/database/postgres/row-level-security) |

✨ **감사합니다!** ✨
