# 2026-03-10 Teacher Work OS - 영어교사 통합 업무 플랫폼 MVP 구현 및 배포

## 프로젝트
- **Repo**: [smilepat/teacher-work-os](https://github.com/smilepat/teacher-work-os)
- **Stack**: Next.js 16 (App Router), JavaScript, CSS Modules, localStorage
- **배포**: [teacher-work-os.vercel.app](https://teacher-work-os.vercel.app)

## 작업 내용

### 1. PRD 기반 MVP 설계
- 영어교사 연간 업무 흐름을 분석한 PRD를 기반으로 MVP 구현 계획 수립
- 8개 핵심 모듈 선정: 대시보드, 수업, 시험출제, 서술형평가, 메시지, 자료, 링크, 건강
- 기술 스택: Next.js + localStorage (MVP) → Phase 2에서 Supabase 전환 예정

### 2. 디자인 시스템 & 프로젝트 셋업
- Deep Indigo/Violet 다크 테마 디자인 시스템 (`globals.css`)
- Noto Sans KR + Inter 폰트
- 접기/펼치기 가능한 사이드바 네비게이션 (8개 메뉴)
- localStorage 기반 데이터 레이어 + 시드 데이터 (`lib/store.js`)

### 3. 8개 MVP 모듈 구현
1. **홈 대시보드** (`/`): 일정, 할일, 숙제현황, 건강요약, 빠른링크
2. **수업 준비** (`/lessons`): 수업안 CRUD, 학년 필터, 회고 메모
3. **시험 출제** (`/exams`): 문항 은행, 시험지 빌더, 유형/난이도 필터
4. **서술형 평가** (`/essay`): 루브릭 관리, 기준별 채점, 피드백 템플릿
5. **메시지/숙제** (`/messages`): 반별 발송, 숙제 부여, 제출 추적
6. **자료 라이브러리** (`/resources`): 태그 검색, 그리드/리스트 뷰, 즐겨찾기
7. **빠른 링크** (`/links`): 카테고리별 정리, 즐겨찾기
8. **건강 모니터링** (`/health`): 성대/무릎 일일체크, 주간 차트, 건강 팁

### 4. 배포
- GitHub 레포 생성 및 push
- Vercel 프로덕션 배포 완료

## 주요 파일 변경
| 파일 | 변경 |
|------|------|
| `app/globals.css` | 프리미엄 다크 테마 디자인 시스템 |
| `app/layout.js` | 루트 레이아웃 + Google Fonts |
| `app/components/Sidebar.js` | 사이드바 네비게이션 |
| `app/lib/store.js` | localStorage 데이터 레이어 + 시드 데이터 |
| `app/page.js` | 홈 대시보드 |
| `app/lessons/page.js` | 수업 준비 모듈 |
| `app/exams/page.js` | 시험 출제 모듈 |
| `app/essay/page.js` | 서술형 평가 모듈 |
| `app/messages/page.js` | 메시지/숙제 모듈 |
| `app/resources/page.js` | 자료 라이브러리 |
| `app/links/page.js` | 빠른 링크 허브 |
| `app/health/page.js` | 건강 모니터링 |

## 배포 URL
- Vercel: https://teacher-work-os.vercel.app

## 기술 스택
- Next.js 16.1.6 (App Router, Turbopack)
- JavaScript (no TypeScript)
- CSS Modules + 글로벌 CSS 변수 디자인 시스템
- localStorage (MVP 데이터 영속)
- Vercel (프로덕션 배포)
