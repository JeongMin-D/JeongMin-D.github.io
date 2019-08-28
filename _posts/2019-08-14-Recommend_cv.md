---
title: "5.이력서 자동추천 시스템"
date: 2019-08-14
categories: NLP Deep_Learning Word2vec tokenizer Recommender_System
---
# 이력서 자동추천 시스템
많은 양의 이력서 데이터를 사람이 모두 읽고 판단할 수 없다. 그렇기떄문에 이력서 데이터에 대해 AI가 미리 학습하고 사람에게 추천하는 시스템이 필요하다.
## 이력서 자동 추천 시스템 아이디어

### 1. 키워드 기반 자동 추천 시스템
각 부서별 원하는 인재상에 대한 키워드를 추출하여, 이력서의 키워드와 유사도를 비교하여 추천하는 시스템.

#### 장점
- 전체 이력서를 살펴볼 필요없이 키워드에 적합한 인재부터 확인하여 평가 할 수있음.

#### 단점
- 키워드 추출에 오차가 생기면 잘못 분류되는 이력서가 존재할 것.

#### 프로세스 처리 과정
- 사용자가 원하는 인재상을 키워드, 문장 등으로 입력하여 학습.
- 전체 이력서를 입력하여 키워드를 추출.
- 사용자 입력 키워드와 이력서 키워드의 유사도를 판단하여 유사도가 높은 순서대로 사용자에게 추천.

#### 사용 모듈
- pdfminer(pdf 파일 바로 읽기)
- [OCR 모듈](https://jeongmin-d.github.io/nlp/deep_learning/word2vec/tokenizer/recommender_system/OCR/)
- [TextRank](https://jeongmin-d.github.io/NLP_LInk/[summarize]TextRank.html)
### 2. 사용자 패턴 인식
사용자가 선택하는 이력서들의 키워드나 특성을 파악하여 그 패턴에 따라 사용자에게 이력서를 추천해주는 시스템.

#### 장점
- 사용자의 패턴을 분석하기 때문에 사용자에게 가장 적합한 이력서를 추천.

#### 단점
- 사용자의 패턴에 벗어나는 적합한 인재의 이력서를 필터링 할 수있음.

#### 프로세스 처리 과정
- 전체 이력서에 대한 특성,키워드 분석을 진행
- 사용자가 선택한 이력서들의 특성, 키워드를 학습.
- 다음 이력서를 선택할 때, 자동으로 패턴에 따른 이력서를 추천.

#### 사용 모듈
- pdfminer(pdf 파일 바로 읽기)
- [OCR 모듈](https://jeongmin-d.github.io/nlp/deep_learning/word2vec/tokenizer/recommender_system/OCR/)
- [TextRank](https://jeongmin-d.github.io/NLP_LInk/[summarize]TextRank.html)
- Word2vec

### 3. 기존 합격자 이력서 데이터 학습 기반 추천 시스템
기존 합격자들의 데이터를 분석하여 비슷한 패턴을 가진 이력서들을 우선순위로 추천.

#### 장점
- 기존 합격자의 데이터를 활용하기 때문에 가장 기업에 적합한 인재를 추천.

#### 단점
- 기존 합격자 데이터를 사용하는 것이 어려움.
- 새로운 특성을 가진 인재를 추천하는 것이 어려움.

#### 프로세스 처리 과정
- 기존 합격자들의 이력서에 필요 데이터를 추출.
- 딥러닝을 통한 패턴 학습.
- 학습한 패턴에 따른 이력서 추천.

#### 사용 모듈
- pdfminer(pdf 파일 바로 읽기)
- [OCR 모듈](https://jeongmin-d.github.io/nlp/deep_learning/word2vec/tokenizer/recommender_system/OCR/)
- [TextRank](https://jeongmin-d.github.io/NLP_LInk/[summarize]TextRank.html)
- [Word2vec](https://jeongmin-d.github.io/nlp/deep_learning/word2vec/Word2vec/)
- opencv-python

### 2. 요약문, 키워드 추출 모듈
- [TextRank](https://jeongmin-d.github.io/NLP_LInk/[summarize]TextRank.html)
- [LexRankr](https://jeongmin-d.github.io/NLP_LInk/[summarize]LexRankr.html)
- [gensim_summarizer](https://jeongmin-d.github.io/NLP_LInk/[summarize]gensim_summarizer.html)
