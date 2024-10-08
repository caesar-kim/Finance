# 3주차. 금위머 10장 자연어 처리, 파구로 3장 평균분산 전략 , 7장 멀티 팩터 전략

## 10장. 자연어 처리 (p.403)
-  
  - 머신러닝에 의존해 인간 언어에서 의미 도출. 텍스트 분석, 텍스트 마이닝, 전산 언어학, 콘텐츠 분석 등 다양한 이름으로 불림.
  - SEC에서는 회계 사기 탐지용으로 일찍이부터 도입되었다.
  - 뉴스 또한 금융 시장에 강력한 영향을 미친다는 것은 잘 알려져 있음. 수많은 뉴스에서 데이터를 추출하는 것은 개인이 하기 어려운 일.
    - 또한 주관적 해석이 아닌 일정 수준의 객관성과 일관성도 부여하고, 실수도 줄일 수 있다.
### 10.1. 자연어 처리: 파이썬 패키지
- 10.1.1 NLTK
  - 가장 유명한 NLP 라이브러리. 그러나 학습곡선이 가파르고 어려운 기능이 있다.
- 10.1.2. TextBlob
  - NLTK 위에 빌드된다. 텍스트 처리를 단순화 한다.
- 10.1.3. spaCy
  - 빠르고 능률적이며 기성 제품화 되도록 설계됨. 철학은 단일 목적에 하나의 가장 좋은 알고리즘만 제공하는 것.
### 10.2. 자연어 처리: 이론 및 개념
- 
  - 공통된 순차적 단계를 거친다.
  - 텍스트 입력 > 전처리 > 특성 표현 > 추론 > 평가
- 10.2.1. 전처리
  - 전처리의 주요 구성요소는 토큰화, 불용어 제거, 형태소 분석, 기본형식화, 품사 태깅, 명명 갱체 인식.
  - 토큰화
    - 의미 있는 세그먼트로 분할하는 작업. 단어, 구둣점, 숫자, 특수문자 등.
    - 미리 정해진 규칙 셋을 통해 문장을 토큰 목록으로 변환.
```python
# 토큰화
text = "This is a tokenize test"

from nltk.tokenize import word_tokenize
word_tokenize(text)

# textblob 사용하면
TextBlob(text).words

# 불용어 제거
text = "S&P and NASDAQ are the two most popular indices in US"
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
stop_words = set(stopwords.words('english'))  # 영어 기반 기본 불용어 집합
text_tokens = word_tokenize(text)
tokens_witout_sw = [word for word in text_tokens if not word in stop_words]
print(tokens_without_sw)

# 형태소 분석
parsed_text = word_tokenize(text)
# 형태소 초기화
from nltk.stem.snowball import SnowballStemmer
stemmer = SnowballStemmer('english')
# 단어별 형태소
[(word, stemmer.stem(word)) for I, word in enumerate(parsed_text)
if word.lower() != stemmer.stem(parsed_text[i])]

# 기본형식화
from textblob import Word
parsed_data = TextBlob(text).words
[(word, word.lemmatize()) for i, word in enumerate(parsed_data) if word != parsed_data[i].lemmatize()]

# 품사 태깅
TextBlob(text).tags

# 명명 개체 인식
for entity in nlp(text).ents:
    print("Entity: ", entity.text)    # Google, U.K., $1 billion 이 entity로 출력됨.
from spacy import displacy
displacy.render(nlp(text), style="ent", jupyter=True)    # 이 기능으로 시각화 가능.

# 위의 모든 단계를 한 번에: spaCy
Python code text = 'Google is looking at buying U.K. startup for $1 billion'
doc = nlp(text)
pd.DataFrame([t.text, t.is_stop, t.lemma_, t.pos_]
    for t in doc], columns=['Token', 'is_stop_word', 'lemma', 'POS'])
```
  - 불용어 제거
    - 매우 일반적인 단어는 종종 제외하는데 이를 불용어라고.
  - 형태소 분석
    - 변형된(또는 파생된) 단어를 어간, 어기, 어근 형식으로 줄이는 과정. ~~s, ~~ing, ~~ed, ~~ization 같은 것들을 모두 ~~으로.x
  - 기본형식화
    - 형태소 분석을 약간 변형한 것. 형태소 분석은 종종 실재하지 않는 단어도 생성. 기본형은 실제 단어이다. ran도 run으로 바꿀 수 있다.
  - 품사 태깅part of speech PoS
    - 문법 범주에 토큰 할당하는 과정. 단어가 사용되는 방식을 나타내는 언어 신호를 제공하므로 매우 유용.
    - 역사적으로 히든 마코프 모델로 태거 만들었으나 최근에는 인공신경망도 활용.
