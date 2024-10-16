# llama3-text2sql-korean

## 🔭 Overview

- 이 repo에는 Amazon Sagemaker를 이용해 **llama3-8b 모델**을 **한국어 호환 text to sql** 용도로 파인 튜닝하는 예제를 작성했습니다.

- Amazon Bedrock Imported Models에 파인 튜닝을 완료한 모델을 업로드하고 모델의 성능을 평가하는 방식도 보실 수 있습니다.

### 개발 환경

- Amazon Sagemaker Notebook instance (ml.t3.medium)
- `conda_pytorch_p310` 이용
- Amazon Bedrock Imported Models 이용 추론

### 사용 모델 및 데이터 셋

- 사용 모델: [Meta-Llama-3-8B](https://huggingface.co/meta-llama/Meta-Llama-3-8B)
- 사용 데이터 셋: [b-mc2/sql-create-context](https://huggingface.co/datasets/b-mc2/sql-create-context)

#### 데이터 셋 예시

```
Answer: SELECT COUNT(*) FROM head WHERE age > 56
Question: How many heads of the departments are older than 56 ?
Context: CREATE TABLE head (age INTEGER)
```

- Amazon Translate 이용하여 user question을 한국어로 번역한 후 사용

#### 번역 및 전처리 예시

```
{"messages":[{"content":"You are a powerful text-to-SQL model. Your job is to answer questions about a database.You can use the following table schema for context: CREATE TABLE table_name_52 (bronze VARCHAR, rank VARCHAR, total VARCHAR, nation VARCHAR)","role":"system"},{"content":"Return the SQL query that answers the following question: 총 합계가 3보다 작고 폴란드 국가이고 순위가 4보다 큰 브론즈의 총계는 얼마입니까?","role":"user"},{"content":"SELECT COUNT(bronze) FROM table_name_52 WHERE total < 3 AND nation = \"poland\" AND rank > 4","role":"assistant"}]}

```

## 🗂️ 디렉토리 구조

```text

.
├── README.md
├── datasets        # 전처리가 완료된 full 데이터셋
│   ├── ko_test_dataset.json
│   └── ko_train_dataset.json
├── images          # 실습 가이드용 이미지 셋
│
├── notebook        # 실습 수행용 파이썬 노트북
│   ├── 0_setup.ipynb
│   ├── 1-1_full_data_preprocessing.ipynb # 프로덕션 용 full 데이터 셋
│   ├── 1_data_preprocessing.ipynb        # 실습용 small sample 데이터 셋
│   ├── 2_fine_tuning.ipynb
│   ├── 3_deploy.ipynb
│   └── 4_evaluation.ipynb
└── scripts         # 파인 튜닝에 필요한 스크립트
    ├── requirements.txt
    └── run_fsdp_qlora_llama3.py

```

프로젝트의 주요 구성 요소는 다음과 같습니다:

- [0_setup.ipynb](notebook/0_setup.ipynb): 초기 환경 설정 과정
- [1_data_preprocessing.ipynb](notebook/1_data_preprocessing.ipynb): 데이터 전처리 과정 (small sample 데이터 이용, 프로덕션 용 full 데이터 셋은 [1-1_full_data_preprocessing.ipynb](notebook/1-1_full_data_preprocessing.ipynb) 노트북 이용)
- [2_fine_tuning.ipynb](notebook/2_fine_tuning.ipynb): 모델 파인 튜닝 과정
- [3_deploy.ipynb](notebook/3_deploy.ipynb): Amazon Bedrock에 모델 배포 과정
- [4_evaluation.ipynb](notebook/4_evaluation.ipynb): 모델 평가 과정

## 📝 References

- [SageMaker 에서 Llama3.1 8B 파인 튜닝, 모델 배포 및 추론 하기](https://github.com/aws-samples/aws-ai-ml-workshop-kr/tree/master/genai/aws-gen-ai-kr/30_fine_tune/03-fine-tune-llama3/llama3-1)
- [Amazon Bedrock Samples](https://github.com/aws-samples/amazon-bedrock-samples/blob/main/custom_models/import_models/llama-3/customized-text-to-sql-model.ipynb)
- [Import a fine-tuned Meta Llama 3 model for SQL query generation on Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/import-a-fine-tuned-meta-llama-3-model-for-sql-query-generation-on-amazon-bedrock/)
