# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김시온
- 리뷰어 : 강지수


# PRT(Peer Review Template)
- [X]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 한국어-영어 병렬 데이터의 중복 제거와 전처리부터 Mecab 형태소 분석, Tokenizer 구축, GRU Encoder-Decoder와 Bahdanau Attention 구현, 학습 및 번역, Attention Map 시각화까지 완~전 매끄럽게 전체 파이프라인이 연결되어 있습니다.
    - 메인 모델은 10 epoch 동안 학습되었으며 Train Loss가 5.2959 → 1.0642로 지속적으로 감소해 모델이 잘 학습되었습니다.
    <img width="621" height="187" alt="스크린샷 2026-09-10 오전 10 27 31" src="https://github.com/user-attachments/assets/5924527b-10cb-45dd-b1fa-1c517446c815" />
    <img width="619" height="249" alt="스크린샷 2026-09-10 오전 10 27 46" src="https://github.com/user-attachments/assets/66f20859-dbe0-4f7d-b699-48ee6218c78d" />

    - 매 epoch마다 4개의 고정 문장을 번역하여 학습 과정에서 번역 결과가 어떻게 변화하는지도 확인하였고, 최종적으로 Attention Map까지 시각화해두었는데, 저는 한글 호환 안되서 깨졌는데 시온님은 완전 잘 하셔서 부러웠습니다.
  <img width="777" height="494" alt="스크린샷 2026-09-10 오전 10 26 41" src="https://github.com/user-attachments/assets/c572ed7a-3be6-4de2-afb6-6e06ad45512f" />

    

- [X]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - Attention의 hidden과 encoder_outputs의 shape을 주석으로 명시하여 Decoder의 현재 hidden state와 Encoder 전체 출력이 어떻게 비교되는지 이해하는 데 도움이 되었습니다.
    - Encoder에는 "한국어(소스 언어)를 입력받는 Encoder", Decoder에는 "영어(타깃 언어)를 생성하는 Decoder"라는 docstring이 있어 각 모듈의 역할이 명확했습니다.
    <img width="769" height="448" alt="스크린샷 2026-09-10 오전 10 24 34" src="https://github.com/user-attachments/assets/2af1f40e-bf27-4161-b951-8d1b7dc441eb" />


- [X]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - 모델 크기가 커질수록 5 epoch Train Loss는 3.25 → 2.15 → 1.24로 더 빠르게 감소했지만 학습시간도 증가하여 모델 용량과 연산 비용 사이의 trade-off를 직접 비교한 점이 인상적이었습니다.
      <img width="739" height="456" alt="스크린샷 2026-09-10 오전 10 22 23" src="https://github.com/user-attachments/assets/46fc439a-4159-4a9f-b356-3d12097a04d8" />


- [X]  **4. 회고를 잘 작성했나요?**
    - 이전 실습과 달리 이번에는 토큰화 방식과 하이퍼파라미터 비교에 더 초점을 두었고, 이를 자세히 분석한 과정이 만족스러웠다고 회고하고 있어요. DLThon 때에도 실험 설계를 되게 잘 하셨었는데 이번에도 역시 FM 스타일입니다. 

- [X]  **5. 코드가 간결하고 효율적인가요?**
    - 전처리, 번역, Attention 시각화, token 통계 계산 등이 각각 함수로 분리되어 있어 코드의 역할을 파악하기 쉽습니다.
    - 추가 실험에서 run_experiment()에 tokenizer별 sequence, vocabulary size, encoding 함수, Embedding/Hidden dimension 등을 parameter로 전달하도록 구성하여 Mecab/SentencePiece 실험과 Small/Medium/Large 실험에서 동일한 코드를 재사용하고 있습니다.
      <img width="789" height="578" alt="스크린샷 2026-09-10 오전 10 19 35" src="https://github.com/user-attachments/assets/9374ae12-781e-44b2-93ed-1af0ce029290" />



# 회고(참고 링크 및 코드 개선)
시온님의 프로젝트에서는 tokenizer 비교에 더해 Embedding/Hidden Size까지 Small, Medium, Large로 변경하여 모델의 capacity와 학습 속도 비교까지 완수하신 점이 멋집니다. 
깔끔하고 센스있는 프로젝트 문서 구성 덕분에 읽기에도 편했어요. 고생 많으셨습니다.!
