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
### 7.2. 국면 예측 (p.327)
### 7.3. 멀티 팩터 시뮬레이션 (p.347)
