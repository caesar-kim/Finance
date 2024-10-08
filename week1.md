# 1주차
## 제5장. 지도학습: 회귀(시계열 모델) (p.107)
- 회귀 기반 지도학습
    - 예측 모델. 목표와 예측 변수 간 관계 모델링하고 가능한 연속적 출력값을 예측하는 것. 금융에서 가장 많이 사용하는 머신러닝.
    - 애널리스트의 관심사는 투자 기회를 예측한다는 점에서 이런 모델은 적합하다.
    - 대량의 데이터 및 처리 기술 이용 가능으로, 자산 가격 예측 뿐 아니라 포트폴리오 관리, 보험, 상품 가격, 헤징, 위험 관리 등 다양하게 사용 가능.
- 금융 산업은 상당히 많은 자산 모델링과 예측 문제가 시간 구성요소와 연속 출력의 추정과 관계됨.
    - 시계열 모델 다루는 것이 중요.
    - 가장 광범위한 형태의 시계열 분석은 과거 일련 데이터에서 무슨 일이 발생했는지 추론하고 앞으로 발생할 것을 예측하는 것.
    - 대부분의 시계열 모델은 모수적parametric 이지만, 대부분의 지도회귀 모델은 비모수적nonparametric.
        - 예측을 위해 시계열 모델은 주로 예측 변수의 과거 데이터를 사용하고, 지도학습은 외생 변수를 사용한다.
        - 그러나 지도회귀는 시간 지연 접근방식을 통해 예측 변수의 과거 데이터를 포함할 수 있으며, 시계열 모델은 예측을 위해 외생변수를 사용할 수도 있다.
        - 따라서 서로 유사하다. 최종 출력 측면에서도 두 모형은 변수의 가능한 결과의 연속 집합을 추정한다.

# 5.1. 시계열 모델 (p.110)
- 시계열은 시간 지수로 정렬한 수의 순서이다.
# 5.1.1. 시계열 명세 (p.110)
- 시계열은 다음의 구성요소로 나눌 수 있음.
- 추세 요소
    - 추세는 시계열에서 일관된 방향으로의 이동.
    - 추세는 결정론적deterministic이거나 확률적stochastic.
        - 전자는 추세에 대한 근본적 근거 제시하는 반면, 후자는 시계열의 임의적 특성을 나타낸다.
- 계절 요소
    - 시계열에는 계절적 변동이 따른다. 비즈니스 영업이나 기후 수준은 나타내면 더욱 그렇다.
    - 퀀트 금융에서 계절적 변동을 종종 볼 수 있다. 휴가철이나 연간 기온변동 같은 것들.
- 시계열 y(t)는 S(t)+T(t)+R(t) 로 표현 가능.
    - 계절요소 + 추세요소 + 나머지 요소
    - 시계열은 구성요소로 나누는 코드는 다음과 같다.
```python
import statsmodels.api as sm
sm.tsa.seasonal_decompose(Y, freq=52).plot()
```
- 시계열은 구성요소로 나누면 이해하기 더 쉽고, 예측을 더 잘하기 위해 움직임 식별 수월.
- 도표의 각각의 스케일이 다르다.
# 5.1.2. 자기상관과 고정성 (p.112)
- 자기상관
    - 많은 경우 시계열의 연속적 요소가 상관관계를 보임.
        - 시계열에서 연속된 점들이 변화하면 그에 따라 서로 영향을 받음.
        - autocorrelation은 관측치 간 유사성 의미하는 것으로, 관측치 간 시간 지연의 함수로 나타냄.
        - 이러한 관계를 자기회귀모델로 모델링가능.
        - 자기회귀autoregression이란 변수 자신에 대한 회귀가 있음을 말하는 것.
        - 관심 있는 변수를 그 변수 과거 값을 선형적으로 조합해 예측한다.
        - 여기서 epsilon은 화이트 노이즈.
        - 자기회귀는 다중회귀와 비슷하지만 예측자로 지연값 y(t)를 갖는다.
        - 이것이 AR(p) 모델이라 불리는 p차 자기회귀모델이다.
