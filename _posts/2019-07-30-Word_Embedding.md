---
title: "3.Word_Embedding"
date: 2019-07-30
categories: NLP Deep_Learning Word2vec GloVe fasttext kor2vec ELMo
---

# Word Embedding 비교
## word2vec
[word2vec 설명 및 source code](https://jeongmin-d.github.io/nlp/deep_learning/word2vec/Word2vec/)
## Glove
### 개념
*	미국 스탠포드대학에서 개발한 word embedding 방법론 중 하나.
*	카운트 기반의 LSA와 예측 기반의 word2vec의 단점을 언급하고, 단점을 보완한다는 목적을 가지고 나온 모델.
*	임베딩된 두 단어벡터의 내적이 말뭉치 전체에서의 동시 등장확률 로그값이 되도록 목적함수를 정의.
## fasttext
### 개념
*	페이스북에서 개발한 모델.
*	Word2vec 이후에 나온 모델이기 때문에 매커니즘 자체는 word2vec의 확장.
*	Word2vec은 단어를 쪼개질 수 없는 단위로 생각하였지만, fasttext는 단어안의 내부 단어를 고려하여 학습.
*	단어에 대해 n만큼 분리하여 임베딩. 그래서 모르는 단어 또는 빈도 수가 적었던 단어에 대한 대응이 word2vec보다 정확도가 높은 경향을 보임.
## kor2vec
### 개념
*	OOV 없이 빠르고 정확한 한국어 Embedding
*	Word2vec의 문제점을 해결하기 위해 CNN 기반으로 한 char-word 임베딩을 한국어에 적용하여 만듬.
*	Embedding 학습방법: Skip-gram based embedding training
*	Char-word Encoder 모델 구조: [Yoon Kim`s Character-Aware Neural Language Modeling]( https://arxiv.org/abs/1508.06615)
## ELMo
### 개념
*	기학습된 언어 모델을 이용해 어휘 임베딩을 생성하는 방법.
*	여러 레이어로 된 양방향 순환신경망 언어 모형을 사용.
*	임베딩을 사용할 때도 해당 벡터가 주변 단어에서 추론된 맥락 정보에 따라 가변적일 수 있도록 만들었다는 의미에서 맥락화된 어휘 임베딩이라는 표현을 씀.
*	ELMo는 언어 모델로써 양방향 LSTM(biLM)을 이용. 순방향은 주어진 문장에서 시작부터 n개의 단어를 보고, n+1번쨰 단어를 맞추게 됨. 역방향은 역방향으로 n개의 단어를 보고 n-1번쨰 단어를 맞추게 됨.
*	다른 모델에 사용될 어휘 임베딩 벡터를 구성하기 위해 이 모든 정보를 종합하여 사용. 기존 어휘 임베딩, 순방향 LTSM의 잠재 상태, 역방향 LTSM의 잠재 상태를 모두 종합하여 붙인 벡터가 ELMo 임베딩이 됨.
