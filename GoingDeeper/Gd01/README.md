# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김시온
- 리뷰어 : 박희지


# PRT(Peer Review Template)
- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    1) 인식결과의 시각화 및 성능 분석을 적절히 수행하였다.
       - `visualize_cam_on_image()`로 CAM과 Grad-CAM 모두 alpha blending overlay를 수행했다. Grad-CAM은 layer1~layer4까지 계층별로 나누어 시각화하여 층이 깊어질수록 활성 영역이 객체로 수렴하는 과정도 확인하였다.
         
         <img height="500" alt="스크린샷 2026-08-28 170721" src="https://github.com/user-attachments/assets/332a287f-c9a5-4906-9899-b137f0454a5e" />
         <img height="500" alt="스크린샷 2026-08-28 170735" src="https://github.com/user-attachments/assets/37ba4c97-a46f-44f8-8f27-4bdcfcaec41e" />
         
       - `get_bbox()`로 임계값 0.5 기준 활성 영역의 최소/최대 좌표를 추출하고, `visualize_both_bbox_on_image()`로 예측 박스와 정답 박스를 겹쳐 비교했다.
         
         <img height="500" alt="image" src="https://github.com/user-attachments/assets/b6a6cd6e-85d8-44fc-9ef9-52103dc576dd" />
       - `get_iou()` 구현 후 단일 샘플 비교 → 막대그래프 시각화 → valid set 10개 샘플 평균까지 계산했다.
         
         <img height="464" alt="image" src="https://github.com/user-attachments/assets/938fb31a-1efe-42c8-8b05-b13b2ec94724" />
         <img width="425" height="412" alt="image" src="https://github.com/user-attachments/assets/c5006b8b-505d-4366-ada3-07989a024d5e" />

    2) 분류근거를 설명 가능한 CAM을 얻었다.
       - `generate_cam()`은 layer4의 feature map을 확보하고, 예측 클래스에 해당하는 fc 가중치와 채널별 가중합을 계산한 뒤 ReLU와 min-max 정규화를 적용하여 생성하였다.
         
         <img height="500" alt="image" src="https://github.com/user-attachments/assets/34bcf9a9-425c-49b0-8be0-065ff26ed53d" />
       
       - `generate_grad_cam()`은 대상 레이어의 gradient를 받아 채널별 공간 평균을 가중치로 사용한다. `target_layer_name`을 인자로 받아 layer1~4에 모두 적용 가능하도록 일반화한 점이 좋다.
         
         <img height="700" alt="image" src="https://github.com/user-attachments/assets/58541879-86b2-413c-9e4d-7beb3d80d804" />
         
    3) CAM을 얻기 위한 기본모델의 구성과 학습이 정상적으로 진행되었다.
       - `resnet50`을 사용하였고, `ResNet50 + GAP + DenseLayer` 구조 요건을 충족하였다. train accuracy 22.18% → 65.48%로 상승하였고, loss/accuracy 곡선을 함께 기록·시각화하여 수렴을 확인할 수 있게 한 점이 좋다.
         
         <img width="1102" height="363" alt="image" src="https://github.com/user-attachments/assets/2e2563aa-46d0-41c0-8e14-4a97c1b1001c" />


- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 가장 이해 난이도가 높은 코드는 Grad-CAM 생성 함수이다. forward hook과 backward hook을 동시에 걸어 두 단계를 분리해서 이해해야 하며, gradient를 그대로 쓰는 것이 아니라 채널별 공간 평균을 가중치로 환산하는 단계가 Grad-CAM의 핵심 아이디어인데 코드만 보면 `torch.mean(grads, dim=(1,2))` 한 줄이라 이해하기가 어렵기 때문이다.
     
      해당 함수에 실행 단계마다 주석이 달려 있어서 코드를 이해하기 편했다. `torch.mean(grads, dim=(1,2))`에 "(weight 역할)"이라고 작성되어 이 값이 CAM의 FC 가중치를 대체하는 변수임을 알 수 있었다.
      
      <img height="700" alt="image" src="https://github.com/user-attachments/assets/9b3cfb2f-d742-4bec-9443-5ea6d4cb6a4a" />


- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    1) valid set 10개 샘플 평균 IoU로 확장
        seed로 재현성을 확보한 뒤 valid set에서 10개를 샘플링해 CAM/Grad-CAM IoU를 각각 계산하고 평균(0.2308)을 산출했다.단일 샘플 결과를 일반화하지 않고 평균 IoU를 계산한 점이 인상깊었다.
       
       <img  height="600" alt="image" src="https://github.com/user-attachments/assets/169427ea-754a-4734-b467-811207a04059" />
       
    3) 학습 곡선 기록
       `train_model()`이 epoch별 loss/accuracy를 history에 누적하도록 설계하고, 수렴 여부를 그래프로 확인할 수 있도록 epoch별 기록을 저장했다. 이후 Loss/Accuracy Curve를 시각화해 수렴 여부를 확인했습니다.


- [x]  **4. 회고를 잘 작성했나요?**
    - 결론에 valid set 10개 샘플의 평균 IoU가 CAM/Grad-CAM 모두 0.2308로 동일했다는 실험 결과를 명시하고, layer4 기준으로 두 기법이 유사한 영역에 주목했다고 해석을 적었다. 또한, 단일 샘플이 아닌 10개 평균으로 확인하니 값이 달라졌다는 관찰을 남기고 배운 점과 후속 방향을 작성했다.
      
      <img width="1086" height="203" alt="image" src="https://github.com/user-attachments/assets/6a6302b8-f46e-4d21-afbc-403472649c9b" />


- [x]  **5. 코드가 간결하고 효율적인가요?**
    - 재사용 가능한 단위로 코드가 잘 분리되어 있다.
        | 함수 | 역할 |
        |---|---|
        | `process_mat_file()` | .mat 기반 폴더 재구성 |
        | `StanfordDogsDatasetWithBBox` | bbox 파싱 및 스케일링 |
        | `train_model()` | 학습 |
        | `unnormalize()` | 이미지 복원 |
        | `generate_cam()` / `generate_grad_cam()` | CAM / Grad-CAM 생성 |
        | `visualize_cam()` / `visualize_cam_on_image()` | 단독 / overlay 시각화 |
        | `get_bbox()` / `get_iou()` | 바운딩 박스 추출 / IoU 계산 |
     - `generate_grad_cam` 함수에서 대상 레이어를 문자열로 받아 `dict`으로 조회하는 구조라, 함수 수정 없이 layer1 ~ layer4를 갈아끼우며 실험할 수 있도록 구성하여 확장성을 높였다.
       
       <img height="500" alt="image" src="https://github.com/user-attachments/assets/ad5e97b2-d2bc-4053-8067-a4d03592cb41" />



# 회고(참고 링크 및 코드 개선)
```
CAM과 Grad-CAM의 IoU가 완전히 일치한 이유에 대해 자세히 적어주셨으면 더욱 좋았을 것 같습니다.
저도 두 값이 동일하게 나와 확인해 봤는데, ResNet50은 마지막 conv 출력이 GAP를 거쳐 바로 FC로 이어지는 구조입니다. 이 경우 Grad-CAM이 역전파로 구하는 채널 가중치가 결국 CAM이 쓰는 FC 가중치와 같은 값에 도달하게 되고, 남는 스케일 차이도 마지막 min-max 정규화에서 사라집니다. 그래서 두 맵이 사실상 같은 결과가 되었다고 생각합니다.
```
