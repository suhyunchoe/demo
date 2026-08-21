# Daily Paper Recommendations — 2026-08-19

## 검색 개요
- **연구 기준일**: 2026-08-19 (요청된 `REQUESTED_DATE`)
- **실제 검색 창**: 2026-08-19 (당일) → 결과 없음 → 2026-08-13 ~ 2026-08-19 (7일) → 1건 → 2026-07-20 ~ 2026-08-19 (30일) → 채택 기준 충족
- **검색 쿼리**: `chest X-ray multi-label classification external validation`

## 코호트 개요 (환자 단위 값 제외, 집계값만 제공)
- 총 272건의 영상 연구, 153명의 고유 환자
- 성별: 남성 136건(평균 연령 48.7세), 여성 136건(평균 연령 54.3세)
- 촬영 자세: PA 184건, AP 88건
- 5개 기관(INST01–05, 48~62건씩) · 9개 장비(DEV01–09, 15~40건씩)에서 수집된 다기관 데이터
- 소견 라벨(findings_label)은 다중 레이블 구조: "No Finding" 145건이 가장 많고, Infiltration(21), Atelectasis(16), Nodule(7), Fibrosis(6), Effusion(6), Cardiomegaly(5) 및 Effusion+Infiltration, Atelectasis+Infiltration 등 복합 소견 조합이 다수 존재
- 전체 레코드가 `is_synthetic = true`로 표시된 합성/마스킹 데이터셋

이 특성상 코호트는 **다기관·다장비 흉부 X-ray, 다중 소견(multi-label) 분류** 문제와 가장 밀접하게 연결됩니다.

## 채택된 논문 3편

### 1. CLEAR: an auditable foundation model for radiology grounded in clinical concepts
*Nature Biomedical Engineering, 2026*
- **왜 관련 있는가**: 87만 건 이상의 이미지-리포트 쌍으로 학습된 흉부 X-ray 파운데이션 모델을 미국·유럽·아시아 4개 외부 데이터셋에서 검증. 임상 개념 기반 다중 소견 분류와 감사가능성(auditability)을 함께 제시.
- **우리 데이터와의 연결**: 5개 기관·9개 장비, 다중 소견 라벨 구조라는 점에서 기관 간 일반화 평가가 직접 관련됨.
- **한계**: 판독 리포트 텍스트를 활용하지 않는 우리 데이터로는 CLEAR의 정확도나 감사가능성 수준을 재현할 수 없음.

### 2. Grounding Radiology Report Findings into Medical Image Segmentation (CF2Seg)
*npj Digital Medicine, 2026*
- **왜 관련 있는가**: 판독 소견 텍스트를 직접 활용해 흉부 X-ray 병변을 국소화하는 프레임워크로, 다기관·다양한 흉부 병리에서 분포 이동에 대한 강건성을 평가함.
- **우리 데이터와의 연결**: findings_label의 다중 소견 조합과 5개 기관 분포 차이가 CF2Seg가 다루는 문제 설정과 직접 맞닿음.
- **한계**: 병변 픽셀 단위 주석이나 상세 판독 텍스트가 없어 국소화 정확도 자체는 검증 불가.

### 3. A foundation model for acute abdomen diagnosis stratification and triage on noncontrast computed tomography (AbdomenNet)
*Nature Communications, 2026*
- **왜 관련 있는가**: 3개 독립 외부 기관 코호트 검증과 다중 판독자 교차 연구(리더 스터디)를 통해 AI 보조가 판독 정확도와 시간에 미치는 영향을 정량화한 방법론적 사례.
- **우리 데이터와의 연결**: 다기관·다장비 영상 코호트에서 AI 보조 도구의 일반화 성능을 평가하는 연구 설계 참고.
- **한계**: 대상 modality(복부 비조영 CT)와 질환군(급성복증)이 우리 흉부 X-ray 코호트와 달라 수치 직접 비교는 불가.

## 참고
- 이 목록은 자동 검색 및 코호트 매칭 결과이며, **임상 적용 전 반드시 담당 의료진의 검토가 필요합니다.**
