# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김시온
- 리뷰어 : 조영근


# PRT(Peer Review Template)
- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 텍스트 전처리, SentencePiece 토크나이저 학습, 데이터셋 및 데이터로더 구축, 모델 정의, 학습 및 평가 루프, 토크나이저/모델별 비교 실험을 누락 없이 구현
    - 특히 NSMC 데이터셋을 대상으로 SentencePiece의 unigram 및 bpe 알고리즘을 적용
    - 형태소 분석기(Mecab)와의 성능을 정량적으로 비교하여 체계적인 결론을 도출
    - <img width="437" height="363" alt="image" src="https://github.com/user-attachments/assets/56fe63ad-7c9b-4700-8b72-06c4ae63192a" />


- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 코드 전반에 걸쳐 핵심 로직과 함수에 대한 주석과 마크다운 설명이 적절히 배치
    - SentencePiece 학습 함수, 데이터로더 생성 함수, 실험 자동화 함수 매개변수의 역할과 작동 원리를 잘 볼 수 있었음
    - 실험 블록마다 주석을 통해 코드의 존재 이유가 잘 기술되어 있음
    - <img width="776" height="682" alt="image" src="https://github.com/user-attachments/assets/9b4dea10-9ae3-4963-ab5b-4b67fad3da0f" />


- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - 모델 아키텍처와 토크나이저 간의 비교 실험을 심도 있게 수행
    - CNN 모델을 기준으로 토크나이저 성능을 통제된 조건에서 비교
    - 병렬 처리 효율과 표현력 측면에서 어떤 토크나이저가 우수한지 실증적인 근거를 마련
    - <img width="586" height="175" alt="image" src="https://github.com/user-attachments/assets/4532cdcf-d0fc-4ffd-87fa-f8ee46780d5a" />
    - <img width="706" height="25" alt="image" src="https://github.com/user-attachments/assets/debf2642-c0a2-44be-9e24-ce61307c2c54" />
    - <img width="494" height="67" alt="image" src="https://github.com/user-attachments/assets/ab00d844-0f45-4908-b700-f2d37aaf7136" />
    - <img width="500" height="27" alt="image" src="https://github.com/user-attachments/assets/1afeb96a-1c85-4f64-a405-4825f00b67fc" />


- [x]  **4. 회고를 잘 작성했나요?**
    - <img width="869" height="805" alt="image" src="https://github.com/user-attachments/assets/1d6d66b9-e853-49db-a3f8-149c3a59e224" />


- [x]  **5. 코드가 간결하고 효율적인가요?**
    - 데이터 로딩, 전처리, 토크나이징, 모델 빌드, 학습, 평가 함수 및 클래스 단위로 모듈화
    - 전역 설정(CONFIG) 딕셔너리를 도입하여 하이퍼파라미터를 일괄 관리함으로써 확장성과 가독성을 높음
    - <img width="501" height="565" alt="image" src="https://github.com/user-attachments/assets/aa152510-e12d-45d5-8531-3204cc598fe0" />


# 회고(참고 링크 및 코드 개선)
```python
# 리뷰어의 회고를 작성합니다.
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
