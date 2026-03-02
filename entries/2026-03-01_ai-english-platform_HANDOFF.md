# Handoff Document — AI English Platform

**Date**: 2026-03-01
**Project**: ai-english-platform
**Location**: `C:\Users\user\Documents\GitHub\ai-english-platform\`
**GitHub**: `smilepat/ai-english-platform` (private, branch: main)
**PRD**: `PRD_v5.md` (v5.0.2 — 최종 사양서)

---

## Current State

### Phase 0: Complete
- 40 Prisma models → Supabase PostgreSQL 17 (ap-northeast-1)
- 55+ RLS policies across 10 SQL migration files
- Auth system: Supabase SSR + RBAC middleware + 3 auth paths
- UI shells: 4 role-specific dashboards (admin/teacher/student/parent)
- API patterns: withAuth/withRole HOFs, cursor-based pagination, Zod validation
- Core routes: org/create, class/create, notification/list, notification/read-all
- Vitest configured, Vercel deployment config ready

### Git History
```
d7da873 chore: remove TEST_ACCOUNTS.md from repo
f57cd18 docs: add push diary
95f07fc feat: Phase 0 complete (93 files, +19,729 lines)
ffda44f docs: PRD v3→v4→v5.0.2
```

### Uncommitted Changes
- `PRD_v3.md` deleted (git tracked, needs commit)
- `PRD_v4.md` deleted (git tracked, needs commit)
- → 다음 세션에서 `git add -A && git commit` 필요

---

## Supabase Connection

- **Project**: `rpxvlskmlrhivbiamicb`
- **Region**: aws-1-ap-northeast-1 (Tokyo)
- **DB password**: `iloveLD12abc`
- **Connection**: `sslmode=require` 필수
- **DATABASE_URL**: `postgresql://postgres.rpxvlskmlrhivbiamicb:iloveLD12abc@aws-1-ap-northeast-1.pooler.supabase.com:6543/postgres?pgbouncer=true&sslmode=require`
- **DIRECT_URL**: `postgresql://postgres.rpxvlskmlrhivbiamicb:iloveLD12abc@aws-1-ap-northeast-1.pooler.supabase.com:5432/postgres?sslmode=require`

### Applied SQL Migrations (via Supabase SQL Editor)
| File | Content |
|------|---------|
| 001_auth_trigger.sql | `handle_new_user()` — auth.users → public.User sync |
| 002_rls_helpers.sql | 5 helper functions (all with `auth.uid()::text`) |
| 003_indexes.sql | pg_trgm + 5 performance indexes |
| 004_rls_org_user.sql | Organization, User, UserOrganization policies |
| 005_rls_class.sql | Class, ClassEnrollment policies |
| 006_rls_assignment.sql | Assignment, Submission, AssignmentTarget policies |
| 007_rls_learning.sql | VocabMemory, SubskillMemory, Attempt, etc. policies |
| 008_rls_content.sql | Item, Vocabulary, Topic, Passage, etc. policies |
| 009_rls_notification.sql | Notification, NotificationSetting, ParentLink policies |
| 010_rls_teaching.sql | LessonPlan, TeacherOverride, ReadingLog policies |

### Seed Data (applied via `npx prisma db seed`)
- 46 Subskills (V01-V10, R01-R15, G01-G06, S01-S05, F01-F02, L01-L08)
- 33 ErrorTypes (E01-E33)
- Test org: `test-org-001` (테스트 학원, invite code: TESTORG)

### Test Accounts (Supabase auth + public.User)
| Role | Email | Password | Dashboard |
|------|-------|----------|-----------|
| org_admin | eltkorea@gmail.com | iloveLD12 | /admin/dashboard |
| teacher | teacher@test.com | test1234 | /teacher/dashboard |
| student | student@test.com | test1234 | /student/dashboard |
| parent | parent@test.com | test1234 | /parent/dashboard |

---

## Key Decisions & Gotchas

1. **`auth.uid()::text` casting**: Prisma `User.id` is `text`, Supabase `auth.uid()` returns `uuid` → 모든 RLS policy에 `::text` cast 필수
2. **`sslmode=require`**: Supabase connection pooler 접속 시 반드시 필요
3. **`prisma db push`** (not `migrate dev`): Supabase managed DB에서 shadow DB 불가
4. **No `src/` directory**: PRD Section 28 구조 — `app/`, `components/`, `lib/` 프로젝트 루트에 위치
5. **Prisma for schema, Supabase client for runtime**: Prisma는 Edge Runtime 미지원 → middleware + Server Components는 `@supabase/ssr` 사용
6. **Route group 충돌**: `(admin)/dashboard` 등이 `/dashboard`로 충돌 → `/admin/dashboard` 등 실제 경로 사용

---

## Resolved Issues This Session

1. **VS Code 10k 문제**: `C:\Users\user\.git` (빈 git repo) 삭제로 해결
2. **PRD v3/v4 삭제**: v5로 대체된 구 버전 파일 삭제 (커밋 대기 중)
3. **Push Diary 워크플로우**: "push diary" 명령으로 `E:/mydev-diary/entries/`에 작업 기록 → GitHub push

---

## Next Steps (Phase 1 준비)

PRD v5.0.2에 따른 다음 작업:
1. PRD v3/v4 삭제 커밋 & push
2. Vercel 배포 테스트
3. Phase 1 시작: Teacher Dashboard 실제 기능 구현
   - Class CRUD (학급 생성/수정/삭제)
   - Student management (학생 초대/등록/탈퇴)
   - Assignment creation flow
4. Student Dashboard: 5D Knowledge Profile 시각화

---

## File Structure Overview

```
ai-english-platform/
├── app/
│   ├── (auth)/          # login, signup, join pages
│   ├── admin/           # org_admin dashboard
│   ├── teacher/         # teacher dashboard
│   ├── student/         # student dashboard
│   ├── parent/          # parent dashboard
│   ├── api/             # API routes (auth, org, class, notification)
│   ├── globals.css      # Tailwind v4 OKLCH theme
│   └── layout.tsx       # Root layout with ThemeProvider
├── components/
│   ├── layout/          # dashboard-shell, sidebar, header
│   └── ui/              # 23 shadcn/ui components
├── lib/
│   ├── api/             # with-auth, with-role, pagination, validation
│   ├── constants/       # roles, dimensions, navigation
│   ├── prisma/          # client singleton
│   ├── supabase/        # client, server, middleware helpers
│   └── utils/           # invite-code, cn()
├── prisma/
│   ├── schema.prisma    # 40 models
│   └── seed.ts          # subskills + error types + test org
├── supabase/migrations/ # 10 SQL files (trigger, helpers, indexes, RLS)
├── types/               # api.ts
├── tests/               # setup.ts
├── PRD_v5.md            # Final spec
├── PUSH_DIARY.md        # Push history
└── TEST_ACCOUNTS.md     # Local only (gitignored)
```
