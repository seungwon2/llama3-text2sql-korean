# llama3-text2sql-korean

## 🔭 Overview

이 repo에는 Amazon Sagemaker와 Amazon Translate을 이용해 **llama3-8b 모델**을 **한국어 text to sql** 용도로 파인 튜닝하는 예제를 작성했습니다.

### 개발 환경

- Amazon Sagemaker Notebook instance (ml.t3.medium)
- `conda_pytorch_p310` 이용

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
├── setup.ipynb
├── notebook/
│   ├── 1_data_preprocessing.ipynb
│   ├── 2_fine_tuning.ipynb
│   └── 3_evaluation.ipynb
├── datasets/           # 한국어 번역 및 전처리가 완료된 데이터셋
│   ├── ko_test_dataset.json
│   └── ko_train_dataset.json
└── script/
```

프로젝트의 주요 구성 요소는 다음과 같습니다:

- [0_setup.ipynb](notebook/0_setup.ipynb): 초기 환경 설정을 위한 노트북
- [1_data_preprocessing.ipynb](notebook/1_data_preprocessing.ipynb): 데이터 전처리 과정
- [2_fine_tuning.ipynb](notebook/2_fine_tuning.ipynb): 모델 파인 튜닝 과정
- [3_evaluation.ipynb](notebook/3_evaluation.ipynb): 모델 평가 과정