- 명명 개체 인식(NER: named entity recognition)
    - 텍스트에서 명명된 개체를 찾아 미리 정의된 범주로 분류하려는 데이터 전처리에서 선택적 다음 단계이다.
    - 이 범주에는 사람, 조직, 위치, 시간, 표현, 수량, 금전적 가치, 백분율, 이름.
- spaCy
    - spaCy로 한 단계로 수행 가능. 텍스트에 대해 nlp 호출하면 spaCy는 텍스트를 토큰화 하여 문세 객체 생성.
    - 그리고 처리 파이프라인을 통해 여러  단계로 처리됨. 기본 모델에는 태거, 파서, 개체 인식기로 이루어짐.
- 그 외의 전처리
    - 웹사이트 스크랩 데이터는 HTML 태그 제거
    - PDF는 텍스트 형식으로변환
    - 종속성 구문 분석
    - 상호 참조 해결
    - 삼중항 추출
    - 관계 추출 등

- 10.2.2. 특성 표현 (p. 413)
    - 특성 표현에는 2가지가 있다. 1) 알려진 단어의 어휘 2) 알려진 단어의 수
    - 특성 표현 방법 일부는 단어 모음, TF-IDF, 단어 임베딩, 사전 훈련된 모델, 딥러닝 기반 특성 표현 
### 10.3. 실전 문제1: NLP 및 감정 분석 기반 거래 전략
### 10.4. 실전 문제2: 챗봇 - 디지털 어시스턴트
### 10.5. 실전 문제3: 문서 요약
### 10.6. 맺음말
### 10.7. 연습 문제

## 3장. 평균-분산 전략 구현 및 시뮬레이션 분석
### 3.1. 평균-분산 전략 구현 (p.93)
- 시뮬레이션이란 과거 시장 데이터 통해 투자 전략 성과를 평가하는 방법을 말한다.
    - 과거 시장 데이터에 투자 전략 적용해 거래 재현하여 주어진 기간동안 전략의 성과 평가 가능.
    - 데이터 수집 -> 투자 전략 -> 시뮬레이션 -> 시뮬레이션 분석 이라는 개발 단계.
    - 데이터 수집 > 투자 전략(투자 유니버스 정의 > 평균-분산 최적화) >
    - 시뮬레이션(주문 생성 > 거래실행 및 비용처리 > 결과 기록) > 시뮬레이션 분석
- 가정 사항
    - 모든 주문은 다음 거래일 시가로 체결.
    - 거래 시 수수료, 슬리피지는 고려하지만 세금은 무시.
- 3.1.1. 데이터 수집
- PyKrx 라이브러리는 한국거래소 등에서 지수, 주식, ETX, 채권 같은 시장 데이터를 scraping 할 수 있는 API를 제공한다.
- PyKrx 라이브러리 stock 모듈의 API 함수들
    - get_market_ticker_list 지정된 시장에 상장된 종목 코드 리스트 반환
    - get_market_ticker_name 종목 코드의 종목이름 조회
```python
from pykrx import stock

# 주가 조회 함수 get_market_ohlcv() 함수
# 특정 거래일 기준 전 종목 주가 조회, 지정 기간 동안 특정 종목 주가 조회 2가지로 사용 가능.
# 주가는 OHLCV로, 시가, 고가, 저가, 종가, 거래량
get_market_ohlcv("20200831", market="KOSDAQ") # 해당 일의 코스닥 전 종목 주가 데이터
get_market_ohlcv("20200810", "20201212", "005930", adjusted=False) # 수정 주가가 아닌 삼성전자 일별 주가 데이터를 0810부터 1212까지.
# 일반적으로 정보 제공하는 서버에서 대량 요청은 자동으로 차단한다. 따라서 time 모듈로 1초 정도 지연시켜가며 요청하도록.
import time
ticker_list = ['005930', '000020', '035720']
for ticker in ticker_list:
    df = stock.get_market_ohlcv('20181210', '20181212', ticker)
    print(df.head())
    time.sleep(1)
```
- 주가 조회 및 인덱스 조회 함수
    - load_stock_data()와 load_index_data()

