# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김시온
- 리뷰어 : 김나연


# PRT(Peer Review Template)
- [X]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - MLM, NSP task의 특징이 잘 반영된 pretrain용 데이터셋 생성과정이 체계적으로 진행되었다.
        - <img width="1040" height="745" alt="image" src="https://github.com/user-attachments/assets/6be2ce91-5916-4c35-b199-9fe7bd2c5e8f" />
    - 학습진행 과정 중에 MLM, NSP loss의 안정적인 감소가 확인되었다.
        - <img width="1702" height="425" alt="image" src="https://github.com/user-attachments/assets/140c557e-c3fe-4537-9079-c789a96e4bac" />
    - 학습된 모델 및 학습과정의 시각화 내역이 제출되었다.
        - <img width="875" height="743" alt="image" src="https://github.com/user-attachments/assets/25472fd8-cc9a-49c2-a3d6-e65068175a5d" />

- [X]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 프로젝트 최상단에 전체 flow를 설명하는 텍스트가 이해를 돕고 있다.
        - <img width="733" height="766" alt="image" src="https://github.com/user-attachments/assets/5d48900f-7fd8-431b-865e-90a64c930dee" />

- [X]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - vocab_size를 비교하는 실험을 진행하였다. (4000, 8000, 16000)
        - <img width="875" height="743" alt="image" src="https://github.com/user-attachments/assets/25472fd8-cc9a-49c2-a3d6-e65068175a5d" />
    - 모델 크기에 따른 성능을 비교하는 실험을 진행하였다.
        - size_configs = {
                "small":  dict(d_model=64, n_head=4, d_head=16, d_ff=256, n_layer=2),
                "base":   dict(d_model=96, n_head=4, d_head=24, d_ff=384, n_layer=2),
                "large":  dict(d_model=96, n_head=4, d_head=24, d_ff=384, n_layer=4),
          }
        - <img width="855" height="746" alt="image" src="https://github.com/user-attachments/assets/88cad66c-cc16-4917-96ee-7a3cfc821a95" />
    - mask_prob만 바꿔서 pretrain 데이터를 다시 만든 다음 비교하는 실험을 진행하였다.
        - <img width="863" height="747" alt="image" src="https://github.com/user-attachments/assets/d3256f44-b7d6-4951-be9c-0f65f74df8e1" />
    - <img width="1622" height="458" alt="image" src="https://github.com/user-attachments/assets/c6c6b203-eb0f-4490-a5b1-b7530efae08c" />

- [X]  **4. 회고를 잘 작성했나요?**
    - <img width="1688" height="167" alt="image" src="https://github.com/user-attachments/assets/2a07e040-0e07-43be-a61f-95500df05c57" />

- [X]  **5. 코드가 간결하고 효율적인가요?**
    - 데이터 전처리 함수, 데이터 로딩, BERT 모델 구현, Loss / Accuracy / LR 스케줄, 비교실험을 함수화하여 재사용성을 높이고 있다.
        - <img width="927" height="752" alt="image" src="https://github.com/user-attachments/assets/cbb72ecc-ac84-40f7-854c-14a7155cc4f9" />


# 회고(참고 링크 및 코드 개선)
```python
저는 실험 하나만 해도 시간이 부족해서 더 못했는데 대단하세요...! 이번 프로젝트도 수고하셨습니다
```