- 고정성
    - 시계열의 통계적 특성이 시간에 걸쳐 변하지 않는 것.
    - 추세나 계절성이 있는 시계열은 고정적이지 않다. 추세와 계절성이 여러 시간에 걸쳐 시계열 값에 영향 주기 때문.
    - 그러나 화이트 노이즈 시계열은 고정적이다. 임의의 시간에 관찰할 때 항상 비슷한 패턴을 보여서 관찰이 무의미 하기 때문.
    - 고정적이려면 추세가 없어야 한다. 분산도 일정해야 한다. 공분산도 일정해야 한다.
        - 고정 시계열이면 미래 값 예측이 쉽다. 통계적 모델 대부분 효과적이고 정확한 예측을 위해 고정 계열이 필요하다.
    - 그러나 시계열의 비고정성 원인은 추세와 계절성이다. 변환을 해야 한다.
- 디퍼런싱
    - 시계열을 고정적으로 만드는 방법의 하나.
        - 시계열 연속항 간 차를 계산하는 것으로 변동하는 평균을 제거하기 위해 수행.
        - y(t) - y(t-1) 을 통해 구현.
        - 1차 시계열이 화이트 노이즈라면 원 시게열을 1차 비고정 시계열이라고 함.
# 5.1.3. 기존 시계열 모델 (ARIMA 모델 포함) (p.114)
- 예측하기 위해 시계열 모델링 법은 여러 가지 있다.
    - 대부분 시계열 모델은 시계열에 내재된 자기상관과 고정성 해결하면서, 추세, 계절, 잔여 요소 포함을 목표로 한다.
    - 널리 사용되는 것은 ARIMA
- ARIMA autoregressive integrated moving average
    - 고정성을 자기회귀와 이동평균 모델을 합친 것.
        -AR(p): 일정한 시간 지연으로 이전 계열값에 따라 달라진다고 가정하고 시계열을 자신의 계열에 회귀하는 자기 회귀
        - I(d): 통합 차수. 시계열이 고정성 갖기 위해 필요한 차수.
        - MA(q): 이동평균. 현재 오류가 일정한 시간 지연q로 이전 오류에 따라 달라진다고 가정하고 시계열 오류를 모델링 한 것.
        -ARIMA(p, d, q)라고 한다.
        - (1, 0, 0)차 ARIMA 모델을 적합화 하는 파이썬 코드.
```python
from statsmodels.tsa.arima_model import ARIMA
model = ARIMA(endog=Y_train, order=[1, 0, 0])
```
- ARIMA 계열의 변형
    - ARIMAX: 외생 변수가 있는 ARIMA
    - SARIMA: 계절성S 요소를 포함한 것.
    - VARMA: 모델을 다변수로 확장하여 여러 변수 동시에 예측할 때 필요.
# 5.1.4. 시계열 모델링에 대한 딥러닝 접근방식 (p.116)
- ARIMA 같은 기존 시계열 모델은 많은 문제에 잘 적용되고 효과적. 하지만 한계도 있다.
    - 기존 시계열 모델은 선형 함수이거나 선형 함수의 단순 변형에 불과.
        -   따라서 시간 의존성 같은 수동적으로 진단된 매개변수가 필요해 왜곡된 데이터나 완전하지 않은 데이터에서 성능 안 좋음.
    - 시계열 분야 딥러닝 발전은 최근에는 순환신경망RNN이 주목.
        - 구조와 비선형성 같은 패턴을 찾고 다중 입력 변수로 문제를 모델링한다.
        - 그래서 상대적으로 완전하지 않은 데이터에 안정적.
            - RNN은 한 단게 연산에서 얻은 출력을 다음 단계 연산을 위한 입력으로 사용해 반복적으로 전환되는 상태를 유지함.
            - 과거 얻은 데이터로 미래 예측 하기 때문에 시계열 모델이라고 할 수 있다.
            - 시계열 예측을 위한 딥러닝 모델을 살펴볼 것.
