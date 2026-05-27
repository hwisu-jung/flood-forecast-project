# 수문 예측 프로젝트

## 구성
- `01_data_pipeline`: 데이터 수집/전처리/피처 구성
- `02_model_development`: 모델 정의/학습/검증
- `03_realtime_pipeline`: 실시간 추론 및 대시보드
- `04_artifacts`: 모델 가중치/검증 결과/샘플 출력
- `docs`: 프로젝트 설명 문서

## 빠른 실행
1. 가상환경 생성 후 `pip install -r requirements.txt`
2. `.env`에 API 키 입력
3. `python 03_realtime_pipeline/realtime_dashboard_server.py`

## 보안
- 이 패키지의 `.env`는 키가 비어있는 전송용 샘플입니다.
- 실제 키는 로컬 환경에서만 입력하고 저장소에 커밋하지 마세요.
