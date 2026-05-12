# 2026-05-11 csat-reading-text-db HANDOFF

## 한 줄 요약

**Phase 2 마무리 (v1.7 LLM 자동 폴백, v1.8 P2-2 질의 로그)** + Claude Code 환경 안정성 보강(API 타임아웃 환경변수). 다음 세션은 **P3 로드맵**(데이터 사전 분리 / SQL AST 검증 / Cypher 통합) 진입 가능 상태.

---

## 프로젝트 상태 스냅샷

| 항목 | 값 |
|---|---|
| Repo | `smilepat/csat-reading-text-db` |
| Local | `C:\Users\user\Documents\GitHub\csat-reading-text-db\` |
| Branch | `main` (origin/main 과 동기화 완료) |
| Last commit | `958d46e` — P2-2 완료: 질의 로그 + 분석 스크립트 |
| Deployed | https://csat-reading-text-db.vercel.app (Vercel team: prompt-improvement-dm-pat) |
| LLM | `LLM_PROVIDER=auto` — Anthropic 키 있으면 Claude Sonnet 4.6, 없으면 Gemini 2.0-flash 자동 폴백 |
| DB | csat-text-graph-maker Turso (565 passages, **read-only 토큰**) |

---

## 완료된 작업

### v1.7 — LLM provider 자동 폴백 (커밋 `e3a92a6`)

**문제**: v1.0 보안 키 회전 직후 Anthropic 새 키가 잔액 0 organization에 발급되어 production 깨질 뻔함.

**해결**: `lib/ai/llm.ts` 에 `resolveProvider()` 함수 신설.

- `LLM_PROVIDER=auto` (기본값) 지원
- `LLM_PROVIDER=anthropic` 인데 키 비어 있으면 **Gemini 로 자동 폴백** (warn 로깅)
- Gemini 키도 없을 때만 명시 에러
- `isNonEmpty()` 헬퍼로 짧은/빈 문자열 모두 미설정 취급 (`length > 10` 가드)

**파일**:
- `lib/ai/llm.ts` — 핵심 로직
- `.env.example` — `auto`/`google`/`anthropic` 3값 설명 갱신
- `.gitignore` — `.vercel` 추가
- `PRD.md` v1.7

### v1.8 — P2-2 질의 로그 + 분석 스크립트 (커밋 `958d46e`)

**목적**: 사용자 질의 패턴 익명 수집 → few-shot 보강 → 정확도 진화 사이클 (PRD v2.0 외부 공개 조건).

**산출물**:

| 파일 | 역할 |
|---|---|
| `lib/query-log.ts` | Upstash Redis 30일 TTL, SHA256 IP 해시 16자, fail-open |
| `app/api/query/route.ts` | 5개 응답 경로(ok/parse_fail/llm_fail/validate_fail/db_fail)에 `recordQueryLog()` 삽입 |
| `scripts/analyze-logs.mjs` | Top 10 실패 질문 + stage 분포 + 평균/중앙값 응답시간 + provider 분포 |
| `.env.example` | `LOG_IP_SALT` 항목 추가 |
| `package.json` | `npm run logs:analyze` 스크립트 추가 |

**스키마**:

```ts
{
  id: string;            // crypto.randomUUID()
  ts: number;            // epoch ms
  question: string;
  generated_sql: string | null;
  stage: 'ok' | 'parse_fail' | 'llm_fail' | 'validate_fail' | 'db_fail';
  duration_ms: number;
  ip_hash: string | null;  // LOG_IP_SALT 미설정 시 null
  llm_provider: string;
  result_rowcount: number | null;
}
```

**보안 정책**:
- IP는 SHA256(salt + ip).slice(0, 16) — 역추적 불가
- `LOG_IP_SALT` 환경변수 8자 이상일 때만 해시, 아니면 `ip_hash=null`
- Upstash env 미설정 시 자동 비활성 (fail-open) — 로컬·기존 동작 영향 없음
- 30일 TTL 후 자동 삭제

### 보너스 — Claude Code 환경 안정성

`C:\Users\user\.claude\settings.json` 에 `env` 섹션 신설:

```json
"env": {
  "API_TIMEOUT_MS": "600000",
  "BASH_DEFAULT_TIMEOUT_MS": "300000",
  "BASH_MAX_TIMEOUT_MS": "600000"
}
```

**Why**: 한국-미국 네트워크 + Opus 4.7 1M 컨텍스트 조합에서 `Stream idle timeout - partial response received` 오류 발생. 적용에는 Claude Code **재시작 필요**.

---

## 다음 세션 시작 명령

```bash
cd "C:\Users\user\Documents\GitHub\csat-reading-text-db"
git status                          # main, clean
git log --oneline -3                # 958d46e (P2-2), e3a92a6 (v1.7), 6652cb8 (P2-1)
code docs/IMPROVEMENT_PRD_v2.md     # P3 로드맵 §3-4 ~ §3-6 참조
```

---

## 남은 로드맵 — P3 (점진 보강)

`docs/IMPROVEMENT_PRD_v2.md` §3-4 ~ §3-6:

| Phase | 작업 | 산출물 후보 |
|---|---|---|
| P3-1 | 데이터 사전 분리 | system prompt 안의 도메인 지식 → 별도 문서 |
| P3-2 | AST 기반 SQL 검증 | 정규식 → `node-sql-parser` 같은 AST 검증으로 강화 |
| P3-3 | csat-graphdb-318 Cypher 통합 (v3.0 후보) | Neo4j 그래프 쿼리도 같은 인터페이스에서 |

> **P2-3 운영 후속 작업** (필요 시): Upstash Redis 인스턴스 신규 생성 + `UPSTASH_REDIS_REST_URL`/`TOKEN`/`LOG_IP_SALT` 를 `.env.local` + Vercel env에 등록 → 실제 로그 수집 활성화 → 1주 후 `npm run logs:analyze` 로 패턴 추출 → 골든셋 보강.

---

## 환경 변수 출처 (다른 PC 셋업용)

| 변수 | 출처 |
|---|---|
| `TURSO_DATABASE_URL` / `TURSO_AUTH_TOKEN` | `vercel env pull` (csat-text-graph-maker Vercel prod) — **반드시 read-only 토큰** |
| `ANTHROPIC_API_KEY` | https://console.anthropic.com 발급 (organization 잔액 확인 필수) |
| `GOOGLE_GENERATIVE_AI_API_KEY` | https://aistudio.google.com/app/apikey 발급 |
| `LLM_PROVIDER` | `auto` (권장 기본값) |
| `UPSTASH_REDIS_REST_URL` / `TOKEN` | https://console.upstash.com (P2-1 레이트리밋 + P2-2 로그 공용 인스턴스) |
| `LOG_IP_SALT` | 임의 8자 이상 문자열, 운영 환경에서만 설정 |

---

## 주요 결정 사항

1. **ID 생성**: `crypto.randomUUID()` 내장 사용 (PRD v2 명세는 `ulid` 였으나 외부 의존성 회피).
2. **IP salt**: 환경변수 `LOG_IP_SALT` 방식 채택. 미설정 시 `ip_hash=null` (해시 자체 건너뜀).
3. **fail-open 일관성**: P2-1 레이트리밋과 동일하게 Upstash env 미설정 시 자동 비활성. 로컬 개발자 영향 0.
4. **로그 저장 실패 silent fail**: `recordQueryLog()` 내부 try/catch → 응답 막지 않음.

---

## 메모리·환경 변경

- **diary repo 이관**: `E:\mydev-diary\` (현재 E: 드라이브 분리됨) → `C:\Users\user\Documents\GitHub\push-diary\` 에서 작업.
- **Claude Code settings.json**: API/Bash 타임아웃 3종 환경변수 추가 (Claude Code 재시작 시 적용).
