# Early Stopping

# TensorBoard 및 EarlyStopping을 활용한 모델 훈련 프로젝트

이 프로젝트는 **TensorFlow**를 사용하여 모델 훈련 중 **TensorBoard**와 **EarlyStopping** 콜백을 활용하는 방법을 연습하는 예제입니다. 이 두 가지 콜백을 통해 훈련 과정을 시각화하고, 과적합을 방지할 수 있습니다.

## 프로젝트 개요

이 프로젝트의 목표는:

1. **TensorBoard**를 활용하여 훈련 중 손실 값, 정확도 등을 시각화하여 훈련 과정을 모니터링합니다.
2. **EarlyStopping** 콜백을 사용하여 모델 훈련이 과적합되는 것을 방지하고, 성능이 개선되지 않으면 훈련을 일찍 종료합니다.

## 프로젝트 실행 방법

1. **TensorBoard 콜백 사용**
    - **TensorBoard**는 훈련 중 모델의 성능을 실시간으로 시각화하는 도구입니다. `TensorBoard` 콜백을 설정하여 훈련 중 로그를 기록하고, 훈련 진행 상황을 모니터링할 수 있습니다.
    
    ```python
    python
    코드 복사
    from tensorflow.keras.callbacks import TensorBoard
    import time
    
    # TensorBoard 로그 디렉토리 설정
    tensorboard = TensorBoard(log_dir='logs/{}'.format('첫모델' + str(int(time.time()))))
    
    # 모델 훈련 시 TensorBoard 콜백 추가
    model.fit(trainX, trainY, validation_data=(testX, testY), epochs=3, callbacks=[tensorboard])
    
    ```
    
    - `log_dir`: 훈련 로그가 저장될 디렉토리 경로를 설정합니다. `time.time()`을 사용하여 고유한 폴더 이름을 생성하여 로그 파일을 저장합니다.
    - 훈련 과정에서 **손실 값**, **정확도**, **모델 가중치** 등의 정보를 TensorBoard에서 실시간으로 시각화할 수 있습니다.
2. **EarlyStopping 콜백 사용**
    - **EarlyStopping**은 검증 데이터셋에 대해 성능이 개선되지 않으면 훈련을 조기에 종료하는 콜백입니다. 이 방법을 사용하면 과적합을 방지하고 훈련 시간을 줄일 수 있습니다.
    
    ```python
    python
    코드 복사
    from tensorflow.keras.callbacks import EarlyStopping
    
    # EarlyStopping 설정: 검증 손실(val_loss)이 개선되지 않으면 훈련 종료
    es = EarlyStopping(monitor='val_loss', patience=5, mode='min')
    
    # 모델 훈련 시 EarlyStopping 콜백 추가
    model.fit(trainX, trainY, validation_data=(testX, testY), epochs=100, callbacks=[tensorboard, es])
    
    ```
    
    - `monitor`: 성능을 모니터링할 지표를 설정합니다. 여기서는 `val_loss`를 사용하여 검증 데이터셋의 손실 값을 모니터링합니다.
    - `patience`: 성능이 개선되지 않아도 몇 에폭(epoch) 동안 훈련을 계속할지 설정합니다. 예를 들어, `patience=5`는 5번의 에폭 동안 성능이 개선되지 않으면 훈련을 종료합니다.
    - `mode`: 성능이 개선되는 방향을 설정합니다. `min`은 손실이 최소화될 때 성능 개선이 이루어졌다고 판단합니다.
3. **훈련 로그 시각화 (TensorBoard)**
    - 훈련이 완료된 후, TensorBoard를 사용하여 훈련 과정을 시각화할 수 있습니다.
    - 터미널에서 다음 명령어로 TensorBoard를 실행하여 훈련 로그를 시각화합니다.
    
    ```bash
    bash
    코드 복사
    tensorboard --logdir=logs
    
    ```
    
    - 웹 브라우저에서 `http://localhost:6006` 주소로 접속하여 훈련 과정을 확인할 수 있습니다.

## 파일 구조

```bash
bash
코드 복사
프로젝트 폴더/
│
├── logs/                     # TensorBoard 로그 파일
│   └── 첫모델1637027047      # 훈련 로그 디렉토리
│
├── train.py                  # 모델 훈련 스크립트
└── README.md                 # 프로젝트 설명 파일

```

## 주요 기능

1. **TensorBoard**: 훈련 중 손실 값, 정확도 등을 시각화하여 모델 성능을 실시간으로 모니터링할 수 있습니다.
2. **EarlyStopping**: 훈련 중 성능 개선이 없으면 훈련을 일찍 종료하여 과적합을 방지할 수 있습니다.

## 필수 라이브러리

이 프로젝트를 실행하기 위해서는 다음의 라이브러리가 필요합니다.

```bash
bash
코드 복사
pip install tensorflow

```

## 활용 방법

- **TensorBoard**: 훈련 로그를 시각화하여 모델의 학습 과정을 모니터링하고, 훈련 중 성능이 어떻게 변화하는지 실시간으로 확인할 수 있습니다.
- **EarlyStopping**: 모델 훈련을 자동으로 종료하여 성능 개선이 없을 경우 불필요한 시간 소모를 줄일 수 있습니다. 과적합을 방지하는 데 유용합니다.

## 참고 사항

- **훈련 데이터셋**과 **검증 데이터셋**(testX, testY)은 별도로 준비해야 합니다.
- `log_dir`의 경로는 훈련마다 고유하게 생성되는 로그 파일을 저장하므로, 훈련을 여러 번 진행하면 각각의 훈련 로그가 다른 디렉토리에 저장됩니다.
