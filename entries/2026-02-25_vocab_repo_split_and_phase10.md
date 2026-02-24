# Dev Diary Entry

**Date**: 2026-02-25
**Project**: vocabulary-db / vocab-cat-test / vocab-graph-db

## Tasks Completed

### Phase 10: Mixed Question Type Implementation
- Activated all 6 question types in adaptive test (previously only Type 1 was used)
- Added `choose_question_type()` to session_manager.py for dynamic type selection
- Progressive introduction: items 0-4 use Types [1,2], items 5-14 use [1,2,3,5], items 15+ all types
- Added question type selector UI in SurveyScreen (7 options: Mixed + Types 1-6)
- Applied per-type difficulty modifiers (`QUESTION_TYPE_B_MODIFIER`)
- All 162 tests passed, frontend build successful

### Cloud Run + Vercel Redeployment
- Backend redeployed to Cloud Run (revision 00002-pj7)
- Frontend redeployed to Vercel with updated build
- Both apps confirmed working at production URLs

### Documentation
- Created `IRT_어휘평가_만들기_가이드.md` — elementary-level explanation of the entire IRT CAT system
- Created `프로젝트_개발과정_및_최종앱_분석.md` — comprehensive development history and final app analysis

### Repository Split (3-way)
- Analyzed dependencies and determined monorepo should be split
- **vocabulary-db** (https://github.com/smilepat/vocabulary-db) — data archive only (01-06 folders + CSVs)
- **vocab-graph-db** (https://github.com/smilepat/vocab-graph-db) — Graph DB visualization app (Node.js + vis.js)
- **vocab-cat-test** (https://github.com/smilepat/vocab-cat-test) — IRT CAT diagnostic test (FastAPI + React)
- Each repo includes its required data files (CSV, graph JSON)
- Updated config.py graph path: `07_Graph_DB_Project/vocabulary_graph.json` → `vocabulary_graph.json`
- Updated Dockerfile to include vocabulary_graph.json
- Cleaned original repo: removed app code, deployment files, redundant backup CSVs
- Updated README with cross-links to separated repos
- All 3 repos backed up to E: drive

## Key Files Modified
- `irt_cat_engine/api/session_manager.py` — choose_question_type(), adjust_item_difficulty()
- `irt_cat_engine/api/routes_test.py` — mixed-mode branching
- `irt_cat_engine/api/schemas.py` — default question_type 1→0
- `irt_cat_engine/frontend/src/components/SurveyScreen.tsx` — question type selector
- `irt_cat_engine/frontend/src/App.tsx` — pass question_type from survey
- `irt_cat_engine/config.py` — graph path updated for standalone repo

## Deployment URLs
- Frontend: https://vocab-cat-test.vercel.app
- Backend API: https://vocab-cat-api-440067489993.us-central1.run.app
- Graph DB: https://vocab-graph-db.vercel.app
