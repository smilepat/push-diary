# vocab-master-system - Graph Explorer & VocaLearn 기획

**Date**: 2026-03-01
**Project**: vocab-master-system + vocalearn
**Session**: Graph DB 개념 정리, Graph Explorer 구현, VocaLearn PRD 작성

---

## 📋 Tasks Completed

### 1. Graph DB 개념 설명 (교육용 문서)

- "진짜 그래프DB vs 우리 시스템" 비교 섹션 추가
- 3가지 DB 유형 비교 (스프레드시트 / 관계형+그래프 패턴 / 네이티브 그래프DB)
- Index-Free Adjacency 개념 설명
- 초등학생 수준 FAQ + 비유 추가

### 2. Graph Explorer 구현 (vis-network 시각화)

- `E:\vocab-graph-db\vocabulary_graph.json` (48MB, 76K 노드) 활용
- Word 노드 + 의미 관계만 추출 → `word_graph.json` (3.2MB 경량판)
  - 14,146개 단어 노드
  - 42,506개 관계 (동의어/반의어/상위어/하위어)
- vis-network 기반 인터랙티브 그래프 시각화
- BFS 탐색 (1~5단계 깊이)
- 최단 경로 탐색 (BFS)
- 노드 클릭 → 상세 정보 / 더블클릭 → 재탐색
- CEFR 레벨별 색상 구분
- 관계 유형 필터 (동의어/반의어/상위어/하위어)
- Lazy Loading (섹션 활성화 시 데이터 로드)

### 3. Relational DB vs RAG 분석

- 현재 시스템 검색 방식: SQL 기반 Relational DB (RAG 아님)
- RAG 도입 필요성 분석
  - 학습 진단: SQL(기계적) vs RAG(맞춤 처방) 비교
  - 학습 컨텐츠: 고정 템플릿 vs AI 생성 비교
- 결론: 현재 단계에서는 불필요, 학생용 앱 시 도입

### 4. VocaLearn 로드맵 작성

- 현재 앱 = 데이터 구축 도구 (독립 유지)
- VocaLearn = 새 RAG 기반 학습 앱 (별도 프로젝트)
- 4 Phase 로드맵: Data Pipeline → RAG Engine → 학습 앱 → 고급 기능
- 기술 스택: Next.js 14 + Turso + Pinecone + Gemini

### 5. VocaLearn PRD 작성

- `E:\vocalearn\PRD.md` 생성
- 12개 기능 요구사항 (P0~P3)
  - P0: 인증, 학습 카드, SRS, 기본 퀴즈
  - P1: AI 튜터(RAG), AI 진단, 적응형 퀴즈
  - P2: 의미 맵, 대시보드
  - P3: 음성, 게이미피케이션, 소셜
- DB 설계: 5개 테이블 (students, progress, wrong_patterns, quiz, chat)
- API 설계: 9개 엔드포인트
- UI/UX: 모바일 퍼스트, 다크 모드
- 비용: 무료 티어 운영 가능 ($0/월)

---

## 🗂️ Files Changed

### vocab-master-system (3 commits)

1. `public/index.html` — Graph Explorer 섹션 + 사이드바 메뉴 추가
2. `public/js/graph-explorer.js` — vis-network 시각화 (신규, 10KB)
3. `public/css/styles.css` — Graph Explorer CSS 추가
4. `public/data/word_graph.json` — 경량 그래프 데이터 (신규, 3.2MB)

### vocalearn (신규 프로젝트)

5. `E:\vocalearn\PRD.md` — 상세 PRD 문서 (신규)

---

## 🎯 Technical Decisions

### 1. 48MB JSON → 3.2MB 경량화

- 원본: 76,561 노드, 137,414 엣지 (48MB)
- 경량판: Word 노드 + 의미 관계만 (14K 노드, 42K 엣지)
- Vercel 배포 크기 제한 고려

### 2. 클라이언트 사이드 그래프 탐색

- 서버 API 없이 브라우저에서 직접 BFS/최단경로 연산
- 기존 서버 코드 변경 불필요 → 안전

### 3. 두 시스템 완전 분리

- Vocab Master DB: 데이터 구축 & 관리 (현재)
- VocaLearn: RAG 기반 학습 앱 (향후, 별도 프로젝트)
- 데이터 흐름: 단방향 (관리→학습, 읽기 전용)

---

## 📊 System Status

### Production

- ✅ <https://vocab-master-system.vercel.app> (배포 완료)
- ✅ Graph Explorer 기능 활성화 (🔮 사이드바 메뉴)

### Git

- ✅ Branch: master
- ✅ Latest: f14b5c1 (Graph Explorer)
- ✅ Pushed to origin

---

## 📌 Next Steps

1. **VocaLearn Phase 1 구현 시작**
   - Next.js 프로젝트 초기화
   - NextAuth 인증 설정
   - Turso DB 스키마 생성
   - Pinecone 벡터 동기화 스크립트

---

**Total Session Time**: ~2 hours
**Commits**: 3 (vocab-master-system)
**Files Created**: 3 new, 2 modified
**Deployment**: ✅ Vercel production
