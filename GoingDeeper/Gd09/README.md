# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김시온
- 리뷰어 : 박희지


# PRT(Peer Review Template)
- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    1) 잠재적 표현의 변화가 모델 출력에 미치는 영향을 관찰하였다.
       - prompt_a/prompt_b 설정 후 (77,768) 임베딩 추출, torch.linspace(0,1,9)로 9단계 선형 보간, 고정 초기 노이즈로 순수 임베딩 변화만 관찰되도록 통제함.
       - 보간 이미지 그리드 시각화와 CLIP 유사도 곡선을 그리고 α=0.50에서 A/B 교차 지점을 정량적으로 발견함.
      
         <img width="1089" height="434" alt="image" src="https://github.com/user-attachments/assets/f79f63f3-3393-49be-a240-659faa481eba" />

    2) Stable diffusion 모델의 dreambooth 미세조정을 실습하였다.
       - raw 이미지 5장을 train 4 / valid 1로 분리해 저장한 후 `accelerate launch`로 실제 학습(prior preservation, class 이미지 100장 자동 생성, 400 step)을 실행함.
       - 학습된 파이프라인으로 대상이 담긴 이미지를 생성하고 원본 SD와 비교함.
      
         <img width="1097" height="1022" alt="image" src="https://github.com/user-attachments/assets/72ebd650-bc8e-4524-ba2f-2423f7d62ae4" />

    3) 취향이 담긴 생성 이미지를 만들었다.
       - LoRA 파일을 다운로드 후 `digiplay/hellofantasytime_v1.22` 체크포인트를 로드하고 LoRA를 적용해 파이프라인을 구축함.
       - LoRA scale별(0.0/0.4/0.8/1.2)로 이미지를 생성하고 격자로 비교함.
      
         <img width="1105" height="591" alt="image" src="https://github.com/user-attachments/assets/4e18ec0b-b888-4d34-89b1-336f6b19a4dc" />


- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 가장 복잡한 블록은 `ManualLoRA`라고 생각한다. `pipe.load_lora_weights()`가 실패한 뒤 LoRA 수식(`W' = W + scale · (α/r) · (up @ down)`) 자체를 텐서 연산으로 재구현한 블록이기 때문이다.
       - 클래스 상단에 주석으로 수식을 명시해 무엇을 하는 코드인지 바로 파악됨.
       - 버전 차이로 인해 무엇을 수정했는지를 작성해 이후에 동일한 key 불일치가 발생하더라도 코드만 보고 바로 따라할 수 있음.
       - apply() 메서드의 주석으로 scale을 바꿔가며 반복 호출해도 누적 오차 없이 매번 원본 가중치 기준으로 재계산한다는 설계 의도가 드러남.
       - 전부 인라인 # 주석으로만 설명되어 있어 한 줄 단위의 코드 동작은 이해할 수 있지만, 클래스나 함수 전체가 무엇을 위한 것인지에 대한 설명은 부족함. __init__처럼 인자·반환값·부작용이 있는 함수에는 """docstring"""이 있었다면 이해에 더 도움이 되었을 것 같음
      
         <img width="755" height="817" alt="image" src="https://github.com/user-attachments/assets/1201e3d5-bebe-48f4-a286-8ed80cb0af0c" />


- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 해결한 기록을 남겼거나
새로운 시도 또는 추가 실험을 수행해봤나요?**
    1) 디버깅 기록
       - `pipe.load_lora_weights(...)` 호출이 `IndexError: list index out of range`로 실패하자, `try/except`로 잡아 원인 설명(print(f"diffusers 로더 실패 ..."))을 출력하고, 파이프라인을 새로 로드해 ManualLoRA로 대체함.
       - `inspect_lora()`로 key 접두어를 확인해 LoHa/LoKr/SDXL 계열이 아닌 표준 LoRA인지 먼저 검증하고, 아니면 명확한 안내 메시지와 함께 RuntimeError를 던지는 방어 코드를 구현하여 사전 실패 가능성을 줄이도록 설계함

     <img width="963" height="533" alt="image" src="https://github.com/user-attachments/assets/5804a920-ef73-4d7b-a53b-e6444ac40eeb" />
       
    3) 추가 실험
       - CLIP-I/CLIP-T + "식별자 없는 일반 개" 기준선 비교 등 정량 평가를 진행하고 LoRA scale 별 비교를 수행함.
       - 기준선 비교는 valid/train 유사도 차이로 학습 대상을 단순히 외운 것인지를 검증하려는 점에서 좋았음.
    
         <img width="1096" height="524" alt="image" src="https://github.com/user-attachments/assets/0bcc40b2-eda3-476d-8672-36ba3fca1e5b" />


- [x]  **4. 회고를 잘 작성했나요?**
    - Part 1~3 각각에 대해 구체적 수치를 근거로 결과를 해석하였고, "학습 이미지를 외운 징후는 없었다" 등 결과에 대한 판단까지 제시하였다.
    - 패키지 설치 → 유틸 준비 → Part 1/2/3 → 결과 분석 순서를 실행 플로우를 제시하였다.
    - 회고에서 프로젝트에 집중하지 못한 아쉬움을 작성하였고, 공감되었다.
     
      <img width="1104" height="563" alt="image" src="https://github.com/user-attachments/assets/90438bff-e61f-40f2-8fa1-807e79c3707b" />


- [x]  **5. 코드가 간결하고 효율적인가요?**
    - `@dataclass CFG`로 seed/모델 경로/Part별 설정을 한 곳에 모아 관리하여 실험 조건 변경 시 CFG만 수정하면 되는 구조로 재현성과 가독성이 좋다.
    - `set_seed`, `free_gpu`, `load_pipe` 등 반복되는 작업이 모두 함수로 분리되어 파일 전체에서 재사용 가능하다. 특히, `show_grid`는 col/row/img 타이틀을 모두 지원하는 범용 시각화 함수로 잘 설계되었다.
     
      <img width="789" height="1087" alt="image" src="https://github.com/user-attachments/assets/8dcb4fbc-9c12-44b1-bd20-504d6d027677" />



# 회고(참고 링크 및 코드 개선)
```
Dreambooth에서 instance 이미지를 train/valid로 분리해 CLIP-I(valid)와 CLIP-I(train)를 따로 계산한 점이 인상깊었습니다.
두 값의 차이가 작다는 걸 근거로 "학습 이미지를 외운 게 아니라 대상의 특징을 일반화해서 학습했다"고 판단한 부분을 보고,
valid를 나누지 않으면 암기인지, 일반화인지 구분할 방법이 없다는 걸 깨달았습니다.
