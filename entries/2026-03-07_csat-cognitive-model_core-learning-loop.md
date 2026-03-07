# 2026-03-07 — Ontology CSAT English App: 핵심 학습 루프 완성 (Phase 1)

## Project
ontology-csat-cognitivemodel-englishapp

## Summary
학생의 실제 학습 프로세스가 작동하도록 핵심 3대 엔진(지식 그래프, NLP 인지 파서, BKT 채점)을 구현하고, 프론트엔드에 학생 선택/학습 이력/오답 해설 기능을 추가하여 Core Learning Loop를 완성했다.

## Tasks Completed

### 백엔드 — In-Memory Knowledge Graph Engine (`graphEngine.js`)
- **그래프 구조**: Student, Tag, Passage 노드 + HAS_MASTERY, REQUIRES, ATTEMPTED 엣지 (18 nodes, 48 edges)
- **BKT 채점**: Bayesian Knowledge Tracing 수식 (pSlip=0.1, pGuess=0.25, pTransit=0.15) + 반응시간 기반 Speed Factor
- **ZPD 기반 추천**: `recommendMission()` — 학생의 가장 약한 태그를 그래프 탐색으로 찾고, 난이도 범위(±0.3) 내 지문 추천
- **클래스 분석**: `getClassInsights()` — 전체 학생 노드 집계로 실시간 bottleneck 도출

### 백엔드 — NLP 인지 텍스트 파서
- **`POST /api/analyze-text`**: 영어 텍스트의 7가지 인지 부하 태그를 정규식 기반으로 자동 분석
  - D_LONG(장문), D_EMB(내포절), C_CONN(접속사), C_REF(지시 추적), R_INF(추론), R_GLOB(글로벌 맥락), V_LOW(저빈도 어휘)

### 백엔드 — 새 API 엔드포인트
- `GET /api/students` — 전체 학생 목록 (드롭다운용)
- `GET /api/student/:id/history` — 학습 이력 + 정답률 통계

### 프론트엔드
- **학생 선택 드롭다운**: 5명 학생 간 전환 가능 (StudentDashboard.jsx)
- **학습 이력 타임라인**: 최근 미션 결과를 ✅/❌ 카드로 표시 + 정답률
- **오답 해설 패널**: 오답 시 정답 표시 + 인지 태그별 학습 팁 제공 (MissionPlayer.jsx)
- **Next Mission 자동 연결**: 미션 완료 → 바로 다음 미션 생성 (연속 학습)
- **API URL 중앙관리**: `config.js`로 모든 컴포넌트의 API 호출을 단일 변수로 통합

### 인프라
- `.gitignore` 생성 → `node_modules/`, `.env`, `dist/` 제외
- `backend/node_modules` 및 `backend/.env`를 git 추적에서 제거 (보안 + 용량)

## Key Commits
- `cd07f23` — feat: Phase 1 Core Learning Loop - Graph Engine, NLP Parser, Student Selector, History Timeline
- `c06bbee` — feat: Implement adaptive mission generation and interactive mission player components

## Key Learnings
- Neo4j 서버 없이도 In-Memory Graph로 그래프 탐색/추천 알고리즘 구현 가능 → 개발/데모 환경에서 외부 DB 의존성 제거
- BKT(Bayesian Knowledge Tracing) 수식은 4지선다에서 pGuess=0.25로 설정하면 현실적인 마스터리 추정 가능
- Express.js에서 `node server.js` 재시작 시 이전 프로세스가 포트를 점유하는 Windows 특유의 문제 → `netstat -aon | findstr :PORT`로 PID 확인 후 kill 필요

## Tech Stack
- React (Vite), Recharts, Lucide-React
- Express.js, OpenAI API (gpt-4o-mini)
- In-Memory Knowledge Graph (Neo4j 호환 구조)
- NLP Heuristic Parser (regex 기반 7-tag cognitive load analysis)
