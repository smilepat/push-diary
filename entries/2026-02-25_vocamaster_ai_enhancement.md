# Dev Diary Entry

**Date**: 2026-02-25
**Project**: nlp-vocab-hwang (VocaMaster AI)

## Tasks Completed

### Codebase Analysis & Pain Point Identification

- Full analysis of VocaMaster AI (Smart AI Worksheet Generator) codebase
- Identified architecture: React 19 + Vite + Tailwind CSS + Google Gemini AI
- Mapped 9,012-word vocabulary database (Elementary 3-4, 5-6, Middle School)
- Created ANALYSIS_REPORT.md documenting critical/major/minor issues
- Prioritized 4 high-impact features for teacher convenience

### Feature 1: Offline Fallback (오프라인 모드)

- Created `services/offlineParser.ts` — regex-based Korean NLP parser
- Extracts grade level, word count, topic, and specific words from natural language
- Auto-fallback: tries Gemini AI first, switches to offline if API key missing or network fails
- Amber "오프라인" badge in navbar indicates mode

### Feature 2: Question Type Selection (문제 유형 선택)

- Added collapsible filter panel with 11 question type checkboxes
- "전체 선택" / "전체 해제" quick toggle buttons
- Counter badge shows selected/total types
- Filters passed to `generateWorksheet()` to control question pool

### Feature 3: Custom Worksheet Header (문제지 정보 설정)

- Collapsible form: school name, class, teacher name, date, time limit
- Optional student name field (성명란) toggle
- Printable header appears at top of generated worksheet
- Print CSS optimized for clean output

### Feature 4: Worksheet History (문제지 이력)

- Auto-saves up to 20 worksheets to localStorage
- Slide-in sidebar with history list (이력 button in navbar)
- Load previous worksheets or delete individual entries
- Badge counter shows saved worksheet count

### Refactoring & Infrastructure

- Extracted `validateWordsInData()` as reusable function in geminiService.ts
- Added question type filtering logic to `generateWorksheet()`
- New TypeScript types: `WorksheetHeader`, `WorksheetHistoryItem`
- Fixed multiple accessibility lint warnings (title attributes, aria labels)
- Deployed to Vercel: <https://nlp-vocab-hwang.vercel.app>
- Backed up project to E:\nlp-vocab-hwang

## Key Files Modified

- `App.tsx` — integrated all 4 features (UI + logic), ~1000 lines added
- `services/geminiService.ts` — validateWordsInData extraction, type filtering
- `services/offlineParser.ts` — NEW: offline Korean NLP parser (170 lines)
- `types.ts` — WorksheetHeader, WorksheetHistoryItem, questionTypes in GeneratorConfig
- `ANALYSIS_REPORT.md` — NEW: comprehensive codebase analysis

## Deployment URLs

- Vercel: <https://nlp-vocab-hwang.vercel.app>
- GitHub: <https://github.com/smilepat/nlp-vocab-hwang>