- 3.1.2. 평균-분산 최적화
    - 자산 기대 수익률과 공분산 행렬이 필요하다.
    - 가장 쉬운 방법은 과거 수익률 데이터 사용하는 것. 자산 평균 수익률을 기대수익률로, 수익률 표본 공분산 행렬을 자산 간 공분산 행렬로 계산.
    - PyPortfolioOpt에 저으이된 목적함수와 옵티마이저로 평균분산 모델을 최적화 한다. 그 결과 투자 포트폴리오 자산 비중을 얻는다.
- 3.1.3. 거래 흐름 모델링
    - 실제 거래 하듯이 재현할 수 있어야 하고 그에 따른 자산 보유 현황 정확히 추적하는 과정 필요.
- 3.1.4. 평균-분산 시뮬레이
### 3.2. 시뮬레이션 분석 (p.132)
- 3.2.1. 시뮬레이션 결과 전처리
    - 계좌에는 시뮬레이션 히스토리가 저장되어 있다. 분석에 용이한 형태로 전처리.
- 3.2.1.2. 룩백 기간 제거
    - 수익률 계산 시 필요한 룩백 기간을 제거
- 3.2.2. 포트폴리오 성능 지표
    - 수익률: 복리 연간 성장률, 누적 수익률, 상대 수익률
    - 변동성: 연간 변동성, 최대 손실 낙폭
    - 리스크 조정 수익률: 샤프 비율, 소티노 비율, 칼마 비율, 정보 비율
- 3.2.3. 시각화를 통한 시뮬레이션 분석
    - 3.2.3.1. 누적 수익률 곡선 cumulative return curve
    - 3.2.3.2. 단기 수익률 변동 막대그래프
    - 3.2.3.3. 개별 자산 누적 수익 곡선
    - 3.2.3.4. 자산 편입 비중 영역 차트
## 7장. 멀티 팩터 전략
### 7.1. 팩터로 구하는 국면 (p.300)
- 생각을 뒤집어서 전략 수익률을 바탕으로 현재 시장 국면을 정의할 것.
- 7.1.1. 전략별 일별 수익
    - 상승장은 모멘텀, 내실 잇는 기업만 살아남는 시기엔 가치주, 찬 바람 땐 배당주 등 각 상황마다 유리한 주식이 있다.
    - 팩터가 경기 상황에 맞게 활약한다면, 수익률로 경기 상황도 파악할 수 있을 것.
    - 7.1.1.1. 빈도 속성을 유지하면서 데이터 자르기
        - 월별로 데이터 수집했을 때 일자가 일치하지 않을 수 있음. 따라서 데이터 수집 주기를 알아야 함.
    - 7.1.1.2. 데이터 날짜 교정
        - 월별로 데이터 가져오면 데이터 날짜는 매월 말일로 설정된다.
        - 하지만 월별 마지막 거래일 데이터가 필요하다.
    - 7.1.1.3. 전략별 일별 수익 csv 만들기
        - 모든 전략을 동일한 주기로 실행해 수익률을 비교할 수 있어야 한다.
        - 일별로 모두 실행하기엔 오래걸리고 오류 가능성도. 따라서 모든 팩터 전략을 일별 실행하여 CSV로 저장.
        - 2013년 4월 1일부터 데이터 수집. 훈련한 것으로 시뮬레이션은 2017년 2월부터.
- 7.1.2. 경기 국면과 군집
    - 경기순환 business cycle
    - 장기 추세선 위는 호황기와 후퇴기, 아래는 침체기와 회복기.
    - 팩터 수익률도 이렇게 4개 분류로 나누어볼 수 있다.
    - K-mean 군집화를 사용. 비지도 학습니다.
    - 7.1.2.1. 사후 수익률 구하기
        - 일간 수익률로 군집화 하는 것은 날짜 별로 편차도 크고 이상치끼리 하나의 군집화 될수도.
        - 따라서 1개월 수익률을 사용.
        - 또한 구분 경기 국면은 추후 예측에 사용되어야 한다. 사후 1개월 수익률을 구할 것.
