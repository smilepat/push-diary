# AI English Platform — 작업 요약

- **Date**: 2026-03-01
- **Project**: ai-english-platform

## 프로젝트 정보

| 항목 | 내용 |
|------|------|
| **GitHub** | https://github.com/smilepat/ai-english-platform |
| **로컬 경로 (C:)** | `C:\Users\user\Documents\GitHub\ai-english-platform\` |
| **백업 경로 (E:)** | `E:\01_Projects\ai-english-platform\` |
| **스택** | Next.js 15, TypeScript, Tailwind v4, shadcn/ui, Prisma, Supabase |
| **최신 커밋** | `9fca092` (main) |

---

## 로그인 정보

**URL:** `http://localhost:3000` (dev 서버: `npm run dev`)

| 역할 | 이메일 | 비밀번호 | 대시보드 |
|------|--------|----------|----------|
| 기관 관리자 | `eltkorea@gmail.com` | `iloveLD12` | `/admin/dashboard` |
| 교사 | `teacher@test.com` | `test1234` | `/teacher/dashboard` |
| 학생 | `student@test.com` | `test1234` | `/student/dashboard` |
| 학부모 | `parent@test.com` | `test1234` | `/parent/dashboard` |

**초대코드:** `TESTORG` (테스트 학원 가입용)

---

## 금일 완료 작업

### Phase 1-5: Memory Engine + Cron (`78921df`)

Ebbinghaus 망각곡선 기반 SRS(Spaced Repetition System) 구현.

| 파일 | 역할 |
|------|------|
| `types/memory.ts` | Zod 스키마 + 응답 인터페이스 + SRS 상수 |
| `lib/memory/formulas.ts` | 순수 수학 함수 7개 (quality, S 갱신, P_forget, interval, priority) |
| `lib/memory/engine.ts` | processVocabReview (트랜잭션) + buildDailyReviewQueue (배치) |
| `app/api/memory/review-queue/route.ts` | GET — 오늘의 복습 큐 (student) |
| `app/api/memory/review-complete/route.ts` | POST — 복습 완료 → S 갱신 (student) |
| `app/api/memory/stats/route.ts` | GET — 개인 기억 통계 (student) |
| `app/api/memory/class-stats/[classId]/route.ts` | GET — 반 기억 통계 (teacher/org_admin) |
| `app/api/cron/build-review-queue/route.ts` | Cron — 매일 06:00 KST 리뷰 큐 빌드 |
| `vercel.json` | cron 스케줄 추가 (`0 21 * * *` UTC) |
| `tests/unit/memory-formulas.test.ts` | 29개 단위 테스트 전부 통과 |

### UX 개선: 404 수정 + 학생 대시보드 (`9fca092`)

| 구분 | 내용 |
|------|------|
| `components/layout/coming-soon.tsx` | "준비 중" 공통 컴포넌트 |
| 8개 placeholder 페이지 | learn, review, reading, lesson-plans, questions, overrides, billing, reports |
| `app/student/dashboard/page.tsx` | 스켈레톤 → 실데이터 (5D 프로필 bar, 4 stat 카드, 최근 학습 이력) |

---

## Phase 1 전체 진행 현황

| Phase | 내용 | 커밋 | 상태 |
|-------|------|------|------|
| 1-1 | 반 관리 + 학생 등록 | `5d4af5e` | Done |
| 1-2 | 과제 시스템 CRUD | `ef50562` | Done |
| 1-3 | Quick Scan 진단 | `29a424c` | Done |
| 1-4 | 5D Deep Dive 진단 | `b4bca35` | Done |
| 1-5 | Memory Engine + Cron | `78921df` | Done |
| — | 404 수정 + 대시보드 | `9fca092` | Done |
| 1-6 | SRS 복습 큐 UI | — | Next |

---

## 시뮬레이션 결과 — 남은 개선점 (우선순위순)

1. **SRS 복습 UI** — API 완성, 학생용 복습 화면 필요
2. **교사/관리자/학부모 대시보드** — 스켈레톤 → 실데이터
3. **알림 UI** — API는 있으나 프론트 미구현
4. **어휘 학습 페이지** — 현재 "준비 중"
5. **문항 수 확보** — Quick Scan 37개, Deep Dive 50개 → 반복 시 중복 우려
