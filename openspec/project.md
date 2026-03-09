```markdown
# Project Context

## Purpose
발표용 마크다운 문서 프로젝트.
Next.js 프로젝트를 Vercel에 배포하고, 로컬에서는 Cursor와 OpenCode로, 모바일에서는 Jules를 활용하는 AI 개발 워크플로우 시나리오를 문서화하고 발표하는 것이 목표다.

## Tech Stack
- **문서 형식**: Markdown (`.md`)
- **프레임워크 (시나리오 대상)**: Next.js 14+ (App Router)
- **배포 (시나리오 대상)**: Vercel
- **언어 (시나리오 대상)**: TypeScript
- **스타일링 (시나리오 대상)**: Tailwind CSS
- **로컬 AI 에디터**: Cursor
- **로컬 AI CLI**: OpenCode
- **모바일 AI 에이전트**: Jules (Google)
- **버전 관리**: GitHub

## Project Conventions

### Code Style
- 문서 파일은 한국어로 작성하되, 코드 블록 내 코드는 영어 유지
- 마크다운 헤딩은 `#` ~ `###` 3단계까지 사용
- 표(table)로 비교 정보를 정리
- ASCII 다이어그램으로 흐름 시각화
- 코드 블록에는 언어 명시 (` ```bash `, ` ```json ` 등)

### Architecture Patterns
- 단일 문서 구조: 발표 슬라이드처럼 섹션 분리
- 섹션 순서: 개요 → 스택 → 세팅 → 도구별 설명 → 시나리오 시연 → 정리
- 각 도구(Cursor, OpenCode, Jules)는 독립 섹션으로 분리
- 실제 명령어와 시나리오를 포함해 실용성 강조

### Testing Strategy
- 해당 없음 (문서 프로젝트)
- 문서의 정확성은 실제 도구 동작과 대조하여 검증

### Git Workflow
- 브랜치 전략:
  - `main` → 최종 발표 버전
  - `feature/*` → 섹션별 초안 작업
- 커밋 메시지: `docs: [섹션명] 내용 추가/수정` 형식
- PR 생성 → Vercel 프리뷰 URL로 렌더링 확인 후 머지

## Domain Context
- 이 프로젝트는 AI 도구를 활용한 개발 워크플로우를 발표하기 위한 문서다
- 주요 독자: 개발자, 기술 발표 청중
- 핵심 메시지: "언제 어디서든 AI와 함께 개발한다"
- Cursor: GUI 기반 AI 에디터, 프로젝트 전체 컨텍스트 활용
- OpenCode: CLI 기반 AI 에이전트 (sst/opencode), 터미널 워크플로우
- Jules: Google의 비동기 AI 코딩 에이전트, 모바일에서 GitHub 저장소에 직접 연결
- Vercel: GitHub push 만으로 자동 배포, 브랜치별 프리뷰 URL 제공

## Important Constraints
- 발표 문서이므로 가독성 우선 — 과도한 기술 상세보다 흐름과 스토리 중심
- 실제 사용 가능한 명령어와 시나리오만 포함 (허구의 API 금지)
- 문서 분량: 단일 `.md` 파일 기준 적절한 길이 유지

## External Dependencies
- [Next.js](https://nextjs.org/docs) — 풀스택 웹 프레임워크
- [Vercel](https://vercel.com/docs) — CI/CD + 호스팅
- [Cursor](https://cursor.sh) — AI 페어 프로그래밍 에디터
- [OpenCode](https://github.com/sst/opencode) — 터미널 AI 코딩 에이전트
- [Jules](https://jules.google) — Google 비동기 AI 코딩 에이전트
- [GitHub](https://github.com) — 버전 관리 & 배포 트리거

```
