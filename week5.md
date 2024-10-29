## 제8장. 딥러닝 예측을 통한 시장 모니터링
### 8.1. 딥러닝 예측 모델 구성 (p.360)
- 8.1.1. 분석 (p.361)
    - 시계열 분석은 시간 흐름에 따라 변화하는 데이터 분석하는 방법.
        - 추세 변동, 계절 변동, 순환 변동, 불규칙 변동이 있다.
     - 시계열데이터에는 규칙적 시계열과 불규칙적 시계열이 있다.
        - 불규칙 시계열에 규칙성 부여하기 위해 AR, MA, ARMA 등 모델이 제안되었다.
- 8.1.2. 딥러닝 모델 (p.363)
    - RNN 모델 recurrent neural network
        - 순환 구조를 가지며 이전 단계 출력을 현재 단계 입력으로 사용하는 재귀적 구조를 가짐.
        - 과거 정보 기억하과 활용 가능하며 장기적인 패턴 학습 가능.
        - 그러나 오류 누적 및 기울기 소실 문제, 긴 시퀀스에서는 멀리 떨어진 정보를 잘 기억하지 못함.
        - 일반적으로 LSTM, GRU 모델이 있다.
    - 트랜스포머 모델 transformer
        - 주로 자연어 처리에 사용되었지만 시계열 예측에도 가능.
        - self attention mechanism을 사용해 입력 시퀀스의 전역적 종송성을 모델링한다.
        - RNN과 달리 병렬 처리 가능하며 더 긴 시퀀스에서 유리.
        - 잔차연결과 층 정규화를 통해 안정적 학습을 도움. 그러나 모델이 무겁고 학습 시간이 길다.
    - TCN 모델 temporal convolution network
        - ID 합성곱 신경망 구조로 시계열 데이터를 처리.
        - TCN은 고정 크기 필터로 각 시간 단계의 지역적 패턴을 학습하며 다음 시간 단계의 값을 예측한다.
        - 시간적 의존성 캡쳐에 RNN보다 뛰어나다.
        - 학습이 빠르고 하이퍼파라미터 조정이 적다.
    - 이 장에서는 RNN과 현재 시계열 예측에서 SOTA 모델인 NLinear, SCINet에 대해 알아볼 것.
- 8.1.3. RNN (p.365)
    - GRU 활용한 RNN 모델로 분석 진행한다. Gated recurrent unit의 약자로 RNN의 한 종류
        - 장기 의존성 문제를 해결하기 위해 게이트 구조로 설계된 셀로, 시퀀스 데이터 모델링에 주로 사용.
            - 장기 의존성 문제는 은닉층 과거 정보가 마지막까지 전달되지 못하는 현상.
        - Gate에는 업데이트 게이트와 리셋 게이트라는 2가지 사용.
- 8.1.3.1. RNN 모델 함수
```python
class RNNModel(nn.Module):
    def __init__(self, input_dim, hidden_size, num_layers, output_dim):    # dim은 입력x에 예상되는 특징 수. numlayer는 순환계층의 수
        super(RNNModel, self).__init__()

        self.hidden_size = hidden_size
        self.num_layers = num_layers
        self.gru = nn.GRU(input_dim, hidden_size, num_layers, batch_first=True)
        self.fc = nn.Linear(hidden_size, output_dim)

    def forward(self, x):    # 모델의 순방향 패스를 정의함.
        h0 = torch.zeros(self.num_layers, x.size(0), self.hidden_size.to(x.device)
        out, _ = self.gru(x, h0)
        out = out[:, -1, :]
        out = self.fc(out)
        return out
```
- 8.1.4. SCINet (p.367)
        - 딥러닝 모델 중 TCN 구조를 발전시킨 모델이다.
        - 다양한 시계열 해상도로부터 특징 반복적으로 추출하고 이들을 서로 인터렉션 하는 계층적 구조를 제안함.
          - 
- 8.1.5. NLinear (p.371)
### 8.2. 딥러닝 예측 모델 시뮬레이션 (p.374)
- 8.2.1. 딥러닝 학습 개요 (p.374)
- 8.2.2. ETF 기반 수시 리밸런싱 (p.382)
- 8.2.3. 평균-분산 전략 기반 수시 리밸런싱 (p.400)
- 8.2.4. 시뮬레이션 결과 비교 (p.406)
## 제9장. 고급 최적화 전략
### 9.1. 블랙-리터만 알고리즘 (p.410)
- 9.1.1. 블랙-리터만 전략 이론 (p.410)
- 9.1.2. 블랙-리터만 전략 구현하기 (p.416)
- 9.1.3. 블랙-리터만 전략 시뮬레이션 (p.429)
### 9.2. 리스크 패리티 알고리즘 (p.430)
- 9.2.1. 블랙-리터만 전략과 리스크 패리티 전략 (p.431)
- 9.2.2. 리스크 패리티 전략 이론 (p.432)
- 9.2.3. 리스크 패리티 전력 구현하기 (p.434)
- 9.2.4. 리스크 패리티 전략 시뮬레이션 (p.442)
