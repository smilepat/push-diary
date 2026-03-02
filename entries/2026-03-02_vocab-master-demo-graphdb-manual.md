# vocab-master-demo - Graph DB 설명서 추가

**Date**: 2026-03-02
**Project**: vocab-master-demo
**Session**: 사용자 매뉴얼에 Graph DB 작동방식/용어 설명 섹션 추가

---

## 📋 Tasks Completed

### 1. Graph DB (그래프 데이터베이스) 설명서 카드 추가

- Dashboard > 📘 사용자 설명서 패널 맨 하단에 새 카드 섹션 삽입
- 타이틀: "🗄️ Graph DB (그래프 데이터베이스) 설명서"
- 상단 안내 배너: "⚠️ 이 앱은 데모 앱입니다" + Neo4j + Turso 하이브리드 구조 설명
- 접기/펼치기 토글 기능 (카드 헤더 클릭 시 ▼/▶)

### 2. 설명서 7개 섹션 구성

| 섹션 | 내용 |
|------|------|
| 1. 기본 용어 | Node, Edge, Label, Property, Direction — 3열 테이블 + ASCII 그래프 |
| 2. 노드/엣지 구조 | Node(단어)와 Edge(관계)의 JSON 데이터 구조 (2열 그리드) |
| 3. 관계 유형 | SYNONYM_OF ~ WORD_FAMILY 8가지 관계 (4열 테이블) |
| 4. 핵심 알고리즘 | BFS(너비 우선 탐색) + Shortest Path ASCII 시각화 |
| 5. 하이브리드 아키텍처 | JSON / Neo4j / Turso 3계층 컬러 카드 + 쓰기/읽기 데이터 흐름 |
| 6. Cypher vs SQL | 동일 쿼리 비교 (다크 코드 블록) |
| 7. 용어 사전 | Traversal, Hop, Fallback, Dual-Write 등 9개 용어 |

- 하단에 📌 한 줄 요약 (노란 하이라이트 박스)

### 3. CSS 스타일 추가

- `.um-table thead th` — 테이블 헤더 스타일 (배경색, 굵은 글꼴, uppercase)
- `.um-section.collapsed` — 접기/펼치기 display:none 토글

---

## 🗂️ Files Changed

### vocab-master-demo (2 commits)

1. `public/index.html` — Graph DB 설명서 카드 193줄 추가 (user-manual-panel 내)
2. `public/css/styles.css` — um-table thead + collapsed 클래스 16줄 추가
3. `scripts/inspect-db.js` — DB 검사 스크립트 업데이트

---

## 🎯 Technical Decisions

### 1. 사용자 매뉴얼 패널 내부 배치

- 기존 "📚 핵심 용어 사전" 카드 바로 뒤, `</div> <!-- /user-manual-panel -->` 앞에 삽입
- 기존 매뉴얼 구조와 동일한 `.card` + `.um-section` 패턴 사용

### 2. 접기/펼치기 기능

- 내용이 길어 카드 헤더 클릭으로 토글 가능하도록 구현
- `.um-section.collapsed { display: none }` + inline onclick 핸들러

### 3. 시각적 디자인

- 3계층 아키텍처: 보라/파랑/초록 그라디언트 카드로 시각 구분
- Cypher/SQL 비교: 다크 코드 블록 (#1e293b 배경)으로 대비 극대화
- 한 줄 요약: 노란 그라디언트 박스 + 좌측 4px 보더 강조

---

## 📊 System Status

### Production

- ✅ <https://vocab-master-demo.vercel.app> (자동 배포)
- ✅ Graph DB 설명서 (📘 사용자 설명서 > 하단 카드)

### Git

- ✅ Branch: master
- ✅ Latest: 3c78440 (inspect-db.js update)
- ✅ Previous: a217070 (Graph DB 설명서 추가)
- ✅ Pushed to origin

---

**Total Session Time**: ~20 minutes
**Commits**: 2 (vocab-master-demo)
**Files Modified**: 3 (index.html, styles.css, inspect-db.js)
**Lines Added**: ~209
**Deployment**: ✅ Vercel production (auto-deploy)
