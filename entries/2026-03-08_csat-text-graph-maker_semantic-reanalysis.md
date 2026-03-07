# 2026-03-08 | CSAT Text Graph Maker — Semantic Re-analysis 완료

## Project
- **Repo**: `smilepat/csat-text-graph-maker`
- **Location**: `E:\01_Projects\csat-text-graph-maker\`

## Summary (요약)
565개 수능 영어 지문에 대해 Gemini 기반 LLM semantic analysis + embedding 생성을 완료했다.
Zod v4 + AI SDK v6 + Gemini 호환성 이슈를 해결하고, 배치 스크립트로 전체 지문을 처리했다.
Graph visualization, passage filters, Firebase Auth UI도 함께 구현/커밋했다.

## Tasks Completed
- [x] Graph visualization 커밋 (react-force-graph-2d, Topic Network + Passage Graph)
- [x] Embedding + SIMILAR_TO + passage list filters 커밋
- [x] Firebase Auth 인프라 구축 (AuthProvider, login page)
- [x] Semantic analyzer Zod v4 호환성 수정 (`mode: 'json'`, simplified schema)
- [x] Gemini API 키 설정 및 테스트
- [x] 배치 re-analysis 스크립트 작성 (`scripts/reanalyze-semantic.ts`)
- [x] 565/565 semantic analysis 완료 (topics, concepts, skills, discourse, sentence roles)
- [x] 565/565 embedding 생성 완료 (`gemini-embedding-001`)
- [x] 598 SIMILAR_TO relationships 생성 (cosine similarity > 0.80)
- [x] Neo4j enrichment: Topic 4589, Concept 1460, Skill 10, DiscourseStructure 9
- [x] GitHub push 완료

## Key Learnings
1. **Zod v4 + generateObject 비호환**: `.describe()`, `.min()`, `.max()`, `.enum()` 사용 시 Gemini structured output 실패. `mode: 'json'`으로 전환하고 schema를 단순 `z.string()`/`z.number()`로 변경, 제약조건은 프롬프트 텍스트에 기술.
2. **Embedding 모델명**: `text-embedding-004` 사용 불가. API 모델 목록 조회 후 `gemini-embedding-001`이 올바른 모델명임을 확인.
3. **dotenv `.env.local` 로딩**: `import 'dotenv/config'`는 `.env`만 로드. `.env.local`은 `dotenv.config({ path: '.env.local' })` 명시 필요.
4. **배치 처리 성공률**: 1.5초 딜레이로 552개 지문 100% 성공 (Gemini 2.0 Flash rate limit 여유).

## Git Commits (이번 세션)
- `21c2e7e` — feat: graph visualization (react-force-graph-2d)
- `348c2fe` — feat: embedding + similarity + passage filters
- `8a54754` — feat: Firebase Auth login/provider
- `ca8dcc3` — fix: semantic analyzer zod v4 + Gemini JSON mode

## Data Status
| Metric | Count |
|--------|-------|
| Passages | 565 |
| Semantic Analysis | 565/565 |
| Embeddings | 565/565 |
| Neo4j Topics | 4,589 |
| Neo4j Concepts | 1,460 |
| SIMILAR_TO edges | 598 |

## Remaining
- Firebase 프로젝트 생성 (Firebase Console에서 수동 설정 필요)
- Dashboard 페이지 실제 데이터 연동 (현재 정적 데이터)