- 순환 신경망
    - 순환이 붙은 이유는 순서의 각 요소에서 같은 일을 수행하고 요소 출력이 이전 요소의 연산에 의존하기 때문.
    - RNN 모델은 메모리가 있어 일정 시점까지 연산한 결과 정보를 저장함.
        - 각 망은 메시지를 다음 망으로 전달함.
        - X(t)는 t 시점 입력, O(t)는 t 시점 출력, S(t)는 t시점 은닉층. 이것은 망의 기억 저장소로, 이전 은닉층과 현 시점에서의 입력에 기반해서 계산함.
        - RNN 주요 특성은 이 은닉층으로, 이 층 순서에 대한 정보를 저장하고 필요시 이용함.
- 장단기 메모리LSTM
    - LNN의 한 종류로 장기 의존 문제 해결을 위해 설계됨. 오랜 기간동안 정보를 기억한다.
    - 이 모델은 셀의 집합으로 되어 있고, 각 셀은 데이터 순서를 기억.
    - 데이터 흐름을 감지하고 저장함.
    - 셀은 과거 모듈을 현재 모듈과 연결시켜 정보를 과거 시간에서 현재 시간으로 전달함.
    - 각 셀은 gate게이트가 있어서 다음 셀로의 전달 시 데이터를 제거, 여과, 추가할 수 있음.
    - 인공신경망층을 기반으로 한 게이트를 이용해 셀로 전달하는 데이터를 통과시키거나 제거할 수 있음.
    - 각 층은 0~1 사이의 값을 가지는 수를 만들고, 이것으로 각 셀을 통과하는 데이터 양을 설명할 수 있음.
        - 값이 0이라면 어떠한 데이터도 통과 못하고, 1이면 모든 데이터가 통과한다는 것.
    - 셀의 상태를 통제할 목적으로 세 유형의 게이트를 가짐.
        - 망각 게이트
            - 0~1 사이 수 출력. 1은 완전히 기억 0은 완전히 잊어버림. 이 게이트는 과거를 잊을지 기억할지 조건적으로 결정
        - 입력 게이트
            - 셀에 저장할 새로운 데이터를 선택
        - 출력 게이트
            - 각 셀에서 무엇을 생성할지 결정. 생성되는 값은 셀 상태와 여과되고 새로 추가된 데이터를 기반으로 함.
    - 케라스로 단 몇 줄 코드로 LSTM 신경망 모델 정의하고 훈련 가능.
    - keras.layers에 있는 LSTM 모듈로 신경망 구현하고 X_train_LSTM 변수로 훈련함.
    - 이 신경망은 LSTM이 50개 있는 은닉층과 하나의 값을 예측하는 출력층으로 구성됨.
    - 학습과 구현 측면에서 LSTM은 ARIMA 모델에 비해 더 많은 미세조정 옵션을 제공함.
    - 딥러닝 모델은 기존 시계열 모델에 비해 장점이 많지만 복잡하고 훈련하기 어렵다.
```python
from keras.models import Sequential
from keras.layers import Dense
from keras.optimizers import SGD
from keras.layers import LSTM

def create_LSTMmodel(learn_rate=0.01, momentum = 0):
        # 모델생성
        model = Sequential()
        model.add(LSTM(50, input_shape=(X_train_LSTM.shape[1], X_train_LSTM.shape[2])))
        # 필요 시 더 많은 셀 추가
        model.add(Dense(1))
        optimizer = SGD(lr=learn_rate, momentum=momentum)
        model.compile(loss='mse', optimizer='adam')
        return model

LTSMModel = create_LSTMmodel(learn_rate=0.01, momentum=0)
LTSMModel_fit = LSTMModel.fit(X_train_LSTM, Y_train_LSTM, validation_data=(X_test_LSTM, Y_test_LSTM), epochs=330, batch_size=72, verbose=0, shuffle=False)
```
# 5.1.5. 지도학습 모델을 위한 시계열 데이터 수정 (p.119)
- 시계열은 시간 지수로 정렬된 순차적 연속 수
    - 지도학습 문제처럼 예측변수와 예측되는 변수 집합으로 재구성 가능.
    - 이전 시간 단계를 입력 변수로, 다음 시간 단계를 출력 변수로 정해 재구성.
    - 관측치 간 순서는 그대로, 대신 처음과 마지막 행은 X나 Y 값이 없어서 제거.
    - Pandas의 shift() 함수 사용.
