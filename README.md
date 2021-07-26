# KoBERT_DIS_NER

KoBERT + CRF를 이용한 개체명 인식 모델.

**KoBERT**와 **CRF**를 이용해서 **음절단위**로 학습시켰으며  질병 명을 인식하는 것을 목표로 만듬.



# 시스템 개요

먼저, 질병명 인식 모델을 만들게 된 이유는 반려동물 예진 시스템을 만들기 위해서 만들어짐. 

 사용자가 반려동물의 이상 행동에 대한 상태(문장)를 입력하면 입력받은 상태와 유사한 사례의 답변을 통해 반려동물의 상태를 간단하게 예진 가능하게 하는 것을 목표로 하는 시스템. 

KoBERT_DIS_NER은 사용자가 입력한 문장과 유사한 사례 중 질병이 포함된 문장을 찾아서 인터페이스에 띄우는 것이 목표.



# 데이터 수집

- 데이터 출처: 네이버 지식 iN 전문의 답변 Q&A

  |      |  사람   |  동물  |
  | :--: | :-----: | :----: |
  | 학습 | 303,227 | 15,732 |
  | 검증 | 102,370 | 2,757  |

## 데이터 예시

### 태그

```
{
	"[CLS]": 1,
	"[SEP]": 2,
	"[PAD]": 0,
	"[MASK]": 3,
	"O": 4,
	"B-DIS": 5,
	"I-DIS": 6
}
```

> tog_to_index: 1, 4, 4, 4, 4, 4, 4, 4, 5, 6, 4, 4, 4, 4, 4, 4, 2
>
> sent: 선홍색출혈은항문출혈입니다.



# 학습

KoBERT_DIS_NER은 동물 질병명 인식을 위해 만들기 시작했지만, 데이터 수가 부족 약 1만 8천 문장으로 부족.

부족한 데이터의 해결방안으로 **사람 질병 문장**을 이용.

동물과 사람의 질병명이 유사한 것을 이용.

1. 사람 질병 문장을 이용하여 모델 학습
2. 이후 동물 질병 문장을 이용해서 추가 학습

사람 질병의 경우, 학습 셋의 약 47,700문장을 학습 시킴.

 -> 그 이후로 학습을 진행할 경우 loss는 상승하고 acc는 하락하는 추새를 보였기 때문.

동물 질병의 경우 학습 셋을 모두 학습 시킴.



# 정확도

**Seqeval**의 F1_Score 적용.

|          | 추가학습 전 | 추가학습 후 |
| -------- | ----------- | ----------- |
| F1_Score | 0.71        | 0.94        |

추가학습을 했을 때 약 0.23정도 정확도 상승.

# 후기
이번 개체명 인식을 만들며 느낀 점은 동물 질병 문장이 많이 없어서 검증을 해도 믿을만한 정보인가 확신이 서지 않았다. 그 부분에 대해 아직도 아쉬움이 남아있는 것 같다. 

하지만 사람 질병 문장을 이용했을 때 동물 질병 개체명 인식에 유의미한 결과가 있다는 것이 이번 모델을 만들면서 의미 있는 부분이 아니었나 싶다.

# 모델

- [DIS_NER](https://drive.google.com/drive/folders/16ie8dPyEVzzcDFEFRyPfcI8SdSBMi3g6?usp=sharing)

# Reference Repo

1. [KoBERT](https://github.com/SKTBrain/KoBERT)
2. [KoBERT_CRF_NER](https://github.com/eagle705/pytorch-bert-crf-ner#reference-repo)
3. [KoCharELECTRA](https://github.com/monologg/KoCharELECTRA)
