## Context

이 문서는 단순 툴 소개가 아닌 **기술 아티클 형식의 발표 자료**다.
독자: 사내 프로젝트에 얽매여 개인 사이드 프로젝트를 제대로 관리하지 못하는 프론트엔드 개발자.
목표: AI 에이전트를 조합(오케스트레이션)하면 1인 풀스택 팀이 가능하다는 것을 실사례로 설득.

## Goals / Non-Goals

- **Goals**
  - 실제 운영 중인 프로젝트(독박게임)를 기반으로 신뢰성 확보
  - Jules의 최신 기능(Gemini 3, MCP, CI Fixer, Continuous AI)을 타임라인으로 정리
  - Cursor Automations와 Jules의 차이점(명령형 vs 주도형)을 명확히 구분
  - 각 Phase마다 독자가 즉시 따라할 수 있는 실행 가능한 명령어 제공

- **Non-Goals**
  - 독박게임 소스코드 전체 공개 (URL과 맥락 소개만)
  - Supabase/Next.js 기초 튜토리얼 (이미 알고 있다고 가정)
  - Cursor vs Copilot 비교 우위 논쟁

## Decisions

### 문서 파일명
- `ai-agentic-workflow.md` 사용
- 이유: 검색 친화적이며 내용을 정확히 반영

### 스토리라인 구조
- 4-Phase 순차 진행: 시간 흐름(과거 → 현재 → 미래) + 공간 확장(로컬 → 모바일 → 자동화)
- 각 Phase는 독립적으로 읽어도 이해 가능하되, 흐름으로 읽으면 전체 그림이 완성

### 다이어그램 형식
- Mermaid `flowchart TD` 사용 (GitHub/Vercel에서 렌더링 지원)
- ASCII 다이어그램 병행 사용 (범용성 보장)

### Jules 모델 히스토리 표현
- 타임라인 표로 정리: Gemini 2.5 Pro(베타) → Gemini 3 Flash(2026.01) → Gemini 3 Pro(2025.11 Ultra, 이후 전체)

### Cursor Automations 설명 방식
- Jules와의 비교 매트릭스 제공: 트리거 방식, 실행 환경, 적합한 유스케이스
- 실제 트리거 유형 표로 정리 (Schedule, GitHub, Slack, Linear, PagerDuty, Webhook)

## Risks / Trade-offs

- Jules CLI 명령어가 `jules ask` / `jules run`이 아닌 `jules remote new` 구조임
  → 실제 CLI 문서 기반으로 정확하게 표기 필요 (`jules remote new --session "..."`)
- Cursor Automations는 Cloud Agent 사용량 기반 과금 → 비용 주의사항 명시
- 독박게임 URL(https://dokbakgame.vercel.app)은 실제 운영 중이므로 민감 정보 노출 주의

## Open Questions

- 기존 `workflow-presentation.md` 삭제 여부: 승인 후 결정 (기본: 유지하되 deprecated 표시)
