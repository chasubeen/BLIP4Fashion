# **👗 BLIP4Fashion 👟**

## **1. 프로젝트 소개**
- 해당 프로젝트는 패션 이미지 캡션 생성의 성능을 크게 향상시키기 위해 `BLIP` 모델을 fine-tuning 하는 것을 주요 목표로 삼았다.
  - [BLIP-Original Paper](https://arxiv.org/abs/2201.12086)
- 패션 도메인의 특수성을 고려하여, 다양한 패션 스타일 및 액세서리, 의류 아이템을 포함한 고품질의 데이터셋을 구축하였고, 이를 활용해 모델을 학습시켰다.
  - 이 과정에서 모델이 패션 관련 어휘와 문맥을 깊이 있게 이해하고 이를 바탕으로 더욱 정확하고 맥락에 맞는 캡션을 생성할 수 있도록 유도하였다.
  - [Related Paper](https://arxiv.org/abs/2008.02693)
- 성능 평가는 BLEU, METEOR, CIDEr 등 다양한 평가 지표를 사용하여 진행되었으며, 이를 통해 생성된 캡션의 품질을 객관적으로 평가하고 개선하였다.
- 단순히 기술적인 성과에 그치지 않고, e-커머스 플랫폼에서의 제품 설명 자동화 및 디지털 패션 매거진의 콘텐츠 생성, 그리고 패션 관련 데이터의 효율적인 관리에 있어 실질적인 가치를 제공하고자 하였다.

## **2. 참여자**
|**팀원 1**|**팀원 2**|**팀원 3**|**팀원 4**|**팀원 5**|
|:----------:|:----------:|:----------:|:----------:|:----------:|
|<img src="" width = 100 height = 100>|<img src = "" width = 100 height = 100>|<img src = "" width = 100 height = 100>|<img src = "" width = 100 height = 100>|<img src = "https://avatars.githubusercontent.com/u/98953721?v=4" width = 100 height = 100>|    
|[방선유](https://github.com/syou-b)|[배주원](https://github.com/baejuwon-30)|[장윤서](https://github.com/jy00nse0)|[손소현](https://github.com/sonso1598)|[차수빈](https://github.com/chasubeen)|


## **3. 진행 과정**
### **기간**
- 2024.06.29(토) ~ 2024.08.20(화)  
### **세부 일정**  
<img src = "https://github.com/user-attachments/assets/7294d6ac-df55-4b1c-a748-5dbd13753bac" width = 800 height = 400>  

### **역할**
  
|**이름**|**역할**|
|:-----:|:----------:|
|방선유|선행 연구자료 조사, 모델링 - Trial 2, 추론|
|배주원|선행 연구자료 조사, 데이터셋 탐색, 모델링 - Trial 3|
|장윤서|선행 연구자료 조사, 모델링 - Trial 2, 학습 최적화 방향 설정|
|손소현|주제 구체화, 데이터셋 탐색, 데이터 전처리, 모델링 - Trial 1|
|차수빈|목표 및 방향성 설정, 데이터 전처리, 모델링 baseline 설정, 실험 설계(목적 함수 개선), 모델링 - Trial 4|

## **4. 파일 구조**
- [data - Google Drive](https://drive.google.com/drive/folders/1Jn6F0IrIfIhWk8XlFVC-B80lstcrb2Fk?usp=sharing)

```
├── BLIP4Fashion
│   ├── 1. Preprocessing.ipynb (데이터 전처리 파일)
│   ├── 2. Modeling
│   |   ├── modeling_trial1.ipynb
│   │   ├── modeling_trial2.ipynb
│   │   ├── modeling_trial3.ipynb
│   │   └── modeling_trial4.ipynb
│   ├── 3. Inference
│   |   ├── BLIP_inference_trial1.ipynb
│   │   ├── BLIP_inference_trial1.ipynb
│   │   ├── BLIP_inference_trial1.ipynb
│   │   └── BLIP_inference_trial1.ipynb
│   └── README.md
```

## **5. 프로젝트 흐름 및 사용 기술**
### **1) 데이터셋 준비 및 전처리**
- FACAD 패션 이미지 데이터셋 활용
- Train: Validation: Test = 10,000: 1,000: 2,000
- 이후 이미지 파일 및 annotation 파일을 전처리하여 COCO Captions 포맷에 맞게 json 파일로 변환

### **2) 모델 설정**
- BLIP(ViT-L) 모델 사용
- parameter settings
  - batch_size = 16
  - optimizer = AdamW
  - caption max_length = 50
  - epochs = 10 

### **3) Fine-Tuning methodology**
- PEFT(Parameter-Efficient Fine-Tuning): 메모리 및 계산 자원 절약
- LoRA(Low-Rank Adaptation): 행렬의 랭크를 줄여 효율적 학습 도모

### **4) Modeling Settings**
- Trial 1 vs 2: 텍스트 디코더만 풀어서(encoder freezing) 튜닝 진행
  - Trial 1: 강화학습 메트릭(RL metric) 없이 학습
  - Trial 2: 강화학습 메트릭을 포함하여 학습
- Trial 3 vs 4: 텍스트 디코더 + 이미지 인코더의 마지막 레이어 6개를 풀어 파인 튜닝
  - Trial 3: 강화학습 메트릭 없이 학습
  - Trial 4: 강화학습 메트릭을 포함하여 학습

### **5) 평가 및 결과 분석**
- 평가 지표: BLEU, METEOR, ROUGE-L, CIDEr, SPICE 활용
  - 일반화 성능: RL metric을 사용하지 않은 Trial이 더 나은 일반화 성능을 나타냄.
 - RL Metrics
   - ALS(Attribute-Level Score): 생성된 캡션과 정답 캡션 간 속성 일치 비율 평가
   - SLS(Semantic-Level Score): 생성된 캡션이 정답 카테고리를 반영하는지 평가
 - Trial 결과 비교)
   - Trial 1 vs Trial 3: 디코더만 튜닝한 경우보다 이미지 인코더도 일부 튜닝한 경우 더 좋은 성능을 보임
   - Trial 2 vs Trial 4: RL metric을 적용한 경우가 패션 캡셔닝에서 필요한 특성들을 더 잘 반영함


## **6. 프로젝트 관련 자료**
- [프로젝트_Notion 페이지](https://euron6th.notion.site/fed2e1e55ca8401e8c715357b20dbe0c?pvs=4)
- [발표자료]()

---
## **회고**
### **방선유**
- 
-

### **배주원**
- 
-

### **장윤서**
- 
- 

### **손소현**
- 
- 

### **차수빈**
- 이번 프로젝트는 멀티모달 관련하여 진행한 첫 연구였기에 더욱 뜻깊었다.
  - 텍스트와 이미지를 동시에 다루는 작업은 이전에 경험하지 못한 새로운 도전이었고, 특히 다양한 데이터를 통합하여 모델을 개선하는 과정이 매우 흥미로웠다.
- 프로젝트를 진행하면서 Transformer 기반의 BLIP 모델을 모든 레이어에서 파인 튜닝하는 데 상당한 메모리와 계산 자원이 필요하다는 점에서 어려움을 겪었다. 학부생 수준의 연구 환경에서는 이러한 자원적 한계가 큰 도전이었다.
  - 이 문제를 해결하기 팀원들과 논의 중 PEFT와 LoRA 같은 파라미터 효율적 미세조정 기법을 새로 알게 되었고, 이를 적용함으로써 자원 효율성을 유지하면서도 높은 성능을 유지하는 모델 튜닝이 가능하다는 것을 확인할 수 있었다.
  - 이러한 수정 과정을 통해 자원 효율적인 모델 튜닝을 달성할 수 있었고, 프로젝트 진행 시 자원 관리 또한 주요하게 신경써야 할 부분인 것을 다시 한 번 깨닫게 된 계기였다.
- 또한 패션 도메인에 맞춘 데이터 준비와 전처리가 모델 성능 향상에 중요한 역할을 했음을 깨닫게 되었고, 이 경험을 통해 도메인 지식과 기술의 결합이 전문적인 작업을 수행하는 데 필수적임을 느낄 수 있었다.