- 다음 단계 예측을 위해 이전 시간의 값을 사용하는 것을 슬라이딩 윈도우sliding window, 시간 지연 time delay, 지연 방법 lag라고도 함.
# 5.2. 실전 문제 1: 주가 예측 (p.120)
- 주가 예측하기.
    - 머신러닝은 과거 데이터 기반으로 주가 예측하는 일에 제격이다.
    - 직전 단계 뿐 아니라 몇 단계 앞선 시점도 예측.
    - 주가 예측에 일반적으로 유용한 특성
        - 상관자산
            - 경쟁자, 고객, 자본, 정책 등 다양한 것들과 상관관계.
        - 기술지표
            - 이동평균, 모멘텀 등
        - 가치 분석
            - 성과 보고서나 뉴스를 주로 사용
    - 이 문제에서 사용할 것
        - 다양한 머신러닝 모델과 시계열 모델
        - 차트의 여러 종류, 즉 밀도, 상관, 산점도를 이용한 시각화
        - 딥러닝LSTM
        - ARIMA 모델을 위한 격자 탐색 구현
        - 모델별 결과 해석과 잠재적 과적합, 과소적합 분석
- (1) 문제정의
    - MSFT의 주 단위 수익을 예측되는 변수로 사용. 무엇이 주가에 영향 주는지 이해하기 위해 가능한 많은 정보 포함.
    - 특성 상관 자산에 집중할 것.
    - 주식(IBM, 알파벳), 환율(달러/엔, 파운드/달러), 인덱스(SNP, 다우), 변동성 지수
    - 야후 파이낸스와 FRED에서 가져옴.
    - 주가를 예측하고, 시계열의 단계별 인프라와 프레임워크 시연하고, 지도 회귀 기반 모델링도 설명. 2010년부터 10년 간 일간 종가 사용.
