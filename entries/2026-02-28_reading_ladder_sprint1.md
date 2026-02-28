# Dev Diary Entry

**Date**: 2026-02-28
**Project**: reading-ladder (EFL Reading Ladder)

## Summary

Sprint 1 complete — scaffolded the entire EFL Reading Ladder web app from the APP_DEV_PRD.md v2.0 spec. Full auth, onboarding with placement test, and dashboard skeleton are working locally.

## Tasks Completed

### Project Setup
- Created Next.js 16.1.6 project at `E:\01_Projects\reading-ladder\`
- Stack: TypeScript, Tailwind CSS v4 (OKLCH), shadcn/ui (new-york), React 19
- Installed 17 shadcn/ui components
- GitHub repo: `smilepat/reading-ladder` (private)

### Prisma Schema + Database
- Full schema with 24 models (User, ReadingText, TextParagraph, TextVocabulary, TextGrammarTag, TextQuestion, TextExercise, ReadingProgress, PlacementResult, Achievement, WordBankEntry, Class, Assignment, etc.)
- SQLite for local dev with better-sqlite3 adapter
- Prisma 7.4.2 with `prisma-client-js` generator

### Auth System
- Cookie-based auth (userId:sessionToken, httpOnly, 7-day expiry)
- 4 API routes: signup, signin, signout, session
- AuthProvider context with session restore
- Middleware for route protection + onboarding redirect

### Onboarding Flow
- 3-step wizard: Profile (name, grade, role) → Placement Test → First Text
- 10-question placement test spanning Stage 1-7 difficulty
- Stage assignment algorithm (highest stage with 70%+ correct)
- PlacementQuiz, PlacementResult, ProfileStep components

### Dashboard
- Stats cards (texts completed, streak, current stage, vocab count)
- Stage progress bars for all 7 stages
- Dashboard API with streak calculation

### Content Preparation Manual
- `docs/CONTENT_PREPARATION_MANUAL.md` (751 lines)
- Covers all 7 stages, 5D vocab tagging, question types, CSAT alignment, JSON format

## Key Technical Issues & Solutions

### Prisma 7 + Next.js 16 + Windows Compatibility
1. **Turbopack FATAL error**: Junction point creation fails on Windows with Prisma. Fixed by using `--webpack` flag for dev/build.
2. **Prisma 7 engine type "client"**: Requires adapter, not direct connection. Installed `@prisma/adapter-better-sqlite3` + `better-sqlite3`.
3. **Adapter export name**: `PrismaBetterSqlite3` (not `PrismaBetterSQLite3` — casing matters).
4. **Adapter API**: Takes `{ url: "file:..." }` config object, NOT a raw Database instance.
5. **Database file location**: `prisma.config.ts` resolves `file:./dev.db` relative to project root, not `prisma/` directory.

### Tailwind v4 Differences
- Uses `@import "tailwindcss"` + `@theme inline {}` (not `tailwind.config.ts`)
- OKLCH color space (not HSL)
- shadcn/ui components auto-configured for OKLCH

## Files Created (70 files, ~19,000 lines)

### Core
- `prisma/schema.prisma` — 24 models
- `lib/db.ts` — Prisma client + 17 helper functions
- `lib/constants.ts` — Stages, CEFR, categories, achievements, etc.
- `lib/types.ts` — TypeScript types
- `lib/neo4j.ts` — Vocab API proxy
- `middleware.ts` — Route protection

### Pages
- `app/page.tsx` — Landing page
- `app/(auth)/login/page.tsx`, `signup/page.tsx` — Auth pages
- `app/(onboarding)/welcome/page.tsx`, `placement/page.tsx` — Onboarding
- `app/(main)/dashboard/page.tsx` — Dashboard

### API Routes
- `app/api/auth/{signup,signin,signout,session}/route.ts`
- `app/api/placement/route.ts`
- `app/api/user/{profile,onboard}/route.ts`
- `app/api/dashboard/route.ts`

### Components
- `components/layout/{main-nav,user-nav,sidebar}.tsx`
- `components/onboarding/{placement-quiz,placement-result,profile-step}.tsx`
- `components/providers/{auth-provider,theme-provider}.tsx`
- 17 shadcn/ui components

## Next Steps (Sprint 2)
- Content pipeline scripts (text analyzer, seed data generator)
- Stage Map page with visual progression
- Reading viewer with paragraph-level interaction
- Text seed data (at least 3 texts per stage)
- Neo4j vocabulary integration testing

## Lessons Learned
- Prisma 7 is a major breaking change — adapter pattern is now mandatory for `prisma-client-js` generator
- Next.js 16 deprecates `middleware.ts` in favor of `proxy.ts` — works for now but will need migration
- Always check package exports with `Object.keys(require('...'))` when constructor names don't match docs
- Windows + Turbopack + Prisma junction points = known fatal bug, use webpack fallback
