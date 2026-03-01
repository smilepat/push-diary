# AI English Platform — Phase 1-5 Memory Engine

- **Date**: 2026-03-01
- **Project**: ai-english-platform
- **Repo**: smilepat/ai-english-platform
- **Commit**: `78921df`

## Summary

Phase 1-5 Memory Engine (SRS) + Cron 구현 완료. Ebbinghaus 망각곡선 기반 Spaced Repetition System을 구축하여 학생 어휘 기억 강도(S)를 추적하고, 매일 06:00 KST cron이 P_forget 계산 후 ReviewQueue를 자동 빌드하는 시스템.

## Tasks Completed

- [x] `types/memory.ts` — Zod 스키마 + 응답 인터페이스 + SRS 상수
- [x] `lib/memory/formulas.ts` — 순수 수학 함수 7개 (quality, S 갱신, P_forget, interval, priority)
- [x] `lib/memory/engine.ts` — processVocabReview (트랜잭션) + buildDailyReviewQueue (배치)
- [x] `app/api/memory/review-queue/route.ts` — GET 오늘의 복습 큐 (student)
- [x] `app/api/memory/review-complete/route.ts` — POST 복습 완료 → S 갱신 (student)
- [x] `app/api/memory/stats/route.ts` — GET 개인 기억 통계 (student)
- [x] `app/api/memory/class-stats/[classId]/route.ts` — GET 반 기억 통계 (teacher/org_admin)
- [x] `app/api/cron/build-review-queue/route.ts` — 일일 배치 (06:00 KST = 21:00 UTC)
- [x] `vercel.json` — cron 스케줄 추가
- [x] `tests/unit/memory-formulas.test.ts` — 29개 단위 테스트 전부 통과
- [x] E: 드라이브 동기화 완료

## Key Formulas (PRD Section 10)

```
quality = 0.6 * correct + 0.2 * (1 - rt_norm) + 0.2 * conf_norm
S_new = clamp(S_old * (1 + 0.35 * (quality - 0.5)), 0.5, 60)
P_forget(t) = 1 - exp(-t / S)
interval = S * -ln(1 - 0.85) ≈ S * 1.897
Priority: HIGH >= 0.80, MEDIUM >= 0.60, LOW < 0.60
```

## Key Learnings

- Prisma Vocabulary 모델은 `lemma`/`meaningKo` 필드명 사용 (word/meaning 아님)
- ClassEnrollment 모델명 (enrollment 아님)
- discriminated union 패턴에서 early return 후에도 TS가 narrowing 못 함 → `auth.class!` 필요
- vitest globals:true 설정으로 import 없이 describe/it/expect 사용 가능

## Phase 1 Progress

| Phase | 내용 | 상태 |
|-------|------|------|
| 1-1 | 반 관리 + 학생 등록 | Done |
| 1-2 | 과제 시스템 CRUD | Done |
| 1-3 | Quick Scan 진단 | Done |
| 1-4 | 5D Deep Dive 진단 | Done |
| 1-5 | Memory Engine + Cron | Done |
| 1-6 | SRS 복습 큐 UI | Next |
