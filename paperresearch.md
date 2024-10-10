## 논문 1
- Forecasting Bitcoin with technical analysis: A not-so-random forest?
  - Abstract: This paper uses data sampled at hourly and daily frequencies to predict Bitcoin returns. We consider various advanced non-linear models based on a multitude of popular technical indicators that represent market trend, momentum, volume, and sentiment. We run a robust empirical exercise to observe the impact of forecast horizon, model type, time period, and the choice of inputs (predictors) on the forecast performance of the competing models. We find that Bitcoin prices are weakly efficient at the hourly frequency. In contrast, technical analysis combined with non-linear forecasting models becomes statistically significantly dominant relative to the random walk model on a daily horizon. Our comparative analysis identifies the random forest model as the most accurate at predicting Bitcoin. The estimated measures of the relative importance of predictors reveal that the nature of investing in the Bitcoin market evolved from trend-following to excessive momentum and sentiment in the most recent time period.
  - 이 백서에서는 시간 및 일일 빈도로 샘플링된 데이터를 사용하여 비트코인 수익률을 예측합니다. 시장 추세, 모멘텀, 거래량 및 심리를 나타내는 다양한 인기 기술 지표를 기반으로 다양한 고급 비선형 모델을 고려합니다. 우리는 강력한 경험적 연습을 실행하여 예측 지평선, 모델 유형, 기간 및 입력(예측 변수) 선택이 경쟁 모델의 예측 성능에 미치는 영향을 관찰합니다. 우리는 비트코인 가격이 시간당 주파수에서 약하게 효율적이라는 것을 발견했습니다. 반면, 비선형 예측 모델과 결합된 기술적 분석은 일봉에서 무작위 보행 모델에 비해 통계적으로 유의미하게 우세해집니다. 저희의 비교 분석 결과, 무작위 포레스트 모델이 비트코인을 예측하는 데 가장 정확한 것으로 나타났습니다. 예측 변수의 상대적 중요도를 추정한 결과, 비트코인 시장에 대한 투자의 성격이 가장 최근 기간 동안 추세 추종에서 과도한 모멘텀과 정서로 진화한 것으로 나타났습니다

## 기타 논문
- 딥러닝분석과 기술적 분석 지표를 이용한 한국 코스피주가지수 방향성 예측
- 주식 투자자의 의사결정 지원을 위한 데이터마이닝 도구

- 최근 논문은 주로 딥러닝 위주였음. 또는 간혹 앙상블 기법 등. 기술적 분석 위주로 논문 검색을 했기 때문에, 많은 자료가 없었음. 기술적 분석 논의의 기반은 기본적으로 랜덤워크를 가정하여 기술적 분석은 의미 없다는 것. 기본 가정을 반하는 결과를 내야 하기 때문에 쉽지는 않고, 결과도 안 좋을수도 있을 것 같음.

## 프로젝트
- 1) 텍스트 분석(혹은 LLM)을 활용한 저점 예측
  - 뉴스기사 분석 후 점수가 낮은 종목에 집중해서 다른 기술적 지표와 합쳐서 매수 타이밍 잡기.
- 2) 기술적 분석의 여러 지표들로(캔들, RSI, 스토캐스틱, MA, EMA 등) 딥러닝(혹은 머신러닝)으로 최적의 매수 찾기.
  - 코스닥 종목을 중심으로.

- 텍스트 분석을 할 거면, 자산 배분에 집중해야 할 듯. 개별 종목으로 하면 데이터도 적고, 이미 뉴스는 늦는 경우가 많을 것 같다.
  - 혹은 산업군 별로.
- 기술적 분석은 너무 식상할 수도 있을 것 같다.
  - 산업군 별로 섹터 
