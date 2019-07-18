
# [NLP_Engine]1_Koalanlp
#### https://koalanlp.github.io/koalanlp/

## 0. window 사전설치

1. cmd를 이용하여 Chocolately 설치
    - cmd를 열고 아래 명령 입력.
     ```cmd
     @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
     ```
2. java JDK 설치
    - 해당 cmd 창에 아래 명령 입력.
     ```cmd
     choco install openjdk --version 12.0.0 -y
     ```
3. python3.6.8 설치
    - 해당 cmd에 아래 명령 입력.
    ```cmd
     choco install python --version 3.6.8 -y
    ```

## 1. pip를 이용한 koalanlp 패키지 설치


```python
!pip install koalanlp
```

    Requirement already satisfied: koalanlp in c:\users\bowlmin\anaconda3\envs\py35\lib\site-packages (2.0.9)
    Requirement already satisfied: py4j~=0.10 in c:\users\bowlmin\anaconda3\envs\py35\lib\site-packages (from koalanlp) (0.10.8.1)
    Requirement already satisfied: jip~=0.9.13 in c:\users\bowlmin\anaconda3\envs\py35\lib\site-packages (from koalanlp) (0.9.13)
    Requirement already satisfied: requests in c:\users\bowlmin\anaconda3\envs\py35\lib\site-packages (from jip~=0.9.13->koalanlp) (2.22.0)
    Requirement already satisfied: idna<2.9,>=2.5 in c:\users\bowlmin\anaconda3\envs\py35\lib\site-packages (from requests->jip~=0.9.13->koalanlp) (2.8)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\bowlmin\anaconda3\envs\py35\lib\site-packages (from requests->jip~=0.9.13->koalanlp) (2018.8.24)
    Requirement already satisfied: chardet<3.1.0,>=3.0.2 in c:\users\bowlmin\anaconda3\envs\py35\lib\site-packages (from requests->jip~=0.9.13->koalanlp) (3.0.4)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in c:\users\bowlmin\anaconda3\envs\py35\lib\site-packages (from requests->jip~=0.9.13->koalanlp) (1.25.3)
    

    You are using pip version 10.0.1, however version 19.1.1 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.
    

## 2. 패키지 초기화


```python
from koalanlp.Util import initialize, finalize # 패키지 안에 초기화 모듈 import
```


