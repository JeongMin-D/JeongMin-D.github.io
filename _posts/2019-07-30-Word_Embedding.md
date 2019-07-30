---
title: "3.Word_Embedding"
date: 2019-07-30
categories: NLP Deep_Learning Word2vec GloVe fasttext kor2vec ELMo
---

# Word Embedding 비교

## word2vec
[word2vec 설명 및 source code](https://jeongmin-d.github.io/nlp/deep_learning/word2vec/Word2vec/)
* Apache License
*	단어를 계산에 적용할 수 있는 숫자로 변환해서 다음에 올 단어를 예측하는 모델.
*	다음 단어를 예측하기 위해 Word Embedding을 구현해야 하는데, CBOW 또는 Skip-Gram 알고리즘을 사용.
*	기존의 알고리즘에 비해 자연어 처리에서 엄청난 향상을 가져왔다.

## Glove

### 개념
* Apache License
*	미국 스탠포드대학에서 개발한 word embedding 방법론 중 하나.
*	카운트 기반의 LSA와 예측 기반의 word2vec의 단점을 언급하고, 단점을 보완한다는 목적을 가지고 나온 모델.
*	임베딩된 두 단어벡터의 내적이 말뭉치 전체에서의 동시 등장확률 로그값이 되도록 목적함수를 정의.

## fasttext

### 개념
* MIT License
*	페이스북에서 개발한 모델.
*	Word2vec 이후에 나온 모델이기 때문에 매커니즘 자체는 word2vec의 확장.
*	Word2vec은 단어를 쪼개질 수 없는 단위로 생각하였지만, fasttext는 단어안의 내부 단어를 고려하여 학습.
*	단어에 대해 n만큼 분리하여 임베딩. 그래서 모르는 단어 또는 빈도 수가 적었던 단어에 대한 대응이 word2vec보다 정확도가 높은 경향을 보임.

## kor2vec

### 개념
* Apache License
* Copyright 2018 NAVER.Corp
*	OOV 없이 빠르고 정확한 한국어 Embedding
*	Word2vec의 문제점을 해결하기 위해 CNN 기반으로 한 char-word 임베딩을 한국어에 적용하여 만듬.
*	Embedding 학습방법: Skip-gram based embedding training
*	Char-word Encoder 모델 구조: [Yoon Kim`s Character-Aware Neural Language Modeling]( https://arxiv.org/abs/1508.06615)

## Bert

### 개념
* Apache License
* 구글의 새로운 Language Representation Model.
* NLP AI의 최첨단 딥러닝 모델.
* 언어표현 사전학습의 새로운 방법으로 그 의미는 큰 텍스트 코퍼스를 이용하여 범용목적의 언어이해 모델을 훈련시키는 것.
* 자연언어 처리 태스크를 교육 없이 양방향으로 사전학습하는 첫 시스템.
* word2vec이나 GloVe와 같이 문맥에 의존하지 않는 모델에서는, 어휘에 포함되는 각 단어마다 단어 삽입이라는 특징 표현을 생성.
