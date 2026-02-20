# 2026-02-20 작업 일지: Incidental Learning - Learning Flow Guide 구현

## 작업 요약
- Reading Support Toolbar Phase 2 구현 완료 (Steps 8-12)
- 레이아웃 조정 (Activity 패널 확대)
- Learning Flow Guide 시스템 설계 및 구현
- Vercel 배포 완료

## 상세 내용

### 1. Reading Support Toolbar (Phase 2 완료)
- `src/data/vocab_support.json` - 10개 토픽별 어휘 데이터 (CEFR 레벨, 영한 의미, 품사)
- `src/data/sentence_support.json` - 복합 문장 분석 (패턴, 구문 분석, 한국어 노트)
- `src/data/background_support.json` - 한국어 배경지식 (핵심 개념 포함)
- `GET /api/reading/support/:topic_id` API 엔드포인트 추가
- 3개 토글 버튼: Vocab Help, Sentence Guide, Background
- TreeWalker 기반 안전한 텍스트 하이라이팅 (cloze input 보호)
- CEFR 색상 시스템: B1=회색, B2=파랑, C1=보라, C2=빨강

### 2. 레이아웃 조정
- 1차: `280px 1fr 320px` → `280px 1fr 1fr` (Activity = Graph 동일 크기)
- 2차: `280px 1fr 1fr` → `220px 1fr 2fr` (학습 공간 대폭 확대)

### 3. Learning Flow Guide 시스템 (핵심 작업)
학습자가 처음 앱을 접했을 때 사용 순서를 유도하는 4-컴포넌트 시스템 구현.

#### A. Welcome Dashboard (`renderWelcomeDashboard`)
- Activity 패널에 4단계 학습 가이드 표시
- 단계별 상태: 완료(체크), 활성(번호), 잠김(자물쇠)
- 한국어 설명 포함
- 하단에 AI 추천 학습 카드 통합 표시

#### B. Contextual Hints (`showContextHint`)
- 각 학습 단계에서 1회만 표시되는 안내 배너
- 8초 후 자동 사라짐 + 닫기 버튼
- `localStorage`로 중복 표시 방지
- slide-in/fade-out 애니메이션

#### C. Topic Node Pulsing
- Phase 0에서 Topic 노드(파란 원)에 breathing 애니메이션
- Cytoscape `shadow-opacity` 0.15~0.6 반복
- 첫 Topic 클릭 시 자동 정지

#### D. Phase Advancement Logic
| Phase | Trigger | 안내 |
|-------|---------|------|
| 0→1 | Topic 클릭 | "빈칸을 채워보세요" |
| 1→2 | Section 완료 | "LO 퀴즈를 풀어보세요" |
| 2→3 | LO 퀴즈 답변 | "Stats 패널을 확인하세요" |
| 3→4 | 모든 LO 완료 | Free Learning Mode |

#### State 추가
```javascript
learningPhase: parseInt(localStorage.getItem('learningPhase') || '0'),
suggestionsData: null
```

### 4. CSS 추가 (약 180줄)
- `.welcome-dashboard`, `.welcome-header`, `.welcome-title`
- `.flow-steps`, `.flow-step`, `.flow-step.active/completed/locked`
- `.flow-step-badge`, `.flow-step-content`, `.flow-step-ko`
- `.welcome-suggestions`, `.welcome-suggestion-card`
- `.context-hint`, `.context-hint.hiding`
- `@keyframes hintSlideIn`, `@keyframes hintFadeOut`
- 모바일 반응형 조정

## 수정된 파일
| 파일 | 변경 |
|------|------|
| `public/js/app.js` | learningPhase state, Welcome Dashboard, hints, pulse, phase logic |
| `public/css/style.css` | 레이아웃 변경, Welcome Dashboard/Flow Steps/Hints CSS |
| `src/data/vocab_support.json` | 신규 - 어휘 지원 데이터 |
| `src/data/sentence_support.json` | 신규 - 문장 분석 데이터 |
| `src/data/background_support.json` | 신규 - 배경지식 데이터 |
| `src/routes/reading.js` | Support API 엔드포인트 |

## 해결한 이슈
- `app.js` line 758 구문 오류 (이전 세션의 중복 `},`) 수정
- CSS vendor prefix 순서 경고 (`backdrop-filter` → `-webkit-backdrop-filter`) 수정
- Windows 환경 curl 파이핑 이슈 → raw curl 출력으로 우회

## 배포
- **GitHub**: https://github.com/smilepat/incidental-learning.git
- **Vercel**: https://incidentallearning.vercel.app
- **커밋**: `8988852` - feat: Introduce incidental learning UI with quiz and cloze exercises

## 다음 할 일
- [ ] Learning Flow Guide 실제 사용자 테스트
- [ ] Phase 4 (Free Mode)에서 추가 학습 경로 제안
- [ ] 모바일 환경 Welcome Dashboard 테스트
- [ ] Reading Support Toolbar 실사용 피드백 반영
