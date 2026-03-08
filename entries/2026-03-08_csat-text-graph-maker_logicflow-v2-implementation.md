# LogicFlow v2 — Evidence 기반 마스터리 시스템 구현

- **Date**: 2026-03-08
- **Project**: csat-text-graph-maker
- **Location**: `E:\01_Projects\csat-text-graph-maker\`
- **Commit**: `2de754d` (feat: LogicFlow Phase 0-E)

## Summary

LogicFlow v2 전면 구현 완료. 기존 단순 점수 방식에서 Evidence 누적 기반 마스터리 산출 모델로 전환. 기출 문항 우선 활용 진단 시스템, 템플릿 기반 보강 문항, 인터랙티브 학습 과업, 대시보드 시각화까지 Phase 0~E 전체 구현.

## Tasks Completed

### Phase 0: Foundation
- Auth 중앙 집중화 (`auth.ts` + `constants.ts`) — 7개 API 라우트 통합
- `demo-user` 프로필 자동 생성 (FK constraint 해결)
- Turso DB 인덱스 7개 추가 (`scripts/add-indexes.ts`)
- 진단 문항에서 answer 미노출 확인

### Phase A: Evidence/Mastery 모델
- `evidence-aggregator.ts` — evidence 생성 + summary 자동 집계 + 난이도 캘리브레이션
- `mastery-engine.ts` — 시간 감쇠(14일 반감기), 가중 합산, streak 보너스, 추세 감지
- `difficulty-calibrator.ts` — compositeScore ↔ 실제 정답률 블렌딩
- `skill_evidence_summary` 테이블 추가 (사전 집계)
- `itemMicroSkills`/`learnerEvidence` 스키마 확장

### Phase B: 진단 신뢰도
- `template-items.ts` — B-01/B-02/B-03 템플릿 문항 (LLM 미사용, sentenceRelations 활용)
- `selectBalancedItems()` — easy/medium/hard 난이도 균등 배분
- 기출 문항 부족 시 템플릿 문항 자동 보충

### Phase C: 인터랙티브 학습 과업
- `chunking-task.tsx` — 복합문 절 경계 드래그 (A-02/A-03)
- `linking-task.tsx` — 대명사-선행사 연결 (B-01)
- `sentence-highlight.tsx` — IntersectionObserver 기반 문장 추적
- `use-focus-tracker.ts` — 클라이언트 읽기 행동 캡처 → Layer D evidence

### Phase D: 대시보드 시각화
- `skill-radar.tsx` — 12축 Recharts RadarChart (현재 vs 1등급 목표)
- `gap-table.tsx` — 갭 분석 테이블 (정렬, 색상 코딩, 진행률 바)
- `skill-tree.tsx` — 선수 관계 기반 3-Layer 스킬 트리

### Phase E: 통합
- 대시보드 페이지에 Radar/GapTable/SkillTree 통합
- 진단 페이지에 SentenceHighlight + FocusTracker 통합
- practice/[taskId] 페이지에 interactive task 지원

## E2E Verification Results

| 항목 | 결과 |
|------|------|
| TypeScript 컴파일 | PASS |
| 진단 시작 → 문항 선택 | PASS (기출+템플릿 혼합) |
| 문항 응답 → Evidence 기록 | PASS (6건 정상 적재) |
| Mastery 산출 | PASS (A-03: 100, A-02: 67, A-01: 0) |
| Summary 집계 | PASS (streak, weighted_score 정상) |
| 대시보드/진단/학습 페이지 | 모두 200 OK |
| 추천/과업 생성 API | 모두 200 OK |

## Key Decisions

- **Evidence 가중치**: correct_answer: +10, wrong_answer: -5, task_completion: +8, slow_response: -2
- **시간 감쇠**: 14일 반감기 (2주 전 evidence = 절반 가중치)
- **난이도 캘리브레이션**: 초기 compositeScore + 응답 축적 후 실제 정답률 블렌딩
- **FK constraint 해결**: `user_profiles`에 demo-user 레코드 자동 생성 필요

## File Changes

- **31 files** changed (15 modified + 16 new)
- **3,430 lines** added, 210 deleted
- 주요 신규: evidence-aggregator, mastery-engine, difficulty-calibrator, template-items, 7 components
- 주요 수정: schema.ts, diagnostic-engine.ts, 7 API routes, 4 pages

## Next Steps

- Interactive tasks 추가 (logic_mapping, paraphrase_matching, main_idea_selection)
- Layer D 행동 로깅 고도화 (reread 패턴, 정체 구간 감지)
- 난이도 캘리브레이션 실데이터 축적
- `next build` 검증 (E: exFAT 제약 → C: 드라이브에서 빌드)
