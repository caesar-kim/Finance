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
TextBlob(te
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
### 10.3. 실전 문제1: NLP 및 감정 분석 기반 거래 전략
### 10.4. 실전 문제2: 챗봇 - 디지털 어시스턴트
### 10.5. 실전 문제3: 문서 요약
### 10.6. 맺음말
### 10.7. 연습 문제

## 3장. 평균-분산 전략 구현 및 시뮬레이션 분석
### 3.1. 평균-분산 전략 구현 (p.93)
### 3.2. 시뮬레이션 분석 (p.132)

## 7장. 멀티 팩터 전략
### 7.1. 팩터로 구하는 국면 (p.300)
### 7.2. 국면 예측 (p.327)
### 7.3. 멀티 팩터 시뮬레이션 (p.347)
