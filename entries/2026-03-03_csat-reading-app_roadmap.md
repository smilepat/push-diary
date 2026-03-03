# Dev Diary Entry

**Date**: 2026-03-03
**Project**: csat-reading-app (CSAT Reading Coach)

## Summary

Phase 1 현황 분석 완료 + 개발 로드맵 문서 작성. 전체 코드베이스(엔진, 드릴, 미션 플로우, DB 스키마, 시드 데이터)를 분석하고 Phase 2~6 계획을 정리한 `docs/DEVELOPMENT-ROADMAP.md`를 작성하여 GitHub에 push했다.

## Tasks Completed

### Phase 1 현황 분석
- 프로젝트 전체 구조 파악: 27개 소스 파일, 5개 DB 테이블 스키마, 30개 드릴 + 10개 지문 시드 데이터
- 완성 항목: 인증 인프라, 타입 시스템, 엔진 순수 함수(calculate-delta, generate-assignment), 에러 태그 15종, 드릴 컴포넌트 5종, 미션 플로우 UI
- 미완성 항목: API 라우트 0개, DB 미적용, 프론트엔드 데모 데이터만 사용

### 개발 로드맵 문서 작성
- `docs/DEVELOPMENT-ROADMAP.md` (254줄) 생성
- Phase 1 완료 현황 테이블 정리
- Phase 2: Supabase 연동 + API 6개 (assignment/today, attempt, assignment/complete, scores, history, dashboard/stats)
- Phase 3: Dashboard 실데이터 + recharts 차트
- Phase 4: 짧은 지문 읽기를 미션 플로우에 통합
- Phase 5: 학습자 온보딩 + 초기 진단
- Phase 6: 콘텐츠 확장 (30→150+ 드릴, 10→50+ 지문)
- 기술 부채 목록 + 즉시 시작 가능한 작업 우선순위

### GitHub Push
- 커밋: `76b5748` — `docs: add development roadmap (Phase 1 status + Phase 2-6 plan)`
- 리포: `smilepat/csat-reading-app` (main 브랜치)

## Project Architecture (현재)

```
Stack: Next.js 16 + React 19 + Tailwind 4 + TypeScript + Supabase
DB: 5 tables (short_passages, drills, layer_scores, attempts, assignments)
Engine: calculate-delta (점수 변동), generate-assignment (미션 생성) — 순수 함수
Drills: 5 types (click_pronoun, click_connector, chunk_sentence, reorder_clauses, highlight_modifier)
Error Tags: 15종 (C_PRON 5 + C_CONN 5 + D_절경계 5)
Seed Data: 30 drills (2/tag) + 10 short passages
```

## Next Steps

1. Supabase 프로젝트 설정 + 마이그레이션 적용 + 시드 데이터 삽입
2. `GET /api/assignment/today` 구현 (미션 생성 API)
3. `POST /api/attempt` 구현 (학습 시도 기록 + 점수 갱신)
4. `today-mission.tsx` 데모 데이터 → 실데이터 연동
5. Dashboard + History 페이지 실데이터 연동
