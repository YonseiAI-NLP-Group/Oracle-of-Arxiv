# Oracle of Arxiv : 신기술 예측을 위한 키워드 분석 및 문장 생성에 관한 연구

## Introduction
1. 총 6개의 키워드를 가지고 연도별로 200개의 논문 abstract를 추출한다. (2017년 ~ 2021년)
2. 이를 DTM에 넣어 해당 문서집합 내에서 토픽들의 시간에 따른 변화를 분석한다. 그 결과 연도별로 단어가 특정 토픽에 존재할 확률을 구할 수 있다. (출력되는 토픽은 50개로 설정)
3. LSTM에 해당 데이터를 넣고 2022년의 단어들의 확률 분포를 구한다.
   - Train
      - Data : 2017 ~ 2019 DTM result
      - Label : 2020 DTM result
   - Validation
      - Data : 2018 ~ 2020 DTM result
      - Label : 2021 DTM result
4. 각 토픽 별 2022년에 단어 등장 확률 상위 50 ~ 54등인 키워드를 추출 (중복인 것은 제거하였고 총 10개의 set이 생성)
5. 4번에서 구한 키워드 5개를 GPT에 넣고 Abstract 문장 생성