# v2 전처리 규칙 — 논문 근거 매핑

| v2 규칙 | 적용 내용 | 참고 논문 |
|---------|-----------|-----------|
| 10분 격자 병합 | K-water 10분 + HRFC 17관측소 wide | CNN-GRU 2023 (실시간 10분) |
| 강우·유량 결측=0 | `kwater_강우량`, 유입·방류 등 | CNN-GRU 4.2 |
| 수위 IQR clip k=1.5 | 수위·댐수위 열 | yeoju handoff + 결측 LSTM BoxPlot |
| 수위 짧은 보간+ffill | limit 6스텝(1h), ffill 18스텝(3h) | 다변량 2024 Spline + CNN-GRU ffill |
| Cyclical time encoding | sin/cos(시각·연중일) | 다변량 2024 |
| 1차 차분 | 주요 수위·강우·방류 | 다변량 2024 |
| Lag 6/18/36/72 | 1h·3h·6h·12h | 다변량 lag + 기존 파이프라인 |
| MinMax (train만 fit) | 2024~2025 2년 fit / holdout 10% 지표 | 결측 LSTM Data Scaling |
| 타깃 | HRFC 1007639 여주보(상류) | HRFC 관측 |
| 6시간 후 타깃 | `target_수위_m_h36` (36스텝) | CNN-GRU (6시간 예측) |

## 운영 모델

| 모델 파일 | 근거 |
|-----------|------|
| `hydro_mast_v2.pt` | Hydro-MAST: 하천 그래프 + 학습 지연 + GRU, Direct Multi-Horizon |

## 주의

- 1-step 예측 성능이 매우 높게 나올 수 있음 → 입력에 **자기 수위 lag**가 포함되기 때문(자기회귀). 운영 검증 시 **6시간 선행(h36)** 지표를 함께 볼 것.
- 기존 AI API 24필드와 **입력 스펙이 다름** → `preprocess_v2_spec.json` 기준으로 API 재설계 필요.
