# 2026-03-08 CSAT Cognitive Analysis Lab - Streamlit Cloud 배포

## 프로젝트
- **Repo**: [smilepat/csat-cognitive-analysis-lab](https://github.com/smilepat/csat-cognitive-analysis-lab)
- **Stack**: Python, Streamlit, Google Gemini API, Azure OpenAI
- **배포**: Streamlit Community Cloud

## 작업 내용

### 1. Reading Skills 기반 인지과정 분석 확장 (이전 세션에서 완료)
- `analysis/skills_registry.py` 생성: 14개 수능 문항유형별 60+ reading skills 데이터 정의
- `analysis/cognitive_analyzer.py` 수정: LLM 프롬프트에 Skills_Diagnosis JSON 필드 추가
- `pages/4_인지과정분석.py` 수정: 학습 가이드 탭에 "필요한 읽기 능력" 섹션 추가
- E2E 테스트: "주제" 유형 문항으로 6개 skills 정상 생성 확인

### 2. Import 문제 해결
- Streamlit pages/ 환경에서 `analysis.skills_registry` 모듈을 찾지 못하는 문제 발생
- 원인: 한국어 디렉토리명(`01수능기출문제`)이 포함된 경로에서 sys.path 매칭 실패
- 해결: `importlib.util.spec_from_file_location()`으로 파일 경로를 직접 지정하여 로드
- `genai`, `AzureOpenAI`를 모듈 레벨 import에서 lazy import(함수 내부)로 변경

### 3. Streamlit Cloud 배포
- `.gitignore`, `.streamlit/config.toml` 생성
- `app.py` 수정: `data_parser` (구버전) 대신 `data.loader` 사용
- GitHub repo 생성 및 push (public)
- Streamlit Community Cloud에 배포 완료

## 주요 파일 변경
| 파일 | 변경 |
|------|------|
| `analysis/cognitive_analyzer.py` | importlib 기반 skills_registry 로드, lazy import |
| `analysis/skills_registry.py` | 신규: 14개 유형별 reading skills 정의 |
| `pages/4_인지과정분석.py` | sys.path 추가, Skills Diagnosis UI |
| `app.py` | data_parser -> data.loader 전환 |
| `.gitignore` | 신규 |
| `.streamlit/config.toml` | 신규: 테마, 서버 설정 |

## 배포 URL
- Streamlit Cloud: `https://csat-cognitive-analysis-lab.streamlit.app` (예상)

## 기술 스택
- Python 3.11+
- Streamlit >= 1.30.0
- Google Gemini (gemini-2.0-flash)
- Azure OpenAI (GPT-4o) 지원
- pandas, plotly
