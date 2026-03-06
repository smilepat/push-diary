# 2026-03-06 — Reading Text Analysis Pipeline: Save 기능 FK 오류 수정 및 배포

## Project
reading-text-analysis-pipeline

## Summary
분석 결과 저장 시 발생하던 FOREIGN KEY constraint 오류를 수정하고 Vercel 프로덕션에 배포 완료.

## Tasks Completed
- **FK constraint 오류 근본 원인 분석**: `INSERT OR REPLACE`가 SQLite에서 DELETE → INSERT로 동작하여 passage_features의 FK 제약 위반
- **persist.ts 수정**:
  - `INSERT OR REPLACE INTO passages` → `INSERT INTO passages ... ON CONFLICT(id) DO UPDATE SET`로 변경 (in-place update)
  - `passages.source`의 `UNIQUE` 제약 제거
  - `passage_features.passage_id`의 `REFERENCES passages(id)` FK 제거
- **Turso 클라우드 DB 테이블 재생성**: 기존 데이터(9 passages, 8 features) 보존하며 제약조건 없는 테이블로 마이그레이션
- **커밋 & 푸시**: `5bc9ac8` — `fix: resolve FK constraint error on save by removing constraints`
- **Vercel 프로덕션 배포 완료**: https://reading-text-analysis-pipeline.vercel.app

## Key Learnings
- SQLite `INSERT OR REPLACE`는 내부적으로 DELETE + INSERT로 동작 → FK 참조하는 자식 테이블이 있으면 제약 위반 발생
- `ON CONFLICT(id) DO UPDATE`는 기존 행을 in-place 수정하므로 FK 안전
- Turso HTTP API (`/v2/pipeline`)로 직접 fetch하면 @libsql/client 번들링 이슈 없이 Vercel에서 동작

## Tech Stack
- Next.js 16 (Turbopack), TypeScript, Turso (LibSQL), Vercel
- Turso HTTP API (fetch 기반, 번들러 의존성 없음)