7.1.3. 군집화
    - 군집 개수를 설정해야 함. 그러기 위해서는 군집 개수에 따른 관성inertia를 측정한다.
    - 이는 군집 내 데이터와 군집 중심 사이 거리의 제곱합. 군집화의 품질 측정에 사용됨.
    - 관성이 높으면 멀리 떨어져 있으므로 관성을 줄이는 방향으로 조정해야 함.
    - 7.1.2.1. 관성 측정
        - 군집 수가 많으면 자연스럽게 관성은 줄어든다. 관성 절댓값 보다는 유의미하게 변화하는 부분을 찾는 게 중요.
    - 7.1.2.2. 군집화 실행
    - 7.1.2.3. 군집 평가
        - 군집 분류 결과 판단을 위해서 고차원 데이터를 저차원으로 시각화하는 t-SNE T-distributed Stochastic Neighbor Embedding을 사용함. 데이터 간 유사성 보존하면서도 저차원에서 잘 배치되도록 한다.
- 7.1.4. 전략 가중치 설정하기
    - 군집화로 경기 국면 분류함. 각 국면에서 어떤 팩터가 활약했는지 확인 가능.
    - 7.1.2.3. 군집 및 전략별 수익률
        - 전략이 어떤 군집에서 어떤 수익을 냈는지 알려면 군집 및 전략 별로 평균 수익률 구하기.
    - 7.1.2.4. 군집별 전략 가중치
        - 전략 성능에 비례해 군집 별 각 전략의 가중치를 계산한다.
        - 전략별 가중치는 평균 수익률을 정규화한 뒤 일정 값 이하의 값은 최소 가중치로 대체한 값이다.
### 7.2. 국면 예측 (p.327)
- 이전 장까지는 각 날짜가 어떤 경기에 해당하는지 labelling한 것이다. 이제 시장 국면을 예측해본다.
- 시장 둘러싼 거시경기 변수가 경기 상황을 알려준다고 볼 수 있다.
- 7.2.1. 거시 경기 데이터
    - 거시 경기 및 지수 자료를 얻기 위해 FinanceDataReader 라이브러리 사용.
    - 야후 파이낸스, 네이버 등 크롤링 가능.
    - 7.2.1.1. 거시 경기 데이터 불러오기
    - 7.2.1.2. 거시 경기 데이터 전처리
        - 데이터 취합 기준 날짜가 다르거나 중간에 누락이 있을수도 있다.
    - 7.2.1.3. 거시 경기 데이터 증강
        - 각 변수의 최근 추세 정보를 담는 파생 변수를 만들 것.
        - 단기 방향성 20일, 중기 방향성 60일 변수로 구분하기 위해 데이터를 증강하는 함수를 만든다.
- 7.2.2. 랜덤 포레스트를 통한 군집 예측
    - 특정 날짜에서 거시 경기 변수들로 구성된 벡터가 들어오면 벡터 구성요소를 분석하면서 해당 날짜가 몇 번 군집에 해당하는지 결정나무가 계산할 것.
    - 훈련데이터에서는 DT가 미리 정해진 군집에 맞춰 최적 구조를 학습할 것이고, 학습한 DT로 예측 수행할 것.
    - 7.2.2.1. 데이터 슬라이싱
    - 7.2.2.2. 랜덤 포레스트 적용
    - 7.2.2.3. 점진 학습
    - 7.2.2.4. 개별 루프에서의 전략 가중치 저장
    - 7.2.2.5. 군집 및 전략별 가중치 예측
- 7.2.3. 예측 평가하기
    - 7.2.3.1. 군집 예측 평가
    - 7.2.3.2. 가중치 예측 시각화
### 7.3. 멀티 팩터 시뮬레이션 (p.347)
- 7.3.1. 포트폴리오 준비
    - 7.3.1.1. 전략별 포트폴리오 구하기
    - 7.3.1.2. 멀티 팩터 포트폴리오 계산
        - 전략별 편입 비중을 구했다. 이 값을 전략별 가중치와 곱하면 최종 멀티 팩터 포트폴리오가 구해진다.
- 7.3.2. 전략 실행
    - 7.3.2.1. 멀티 팩터 포트폴리오 준비
        - 타 팩터 전략과 달리 멀티팩터는 포트폴리오가 미리 준비되어 있어 지표 산출할 필요가 없다.
    - 7.3.2.2. 멀티 팩터 전략 실행

