## ADDED Requirements

### Requirement: 에이전틱 AI 워크플로우 발표 문서

발표 문서는 프론트엔드 개발자가 AI 에이전트(OpenSpec, GitHub Copilot Agent, Jules, Cursor Automations)를 조합하여 1인 풀스택 팀을 구현하는 방법을 실사례(독박게임 프로젝트) 기반으로 설명하는 내용을 SHALL 포함해야 한다. 각 Phase는 독립적으로 읽어도 이해 가능하며, 전체 흐름으로 읽으면 완전한 워크플로우가 파악되어야 한다.

#### Scenario: Phase 1 — 로컬 초기 뼈대 구축 흐름 확인

- **WHEN** 독자가 Phase 1을 읽을 때
- **THEN** OpenSpec으로 아키텍처를 선언하고, GitHub Copilot과 Vercel CLI로 보일러플레이트를 구축하며 CI/CD 파이프라인을 연결하는 과정이 명령어와 함께 설명되어 있어야 한다

#### Scenario: Phase 2 — Jules 모바일 병렬 작업 흐름 확인

- **WHEN** 독자가 Phase 2를 읽을 때
- **THEN** Jules의 Gemini 모델 업데이트 히스토리(Gemini 2.5 → 3 Flash → 3 Pro), Jules CLI(`npm install -g @google/jules`, `jules remote new`) 사용법, 모바일에서 PR 생성 및 리뷰 흐름이 포함되어 있어야 한다

#### Scenario: Phase 3 — 로컬 풀스택 시연 흐름 확인

- **WHEN** 독자가 Phase 3을 읽을 때
- **THEN** GitHub Copilot Agent 호출로 `GameResult.tsx` 프론트엔드 코드와 Supabase `game_scores` 테이블 생성 SQL 및 RLS 정책 마이그레이션 파일이 동시에 생성되는 시연 내용이 포함되어 있어야 한다

#### Scenario: Phase 4 — Cursor Automations 인사이트 확인

- **WHEN** 독자가 Phase 4를 읽을 때
- **THEN** Cursor Automations의 트리거 유형(GitHub, Slack, Schedule, Webhook 등), Jules와의 차이점(명령형 vs 이벤트 드리븐 주도형), 그리고 Vercel 빌드 에러를 트리거로 핫픽스 PR을 자동 생성하는 시나리오가 포함되어 있어야 한다

### Requirement: 문서 형식 및 가독성 기준

발표 문서는 기술 블로그 수준의 가독성을 SHALL 갖춰야 하며, 각 Phase마다 Key Takeaway를 MUST 명시해야 한다. 모든 코드 블록에는 언어가 반드시 명시되어야 한다.

#### Scenario: Key Takeaway 포함 여부 확인

- **WHEN** 독자가 각 Phase 섹션의 끝을 읽을 때
- **THEN** `> 💡 Key Takeaway` 형식의 핵심 요약이 1개 이상 포함되어 있어야 한다

#### Scenario: 코드 블록 언어 명시 확인

- **WHEN** 문서에 코드 블록이 포함될 때
- **THEN** 모든 코드 블록에는 언어가 명시(`bash`, `sql`, `typescript`, `json` 등)되어야 한다

#### Scenario: Mermaid 다이어그램 포함 확인

- **WHEN** 독자가 전체 워크플로우 개요 섹션을 읽을 때
- **THEN** Mermaid `flowchart` 형식의 다이어그램이 1개 이상 포함되어 전체 흐름을 시각화해야 한다

### Requirement: Jules 최신 기능 정보 정확성

Jules 관련 내용은 공식 문서(jules.google/docs/changelog) 기반의 정확한 정보를 SHALL 담아야 한다. 모델 업데이트 히스토리와 CLI 명령어는 실제 동작하는 내용만 MUST 포함해야 한다.

#### Scenario: Jules 모델 업데이트 타임라인 포함

- **WHEN** 독자가 Jules 소개 섹션을 읽을 때
- **THEN** Gemini 2.5 Pro(베타, 2025.08) → Gemini 3 Pro(2025.11 Ultra) → Gemini 3 Flash(2026.01 전체) 의 업데이트 흐름이 표 또는 타임라인으로 제시되어야 한다

#### Scenario: Jules Continuous AI 기능 포함

- **WHEN** 독자가 Jules의 자동화 기능을 읽을 때
- **THEN** Scheduled Tasks, Suggested Tasks, CI Fixer, MCP 연동(Supabase 포함) 기능이 설명되어야 한다

#### Scenario: Jules CLI 실제 명령어 포함

- **WHEN** 독자가 Jules CLI 섹션을 읽을 때
- **THEN** `npm install -g @google/jules`, `jules remote new --session "..."`, `jules remote list --session`, `jules remote pull --session <id>` 명령어가 코드 블록으로 포함되어야 한다
