> ⚠️ **이 문서는 deprecated 되었습니다.**  
> 새 발표 문서는 **[ai-agentic-workflow.md](./ai-agentic-workflow.md)** 를 참고하세요.

# 나의 AI 개발 워크플로우
## Next.js 프로젝트 × Vercel 배포 × AI 도구 활용 시나리오

---

## 📌 목차

1. [전체 워크플로우 개요](#전체-워크플로우-개요)
2. [기술 스택](#기술-스택)
3. [프로젝트 세팅 — Next.js + Vercel](#프로젝트-세팅--nextjs--vercel)
4. [로컬 개발 — Cursor & OpenCode](#로컬-개발--cursor--opencode)
5. [모바일 개발 — Jules](#모바일-개발--jules)
6. [배포 프로세스 — Vercel](#배포-프로세스--vercel)
7. [시나리오 시연](#시나리오-시연)
8. [정리 및 Q&A](#정리-및-qa)

---

## 전체 워크플로우 개요

```
[아이디어 / 이슈 발생]
        │
        ▼
┌───────────────────────────────────────┐
│         상황에 따른 도구 선택          │
│                                       │
│  💻 로컬 (PC/Mac)   📱 모바일 (외출)  │
│  ┌─────────────┐   ┌───────────────┐  │
│  │   Cursor    │   │     Jules     │  │
│  │  OpenCode   │   │  (구글 AI)    │  │
│  └─────────────┘   └───────────────┘  │
└───────────────────────────────────────┘
        │
        ▼
   [GitHub Push]
        │
        ▼
   [Vercel 자동 배포]
        │
        ▼
   [프리뷰 URL 확인 / 프로덕션 배포]
```

---

## 기술 스택

| 구분 | 도구 | 역할 |
|------|------|------|
| **프레임워크** | Next.js 14+ (App Router) | 풀스택 웹 프레임워크 |
| **배포** | Vercel | CI/CD + 호스팅 |
| **버전 관리** | GitHub | 소스 관리 & 트리거 |
| **로컬 AI 에디터** | Cursor | AI 페어 프로그래밍 (PC) |
| **로컬 AI CLI** | OpenCode | 터미널 기반 AI 코딩 (PC) |
| **모바일 AI** | Jules (Google) | 모바일 환경 코드 작업 |
| **스타일링** | Tailwind CSS | 유틸리티 CSS |
| **언어** | TypeScript | 타입 안전성 |

---

## 프로젝트 세팅 — Next.js + Vercel

### 1. Next.js 프로젝트 생성

```bash
npx create-next-app@latest my-project \
  --typescript \
  --tailwind \
  --eslint \
  --app \
  --src-dir
```

### 2. GitHub 저장소 연결

```bash
git init
git remote add origin https://github.com/username/my-project.git
git push -u origin main
```

### 3. Vercel 프로젝트 연결

- [vercel.com](https://vercel.com) → **Add New Project**
- GitHub 저장소 선택 → **Import**
- 환경 변수 설정 (`.env.local` 내용 입력)
- **Deploy** 클릭 → 자동 빌드 시작

### 4. 브랜치 전략

```
main          →  프로덕션 배포 (자동)
develop       →  개발 통합 브랜치
feature/*     →  기능 개발 → 프리뷰 URL 자동 생성
```

> 💡 **Vercel의 핵심 장점**: PR을 올리면 **자동으로 프리뷰 URL**이 생성되어
> 배포 전에 실제 환경에서 확인 가능

---

## 로컬 개발 — Cursor & OpenCode

### 🖥️ Cursor — AI 페어 프로그래밍 에디터

**언제 사용하나?**
- 복잡한 컴포넌트 설계 & 구현
- 코드 리팩토링 & 버그 수정
- 프로젝트 전체 컨텍스트 파악이 필요할 때

**주요 활용 방법**

```
Cmd + K   →  인라인 AI 편집 (선택 영역 수정)
Cmd + L   →  AI 채팅 (컨텍스트 포함 질문)
Cmd + I   →  Composer (멀티 파일 동시 편집)
```

**실제 사용 예시**
```
# Cursor Composer 활용
"app/dashboard 페이지에 차트 컴포넌트를 추가하고
 API route도 함께 만들어줘. TypeScript + Tailwind 사용"
→ 여러 파일을 동시에 생성/수정
```

---

### ⌨️ OpenCode — 터미널 AI 코딩 에이전트

**언제 사용하나?**
- 터미널 중심 워크플로우
- 빠른 스크립트 작성 & 자동화
- 에디터 없이 SSH 환경에서 작업

**주요 활용 방법**

```bash
# 파일 생성 및 수정
opencode "Next.js API route 만들어줘 /api/users GET"

# 리팩토링
opencode "이 컴포넌트를 Server Component로 변환해줘"

# 디버깅
opencode "이 에러 수정해줘: [에러 메시지]"
```

**Cursor vs OpenCode 비교**

| | Cursor | OpenCode |
|---|---|---|
| 인터페이스 | GUI 에디터 | CLI 터미널 |
| 컨텍스트 | 프로젝트 전체 | 지정 파일/범위 |
| 적합한 작업 | 복잡한 구현 | 빠른 수정/자동화 |
| 멀티파일 | ✅ Composer | ✅ 가능 |

---

## 모바일 개발 — Jules

### 📱 Jules (Google DeepMind) — 모바일 AI 코딩 에이전트

**언제 사용하나?**
- 이동 중 (출퇴근, 외출)
- PC 없이 급하게 수정이 필요할 때
- 아이디어가 떠올랐을 때 바로 구현

**특징**
- GitHub 저장소에 직접 연결
- 비동기 방식으로 작업 처리
- 브라우저/모바일에서 접근 가능
- 작업 완료 후 PR 자동 생성

**모바일 작업 플로우**

```
[모바일에서 Jules 접속]
        │
        ▼
[GitHub 저장소 선택]
        │
        ▼
[자연어로 작업 지시]
"헤더 컴포넌트에 다크모드 토글 추가해줘"
        │
        ▼
[Jules가 코드 분석 & 수정]  ← 비동기 처리
        │
        ▼
[PR 생성 → 알림 수신]
        │
        ▼
[PC에서 Cursor로 리뷰 & 머지]
        │
        ▼
[Vercel 자동 배포]
```

---

## 배포 프로세스 — Vercel

### 자동 배포 흐름

```
코드 작성 (Cursor / OpenCode / Jules)
        │
        ▼
git commit & push (또는 Jules PR 머지)
        │
        ▼
Vercel 빌드 트리거 (자동)
        │
   ┌────┴────┐
   ▼         ▼
feature/*   main
프리뷰 URL  프로덕션 배포
```

### 환경 구성

| 환경 | 브랜치 | URL 형태 |
|------|--------|---------|
| Production | `main` | `my-project.vercel.app` |
| Preview | `feature/*`, `develop` | `my-project-git-feature-xxx.vercel.app` |
| Development | 로컬 | `localhost:3000` |

### vercel.json 설정 예시

```json
{
  "buildCommand": "npm run build",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "framework": "nextjs",
  "regions": ["icn1"]
}
```

---

## 시나리오 시연

### 📖 실제 작업 시나리오

**상황**: 블로그 프로젝트에 "태그 필터링" 기능 추가

---

#### Step 1 — 출퇴근 중 (모바일 · Jules)

```
Jules에 접속 →
"posts 페이지에 태그 필터링 UI 추가해줘.
 현재 /app/posts/page.tsx 파일 참고해서
 Tailwind로 스타일링, TypeScript 사용"

→ Jules가 비동기로 코드 작성
→ PR #42 생성 완료 알림 수신
```

---

#### Step 2 — 사무실 도착 후 (PC · Cursor)

```
PR #42 내용을 Cursor에서 리뷰
→ Cmd + K 로 일부 로직 개선
→ 코드 머지
→ Vercel 프리뷰 URL 자동 생성
→ 브라우저에서 기능 확인
```

---

#### Step 3 — 빠른 버그 수정 (PC · OpenCode)

```bash
# 터미널에서 바로 수정
opencode "태그 클릭 시 URL 파라미터가 누락되는 버그 수정해줘"
git push origin feature/tag-filter
# Vercel 프리뷰 재빌드 확인
```

---

#### Step 4 — 프로덕션 배포

```bash
git checkout main
git merge feature/tag-filter
git push origin main
# Vercel 프로덕션 자동 배포 완료 🚀
```

---

## 정리 및 Q&A

### ✅ 핵심 가치

> **"언제 어디서든, 어떤 환경에서든 AI와 함께 개발한다"**

| 환경 | 도구 | 가치 |
|------|------|------|
| 💻 로컬 심층 작업 | Cursor | 전체 컨텍스트 기반 AI 협업 |
| ⌨️ 로컬 빠른 작업 | OpenCode | 터미널 중심 즉각 처리 |
| 📱 모바일 비동기 작업 | Jules | 장소 제약 없는 개발 |
| 🚀 배포 자동화 | Vercel | 코드 푸시만으로 배포 완료 |

### 💡 이 워크플로우의 장점

- **컨텍스트 스위칭 최소화** — 상황에 맞는 도구 즉시 활용
- **배포 불안감 제거** — 프리뷰 URL로 항상 검증 후 배포
- **24시간 개발 가능** — 모바일에서도 Jules로 비동기 작업 위임
- **AI 보조로 생산성 극대화** — 반복 작업 자동화, 빠른 프로토타이핑

---

### 📎 참고 링크

- [Next.js 공식 문서](https://nextjs.org/docs)
- [Vercel 배포 가이드](https://vercel.com/docs)
- [Cursor 공식 사이트](https://cursor.sh)
- [OpenCode GitHub](https://github.com/sst/opencode)
- [Jules (Google)](https://jules.google)

---

*작성일: 2026년 3월 9일*
