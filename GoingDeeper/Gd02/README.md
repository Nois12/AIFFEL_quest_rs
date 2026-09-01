# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김시온
- 리뷰어 : 조영근

# PRT(Peer Review Template)
- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 첫 부분에서 프로젝트의 목적과 요구사항을 명확히 제시하고 있습니다.
    - 카메라 이미지에서 사람을 감지하면 정지하고, 차량이 일정 크기 이상이면 정지하는 것을 목표로 정의했습니다.
    - 이후 단순히 모델을 선언하는 데 그치지 않고, KITTI 데이터셋 다운로드 및 구조 확인, 라벨 파싱, 학습용 전처리, RetinaNet 구현, 학습, 체크포인트 저장, 추론, 자율주행 보조 판정 함수, 테스트 채점까지 구현했습니다.
    - 특히 최종 시스템은 self_drive_assist(img_path, size_limit=280, person_size_limit=60) 형태로 이미지 경로를 입력받고 Stop 또는 Go를 반환하도록 구현되었으며, 10장의 테스트 이미지에 대 100점입니다.라는 결과를 출력했습니다.
    - <img width="572" height="555" alt="image" src="https://github.com/user-attachments/assets/ea7e468c-0afe-406e-86f4-013984c78d3b" />


- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 이 프로젝트에서 가장 복잡한 부분은 Anchor Box 생성, GT 박스와의 IoU 매칭, 그리고 타깃 인코딩입니다
    - AnchorBox 클래스와 LabelEncoder 클래스는 RetinaNet의 핵심 동작 원리를 담고 있습니다. 코더는 클래스와 함수 상단에 docstring을 작성하여 각 구성 요소의 역할을 설명했습니다.
    - <img width="571" height="262" alt="image" src="https://github.com/user-attachments/assets/b277ba5f-6f54-489b-ada3-08f89af0e46e" />
    - <img width="587" height="206" alt="image" src="https://github.com/user-attachments/assets/d3acf4b8-e9a7-41dc-9395-31fab2b6bac9" />


- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    - "트러블슈팅 기록"을 명시적으로 남겼습니다. GPU와 CPU 간의 device 불일치 문제, 체크포인트 저장 파일명 불일치 문제, Colab 환경에서의 경로 생성 문제를 원인과 해결책으로 나누어 기록했습니다.
    - 또한, 초기 테스트에서 80점이 나온 원인을 분석하고, 차량 크기 임계값(size_limit)을 300에서 280으로 완화하고 사람 크기 임계값(person_size_limit=60)을 추가하는 실험을 통해 100점을 달성했습니다.
    - <img width="937" height="446" alt="image" src="https://github.com/user-attachments/assets/828af776-7a6b-4fc1-9ad1-55898daa3185" />


- [x]  **4. 회고를 잘 작성했나요?**
    - 마지막 부분에 "회고" 섹션을 작성하여, 객체 탐지가 단순한 모델 호출이 아니라 전처리부터 NMS까지 이어지는 긴 파이프라인임을 깨달았다는 점을 기록했습니다.
    - 특히 전체 코드 실행 플로우를 텍스트 다이어그램으로 그려서 데이터 다운로드부터 최종 평가까지의 흐름을 한눈에 파악할 수 있도록 도왔습니다.
    - <img width="681" height="601" alt="image" src="https://github.com/user-attachments/assets/03d1a579-f317-4d78-80e3-7e9308019382" />


- [x]  **5. 코드가 간결하고 효율적인가요?**
    - 전처리(preprocess_data, resize_and_pad_image), 인코딩(LabelEncoder), 모델 구성(FeaturePyramid, Backbone, RetinaNet), 손실 함수(RetinaNetLoss), 추론(self_drive_assist) 등 각 기능이 독립적인 함수와 클래스로 잘 분리되어 있습니다. PyTorch의 nn.Module을 상속받아 구조적으로 코드를 작성했습니다.
    - <img width="536" height="129" alt="image" src="https://github.com/user-attachments/assets/a9770fb7-149d-4e17-a712-ac66f617ce98" />



# 회고(참고 링크 및 코드 개선)
```python
# 리뷰어의 회고를 작성합니다.
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
