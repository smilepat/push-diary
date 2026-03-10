# CSAT Skills & Tactics — 홈화면 개선 & 사용자 가이드

**날짜**: 2026-03-10
**프로젝트**: CSAT-skills-tactics (LogicFlow Task Framework v2.0)
**Repo**: https://github.com/smilepat/CSAT-skills-tactics
**배포**: https://app-gilt-xi.vercel.app

---

## 작업 요약

### 1. 홈화면 미니 레이더 차트 추가
- Hero 섹션을 2컬럼 그리드로 재구성: 왼쪽 텍스트 + 오른쪽 레이더 카드
- `HomeRadarCard` 컴포넌트: 9개 역량의 현재 점수 vs 1등급 목표 비교 차트
- `dynamic(..., { ssr: false })` 래퍼로 recharts SSR 문제 해결
- 모바일에서는 Hero 아래로 자동 배치 (반응형)

### 2. 4-Layer Ontology 이미지 배치
- `4-layer-ontology.jpg`를 홈화면 3-step 가이드와 Layer A 카드 사이에 배치
- Next.js `Image` 컴포넌트로 최적화 렌더링

### 3. 사용자 가이드 페이지 (/guide)
- **WHY — 필요성 및 목적**: 기존 학습 문제점, 12개 미세 역량 체계, 4-Layer 카드
- **HOW — 작동 원리**: W3Schools 방식, 4단계 흐름(진단→학습→행동분석→마스터리), 레벨 기준표
- **GUIDE — 사용자 설명서**: 페이지별 사용법, 8가지 Task 유형, 모바일 안내, 수능 기출 228문항
- 접이식(collapsible) 섹션 UI

### 4. Hero 카피 리파인
- Before: "W3Schools처럼 이론을 읽고 즉시 연습하는 구조."
- After: "기출문제 반복만으로는 수능 영어가 해결되지 않습니다. 무엇을, 왜 틀렸는지 역량 단위로 진단하고, 부족한 스킬을 집중 훈련하세요."

---

## 기술 이슈 & 해결

| 이슈 | 원인 | 해결 |
|------|------|------|
| MiniRadarWidget 글로벌 layout에서 안 보임 | `ssr: false`는 서버 컴포넌트에서 사용 불가 | `"use client"` 래퍼 컴포넌트에서 `dynamic` import |
| HomeRadarCard 직접 import 시 페이지 깨짐 | recharts SSR 빌드 에러로 전체 페이지 렌더 실패 | `HomeRadarLoader` 클라이언트 래퍼 + `ssr: false` |

---

## 생성/수정 파일

```
app/src/app/page.tsx                          — 홈화면 (레이더, 온톨로지 이미지, 가이드 버튼, 카피 수정)
app/src/app/guide/page.tsx                    — 사용자 가이드 페이지 (신규)
app/src/components/mastery/home-radar-card.tsx — 홈용 인라인 레이더 차트 (신규)
app/src/components/mastery/home-radar-loader.tsx — SSR-safe dynamic 래퍼 (신규)
app/src/components/mastery/mini-radar-widget.tsx — 플로팅 위젯 (미사용, 참고용)
app/src/components/mastery/mini-radar-loader.tsx — 플로팅 위젯 래퍼 (미사용)
app/public/4-layer-ontology.jpg               — 온톨로지 이미지
```

## 커밋 히스토리 (이번 세션)

```
1854837 fix: refine hero copy text
fcb9ed3 feat: add user guide page with app overview, how-it-works, and manual
bc319fb feat: add 4-Layer Ontology image to home page
ff7cca7 feat: add mini radar chart to home page hero section
```
