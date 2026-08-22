# 2026-08-21 논문 추천

## 검색 정보

- **연구 기준일(research date)**: 2026-08-21 (요청된 `REQUESTED_DATE` 없음 → Asia/Seoul 기준 어제 날짜 사용)
- **실제 검색 창(search window)**: 처음 2026-08-21 하루 → 결과 0건, 이어서 2026-08-15~2026-08-21(7일) → 결과 0건, 최종적으로 2026-07-22~2026-08-21(30일)로 확장하여 결과 확보
- **검색 질의(query)**: `chest X-ray multi-institution diagnosis classification` (1차), 보완 질의로 `chest radiograph deep learning multicenter external validation`, `pulmonary radiograph AI screening cohort` 사용

## 코호트 개요 (집계 수준, 환자 단위 값 없음)

- 총 272건의 영상 레코드, 153명의 고유 환자
- 5개 기관 분포: INST01 62건, INST02 56건, INST03 54건, INST05 52건, INST04 48건
- 성별: 남성 136건, 여성 136건 (균형)
- 연령 범위 9–87세, 평균 약 51.5세
- 촬영 자세: PA 184건, AP 88건
- 소견 라벨 상위: No Finding 145건, Infiltration 21건, Atelectasis 16건, Nodule 7건, Fibrosis 6건, Effusion 6건, Cardiomegaly 5건, Pneumothorax 5건 등 다중 병변 조합 다수

이 코호트는 5개 기관에 걸친 흉부 X-ray 데이터로, 정상(No Finding) 비중이 높고 특정 소견(Nodule, Fibrosis, Cardiomegaly 등)은 희소하여 클래스 불균형과 기관 간 이질성이 동시에 존재하는 구조입니다.

## 이미 추천된 논문 제외

`recommended/2026-08-19/`에 이미 존재하는 AbdomenNet(비조영 CT 급성 복부), CF2Seg(소견 기반 분할), CLEAR(감사 가능 방사선 파운데이션 모델)는 이번 검색 결과에도 재등장했으나 중복으로 제외했습니다.

## 축 (axes)

- **다기관 일반화**: 여러 기관/사이트에서 수집된 데이터에 대한 모델의 전이 및 일반화 성능
- **희귀/저빈도 소견 성능**: 클래스 불균형 상황에서 저빈도 진단의 정밀도/재현율 저하 문제
- **판독 보고서 자동생성**: 영상-텍스트 정렬을 통한 자동 판독문 생성 및 임상 활용성

## 추천 논문

### 1. Multicenter evaluation of four large language models for automated spine imaging diagnosis
(축: 다기관 일반화, 희귀/저빈도 소견 성능)

3개 병원, 20,277건의 척추 판독문에 대해 GPT-4o, Claude-4, Qwen-3 Max, DeepSeek-V3.1 네 개의 LLM을 비교한 다기관 코호트 연구입니다. 전반적으로 특이도 90% 이상, 음성예측도 96% 이상으로 우수했지만, 저빈도 질환에서는 정밀도가 19–42%p 하락하는 '롱테일 진단 결손'이 확인되었습니다.

- **우리 데이터와의 연결**: 본 코호트도 272건 중 145건(약 53%)이 No Finding이고 Nodule(7건), Fibrosis(6건), Cardiomegaly(5건) 등 저빈도 소견이 다수 존재해 유사한 클래스 불균형 구조를 가집니다.
- **한계**: 흉부 X-ray 영상 분류가 아닌 척추 판독문 텍스트 기반 LLM 분류 연구이므로 직접 외삽은 어렵습니다.
- 출처: https://doi.org/10.1038/s41746-026-03133-z

### 2. QoQ-Med3: a multimodal reasoning foundation model for clinical analysis
(축: 다기관 일반화)

여러 임상 사이트에서 수집한 이질적 데이터로 학습한 멀티모달 추론 기반 파운데이션 모델이 미공개 외부 사이트(JHU PMAP)에도 전이 가능함을 보인 연구입니다. 초음파·유방촬영 등 이해도가 낮은 모달리티에서 특히 성능 향상이 두드러졌습니다.

- **우리 데이터와의 연결**: 본 코호트는 5개 기관(INST01–05)에서 48–62건씩 수집된 흉부 X-ray로 구성되어 기관 간 이질성 하 전이 성능이라는 논문의 핵심 질문과 맞닿아 있습니다.
- **한계**: 흉부 X-ray 소견 분류에 대한 직접적 검증 결과는 제시되지 않습니다.
- 출처: https://doi.org/10.1038/s41746-026-02945-3

### 3. An explainable generative AI system for video-to-report generation in capsule endoscopy (CE Reporter)
(축: 다기관 일반화, 판독 보고서 자동생성)

다기관·다장비 캡슐내시경 데이터(1002 영상-보고서 쌍)로 학습한 설명 가능한 비디오-보고서 생성 시스템으로, 레지던트 대비 높은 판독 품질과 91.9%의 판독 시간 단축을 보고했습니다.

- **우리 데이터와의 연결**: 본 코호트의 report_text/clinical_info 필드처럼 자유 텍스트 판독 보고서와 영상이 짝지어진 구조는 영상-텍스트 정렬 접근을 참고할 근거가 됩니다.
- **한계**: 정지 영상(흉부 X-ray) 단일 프레임 데이터이며 장시간 비디오 스트림이 아니어서 키프레임 탐지·시간 정렬 기법을 그대로 적용하긴 어렵습니다.
- 출처: https://doi.org/10.1038/s41746-026-03079-2

## 검토 안내

⚠️ 위 추천은 자동화된 검색 및 요약 결과이며, 임상 적용 전 반드시 담당 의료진의 검토와 판단이 필요합니다.
