# Dev Diary Entry

**Date**: 2026-03-01
**Project**: ai-english-platform (AI English Learning Platform)

## Summary

Phase 0 후속 정리 — 불필요한 파일 삭제, 홈 디렉토리 git 오염 해결, push diary 워크플로우 설정.

## Tasks Completed

### 프로젝트 파일 정리
- `PRD_v3.md` (52KB), `PRD_v4.md` (76KB) 삭제 — PRD v5.0.2로 대체된 구 버전
- `.next/` 빌드 캐시 삭제 (269MB) — `npm run dev` 시 자동 재생성
- `TEST_ACCOUNTS.md`는 .gitignore에 추가하여 로컬 전용으로 유지

### VS Code 10k 문제 해결
- **원인**: `C:\Users\user\`에 실수로 `git init`이 실행되어 홈 디렉토리 전체가 git repo로 인식됨
- AppData, Documents, Desktop 등 Windows 시스템 폴더가 untracked files로 감지 → VS Code Source Control에 10,000+ 변경사항 표시
- **해결**: 홈 디렉토리의 `.git` 폴더 삭제 (커밋 0개, 리모트 없는 빈 저장소)

### Push Diary 워크플로우 설정
- `push diary` 명령어로 세션 작업 요약 → `E:/mydev-diary/entries/`에 기록 → GitHub push

## Key Learnings

- Windows 홈 디렉토리에서 `git init` 실수 주의 — VS Code가 모든 하위 파일을 추적하려 함
- Claude Code에 지시할 때: 범위(경로), 대상(파일명), 의도(왜 하는지)를 명확히 전달하면 정확도 향상
