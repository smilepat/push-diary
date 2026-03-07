# 2026-03-08 CSAT Cognitive Analysis Lab - 앱 분리 및 독립 폴더 이동

## 작업 내용
앱 파일을 기존 데이터 디렉토리(`01수능기출문제`)에서 독립된 전용 폴더로 분리 복사하였음.
원본 파일은 전혀 손상되지 않도록 복사(Copy) 방식으로 진행.

## 폴더 구조 변경

### Before (혼재)
```
e:\02_Assets_Data\CSAT_Data\01수능기출문제\
├── 영어수능기출문제_정리.xlsx     ← 원본 데이터
├── csat-ontology-graphDB_learning/ ← 별도 프로젝트
├── analyze_csat.py              ← 구버전 스크립트
├── data_parser.py               ← 구버전 모듈
├── app.py                       ← Streamlit 앱
├── analysis/                    ← 앱 모듈
├── pages/                       ← 앱 페이지
└── ... (앱 + 원본 데이터 혼재)
```

### After (분리)
```
e:\02_Assets_Data\CSAT_Data\
├── 01수능기출문제/           ← 원본 데이터 (변경 없음, 그대로 보존)
└── csat-cognitive-analysis-lab/  ← 앱 전용 폴더 (독립)
    ├── app.py
    ├── config.py
    ├── requirements.txt
    ├── .gitignore
    ├── .streamlit/config.toml
    ├── csat_testitemdb.csv (2.3MB)
    ├── analysis/
    │   ├── cognitive_analyzer.py
    │   ├── skills_registry.py
    │   ├── charts.py
    │   └── statistics.py
    ├── components/
    ├── data/
    ├── generation/
    ├── pages/
    │   ├── 0_API설정.py
    │   ├── 1_문제조회.py
    │   ├── 2_통계분석.py
    │   ├── 3_AI문제생성.py
    │   └── 4_인지과정분석.py
    └── .git/ (GitHub 연동 유지)
```

## 복사된 파일 목록
- app.py, config.py, requirements.txt, .gitignore, csat_testitemdb.csv
- analysis/, components/, data/, generation/, pages/, .streamlit/, .git/

## GitHub
- Repo: smilepat/csat-cognitive-analysis-lab
- .git 폴더도 함께 복사되어 GitHub 연동 유지됨
