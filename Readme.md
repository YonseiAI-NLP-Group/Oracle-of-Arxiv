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

## Dataset
- DTM <br>
  
|keyword|2017년 Data|2018년 Data|2019년 Data|2020년 Data|2021년 Data|
|:---:|:---:|:---:|:---:|:---:|:---:|
|Transformer|200|200|200|200|200|
|Segmentation|200|200|200|200|200|
|GAN|200|200|200|200|200|
|Object detection|200|200|200|200|200|
|Encoder|200|200|200|200|200|
|Decoder|200|200|200|200|200|

각 6개의 키워드에 대하여 연도별 200개의 Abstract를 수집하였다. 해당 Abstract를 gensim을 활용하여 tokenize시키면, 연도별로 총 1200개의 tokenized된 abstract가 생성된다. 이를 DTM에넣고, 총 50개의 topic에 대하여 2017년부터 2021년까지의 단어가 특정 토픽에 존재할 확률을 구하였다. 5년치이므로 각 topic 별로 5 x 22685만큼의 Matrix가 나오게 된다. (단, `22685`는 corpus num)

- LSTM

|Type|Input|Label|Input shape|
|:---:|:---:|:---:|:---:|
|Train|2017 ~ 2019 DTM result|2020 DTM result|3 x 22685|
|Val|2018 ~ 2020 DTM result|2021 DTM result|3 x 22685|
|Test|2019 ~ 2021 DTM result|NaN|3 x 22685|

Train 과정에서는 2017 ~ 2019년 DTM 결과를 input으로 주고 2020 DTM 결과가 나오게끔 튜닝해주면 된다. 결과적으로 2019 ~ 2021년 DTM 결과를 가지고 LSTM에 넣어 2022년 DTM 결과를 예측해주면 된다. 각 토픽 별로 3 x 22685 Matrix가 존재하는 상황이므로, 총 `50개`의 input이 존재하는 것이다.

- GPT3
  
|Type|Input|Input shape|
|:---:|:---:|:---:|
|Test|LSTM test Result|1 x 5|

LSTM의 결과로 나온 2022년의 DTM result 예측값의 상위 50등 ~ 54등에 해당하는 키워드를 뽑아 input으로 넣는다. Input값이 중복되는 경우는 제거하였다.

## Authors
|김우영|김민정|김현식|동용훈|이종우|
|:-:|:-:|:-:|:-:|:-:|
|<img src='https://avatars.githubusercontent.com/u/69796193?v=4' height=80 width=80px></img>|<img src='https://avatars.githubusercontent.com/u/79957757?v=4' height=80 width=80px></img>|<img src='https://avatars.githubusercontent.com/u/96718906?v=4' height=80 width=80px></img>|<img src='https://avatars.githubusercontent.com/u/64018014?v=4' height=80 width=80px></img>||
|[Github](https://github.com/coshin-ssl)|[Github](https://github.com/yjdong9697)|[Github](https://github.com/mjk0150)|[Github](https://github.com/hss05230)||