- (2) 시작하기 - 데이터와 파이썬 패키지 불러오기
```python
# 지도 회귀모델을 위한
from sklearn.linear_model import LinearRegression
from sklearn.linear_model import Lasso
from sklearn.linear_model import ElasticNet
from sklearn.tree import DecisionTreeRegressor
from sklearn.neighbors import KNeighborsRegressor
from sklearn.svm import SVR
from sklearn.ensemble import RandomForestRegressor
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.ensemble import ExtraTreesRegressor
from sklearn.ensemble import AdaBoostRegressor
from sklearn.neural_network import MLPRegressor

# 데이터 분석 및 모델 평가를 위한
from sklearn.model_selection import train_test_split
from sklearn.model_selection import KFold
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import GridSearchCV
from sklearn.metrics import mean_squared_error
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import chi2, f_regression

# 딥러닝 모델을 위한
from keras.models import Sequential
from keras.layers import Dense
from keras.optimizers import SGD
from keras.layers import LSTM
from keras.wrappers.scikit_learn import KerasRegressor

# 시계열 모델을 위한
from statsmodels.tsa.arima_model import ARIMA
import statsmodels.api as sm

# 데이터 준비와 시각화를 위한
import numpy as np
import pandas as pd
import pandas_datareader.data as web
from matplotlib import pyplot
from pandas.plotting import scatter_matrix
import seaborn as sns
from sklearn.preprocessing import StandardScaler
from pandas.plotting import scatter_matrix
from statsmodels.graphics.tsaplots import plot_acf

# 데이터 불러오기
stk_tickers = ['MSFT', 'IBM', 'GOOGL']
ccy_tickers = ['DEXJPUS', 'DEXUSUK']
idx_tickers = ['SP500', 'DJIA', 'VIXCLS']

stk_data = web.DataReader(stk_tickers, 'yahoo')
ccy_data = web.DataReader(ccy_tickers, 'fred')
idx_data = web.DataReader(idx_tickers, 'fred')

# 한 주의 거래일을 5일로 가정하고 수익 계산
# 독립변수로는 상관자산과 다른 주기의 과거 수익 사용.
# 상관자산으로는 IBM과 GOOGL의 5일 지연 수익, 환율(달러/엔, 파운드/달러), 인덱스(SNP, DOW, VIX)
# 그리고 MSFT의 5일, 15일, 30일, 60일 지연 수익도 사용.
# 5일 지연 변수는 시간을 늦추는 방법을 사용한 시계열
return_period = 5
Y = np.log(stk_data.loc[:, ('Adj Close', 'MSFT')]).diff(return_period).shift(-return_period)
Y.name = Y.name[-1]+'_pred'

X1 = np.log(stk_data.loc[:, ('Adj Close', ('GOOGL', 'IBM'))]).diff(return_period)
X1.columns = X1.columns.droplevel()
X2 = np.log(ccy_data).diff(return_period)
X3 = np.log(idx_data).diff(return_period)

X4 = pd.concat([np.log(stk_data.loc[:, ('Adj Close', 'MSFT')]).diff(i) for i in [return_period, return_period*3, return_period*6, return_period*12]], axis=1).dropna()
X4.columns = ['MSFT_DT', 'MSFT_3DT', 'MSFT_6DT', 'MSFT_12DT']

X = pd.concat([X1, X2, X3, X4], axis=1)

dataset = pd.concat([Y, X], axis=1).dropna().iloc[::return_period, :]
Y = dataset.loc[:, Y.name]
X = dataset.loc[:, X.columns]
```
- (3) 탐색적 데이터 분석소
```python
dataset.head()
# 데이터 시각화
# 산점도와 상관행렬 보기
# 선형회귀, 로지스틱 회귀 같은 알고리즘은 데이터 입력 변수 간 상관관계가 높으면 성능이 낮아져서 시각화가 유용.
correlation = dataset.corr()
pyplot.figure(figsize=(15,15))
pyplot.title('Correlation Matrix')
sns.heatmap(correlation, vmax=1, square=True,annot=True,cmap='cubehelix')

# 산점도로 회귀 모델의 모든 변수 간 관계를 시각화
pyplot.figure(figsize=(15,15))
pd.plotting.scatter_matrix(dataset,figsize=(12,12))
pyplot.show()
# 결과를 보니 예측변수와 특성 간 특별한 관계 없는 듯.

# 3.3. 시계열 분석
# 예측변수의 시계열을 추세와 계절성 구성요소로 재구성해본다.
res = sm.tsa.seasonal_decompose(Y,freq=52)
fig = res.plot()
fig.set_figheight(8)
fig.set_figwidth(15)
pyplot.show()
```
- (4) 데이터 준비
- (5) 모델 평가
- (6) 모델 튜닝 및 격자 탐색
- (7) 모델 확정
- (8) 결론
# 5.3. 실전 문제 2: 파생상품 가격 책정 (p.140)
# 5.4. 실전 문제 3: 투자자 위험 감수 및 로보 어드바이저 (p.153)
# 5.5. 실전 문제 4: 수익률 곡선 예측 (p.171)
# 5.6. 맺음말 (p.181)
# 5.7. 연습문제 (p.182)

## 제6장. 지도학습: 분류 (p.183)
# 6.1. 실전문제 1: 사기 탐지 (p.184)
# 6.2. 실전문제 2: 채무 불이행 확률 (p.200)
# 6.3. 실전문제 3: 비트코인 거래 전략 (p.216)
# 6.4. 맺음말 (p.229)
# 6.5. 연습문제 (p.230)
