# Change: 에이전틱 AI 풀스택 워크플로우 발표 문서 신규 작성

## Why

기존 `workflow-presentation.md`는 도구 소개 수준의 범용 문서다.
독자(프론트엔드 개발자)가 실제 사이드 프로젝트에서 AI 에이전트를 어떻게 조합해 1인 풀스택 팀을 구축하는지, 구체적인 실사례(독박게임 프로젝트)와 함께 보여주는 심층 기술 아티클이 필요하다.
2026년 3월 출시된 Cursor Automations까지 포함해 "반응형 AI → 주도형 AI"로의 패러다임 전환 인사이트를 제공한다.

## What Changes

- 기존 `workflow-presentation.md` 파일을 **대체**하는 새 발표 문서 `ai-agentic-workflow.md` 작성
- 독박게임(https://dokbakgame.vercel.app) 실사례 기반 4-Phase 스토리라인 구성
- Jules 최신 업데이트(Gemini 3 Flash → Gemini 3 Pro, MCP, CI Fixer, Continuous AI) 반영
- Jules CLI(`@google/jules`) 실제 명령어 시연 포함
- Cursor Automations(2026.03.05) 이벤트 드리븐 자동화 인사이트 포함
- OpenSpec + GitHub Copilot Agent를 통한 Supabase 풀스택 시연 내용 포함

## Impact

- Affected specs: `presentation-doc` (신규 capability)
- Affected files: `workflow-presentation.md` (deprecated, 유지 또는 삭제 선택), `ai-agentic-workflow.md` (신규 생성)
- Breaking: 없음 (문서 프로젝트이므로 기존 파일 삭제 없이 신규 추가)