```python
initialize(java_options='-Xmx4g', HNN='2.0.5',KMR='2.0.5',
           EUNJEON='2.0.5',ARIRANG='2.0.5',DAON='2.0.5',OKT='2.0.5',
           KKMA='2.0.5',ETRI='2.0.5')
# 코모란, 은전한닢, 아리랑, RHINO, DAON, Okt, 꼬꼬마, 한나눔, ETRI 중 필요한 형태소 분석기 설치 및 초기화.
```

    [1mpy4j.java_gateway[0m Callback Server Starting
    [1mpy4j.java_gateway[0m Socket listening on ('127.0.0.1', 25334)
    [1mjip[0m JVM initialization procedure is completed.
    

- ETRI의 경우 사용권 조항에 동의하고 키를 발급하여야 사용 가능.

## 3. 기초 사용법


```python
# 테스트용 텍스트 (위키피디아: 자연어 처리)
text = "자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다. 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다. 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다. 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다."
```

### 3-1 문단에서 문장 분리하기
- 품사 태깅을 거치지 않은 문장 분리는, 글의 표면 형태만을 토대로 문장을 나누는 것으로, 한나눔과 Okt 분석기만 지원됩니다.
- SentenceSplitter: 문장분리기를 생성.
- sentencesTagged: KoalaNLP가 구현한 문장분리기를 사용하여, 문단을 문장으로 분리.
- Tagger: 품사분석기를 초기화.
- tagSentence: 문장을 품사분석(인자 하나를 문장 하나로 간주)


```python
from koalanlp import API
from koalanlp.proc import SentenceSplitter
# koalanlp 안의 필요한 모듈 불러오기
```

#### 한나눔 api 사용


```python
splitter_HNN = SentenceSplitter(API.HNN) # 한나눔 api를 불러서 문장 분리기 생성.
```


```python
paragraph_HNN = splitter_HNN(text) # 문장 분리
```


```python
print(paragraph_HNN[0]) # 인덱스에 해당하는 문장을 출력
```

    자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다 . 
    


```python
print(paragraph_HNN[1]) # 인덱스에 해당하는 문장을 출력
```

    자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다 . 
    

#### Okt api 사용


```python
splitter_OKT = SentenceSplitter(API.OKT) # OKT api를 불러서 문장 분리기 생성.
```


```python
paragraph_OKT = splitter_OKT(text) # 문장 분리
```


```python
print(paragraph_OKT[0]) # 인덱스에 해당하는 문장을 출력
```

    자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다.
    


```python
print(paragraph_OKT[1]) # 인덱스에 해당하는 문장을 출력
```

    자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다.
    

#### 품사 부착 후 분리하는 방법


```python
from koalanlp.proc import SentenceSplitter, Tagger # koalanlp 안의 필요한 모듈 불러오기
```

#### HNN api 사용


```python
tagger_HNN = Tagger(API.HNN) # koalanlp에서 제공하는 api 입력.
```


```python
tagged_sentence_HNN = tagger_HNN.tagSentence(text)
print(tagged_sentence_HNN)
# 품사분석을 수행할 문단 입력
```

    [자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다 . 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다 . 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다 . 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다 .]
    


```python
paragraph_HNN = SentenceSplitter.sentencesTagged(tagged_sentence_HNN[0])
# tagged_sentence는 각 인자별로 한 문장으로 간주된 List[Sentence]임.
```


```python
print(paragraph_HNN[0])
```

    자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다 .
    

#### Okt api 사용


```python
tagger_OKT = Tagger(API.OKT) # koalanlp에서 제공하는 api 입력.
```


```python
tagged_sentence_OKT = tagger_OKT.tagSentence(text)
print(tagged_sentence_OKT)
# 품사분석을 수행할 문단 입력
```

    [자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다. 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다. 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다. 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다.]
    


```python
paragraph_OKT = SentenceSplitter.sentencesTagged(tagged_sentence_OKT[0])
# tagged_sentence는 각 인자별로 한 문장으로 간주된 List[Sentence]임.
```


```python
print(paragraph_OKT[0])
```

    자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다.
    

### 3-2 품사 분석하기
- 문장 또는 문담을 부넉해서 품사를 부착할 수 있음.
- 결과물은 Sentence 객체가 됨.
- 문단을 기준으로 분석할 때: tag
- 문장을 기준으로 분석할 때: tagSentence
- singleLineString(): 품사분석 결과를, 1행짜리 String으로 변환.


```python
from koalanlp import API
from koalanlp.proc import Tagger
# 필요한 모듈 import
```

#### 은전한닢 api 사용


```python
tagger_EJ = Tagger(API.EUNJEON) # 은전한닢 api 불러오기
```


```python
taggedParagraph_EJ = tagger_EJ(text) # 문단을 분석해서 문장들로 얻기
print(taggedParagraph_EJ)
```

    [자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할 수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나 다 ., 자연 언어 처리는 연구 대상이 언어이 기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어인지 과학과 연관이 깊다 ., 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계 학습 도구를 많이 사용하는 대표적인 분야이 다 ., 정보 검색 , QA 시스템 , 문서 자동 분류 , 신문 기사 클러스터링 , 대화형 Agent 등 다양한 응용이 이루어 지고 있다 .]
    


```python
print(taggedParagraph_EJ[0].singleLineString()) # 첫번째 문장의 품사분석 결과 출력
```

    자연/NNG+어/NNG 처리/NNG 또는/MAJ 자연/NNG 언어/NNG 처리/NNG+는/JX 인간/NNG+의/JKG 언어/NNG 현상/NNG+을/JKO 컴퓨터/NNG+와/JKB 같/VA+은/ETM 기계/NNG+를/JKO 이용/NNG+하/XSV+아서/EC 모사/NNG 하/VV+ᆯ/ETM 수/NNB 있/VV+도록/EC 연구/NNG+하/XSV+고/EC 이/NP+를/JKO 구현/NNG+하/XSV+는/ETM 인공/NNG+지능/NNG+의/JKG 주요/NNG 분야/NNG 중/NNB 하나/NR 다/EF ./SF
    

#### 한나눔 api 사용


```python
tagger_HNN = Tagger(API.HNN) # 한나눔 api 불러오기
```


```python
taggedParagraph_HNN = tagger_HNN(text) # 문단을 분석해서 문장들로 얻기
print(taggedParagraph_HNN)
```

    [자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다 ., 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다 ., 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다 ., 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다 .]
    


```python
print(taggedParagraph_HNN[0].singleLineString()) # 첫번째 문장의 품사분석 결과 출력
```

    자연어/NNG 처리/NNG 또는/MAJ 자연/NNG 언어/NNG 처리/NNG+는/JX 인간/NNG+의/JKG 언어/NNG 현상/NNG+을/JKO 컴퓨터/NNG+와/JC 같/VA+은/ETM 기계/NNG+를/JKO 이용/NNG+해서/NNG 모사/NNG 하/VV+ㄹ/ETM+수/NNB 있/VA+도록/EC 연구/NNG+하고/JC 이/NNG+를/JKO 구현/NNG+하/XSV+는/ETM 인공지능/NNG+의/JKG 주요/NNG 분야/NNG 중/NNB 하나/NR+이/VCP+다/EF ./SF
    

#### 코모란 api 사용


```python
tagger_KMR = Tagger(API.KMR) # 코모란 api 불러오기
```


```python
taggedParagraph_KMR = tagger_KMR(text) # 문단을 분석해서 문장들로 얻기
print(taggedParagraph_KMR)
```

    [자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용하여서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다., 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다., 구현을 위하여 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다., 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이뤄 지고 있다.]
    


```python
print(taggedParagraph_KMR[0].singleLineString()) # 첫번째 문장의 품사분석 결과 출력
```

    자연어/NNP 처리/NNG 또는/MAJ 자연 언어 처리/NNP+는/JX 인간/NNG+의/JKG 언어/NNG 현상/NNG+을/JKO 컴퓨터/NNG+와/JC 같/VA+은/ETM 기계/NNG+를/JKO 이용/NNG+하/XSV+아서/EC 모사/NNG 하/VV+ㄹ/ETM+수/NNB 있/VV+도록/EC 연구/NNG+하/XSV+고/EC 이/NP+를/JKO 구현/NNG+하/XSV+는/ETM 인공지능/NNP+의/JKG 주요/XR 분야/NNG 중/NNB 하/VX+나다/EF+./SF
    

#### 꼬꼬마 api 사용


```python
tagger_KKMA = Tagger(API.KKMA) # 꼬꼬마 api 불러오기
```


```python
taggedParagraph_KKMA = tagger_KKMA(text) # 문단을 분석해서 문장들로 얻기
print(taggedParagraph_KKMA)
```

    [자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할 수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다., 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다., 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다., 정보 검색, QA 시스템, 문서 자동 분류, 신문기사 클러 스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다.]
    


```python
print(taggedParagraph_KKMA[0].singleLineString()) # 첫번째 문장의 품사분석 결과 출력
```

    자연어/NNG 처리/NNG 또는/MAG 자연/NNG 언어/NNG 처리/NNG+는/JX 인간/NNG+의/JKG 언어/NNG 현상/NNG+을/JKO 컴퓨터/NNG+와/JKB 같/VA+은/ETM 기계/NNG+를/JKO 이용/NNG+하/XSV+어서/EC 모사/NNG 하/VV+ㄹ/ETM 수/NNB 있/VV+도록/EC 연구/NNG+하/XSV+고/EC 이/NP+를/JKO 구현/NNG+하/XSV+는/ETM 인공지능/NNG+의/JKG 주요/NNG 분야/NNG 중/NNB 하나/NR+이/VCP+다/EF+./SF
    

#### 아리랑 api 사용


```python
tagger_ARR = Tagger(API.ARIRANG) # 아리랑 api 불러오기
```


```python
taggedParagraph_ARR = tagger_ARR(text) # 문단을 분석해서 문장들로 얻기
print(taggedParagraph_ARR)
```

    [자연 어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터 와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현 하는 인공 지능의 주요 분야 중 하 나다 ., 자연 언어 처리는 연구 대상이 언어 이기 때 문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다 ., 구현을 위해 수학 적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표 적인 분야이다 ., 정보 검색 , QA 시스템 , 문서 자동 분류 , 신문기사 클러스터링 , 대화 형 Agent 등 다양한 응용이 이루어 지고 있다 .]
    


```python
print(taggedParagraph_ARR[0].singleLineString()) # 첫번째 문장의 품사분석 결과 출력
```

    자연/NNG 어/NNG 처리/NNG 또는/MAG 자연/NNG 언어/NNG 처리/NNG+는/JX 인간/NNG+의/JX 언어/NNG 현상/NNG+을/JX 컴퓨터/NNG 와/NNG 같/VV+은/EF 기계/NNG+를/JX 이용/NNG+하/XSV+어서/EF 모사/NNG 할수/NNG 있/VV+도록/EF 연구/NNG+하/XSV+고/EF 이르/VV+ㄹ/EF 구현/NNG 하/VV+는/EF 인공/NNG 지능/NNG+의/JX 주요/NNG 분야/NNG 중/NNG+하/XSV+어/EF 낫/VV+어다/EF ./SF
    

#### OKT api 사용


```python
tagger_OKT = Tagger(API.OKT) # OKT api 불러오기
```


```python
taggedParagraph_OKT = tagger_OKT(text) # 문단을 분석해서 문장들로 얻기
print(taggedParagraph_OKT)
```

    [자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다., 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다., 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다., 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다.]
    


```python
print(taggedParagraph_OKT[0].singleLineString()) # 첫번째 문장의 품사분석 결과 출력
```

    자연어/NNG 처리/NNG 또는/MAG 자연/NNG 언어/NNG 처리/NNG+는/JX 인간/NNG+의/JX 언어/NNG 현상/NNG+을/JX 컴퓨터/NNG+와/JX 같은/VA 기계/NNG+를/JX 이용/NNG+해서/VV 모사/NNG 할수/VV 있도록/VA 연구/NNG+하고/JX 이를/VV 구현/NNG+하는/VV 인공/NNG+지능/NNG+의/JX 주요/NNG 분야/NNG 중/NNG 하나/NNG+다/JX+./SF
    

#### 다온 api 사용


```python
tagger_DA = Tagger(API.DAON) # 다온 api 불러오기
```


```python
taggedParagraph_DA = tagger_DA(text) # 문단을 분석해서 문장들로 얻기
print(taggedParagraph_DA)
```

    [자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다., 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다., 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다., 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다.]
    


```python
print(taggedParagraph_DA[0].singleLineString()) # 첫번째 문장의 품사분석 결과 출력
```

    자연어/NNG 처리/NNG 또는/MAG 자연/NNG 언어/NNG 처리/NNG+는/JX 인간/NNG+의/JKG 언어/NNG 현상/NNG+을/JKO 컴퓨터/NNG+와/JC 같/VA+은/ETM 기계/NNG+를/JKO 이용/NNG+하/XSV+어서/EC 모사/NNG 하/VV+ㄹ/ETM+수/NNB 있/VA+도록/EC 연구/NNG+하/XSV+고/EC 이/NP+를/JKO 구현/NNG+하/XSV+는/ETM 인공지능/NNG+의/JKG 주요/NNG 분야/NNG 중/NNB 하나/NR+이/VCP+다/EF+./SF
    

### 3-3 구문 구조 분석하기
- 문장 또는 문단을 분석해서 각 문장의 구문구조를 묶어낼 수 있음.
- 한나눔 분석기만 구문구조 분석 지원.(API.HNN)
- Sentence 객체에 포함된 SyntaxTree가 됨.
- getSyntaxTree(): 구문분석을 했다면, 최상위 구구조(Phrase)를 돌려줌.
- getTreeString(): 트리구조의 표현을 문자열로 돌려줌.


```python
from koalanlp import API
from koalanlp.proc import Parser
# 필요한 모듈 import
```

#### 한나눔 api 사용


```python
parser = Parser(API.HNN) # 한나눔 api 불러오기
```


```python
parsed = parser(text)
print(parsed)
```

    [자연어처리 또는 자연언어처리는 인간의 언어현상을 컴퓨터와 같은 기계를 이용해서 모사할 수 있도록 연구하고 이를 구현하는 인공지능의 주요분야 중 하나이다., 자연언어처리는 연구 대상이 언어이기 때문에 당연하게도 언어 자체를 연구하는 언어학과언어 현상의 내적기재를 탐구하는 언어인지과학과 연관이 깊다., 구현을 위하여 수학적통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다., 정보검색, QA 시스템, 문서 자동분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이뤄 지고 있다.]
    


```python
print(parsed[0].getSyntaxTree().getTreeString()) # 첫번째 문장의 구문구조 트리 출력.
```

    S-Node()
    | NP-Node()
    | | NP-Node(자연어처리 = 자연어/NNG+처리/NNG)
    | | NP-Node()
    | | | AP-Node(또는 = 또는/MAG)
    | | | NP-Node(자연언어처리는 = 자연/NNG+언어/NNG+처리/NNG+는/JX)
    | VNP-Node()
    | | S-Node()
    | | | NP-Node()
    | | | | VP-Node()
    | | | | | NP-Node()
    | | | | | | NP-Node(인간의 = 인간/NNG+의/JKG)
    | | | | | | NP-Node(언어현상을 = 언어/NNG+현상/NNG+을/JKO)
    | | | | | VP-Node()
    | | | | | | NP-Node()
    | | | | | | | VP-Node()
    | | | | | | | | NP-Node(컴퓨터와 = 컴퓨터/NNG+와/JKB)
    | | | | | | | | VP-Node(같은 = 같/VA+은/ETM)
    | | | | | | | NP-Node(기계를 = 기계/NNG+를/JKO)
    | | | | | | VP-Node()
    | | | | | | | AP-Node(이용해서 = 이용해서/MAG)
    | | | | | | | VP-Node(모사할 = 모사/NNG+하/XSV+ㄹ/ETM)
    | | | | NP-Node(수 = 수/NNB)
    | | | VP-Node(있도록 = 있/VV+도록/EC)
    | | VNP-Node()
    | | | NP-Node()
    | | | | VP-Node()
    | | | | | NP-Node(연구하고 = 연구/NNG+하고/JKB)
    | | | | | VP-Node()
    | | | | | | NP-Node(이를 = 이/NP+를/JKO)
    | | | | | | VP-Node(구현하는 = 구현/NNG+하/XSV+는/ETM)
    | | | | NP-Node(인공지능의 = 인공지능/NNG+의/JKG)
    | | | VNP-Node()
    | | | | NP-Node()
    | | | | | NP-Node(주요분야 = 주요/NNG+분야/NNG)
    | | | | | NP-Node(중 = 중/NNB)
    | | | | VNP-Node(하나이다. = 하나/NR+이/VCP+다/EF+./SF)
    

### 3-4 의존 구조 분석하기
- 문장 또는 문단을 분석해서 각 문장 속 단어 사이의 의존 관계를 묶어낼 수 있음.
- 한나눔, 꼬꼬마, ETRI 분석기만 지원.(API.HNN / API.KKMA / API.ETRI)
- Sentence 객체에 포함된 DepEdge의 List가 됨.
- getDependencies(): 의존구문분석을 했다면, 문장에 포함된 모든 의존구조의 목록을 돌려줌.


```python
from koalanlp import API
from koalanlp.proc import Parser
# 필요한 모율 import
```

#### 한나눔 api 사용


```python
parser_HNN = Parser(API.HNN) # 한나눔 api 불러오기
```


```python
parsed_HNN = parser_HNN(text)
print(parsed_HNN)
```

    [자연어처리 또는 자연언어처리는 인간의 언어현상을 컴퓨터와 같은 기계를 이용해서 모사할 수 있도록 연구하고 이를 구현하는 인공지능의 주요분야 중 하나이다., 자연언어처리는 연구 대상이 언어이기 때문에 당연하게도 언어 자체를 연구하는 언어학과언어 현상의 내적기재를 탐구하는 언어인지과학과 연관이 깊다., 구현을 위하여 수학적통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다., 정보검색, QA 시스템, 문서 자동분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이뤄 지고 있다.]
    


```python
for dep_HNN in parsed_HNN[0].getDependencies(): # 첫번째 문장의 의존구조 출력.
    print(dep_HNN)
```

    NPMOD('자연언어처리는 = 자연/NNG+언어/NNG+처리/NNG+는/JX' → '자연어처리 = 자연어/NNG+처리/NNG')
    APAJT('자연언어처리는 = 자연/NNG+언어/NNG+처리/NNG+는/JX' → '또는 = 또는/MAG')
    NPSBJ('하나이다. = 하나/NR+이/VCP+다/EF+./SF' → '자연언어처리는 = 자연/NNG+언어/NNG+처리/NNG+는/JX')
    NPMOD('언어현상을 = 언어/NNG+현상/NNG+을/JKO' → '인간의 = 인간/NNG+의/JKG')
    NPOBJ('모사할 = 모사/NNG+하/XSV+ㄹ/ETM' → '언어현상을 = 언어/NNG+현상/NNG+을/JKO')
    NPAJT('같은 = 같/VA+은/ETM' → '컴퓨터와 = 컴퓨터/NNG+와/JKB')
    VPMOD('기계를 = 기계/NNG+를/JKO' → '같은 = 같/VA+은/ETM')
    NPOBJ('모사할 = 모사/NNG+하/XSV+ㄹ/ETM' → '기계를 = 기계/NNG+를/JKO')
    APAJT('모사할 = 모사/NNG+하/XSV+ㄹ/ETM' → '이용해서 = 이용해서/MAG')
    VPMOD('수 = 수/NNB' → '모사할 = 모사/NNG+하/XSV+ㄹ/ETM')
    NPSBJ('있도록 = 있/VV+도록/EC' → '수 = 수/NNB')
    VPMOD('하나이다. = 하나/NR+이/VCP+다/EF+./SF' → '있도록 = 있/VV+도록/EC')
    NPAJT('구현하는 = 구현/NNG+하/XSV+는/ETM' → '연구하고 = 연구/NNG+하고/JKB')
    NPOBJ('구현하는 = 구현/NNG+하/XSV+는/ETM' → '이를 = 이/NP+를/JKO')
    VPMOD('인공지능의 = 인공지능/NNG+의/JKG' → '구현하는 = 구현/NNG+하/XSV+는/ETM')
    NPMOD('하나이다. = 하나/NR+이/VCP+다/EF+./SF' → '인공지능의 = 인공지능/NNG+의/JKG')
    NPMOD('중 = 중/NNB' → '주요분야 = 주요/NNG+분야/NNG')
    NPMOD('하나이다. = 하나/NR+이/VCP+다/EF+./SF' → '중 = 중/NNB')
    VNPROOT('ROOT' → '하나이다. = 하나/NR+이/VCP+다/EF+./SF')
    

#### 꼬꼬마 api 사용


```python
parser_KKMA = Parser(API.KKMA) # 꼬꼬마 api 불러오기
```


```python
parsed_KKMA = parser_KKMA(text)
print(parsed_KKMA)
```

    [자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할 수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다., 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다., 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다., 정보 검색, QA 시스템, 문서 자동 분류, 신문기사 클러 스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다.]
    


```python
for dep_KKMA in parsed_KKMA[0].getDependencies(): # 첫번째 문장의 의존구조 출력.
    print(dep_KKMA)
```

    XUNDEF('ROOT' → '하나다. = 하나/NR+이/VCP+다/EF+./SF')
    NP('하나다. = 하나/NR+이/VCP+다/EF+./SF' → '중 = 중/NNB')
    NP('중 = 중/NNB' → '분야 = 분야/NNG')
    NP('분야 = 분야/NNG' → '주요 = 주요/NNG')
    DPMOD('주요 = 주요/NNG' → '인공지능의 = 인공지능/NNG+의/JKG')
    DPMOD('인공지능의 = 인공지능/NNG+의/JKG' → '구현하는 = 구현/NNG+하/XSV+는/ETM')
    VPCNJ('구현하는 = 구현/NNG+하/XSV+는/ETM' → '연구하고 = 연구/NNG+하/XSV+고/EC')
    VPCNJ('연구하고 = 연구/NNG+하/XSV+고/EC' → '있도록 = 있/VV+도록/EC')
    NPAJT('있도록 = 있/VV+도록/EC' → '수 = 수/NNB')
    DPMOD('수 = 수/NNB' → '할 = 하/VV+ㄹ/ETM')
    VPCNJ('할 = 하/VV+ㄹ/ETM' → '이용해서 = 이용/NNG+하/XSV+어서/EC')
    NPOBJ('이용해서 = 이용/NNG+하/XSV+어서/EC' → '기계를 = 기계/NNG+를/JKO')
    DPMOD('기계를 = 기계/NNG+를/JKO' → '같은 = 같/VA+은/ETM')
    NPAJT('같은 = 같/VA+은/ETM' → '처리 = 처리/NNG')
    NP('처리 = 처리/NNG' → '자연어 = 자연어/NNG')
    DPMOD('같은 = 같/VA+은/ETM' → '또는 = 또는/MAG')
    NPAJT('같은 = 같/VA+은/ETM' → '처리는 = 처리/NNG+는/JX')
    NP('처리는 = 처리/NNG+는/JX' → '언어 = 언어/NNG')
    NP('언어 = 언어/NNG' → '자연 = 자연/NNG')
    NPOBJ('같은 = 같/VA+은/ETM' → '현상을 = 현상/NNG+을/JKO')
    NP('현상을 = 현상/NNG+을/JKO' → '언어 = 언어/NNG')
    DPMOD('언어 = 언어/NNG' → '인간의 = 인간/NNG+의/JKG')
    NPAJT('같은 = 같/VA+은/ETM' → '컴퓨터와 = 컴퓨터/NNG+와/JKB')
    NPAJT('할 = 하/VV+ㄹ/ETM' → '모사 = 모사/NNG')
    NPOBJ('구현하는 = 구현/NNG+하/XSV+는/ETM' → '이를 = 이/NP+를/JKO')
    

### 3-5 개체명 인식하기
- 각 문장 속에서 사람이나 장소 등을 나타내는 개체명을 묶어낼 수 있음.
- ETRI 분석기만 지원.(API.ETRI)
- Sentence 객체에 포한된 Entity의 List가 됨.
- getEntities(): 개체명 분석을 했다면, 현재 형태소가 속한 개체명 값을 돌려줌.


```python
from koalanlp import API
from koalanlp.proc import EntityRecognizer
# 필요한 모듈 impotr
```

#### ETRI api 사용


```python
# recognizer = EntityRecognizer(API.ETRI, apiKey=API_KEY) # ETRI api 불러오기
```


```python
# parsed = recognizer(text)
```


```python
# for entity in parsed[0].getEntities(): # 첫번째 문장의 개체명들을 출력합니다.
    # print(entity)
```

### 3-6 의미역 분석하기
- 각 문장 속의 의미역을 찾아 분석.
- ETRI 분석기만 지원.(API.ETRI)
- getRoles(): 의미역 분석을 했다면, 문장에 포함된 의미역 구조의 목록을 돌려줌.


```python
from koalanlp import API
from koalanlp.proc import EntityRecognizer
from koalanlp.proc import RoleLabeler
# 필요한 모듈 import
```

#### ETRI api 사용


```python
# labeler = RoleLabeler(API.ETRI, apiKey=API_KEY) # ETRI api 불러오기
```


```python
# parsed = labeler(text)
```


```python
# for role in parsed[0].getRoles(): # 첫번째 문장의 의미역들을 출력합니다.
    # print(role)
```

### 3-7 사전 사용하기
- 사전에 접근하여 내용을 불러오거나 새 항목을 추가할 수 있음.
- 아리랑, 은전한닢, 한나눔, 꼬꼬마, 코모란, OKT에서 사용가능.(API.ARIRANG / API.EUNJEON / API.HNN / API.KKMA / API.KMR / API.OKT)
- addUserDictionary(): 사용자 사전에, 표면형과 그 품사를 추가함.
- contains(): 사전에 등재되어 있는지 확인.
- importFrom(Dictionary()): 다른 사전을 참조하여, 선택된 사전에 없는 단어를 사용자사전으로 추가.
- getItems(): 사용자 사전에 등재된 모든 항목을 가져옴.
- getBaseEntries(lambda pos: pos.is품사()): 원본 사전에 등재된 항목 중에서, 지정된 형태소의 항목만을 가져옴.


```python
from koalanlp import API
from koalanlp.types import POS
from koalanlp.proc import Dictionary
# 필요한 모듈 import
```

#### 아리랑 api 사용


```python
ARR_Dict = Dictionary(API.ARIRANG) # 아리랑 api 불러오기
```

- 사전에 등록하기


```python
ARR_Dict.addUserDictionary(("코알라NLP", POS.NNP)) # 1개 등록
```


```python
ARR_Dict.addUserDictionary(("코모란", POS.NNP), ("은전한닢", POS.NNP)) # 2개 이상 등록
```

- 사전에 있는지 확인하기


```python
ARR_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True




```python
ARR_Dict.contains("코알라NLP", POS.NNP)
```




    True



- 다른 api 사전에서 단어 불러오기(수행시간이 오래걸림.)


```python
# ARR_Dict.importFrom(Dictionary(API.EUNJEON))
```

- 사전 항목 추출하기


```python
ARR_Dict.getItems() # 사용자 사전 항목
```




    [('코알라NLP', NNP), ('코모란', NNP), ('은전한닢', NNP)]




```python
ARR_Dict.getBaseEntries(lambda pos: pos.isNoun()) # 시스템 사전 항목
```




    <generator object Dictionary.getBaseEntries at 0x0000021E2C275A40>



#### 은전한닢 api 사용


```python
EJ_Dict = Dictionary(API.EUNJEON) # 은전한닢 api 불러오기
```

- 사전에 등록하기


```python
EJ_Dict.addUserDictionary(("코알라NLP", POS.NNP)) # 1개 등록
```


```python
EJ_Dict.addUserDictionary(("아리랑", POS.NNP), ("은전한닢", POS.NNP)) # 2개 이상 등록
```

- 사전에 있는지 확인하기


```python
EJ_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True




```python
EJ_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True



- 다른 api 사전에서 단어 불러오기(수행시간이 오래걸림.)


```python
# EJ_Dict.importFrom(Dictionary(API.ARIRANG))
```

- 사전 항목 추출하기


```python
EJ_Dict.getItems() # 사용자 사전 항목
```




    [('코알라NLP', NNP), ('아리랑', NNP), ('은전한닢', NNP)]




```python
EJ_Dict.getBaseEntries(lambda pos: pos.isNoun()) # 시스템 사전 항목
```




    <generator object Dictionary.getBaseEntries at 0x0000021E2C386D00>



#### 한나눔 api 사용


```python
HNN_Dict = Dictionary(API.HNN) # 한나눔 api 불러오기
```

- 사전에 등록하기


```python
HNN_Dict.addUserDictionary(("코알라NLP", POS.NNP)) # 1개 등록
```


```python
HNN_Dict.addUserDictionary(("한나눔", POS.NNP), ("은전한닢", POS.NNP)) # 2개 이상 등록
```

- 사전에 있는지 확인하기


```python
HNN_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True




```python
HNN_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True



- 다른 api 사전에서 단어 불러오기(수행시간이 오래걸림.)


```python
# HNN_Dict.importFrom(Dictionary(API.EUNJEON))
```

- 사전 항목 추출하기


```python
HNN_Dict.getItems() # 사용자 사전 항목
```




    [('코알라NLP', NNP), ('한나눔', NNP), ('은전한닢', NNP)]




```python
HNN_Dict.getBaseEntries(lambda pos: pos.isNoun()) # 시스템 사전 항목
```




    <generator object Dictionary.getBaseEntries at 0x0000021E2C386DB0>



#### 꼬꼬마 api 사용


```python
KKMA_Dict = Dictionary(API.KKMA) # 꼬꼬마 api 불러오기
```

- 사전에 등록하기


```python
KKMA_Dict.addUserDictionary(("코알라NLP", POS.NNP)) # 1개 등록
```


```python
KKMA_Dict.addUserDictionary(("꼬꼬마", POS.NNP), ("한나눔", POS.NNP)) # 2개 이상 등록
```

- 사전에 있는지 확인하기


```python
KKMA_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True




```python
KKMA_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True



- 다른 api 사전에서 단어 불러오기(수행시간이 오래걸림.)


```python
# KKMA_Dict.importFrom(Dictionary(API.EUNJEON))
```

- 사전 항목 추출하기


```python
KKMA_Dict.getItems() # 사용자 사전 항목
```




    [('코알라NLP', NNP), ('꼬꼬마', NNP), ('한나눔', NNP)]




```python
KKMA_Dict.getBaseEntries(lambda pos: pos.isNoun()) # 시스템 사전 항목
```




    <generator object Dictionary.getBaseEntries at 0x0000021E2C386CA8>



#### 코모란 api 사용


```python
KMR_Dict = Dictionary(API.KMR) # 코모란 api 불러오기
```

- 사전에 등록하기


```python
KMR_Dict.addUserDictionary(("코알라NLP", POS.NNP)) # 1개 등록
```


```python
KMR_Dict.addUserDictionary(("코모란", POS.NNP), ("꼬꼬마", POS.NNP)) # 2개 이상 등록
```

- 사전에 있는지 확인하기


```python
KMR_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True




```python
KMR_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True



- 다른 api 사전에서 단어 불러오기(수행시간이 오래걸림.)


```python
# KMR_Dict.importFrom(Dictionary(API.EUNJEON))
```

- 사전 항목 추출하기


```python
KMR_Dict.getItems() # 사용자 사전 항목
```




    [('코알라NLP', NNP), ('코모란', NNP), ('꼬꼬마', NNP)]




```python
KMR_Dict.getBaseEntries(lambda pos: pos.isNoun()) # 시스템 사전 항목
```




    <generator object Dictionary.getBaseEntries at 0x0000021E2C3DF468>



#### OKT api 사용


```python
OKT_Dict = Dictionary(API.OKT) # OKT api 불러오기
```

- 사전에 등록하기


```python
OKT_Dict.addUserDictionary(("코알라NLP", POS.NNP)) # 1개 등록
```


```python
OKT_Dict.addUserDictionary(("코모란", POS.NNP), ("오케이티", POS.NNP)) # 2개 이상 등록
```

- 사전에 있는지 확인하기


```python
OKT_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True




```python
OKT_Dict.contains("코알라NLP", POS.NNP, POS.NNG)
```




    True



- 다른 api 사전에서 단어 불러오기(수행시간이 오래걸림.)


```python
# OKT_Dict.importFrom(Dictionary(API.EUNJEON))
```

- 사전 항목 추출하기


```python
OKT_Dict.getItems() # 사용자 사전 항목
```




    [('코알라NLP', NNP), ('코모란', NNP), ('오케이티', NNP)]




```python
OKT_Dict.getBaseEntries(lambda pos: pos.isNoun()) # 시스템 사전 항목
```




    <generator object Dictionary.getBaseEntries at 0x0000021E2C3DF8E0>



## 4. 기타 편의기능

### 4-1 한글 분해/조립
- 한글을 초성, 중성, 종성으로 분해하거나, 조합할 수 있음.
- koalanlp-core 모듈만 있어도 가능.


```python
from koalanlp import ExtUtil
# 필요한 모듈 import
```

1. 한글 여부 확인

- isHangul: 현재 문자가 한글 완성형, 조합용 문자인지 확인


```python
ExtUtil.isHangul('가')
```




    [True]




```python
ExtUtil.isHangul('ㄱ')
```




    [True]




```python
ExtUtil.isHangul('a나.')
```




    [False, True, False]



- isHangulEnding: 현재 문자열이 한글(완성/조합)로 끝나는지 확인


```python
ExtUtil.isHangulEnding("갤럭시S는")
```




    True




```python
ExtUtil.isHangulEnding("abc")
```




    False




```python
ExtUtil.isHangulEnding("보기 ㄱ")
```




    True



- isCompleteHangul: 현재 문자가 불완전한 한글 문자인지 확인


```python
ExtUtil.isCompleteHangul('가')
```




    [True]




```python
ExtUtil.isCompleteHangul('ㄱ')
```




    [False]




```python
ExtUtil.isCompleteHangul('a나.')
```




    [False, True, False]



2. 종성으로 끝나는지 확인

- isJongsungEnding: 현재 문자열이 종성으로 끝인지 확인


```python
ExtUtil.isJongsungEnding('각')
```




    True




```python
ExtUtil.isJongsungEnding('가')
```




    False




```python
ExtUtil.isJongsungEnding('m')
```




    False



3. 초, 중, 종성 한글 자모 값으로 분해

- getChosung: 현재 문자에서 초성 자음문자를 분리. 초성이 없으면 None


```python
ExtUtil.getChosung('가')
```




    ['ᄀ']




```python
ExtUtil.getChosung('온')
```




    ['ᄋ']




```python
ExtUtil.getChosung('ㄱㄴㄷ')
```




    [None, None, None]



- getJungsung: 현재 문자에서 중성 모음문자를 분리. 중성이 없으면 None.


```python
ExtUtil.getJungsung('가')
```




    ['ᅡ']



- getJongsung: 현재 문자에서 종성 자음문자를 분리. 종성이 없으면 None.


```python
ExtUtil.getJongsung('가')
```




    [None]




```python
ExtUtil.getJongsung('온')
```




    ['ᆫ']



- dissembleHangul: 현재 문자열을 초성, 중성, 종성 자음문자로 분리하여 새 문자열을 만듬. 종성이 없으면 종성은 쓰지 않음.


```python
ExtUtil.dissembleHangul('각')
```




    '각'




```python
ExtUtil.dissembleHangul("오운")
```




    '오운'



4. 종성 한글 자모값으로 변환

- ChoToJong: 초성 문자를 종성 조합형 문자로 변경


```python
ExtUtil.ChoToJong['ㄵ']
```




    'ᆬ'



5. 한글 자모 범위에 속하는 초,중, 종성인지 확인

- isChosungJamo: 현재 문자가 현대 한글 초성 자음 문자인지 확인


```python
ExtUtil.isChosungJamo('ㄱ')
```




    [False]




```python
ExtUtil.isChosungJamo('\u1100')
```




    [True]



- isJungsungJamo: 현재 문자가 현대 한글 중성 모음 문자인지 확인


```python
ExtUtil.isJungsungJamo('ㅏ')
```




    [False]




```python
ExtUtil.isJungsungJamo('\u1161')
```




    [True]



- isJongsungJamo: 현재 문자가 현대 한글 종성 자음 문자인지 확인


```python
ExtUtil.isJongsungJamo('ㄱab')
```




    [False, False, False]




```python
ExtUtil.isJongsungJamo('\u11A8,.')
```




    [True, False, False]



6. 한글 자모 결합

- assembleHangulTriple: 3개의 자모를 결합


```python
ExtUtil.assembleHangulTriple('\u1100', '\u1161', None)
```




    '가'




```python
ExtUtil.assembleHangulTriple('\u1100', '\u1161', '\u11AB')
```




    '간'




```python
ExtUtil.assembleHangulTriple(None, '\u1161')
```




    '아'




```python
ExtUtil.assembleHangulTriple('\u1100', None, '\u11A8')
```




    '극'



- assembleHangul: 주어진 문자열에서 초성, 중성, 종성이 연달아 나오는 경우 이를 조합하여 한글 문자를 재구성


```python
ExtUtil.assembleHangul("\u1100\u1161\u1102\u1161\u11ab")
```




    '가난'



#### 4-2 영문, 한문 음독
- 알파벳 또는 한문 표기를 읽을 수 있음.
- 한문은 국사편찬위원회의 한자 음가사전과 몇가지 규칙에 따라 한글 음차로 변환.


```python
from koalanlp import ExtUtil
# 필요한 모듈 import
```

- alphaToHangul: 주어진 문자열에서 알파벳이 발음되는 대로 국문 문자열로 표기하여 값으로 돌려줌.


```python
ExtUtil.alphaToHangul("ABCD")
```




    '에이비씨디'




```python
ExtUtil.alphaToHangul("갤럭시S")
```




    '갤럭시에스'




```python
ExtUtil.alphaToHangul("cup")
```




    '씨유피'



- isAlphaPronounced: 주어진 문자열이 알파벳이 발음되는 대로 표기된 문자열인지 확인.


```python
ExtUtil.isAlphaPronounced("에스비에스")
```




    True




```python
ExtUtil.isAlphaPronounced("갤럭시에스")
```




    False



- hangulToAlpha: 주어진 문자열에 적힌 알파벳 발음을 알파벳으로 변환하여 문자열로 반환.


```python
ExtUtil.hangulToAlpha("갤럭시에스")
```




    '갤럭시S'




```python
ExtUtil.hangulToAlpha("에이디에이치디")
```




    'ADHD'



- isHanja: 문자열의 각 문자가 한자 범위인지 확인


```python
ExtUtil.isHanja('國ABC')
```




    [True, False, False, False]




```python
ExtUtil.isCJKHanja('國')
```




    [True]



- hanjaToHangul: 국사편찬위원회 한자음가사전에 따라 한자 표기된 내용을 국문 표기로 전환
    - headCorrection 값이 true인 경우, whitespace에 따라오는 문자에 두음법칙을 자동 적용함.(기본값: True)
    - 두음 법칙이 적용되지 않는 예:
        - 한자 파생어나 합성어에서 원 단어
        - 외자가 아닌 이름


```python
ExtUtil.hanjaToHangul("國篇")
```




    '국편'




```python
ExtUtil.hanjaToHangul("國篇은 오늘")
```




    '국편은 오늘'




```python
ExtUtil.hanjaToHangul("300 兩의 돈")
```




    '300 냥의 돈'




```python
ExtUtil.hanjaToHangul("樂園")
```




    '낙원'



### 5. 패키지 조합
#### 여러 패키지 함께 쓰기
- 여러 분석기의 결과를 세종 품사 분석 결과를 기준으로 상호 변환할 수 있도록 함.
- api 형태로 제공되어 품사 분석 결과를 넣을 수 없는 ETRI 분석기를 제외하면, 대부분의 분석기를 교차하여 사용 가능.


```python
from koalanlp import API
from koalanlp.proc import SentenceSplitter, Tagger, Parser
# 필요한 모듈 import
```


```python
# OKT api로 문장 분리
splitter = SentenceSplitter(API.OKT)
```


```python
# 코모란 분석기로 품사 분석
tagger = Tagger(API.KMR)
```


```python
# 한나눔 분석기로 구문구조 분석
parser = Parser(API.HNN)
```


```python
splits = splitter(text)
```


```python
tagged = tagger.tagSentence(*splits)
```


```python
parsed = parser(tagged)
```


```python
for sent in parsed:
    print(sent.getSyntaxTree().getTreeString())
```

    S-Node()
    | VP-Node()
    | | NP-Node()
    | | | NP-Node(자연어처리 = 자연어/NNG+처리/NNG)
    | | | NP-Node()
    | | | | VP-Node()
    | | | | | AP-Node(또는 = 또는/MAG)
    | | | | | VP-Node(자연 언어 처리는 = 자연 언어 처리/VV+는/ETM)
    | | | | NP-Node()
    | | | | | NP-Node(인간의 = 인간/NNG+의/JKG)
    | | | | | NP-Node(언어현상을 = 언어/NNG+현상/NNG+을/JKO)
    | | VP-Node()
    | | | NP-Node()
    | | | | NP-Node(컴퓨터와 = 컴퓨터/NNG+와/JC)
    | | | | NP-Node()
    | | | | | VP-Node(같은 = 같/VA+은/ETM)
    | | | | | NP-Node(기계를 = 기계/NNG+를/JKO)
    | | | VP-Node(이용하여서 = 이용/NNG+하/XSV+아서/EC)
    | S-Node()
    | | NP-Node()
    | | | VP-Node(모사할 = 모사/NNG+하/XSV+ㄹ/ETM)
    | | | NP-Node(수 = 수/NNB)
    | | VP-Node()
    | | | VP-Node(있도록 = 있/VV+도록/EC)
    | | | VP-Node()
    | | | | VP-Node(연구하고 = 연구/NNG+하/XSV+고/EC)
    | | | | VP-Node()
    | | | | | NP-Node()
    | | | | | | NP-Node()
    | | | | | | | VP-Node()
    | | | | | | | | NP-Node(이를 = 이/NP+를/JKO)
    | | | | | | | | VP-Node(구현하는 = 구현/NNG+하/XSV+는/ETM)
    | | | | | | | NP-Node(인공지능의 = 인공지능/NNG+의/JKG)
    | | | | | | NP-Node(주요분야중 = 주요/XR+분야/NNG+중/NNB)
    | | | | | VP-Node(하나다. = 하/VV+나다/EF+./SF)
    S-Node()
    | S-Node()
    | | NP-Node()
    | | | VP-Node(자연 언어 처리는 = 자연 언어 처리/VV+는/ETM)
    | | | NP-Node()
    | | | | NP-Node(연구 = 연구/NNG)
    | | | | NP-Node(대상이 = 대상/NNG+이/JKS)
    | | VP-Node()
    | | | NP-Node()
    | | | | NP-Node(언어이기 = 언어/NNG+이기/NNG)
    | | | | NP-Node(때문에 = 때문/NNB+에/JKB)
    | | | VP-Node(당연하게도 = 당연/XR+하/XSA+게/EC+도/JX)
    | S-Node()
    | | NP-Node()
    | | | VP-Node()
    | | | | NP-Node()
    | | | | | NP-Node()
    | | | | | | VP-Node()
    | | | | | | | NP-Node()
    | | | | | | | | NP-Node(언어 = 언어/NNG)
    | | | | | | | | NP-Node(자체를 = 자체/NNG+를/JKO)
    | | | | | | | VP-Node(연구하는 = 연구/NNG+하/XSV+는/ETM)
    | | | | | | NP-Node()
    | | | | | | | NP-Node(언어학과 = 언어학/NNG+과/JC)
    | | | | | | | NP-Node(언어현상의 = 언어/NNG+현상/NNG+의/JKG)
    | | | | | NP-Node()
    | | | | | | NP-Node(내적 = 내/NNG+적/XSN)
    | | | | | | NP-Node(기재를 = 기재/NNG+를/JKO)
    | | | | VP-Node(탐구하는 = 탐구/NNG+하/XSV+는/ETM)
    | | | NP-Node()
    | | | | NP-Node(언어인지과학과 = 언어/NNG+인지/NNG+과학/NNG+과/JC)
    | | | | NP-Node(연관이 = 연관/NNG+이/JKS)
    | | VP-Node(깊다. = 깊/VA+다/EF+./SF)
    VNP-Node()
    | VP-Node()
    | | VP-Node()
    | | | NP-Node(구현을 = 구현/NNG+을/JKO)
    | | | VP-Node(위하여 = 위하/VV+아/EC)
    | | VP-Node()
    | | | NP-Node()
    | | | | NP-Node(수학적 = 수학/NNG+적/XSN)
    | | | | NP-Node()
    | | | | | NP-Node(통계적 = 통계/NNG+적/XSN)
    | | | | | NP-Node(도구를 = 도구/NNG+를/JKO)
    | | | VP-Node()
    | | | | AP-Node(많이 = 많이/MAG)
    | | | | VP-Node(활용하며 = 활용/NNG+하/XSV+며/EC)
    | VNP-Node()
    | | AP-Node(특히 = 특히/MAG)
    | | VNP-Node()
    | | | NP-Node()
    | | | | NP-Node(기계학습 = 기계학습/NNP)
    | | | | NP-Node(도구를 = 도구/NNG+를/JKO)
    | | | VNP-Node()
    | | | | AP-Node(많이 = 많이/MAG)
    | | | | VNP-Node()
    | | | | | VP-Node(사용하는 = 사용/NNG+하/XSV+는/ETM)
    | | | | | VNP-Node()
    | | | | | | VNP-Node(대표적인 = 대표/NNG+적/XSN+이/VCP+ㄴ/ETM)
    | | | | | | VNP-Node(분야이다. = 분야/NNG+이/VCP+다/EF+./SF)
    S-Node()
    | NP-Node()
    | | NP-Node()
    | | | NP-Node(정보검색, = 정보검색/NNG+,/SP)
    | | | NP-Node()
    | | | | NP-Node(QA = QA/SL)
    | | | | NP-Node()
    | | | | | NP-Node()
    | | | | | | NP-Node()
    | | | | | | | NP-Node()
    | | | | | | | | NP-Node(시스템, = 시스템/NNG+,/SP)
    | | | | | | | | NP-Node(문서 = 문서/NNG)
    | | | | | | | NP-Node(자동분류, = 자동/NNG+분류/NNG+,/SP)
    | | | | | | NP-Node()
    | | | | | | | NP-Node()
    | | | | | | | | NP-Node(신문기사 = 신문기사/NNP)
    | | | | | | | | NP-Node(클러스터링, = 클러스터링/NNG+,/SP)
    | | | | | | | NP-Node(대화형 = 대/XPN+화형/NNG)
    | | | | | NP-Node(Agent = Agent/SL)
    | | NP-Node(등 = 등/NNB)
    | S-Node()
    | | NP-Node()
    | | | VP-Node(다양한 = 다양/XR+하/XSA+ㄴ/ETM)
    | | | NP-Node(응용이 = 응용/NNG+이/JKS)
    | | VP-Node()
    | | | VP-Node(이뤄지고 = 이루/VV+어/EC+지/VX+고/EC)
    | | | VP-Node(있다. = 있/VX+다/EF+./SF)
    

### 6. Koalanlp 사용 샘플

#### 6-1 문장 분리 기능 사용 예시


```python
# 필요 모듈 import
from koalanlp.Util import initialize
from koalanlp.proc import SentenceSplitter
from koalanlp import API
```


```python
# 패키지 초기화
#initialize(OKT='LATEST')
```


```python
splitter = SentenceSplitter(API.OKT)
```


```python
while True:
    text_al = input("분석할 문장을 입력하세요>> ").strip()

    if len(text_al) == 0:
        break

    sentences = splitter(text_al)

    print("===== Sentence =====")
    for i, sent in enumerate(sentences):
        print("[%s] %s" % (i, sent))
```

    분석할 문장을 입력하세요>> 자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다. 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다. 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다. 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다.
    ===== Sentence =====
    [0] 자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다.
    [1] 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다.
    [2] 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다.
    [3] 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다.
    분석할 문장을 입력하세요>> 
    

#### 6-2 품사분석기 사용 예시


```python
# 필요 모듈 import
from koalanlp.Util import initialize
from koalanlp.proc import Tagger
from koalanlp import API
```


```python
# 패키지 초기화
# initialize(EUNJEON='LATEST')
```


```python
tagger = Tagger(API.EUNJEON)
```


```python
while True:
    text = input("분석할 문장을 입력하세요>> ").strip()

    if len(text) == 0:
        break

    sentences = tagger(text)

    print("===== Sentence =====")
    for i, sent in enumerate(sentences):
        print("===== Sentence #$i =====")
        print(sent.surfaceString()) # 어절의 표면형을 이어붙이되, 지정된 [delimiter]로 띄어쓰기 된 문장을 반환.

        print("# Analysis Result")
        # print(sent.singleLineString())

        for word in sent:
            print("Word [%s] %s = " % (word.getId(), word.getSurface()), end='')
            # 형태소의 어절 내 위치, 어절의 표면형 String.

            for morph in word:
                print("%s/%s " % (morph.getSurface(), morph.getTag()), end='')

            print()
```

    분석할 문장을 입력하세요>> 자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다. 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다. 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다. 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다.
    ===== Sentence =====
    ===== Sentence #$i =====
    자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할 수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나 다 .
    # Analysis Result
    Word [0] 자연어 = 자연/NNG 어/NNG 
    Word [1] 처리 = 처리/NNG 
    Word [2] 또는 = 또는/MAJ 
    Word [3] 자연 = 자연/NNG 
    Word [4] 언어 = 언어/NNG 
    Word [5] 처리는 = 처리/NNG 는/JX 
    Word [6] 인간의 = 인간/NNG 의/JKG 
    Word [7] 언어 = 언어/NNG 
    Word [8] 현상을 = 현상/NNG 을/JKO 
    Word [9] 컴퓨터와 = 컴퓨터/NNG 와/JKB 
    Word [10] 같은 = 같/VA 은/ETM 
    Word [11] 기계를 = 기계/NNG 를/JKO 
    Word [12] 이용해서 = 이용/NNG 하/XSV 아서/EC 
    Word [13] 모사 = 모사/NNG 
    Word [14] 할 = 하/VV ᆯ/ETM 
    Word [15] 수 = 수/NNB 
    Word [16] 있도록 = 있/VV 도록/EC 
    Word [17] 연구하고 = 연구/NNG 하/XSV 고/EC 
    Word [18] 이를 = 이/NP 를/JKO 
    Word [19] 구현하는 = 구현/NNG 하/XSV 는/ETM 
    Word [20] 인공지능의 = 인공/NNG 지능/NNG 의/JKG 
    Word [21] 주요 = 주요/NNG 
    Word [22] 분야 = 분야/NNG 
    Word [23] 중 = 중/NNB 
    Word [24] 하나 = 하나/NR 
    Word [25] 다 = 다/EF 
    Word [26] . = ./SF 
    ===== Sentence #$i =====
    자연 언어 처리는 연구 대상이 언어이 기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어인지 과학과 연관이 깊다 .
    # Analysis Result
    Word [0] 자연 = 자연/NNG 
    Word [1] 언어 = 언어/NNG 
    Word [2] 처리는 = 처리/NNG 는/JX 
    Word [3] 연구 = 연구/NNG 
    Word [4] 대상이 = 대상/NNG 이/JKS 
    Word [5] 언어이 = 언어/NNG 이/VCP 
    Word [6] 기 = 기/ETN 
    Word [7] 때문에 = 때문/NNB 에/JKB 
    Word [8] 당연하게도 = 당연/XR 하/XSA 게/EC 도/JX 
    Word [9] 언어 = 언어/NNG 
    Word [10] 자체를 = 자체/NNG 를/JKO 
    Word [11] 연구하는 = 연구/NNG 하/XSV 는/ETM 
    Word [12] 언어학과 = 언어/NNG 학/NNG 과/JC 
    Word [13] 언어 = 언어/NNG 
    Word [14] 현상의 = 현상/NNG 의/JKG 
    Word [15] 내적 = 내/NNG 적/XSN 
    Word [16] 기재를 = 기재/NNG 를/JKO 
    Word [17] 탐구하는 = 탐구/NNG 하/XSV 는/ETM 
    Word [18] 언어인지 = 언어/NNG 이/VCP ᆫ지/EC 
    Word [19] 과학과 = 과학/NNG 과/JC 
    Word [20] 연관이 = 연관/NNG 이/JKS 
    Word [21] 깊다 = 깊/VA 다/EF 
    Word [22] . = ./SF 
    ===== Sentence #$i =====
    구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계 학습 도구를 많이 사용하는 대표적인 분야이 다 .
    # Analysis Result
    Word [0] 구현을 = 구현/NNG 을/JKO 
    Word [1] 위해 = 위하/VV 아/EC 
    Word [2] 수학적 = 수학/NNG 적/XSN 
    Word [3] 통계적 = 통계/NNG 적/XSN 
    Word [4] 도구를 = 도구/NNG 를/JKO 
    Word [5] 많이 = 많이/MAG 
    Word [6] 활용하며 = 활용/NNG 하/XSV 며/EC 
    Word [7] 특히 = 특히/MAG 
    Word [8] 기계 = 기계/NNG 
    Word [9] 학습 = 학습/NNG 
    Word [10] 도구를 = 도구/NNG 를/JKO 
    Word [11] 많이 = 많이/MAG 
    Word [12] 사용하는 = 사용/NNG 하/XSV 는/ETM 
    Word [13] 대표적인 = 대표/NNG 적/XSN 이/VCP ᆫ/ETM 
    Word [14] 분야이 = 분야/NNG 이/VCP 
    Word [15] 다 = 다/EF 
    Word [16] . = ./SF 
    ===== Sentence #$i =====
    정보 검색 , QA 시스템 , 문서 자동 분류 , 신문 기사 클러스터링 , 대화형 Agent 등 다양한 응용이 이루어 지고 있다 .
    # Analysis Result
    Word [0] 정보 = 정보/NNG 
    Word [1] 검색 = 검색/NNG 
    Word [2] , = ,/SP 
    Word [3] QA = QA/SL 
    Word [4] 시스템 = 시스템/NNG 
    Word [5] , = ,/SP 
    Word [6] 문서 = 문서/NNG 
    Word [7] 자동 = 자동/NNG 
    Word [8] 분류 = 분류/NNG 
    Word [9] , = ,/SP 
    Word [10] 신문 = 신문/NNG 
    Word [11] 기사 = 기사/NNG 
    Word [12] 클러스터링 = 클러스터/NNG 링/JKO 
    Word [13] , = ,/SP 
    Word [14] 대화형 = 대화/NNG 형/XSN 
    Word [15] Agent = Agent/SL 
    Word [16] 등 = 등/NNB 
    Word [17] 다양한 = 다양/NNG 하/XSA ᆫ/ETM 
    Word [18] 응용이 = 응용/NNG 이/JKS 
    Word [19] 이루어 = 이루/VV 어/EC 
    Word [20] 지고 = 지/VX 고/EC 
    Word [21] 있다 = 있/VX 다/EF 
    Word [22] . = ./SF 
    분석할 문장을 입력하세요>> 
    

#### 6-3 의존구문분석 예시


```python
# 필요 모듈 import
from koalanlp.Util import initialize
from koalanlp.proc import Parser
from koalanlp import API
```


```python
# 패키지 초기화
# initialize(KKMA='LATEST')  #: HNN=2.0.4, ETRI=2.0.4
```


```python
parser = Parser(API.KKMA)
```


```python
while True:
    text = input("분석할 문장을 입력하세요>> ").strip()

    if len(text) == 0:
        break

    sentences = parser(text)

    for sent in sentences:
        print("===== Sentence =====")
        print(sent.singleLineString()) # 품사분석 결과를, 1행짜리 String으로 변환.
        print("# Dependency Parse result")

        dependencies = sent.getDependencies() # 의존구문분석을 했다면, 문장에 포함된 모든 의존구조의 목록을 돌려줌.
        if len(dependencies) > 0:
            for edge in dependencies:
                print("[%s]는 [%s]의 %s-%s" % (edge.getDependent().getSurface(),
                                             edge.getGovernor().getSurface() if edge.getGovernor() is not None else "ROOT",
                                             str(edge.getType()),
                                             str(edge.getDepType())))
        else:
            print("(Unexpected) NULL!")
```

    분석할 문장을 입력하세요>> 자연어 처리 또는 자연 언어 처리는 인간의 언어 현상을 컴퓨터와 같은 기계를 이용해서 모사 할수 있도록 연구하고 이를 구현하는 인공지능의 주요 분야 중 하나다. 자연 언어 처리는 연구 대상이 언어 이기 때문에 당연하게도 언어 자체를 연구하는 언어학과 언어 현상의 내적 기재를 탐구하는 언어 인지 과학과 연관이 깊다. 구현을 위해 수학적 통계적 도구를 많이 활용하며 특히 기계학습 도구를 많이 사용하는 대표적인 분야이다. 정보검색, QA 시스템, 문서 자동 분류, 신문기사 클러스터링, 대화형 Agent 등 다양한 응용이 이루어 지고 있다.
    ===== Sentence =====
    자연어/NNG 처리/NNG 또는/MAG 자연/NNG 언어/NNG 처리/NNG+는/JX 인간/NNG+의/JKG 언어/NNG 현상/NNG+을/JKO 컴퓨터/NNG+와/JKB 같/VA+은/ETM 기계/NNG+를/JKO 이용/NNG+하/XSV+어서/EC 모사/NNG 하/VV+ㄹ/ETM 수/NNB 있/VV+도록/EC 연구/NNG+하/XSV+고/EC 이/NP+를/JKO 구현/NNG+하/XSV+는/ETM 인공지능/NNG+의/JKG 주요/NNG 분야/NNG 중/NNB 하나/NR+이/VCP+다/EF+./SF
    # Dependency Parse result
    [하나다.]는 [ROOT]의 X-UNDEF
    [중]는 [하나다.]의 NP-None
    [분야]는 [중]의 NP-None
    [주요]는 [분야]의 NP-None
    [인공지능의]는 [주요]의 DP-MOD
    [구현하는]는 [인공지능의]의 DP-MOD
    [연구하고]는 [구현하는]의 VP-CNJ
    [있도록]는 [연구하고]의 VP-CNJ
    [수]는 [있도록]의 NP-AJT
    [할]는 [수]의 DP-MOD
    [이용해서]는 [할]의 VP-CNJ
    [기계를]는 [이용해서]의 NP-OBJ
    [같은]는 [기계를]의 DP-MOD
    [처리]는 [같은]의 NP-AJT
    [자연어]는 [처리]의 NP-None
    [또는]는 [같은]의 DP-MOD
    [처리는]는 [같은]의 NP-AJT
    [언어]는 [처리는]의 NP-None
    [자연]는 [언어]의 NP-None
    [현상을]는 [같은]의 NP-OBJ
    [언어]는 [현상을]의 NP-None
    [인간의]는 [언어]의 DP-MOD
    [컴퓨터와]는 [같은]의 NP-AJT
    [모사]는 [할]의 NP-AJT
    [이를]는 [구현하는]의 NP-OBJ
    ===== Sentence =====
    자연/NNG 언어/NNG 처리/NNG+는/JX 연구/NNG 대상/NNG+이/JKS 언어/NNG 이/VCP+기/ETN 때문/NNB+에/JKB 당연/XR+하/XSA+게/EC+도/JX 언어/NNG 자체/NNG+를/JKO 연구/NNG+하/XSV+는/ETM 언어/NNG+학과/NNG 언어/NNG 현상/NNG+의/JKG 내적/NNG 기재/NNG+를/JKO 탐구/NNG+하/XSV+는/ETM 언어/NNG 이/VCP+ㄴ지/EC 과학/NNG+과/JKB 연관/NNG+이/JKS 깊/VA+다/EF+./SF
    # Dependency Parse result
    [깊다.]는 [ROOT]의 X-UNDEF
    [인지]는 [깊다.]의 VP-CNJ
    [언어]는 [인지]의 NP-SBJ
    [탐구하는]는 [언어]의 DP-MOD
    [기재를]는 [탐구하는]의 NP-OBJ
    [내적]는 [기재를]의 NP-None
    [현상의]는 [내적]의 DP-MOD
    [언어]는 [현상의]의 NP-None
    [언어학과]는 [언어]의 NP-None
    [연구하는]는 [언어학과]의 DP-MOD
    [당연하게도]는 [연구하는]의 VP-CNJ
    [때문에]는 [당연하게도]의 NP-AJT
    [이기]는 [때문에]의 NP-MOD
    [처리는]는 [이기]의 NP-SBJ
    [언어]는 [처리는]의 NP-None
    [자연]는 [언어]의 NP-None
    [대상이]는 [이기]의 NP-SBJ
    [연구]는 [대상이]의 NP-None
    [언어]는 [이기]의 NP-SBJ
    [자체를]는 [연구하는]의 NP-OBJ
    [언어]는 [자체를]의 NP-None
    [과학과]는 [깊다.]의 NP-AJT
    [연관이]는 [깊다.]의 NP-SBJ
    ===== Sentence =====
    구현/NNG+을/JKO 위하/VV+어/EC 수학적/NNG 통계적/NNG 도구/NNG+를/JKO 많이/MAG 활용/NNG+하/XSV+며/EC 특히/MAG 기계/NNG+학습/NNG 도구/NNG+를/JKO 많이/MAG 사용/NNG+하/XSV+는/ETM 대표적/NNG+이/VCP+ㄴ/ETM 분야/NNG+이/VCP+다/EF+./SF
    # Dependency Parse result
    [위해]는 [ROOT]의 X-UNDEF
    [구현을]는 [위해]의 NP-OBJ
    [분야이다.]는 [ROOT]의 X-UNDEF
    [대표적인]는 [분야이다.]의 DP-MOD
    [사용하는]는 [대표적인]의 DP-MOD
    [활용하며]는 [사용하는]의 VP-CNJ
    [도구를]는 [활용하며]의 NP-OBJ
    [통계적]는 [도구를]의 NP-None
    [수학적]는 [통계적]의 NP-None
    [많이]는 [활용하며]의 DP-MOD
    [도구를]는 [사용하는]의 NP-OBJ
    [기계학습]는 [도구를]의 NP-None
    [많이]는 [사용하는]의 DP-MOD
    [특히]는 [많이]의 DP-MOD
    ===== Sentence =====
    정보/NNG 검색/NNG+,/SP QA/SL 시스템/NNG+,/SP 문서/NNG 자동/NNG 분류/NNG+,/SP 신문/NNG+기사/NNG 클러/NF 스터링/NNG+,/SP 대화/NNG+형/XSN Agent/SL 등/NNB 다양/NNG+하/XSA+ㄴ/ETM 응용/NNG+이/JKS 이루/VV+어/EC 지/VX+고/EC 있/VX+다/EF+./SF
    # Dependency Parse result
    [검색,]는 [ROOT]의 X-UNDEF
    [정보]는 [검색,]의 NP-None
    [QA]는 [ROOT]의 X-UNDEF
    [시스템,]는 [ROOT]의 X-UNDEF
    [분류,]는 [ROOT]의 X-UNDEF
    [자동]는 [분류,]의 NP-None
    [문서]는 [자동]의 NP-None
    [스터링,]는 [ROOT]의 X-UNDEF
    [클러]는 [스터링,]의 NP-None
    [신문기사]는 [클러]의 NP-None
    [Agent]는 [ROOT]의 X-UNDEF
    [있다.]는 [ROOT]의 X-UNDEF
    [지고]는 [있다.]의 VP-CNJ
    [이루어]는 [지고]의 VP-CNJ
    [응용이]는 [이루어]의 NP-SBJ
    [다양한]는 [응용이]의 DP-MOD
    [대화형]는 [다양한]의 NP-AJT
    [등]는 [다양한]의 NP-None
    분석할 문장을 입력하세요>> 
    

#### 6-4 사전 사용 예시


```python
# 필요 모듈 import
from koalanlp.Util import initialize
from koalanlp.proc import Dictionary
from koalanlp.types import POS
from koalanlp import API
```


```python
# 패키지 초기화
# initialize(KMR='LATEST', KKMA='LATEST')  #: HNN, EUNJEON, KKMA, OKT
```


```python
dict = Dictionary(API.KMR)
```


```python
dict.addUserDictionary(("하동균", POS.NNP))
```


```python
dict.addUserDictionary(("나비야", POS.NNP))
```


```python
# print(dict.contains("하동균", POS.NNP, POS.NNG))
```


```python
print(("하동균", POS.NNP) in dict)
```

    True
    


```python
print(', '.join(str(item) for item in dict.getNotExists(True, ("하동균", POS.NNP))))
```

    ('하동균', NNP)
    


```python
print("# 접사 목록")
```

    # 접사 목록
    


```python
# for word in dict.getBaseEntries(POS.isAffix):
#     print("%s (Tag=%s)" % (word[0], str(word[1])))
```


```python
print("# 사용자 사전 목록")
```

    # 사용자 사전 목록
    


```python
for word in dict.getItems():
    print("%s (Tag=%s)" % (word[0], str(word[1])))
```

    코알라NLP (Tag=NNP)
    코모란 (Tag=NNP)
    꼬꼬마 (Tag=NNP)
    하동균 (Tag=NNP)
    나비야 (Tag=NNP)
    


```python
print("---- 타 분석기 사전 불러오기 ----")
```

    ---- 타 분석기 사전 불러오기 ----
    


```python
# dict.importFrom(Dictionary(API.KKMA), False, lambda t: t.isNoun())
```


```python
# print("# 사용자 사전 목록 (30개)")
```


```python
# for word in list(dict.getItems())[:30]:
#    print("%s (Tag=%s)" % (word[0], str(word[1])))
```

#### 6-5 ETRI 분석기 사용 예시


```python
# 필요 모듈 import
from koalanlp.Util import initialize
from koalanlp.proc import RoleLabeler
from koalanlp import API
import os
import sys
```


```python
#API_KEY = os.getenv('API_KEY') if len(sys.argv) <= 1 else sys.argv[1]
```


```python
#if API_KEY is None:
#    print("ETRI 분석기는 API 키 설정이 필요합니다. API_KEY 환경변수를 설정하거나 프로그램 인자로 key를 전달해주세요.")
#    exit(1)
```


```python
# 패키지 초기화
# initialize(ETRI='LATEST')
```


```python
# labeler = RoleLabeler(API.ETRI, apiKey=API_KEY)
```


```python
# recognizer = EntityRecognizer(apiKey=API_KEY)
```


```python
# parser = Parser(apiKey=API_KEY)
```


```python
# tagger = Tagger(apiKey=API_KEY)
```


```python
# while True:
#    text = input("분석할 문장을 입력하세요>> ").strip()
#
#    if len(text) == 0:
#        break

#    sentences = labeler(text)

#    for sent in sentences:
#        print("===== Sentence =====")
#        print(sent.singleLineString())

#        entities = sent.getEntities()
#        if len(entities) > 0:
#            print("# Named Entities")

#            for entity in entities:
#                print("[%s]는 %s 유형의 개체명으로, 형태소 [%s]를 포함합니다." % (entity.getSurface(),
#                                                                str(entity.getFineLabel()),
#                                                                " ".join(str(m) for m in entity)))

#        dependencies = sent.getDependencies()
#        if len(dependencies) > 0:
#            print("# Dependency Parse")

#            for edge in dependencies:
#                print("[%s]는 [%s]의 %s-%s" % (edge.getDependent().getSurface(),
#                                             edge.getGovernor().getSurface() if edge.getGovernor() is not None else "ROOT",
#                                             str(edge.getType()),
#                                             str(edge.getDepType())))

#        roles = sent.getRoles()
#        if len(roles) > 0:
#            print("# Role Labeling")

#            for edge in roles:
#                print("[%s]는 [%s]의 %s" % (edge.getArgument().getSurface(),
#                                          edge.getPredicate().getSurface(),
#                                          str(edge.getLabel())))
```

#### 6-6 확장 기능 사용 예시


```python
# 필요 모듈 import
from koalanlp.Util import initialize
from koalanlp import ExtUtil
```


```python
# 패키지 초기화
# initialize(CORE="LATEST")
```

- 한글 여부 확인


```python
print("ExtUtil.isHangul('가') = %s" % (str(ExtUtil.isHangul('가'))))
```

    ExtUtil.isHangul('가') = [True]
    


```python
print("ExtUtil.isHangul('ㄱ') = %s" % (str(ExtUtil.isHangul('ㄱ'))))
```

    ExtUtil.isHangul('ㄱ') = [True]
    


```python
print("ExtUtil.isHangul('a') = %s" % (str(ExtUtil.isHangul('a'))))
```

    ExtUtil.isHangul('a') = [False]
    


```python
print("ExtUtil.isHangulEnding(\"갤럭시S는\") = %s" % (ExtUtil.isHangulEnding("갤럭시S는")))
```

    ExtUtil.isHangulEnding("갤럭시S는") = True
    


```python
print("ExtUtil.isHangulEnding(\"abc\") = %s" % (ExtUtil.isHangulEnding("abc")))
```

    ExtUtil.isHangulEnding("abc") = False
    


```python
print("ExtUtil.isHangulEnding(\"보기 ㄱ\") = %s" % (ExtUtil.isHangulEnding("보기 ㄱ")))
```

    ExtUtil.isHangulEnding("보기 ㄱ") = True
    


```python
print("ExtUtil.isCompleteHangul('가') = %s" % (ExtUtil.isCompleteHangul('가')))
```

    ExtUtil.isCompleteHangul('가') = [True]
    


```python
print("ExtUtil.isCompleteHangul('ㄱ') = %s" % (ExtUtil.isCompleteHangul('ㄱ')))
```

    ExtUtil.isCompleteHangul('ㄱ') = [False]
    


```python
print("ExtUtil.isCompleteHangul('a') = %s" % (ExtUtil.isCompleteHangul('a')))
```

    ExtUtil.isCompleteHangul('a') = [False]
    

- 종성으로 끝나는지 확인


```python
print("ExtUtil.isJongsungEnding('각') = %s" % (ExtUtil.isJongsungEnding('각')))
```

    ExtUtil.isJongsungEnding('각') = True
    


```python
print("ExtUtil.isJongsungEnding('가') = %s" % (ExtUtil.isJongsungEnding('가')))
```

    ExtUtil.isJongsungEnding('가') = False
    


```python
print("ExtUtil.isJongsungEnding('m') = %s" % (ExtUtil.isJongsungEnding('m')))
```

    ExtUtil.isJongsungEnding('m') = False
    

- 초, 중, 종성 한글 자모 값으로 분해


```python
print("ExtUtil.getChosung('가') = %s" % (ExtUtil.getChosung('가')))
```

    ExtUtil.getChosung('가') = ['ᄀ']
    


```python
print("ExtUtil.getJungsung('가') = %s" % (ExtUtil.getJungsung('가')))
```

    ExtUtil.getJungsung('가') = ['ᅡ']
    


```python
print("ExtUtil.getJongsung('가') = %s" % (ExtUtil.getJongsung('가')))
```

    ExtUtil.getJongsung('가') = [None]
    


```python
print("ExtUtil.getJongsung('각') = %s" % (ExtUtil.getJongsung('각')))
```

    ExtUtil.getJongsung('각') = ['ᆨ']
    


```python
print("ExtUtil.getChosung('ㄱ') = %s" % (ExtUtil.getChosung('ㄱ')))
```

    ExtUtil.getChosung('ㄱ') = [None]
    


```python
print("ExtUtil.dissembleHangul('가') = %s" % (ExtUtil.dissembleHangul('가')))
```

    ExtUtil.dissembleHangul('가') = 가
    


```python
print("ExtUtil.dissembleHangul('각') = %s" % (ExtUtil.dissembleHangul('각')))
```

    ExtUtil.dissembleHangul('각') = 각
    


```python
print("ExtUtil.dissembleHangul(\"가각\") = %s" % (ExtUtil.dissembleHangul("가각")))
```

    ExtUtil.dissembleHangul("가각") = 가각
    

- 종성 한글 자모값으로 변환


```python
print("ExtUtil.ChoToJong['ㄵ'] = %s" % (ExtUtil.ChoToJong['ㄵ']))
```

    ExtUtil.ChoToJong['ㄵ'] = ᆬ
    

- 한글 자모 범위에 속하는 초, 중, 종성인지 확인


```python
print("ExtUtil.isChosungJamo('ㄱ') = %s" % (ExtUtil.isChosungJamo('ㄱ')))
```

    ExtUtil.isChosungJamo('ㄱ') = [False]
    


```python
print("ExtUtil.isChosungJamo('\u1100') = %s" % (ExtUtil.isChosungJamo('\u1100')))
```

    ExtUtil.isChosungJamo('ᄀ') = [True]
    


```python
print("ExtUtil.isJungsungJamo('ㅏ') = %s" % (ExtUtil.isJungsungJamo('ㅏ')))
```

    ExtUtil.isJungsungJamo('ㅏ') = [False]
    


```python
print("ExtUtil.isJungsungJamo('\u1161') = %s" % (ExtUtil.isJungsungJamo('\u1161')))
```

    ExtUtil.isJungsungJamo('ᅡ') = [True]
    


```python
print("ExtUtil.isJongsungJamo('ㄱ') = %s" % (ExtUtil.isJongsungJamo('ㄱ')))
```

    ExtUtil.isJongsungJamo('ㄱ') = [False]
    


```python
print("ExtUtil.isJongsungJamo('\u11A8') = %s" % (ExtUtil.isJongsungJamo('\u11A8')))
```

    ExtUtil.isJongsungJamo('ᆨ') = [True]
    

- 한글 자모 결합


```python
print("ExtUtil.assembleHangulTriple('\u1100', '\u1161', None) = %s" % (ExtUtil.assembleHangulTriple('\u1100', '\u1161', None)))
```

    ExtUtil.assembleHangulTriple('ᄀ', 'ᅡ', None) = 가
    


```python
print("ExtUtil.assembleHangulTriple('\u1100', '\u1161', '\u11AB') = %s" % (ExtUtil.assembleHangulTriple('\u1100', '\u1161', '\u11AB')))
```

    ExtUtil.assembleHangulTriple('ᄀ', 'ᅡ', 'ᆫ') = 간
    


```python
print("ExtUtil.assembleHangul(\"\u1100\u1161\u1102\u1161\u11ab\") = %s" % (ExtUtil.assembleHangul("\u1100\u1161\u1102\u1161\u11ab")))
```

    ExtUtil.assembleHangul("가난") = 가난
    


```python
print("ExtUtil.assembleHangulTriple(jung='\u1161') = %s" % (ExtUtil.assembleHangulTriple(jung='\u1161')))
```

    ExtUtil.assembleHangulTriple(jung='ᅡ') = 아
    


```python
print("ExtUtil.assembleHangulTriple('\u1100', None, '\u11A8') = %s" % (ExtUtil.assembleHangulTriple('\u1100', None, '\u11A8')))
```

    ExtUtil.assembleHangulTriple('ᄀ', None, 'ᆨ') = 극
    


```python
print("ExtUtil.alphaToHangul(\"ABCD\") = %s" % (ExtUtil.alphaToHangul("ABCD")))
```

    ExtUtil.alphaToHangul("ABCD") = 에이비씨디
    


```python
print("ExtUtil.alphaToHangul(\"갤럭시S\") = %s" % (ExtUtil.alphaToHangul("갤럭시S")))
```

    ExtUtil.alphaToHangul("갤럭시S") = 갤럭시에스
    


```python
print("ExtUtil.alphaToHangul(\"cup\") = %s" % (ExtUtil.alphaToHangul("cup")))
```

    ExtUtil.alphaToHangul("cup") = 씨유피
    


```python
print("ExtUtil.isAlphaPronounced(\"에스비에스\") = %s" % (ExtUtil.isAlphaPronounced("에스비에스")))
```

    ExtUtil.isAlphaPronounced("에스비에스") = True
    


```python
print("ExtUtil.isAlphaPronounced(\"갤럭시에스\") = %s" % (ExtUtil.isAlphaPronounced("갤럭시에스")))
```

    ExtUtil.isAlphaPronounced("갤럭시에스") = False
    


```python
print("ExtUtil.hangulToAlpha(\"갤럭시에스\") = %s" % (ExtUtil.hangulToAlpha("갤럭시에스")))
```

    ExtUtil.hangulToAlpha("갤럭시에스") = 갤럭시S
    


```python
print("ExtUtil.hangulToAlpha(\"에이디에이치디\") = %s" % (ExtUtil.hangulToAlpha("에이디에이치디")))
```

    ExtUtil.hangulToAlpha("에이디에이치디") = ADHD
    


```python
print("ExtUtil.isHanja('國') = %s" % (ExtUtil.isHanja('國')))
```

    ExtUtil.isHanja('國') = [True]
    


```python
print("ExtUtil.isCJKHanja('國') = %s" % (ExtUtil.isCJKHanja('國')))
```

    ExtUtil.isCJKHanja('國') = [True]
    


```python
print("ExtUtil.hanjaToHangul(\"國篇\") = %s" % (ExtUtil.hanjaToHangul("國篇")))
```

    ExtUtil.hanjaToHangul("國篇") = 국편
    


```python
print("ExtUtil.hanjaToHangul(\"國篇은 오늘\") = %s" % (ExtUtil.hanjaToHangul("國篇은 오늘")))
```

    ExtUtil.hanjaToHangul("國篇은 오늘") = 국편은 오늘
    


```python
print("ExtUtil.hanjaToHangul(\"300 兩의 돈\") = %s" % (ExtUtil.hanjaToHangul("300 兩의 돈")))
```

    ExtUtil.hanjaToHangul("300 兩의 돈") = 300 냥의 돈
    


```python
print("ExtUtil.hanjaToHangul(\"樂園\") = %s" % (ExtUtil.hanjaToHangul("樂園")))
```

    ExtUtil.hanjaToHangul("樂園") = 낙원
    


```python
finalize()
```

    [1mpy4j.java_gateway[0m Callback Server Shutting Down
    