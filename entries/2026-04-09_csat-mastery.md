# 2026-04-09 csat-mastery

## 프로젝트
CSAT English Mastery - 수능 영어 마스터리 학습 앱

## 완료된 작업

### 컨텐츠 통합 파이프라인 구축 (Phase 1-6)
- DB 스키마 확장: subskills(48개), error_types(33개), SRS 필드, task_items 확장 컬럼
- taxonomy.ts: 48 서브스킬 → 12 마이크로스킬 매핑 + 수능 문항번호(18-45) → 스킬 매핑
- 7개 임포트 API: /api/import, import-generator, import-drills, import-csat, import-csat-csv, import-vocab, seed
- 5개 드릴 타입 프론트엔드: ClickPronoun, ClickConnector, ChunkSentence, HighlightModifier
- SRS 간격반복 엔진 (Ebbinghaus 망각곡선) + /api/review 스케줄링
- AI 문항 생성: Anthropic → Gemini 2.5 Flash 폴백 (/api/generate)
- 검증 API: /api/verify (72셀 매트릭스 리포트)

### 72셀 컨텐츠 채우기
- csat-vocab-db: 7,056문항 (CSAT_Vocabulary.csv 4,024단어 → cloze/유의어 문항 변환)
- csat-generator: 285문항 (usb_csat_mj_generator SQLite DB)
- ai_generated: 214문항 (Claude + Gemini AI 생성)
- csat-기출-db: 86문항 (local.db 수능 기출, 정답없는 108개 삭제)
- ai-english-platform: 87문항 (deepdive + quickscan JSON)
- csat-reading-app: 32문항 (드릴 30 + 지문 2)
- 총합: **7,821문항**, 72셀 모두 5개 이상

### 성능/안정성 수정
- 대시보드 seed 호출 제거 (불필요한 cold start 지연)
- tasks API 페이지네이션 (LIMIT 20 + RANDOM, 450KB→8KB)
- ALTER TABLE 최적화 (PRAGMA 사전 체크)
- blueprint/learn 페이지 재시도 로직 (cold start 대비)

## 주요 변경 파일
- src/lib/db.ts, types.ts, taxonomy.ts, srs.ts
- src/lib/generation/item-generator.ts, prompts.json
- src/lib/parser/csat-exam-parser.ts, answer-keys/2024.json
- src/app/api/ (import, import-generator, import-drills, import-csat, import-csat-csv, import-vocab, generate, review, verify)
- src/app/learn/[slug]/drills/ (ClickPronoun, ClickConnector, ChunkSentence, HighlightModifier)
- src/app/learn/[slug]/page.tsx, src/app/page.tsx

## 기술적 결정
- Anthropic API 크레딧 소진 시 Gemini 2.5 Flash 자동 폴백 (fetch 기반, SDK 불필요)
- csat_all.csv(9,811문항)보다 local.db(565문항, 구조화)를 1차 소스로 선택
- CSAT_Vocabulary.csv → 자동 문항 변환 (CEFR 기반 블록 배정, 유의어 오답지 생성)

## 해결된 이슈
- 정규식 /s 플래그 es2018 호환 문제
- Gemini 2.0 Flash deprecated → 2.5 Flash + thinking 비활성화
- Client disconnected 에러 (seed 호출 + 대용량 응답)
- 정답 없는 문항 108개 삭제

## 미해결 항목
- L 스킬 편중 (7,160/7,821 = 92%) — UX 영향 없음 (RANDOM LIMIT 20)
- D레이어 (TM, ST, SIM) AI 생성만으로 구성 — 실전 모의고사 데이터 보강 권장
