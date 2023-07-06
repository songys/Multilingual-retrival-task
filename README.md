# 다국어 검색 과제(Multilingual retrieval task)
## 1. 정의 
 쿼리와 다른 언어로 작성된 문서 간의 관련성을 학습하고 토큰 임베딩의 정렬을 통해  사용자의 요구에 맞는 답변을 제공



## 2. 필요성

- General purpose language model이 틀린 답(hallucination)을 말하거나 최근 일어난 일은 대답하지 못함 
- 두 가지 문제 모두 지식의 부족 때문인 경우가 많음
(예) 세종대왕 맥북 던짐사건에 대해 알려줘, 어제 날씨 어땠어,  현재 대한민국의 대통령은 누구지?


- 대부분의 초거대 언어 모델은 위와 같은 질문에 대답하지 못하거나 잘못된 답변을 도출할 가능성이 높음
- 이를 해결하고자 하는 방법론의 하나로 다국어 검색의 필요성이 있음


## 3. 다국어 검색  과정

(1) 다국어 Query 입력  : 다양한 언어로 입력된 쿼리
 (관련 서비스 )  2lingual Google Search URL: 2lingual Google Search

(2) 다국어 문서 이해
쿼리와 연관성이 있을 것으로 보이는 다양한 언어로 작성된 문서를 이해하고 해석해야 단순 링크 전달 이상의 서비스가 가능함
(관련 서비스) ChatPDF  URL : ChatPDF - Chat with any PDF!


(3) 유저가 요청한 결과를 도출하는 것이 목적
    검색은 semantic search의 방식으로, 답변은 TOP-K개의 결과를 통한 문장 생성을 통해 이루어지는 것이 일반적
(관련 서비스) cohere
URL : https://www.google.com/url?q=https://docs.cohere.com/docs/multilingual-language-models&sa=D&source=docs&ust=1688582768785468&usg=AOvVaw29yJ7vdoi6dvHJcx8alBCe
![4](https://github.com/songys/Multilingual-retrival-task/assets/8298703/ab50c4b4-7d3e-4543-9067-9e1c613e6d2a)


- 기본적으로 Q&A 데이터가 필요함(단문으로만 할 것인지, 장문으로도 할 것인지는 결정해야 함)
 (한국어 참고 데이터)  KLUE-MRC(klue-benchmark.com)
- 사실인지 아닌지 결정(fact checking)을 자동화하기 어려움
- fact-knowledge 확인에는 휴먼 리소스가 필요하고 사람에게도 어려운 과제가 될 수 있음
- 대화에 포함된 슬롯의 사실성을 체크하는 방식이 도움이 될 수 있을 듯함
- cohere 이상의 성능을 내기 위해서는 양질의 임베딩이 절대적으로 필요함
- 외부 지식의 확장으로 최근의 지식을 찾아서 대답하거나 신뢰할 수 있는 답변을 도출하는 것에 의의가 있음
- 오픈 도메인 챗봇 이상의 확장된 대화 모델이 되기 위해서는 인과 추론에 대한 깊이가 깊어질 필요가 있음
(예) 세종 대왕은 맥북이 생산되기 전에 인물이므로 세종 대왕은 맥북을 던질 수 없다는 추론 능력이 요청됨
- 아래 3번에서 언급할 대부분의 지식 확장 데이터는 위키 피디아 기반이라서 전문 지식이 부족한 한계가 있으나 현재로서는 최선의 결과인 듯함
-추론 능력과 관련하여 Natural Language Inference(KLUE 데이터 참고)나 Adversarial Natural Language Inference (ANLI, Nie et al.) 데이터로 해결하고자 하는 시도가 있었고 추론의 깊이와 관련하여서는 Multi-hop Question Answering 등으로 해결하고자 하는 시도가 있음


- 주어진 질문이 얼마나 사실을 필요로 하는가와 관련된 논의는 Mr. TyDi의 negative_passages (list)나 Multi-hop Question Answering이 참조할 필요가 있음


# 4. 데이터
## (1) Large-Scale CLIR Dataset (jhu.edu)
<논문> Shota Sasaki, Shuo Sun, Shigehiko Schamoni, Kevin Duh, and Kentaro Inui
Cross-lingual Learning-to-Rank with Shared Representations, Proceedings of the 2018 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies (NAACL-HLT), New Orleans, LA, USA. June 2018.


![7](https://github.com/songys/Multilingual-retrival-task/assets/8298703/03ef7e81-9e96-47b7-b101-992222c8f824)


위키피디아 기반

![1](https://github.com/songys/Multilingual-retrival-task/assets/8298703/d263618f-6308-44dd-9142-d58d5652f5c7)



## (2)  Mr. TyDi
(논문) Zhang, Xinyu, Xueguang Ma, Peng Shi, and Jimmy Lin. 2021. Mr. TyDi: A Multi-lingual Benchmark for Dense Retrieval. In Proceedings of the 1st Workshop on Multilingual Representation Learning, pages 127–137, Punta Cana, Dominican Republic. Association for Computational Linguistics.
(깃허브) https://github.com/castorini/mr.tydi

- 11개 언어의 단일 언어 검색을 위한 벤치마크 데이터 세트
- 위키백과에서 많이 검색된 문서 제공
- 해당 문서 내에서 각 구절의 관련성을 표시하고 가능한 경우 최소한의 답변 범위를 식별하도록 함


![tidy](https://github.com/songys/Multilingual-retrival-task/assets/8298703/4fafe2e5-74cc-455d-b455-c45cbdf4e2f1)


데이터 예시(castorini/mr-tydi · Datasets at Hugging Face)

|:---:|:-----------------:|:-----------:|:--------:|
|query_id (string)|query (string)|positive_passages (list)|negative_passages (list)|
|"1"|"로마의 면적은 서울시의 2배인가요?" | [ { "docid": "3228#0", "text": "로마()는 이탈리아의 수도이자 라치오주의 주도로, 테베레 강 연안에 있다. 로마시의 행정구역 면적은 1,285km2로 서울시의 2배정도이고, 2014년 인구는 290여만명이다. 로마시 권역의 인구는 430여만명이다. [1] 로마 대도시현의 인구는 400만이 넘지만 밀라노나 나폴리 대도시현에 비해 면적이 3~4배 넓은 편이고 되려 로마시의 면적과 밀라노와 나폴리의 대도시현의 면적이 비슷하므로 세 도시 모두 300만 정도로 비슷한 규모의 도시라 볼 수 있다.", "title": "로마" } ] | [ { "docid": "819462#0", "text": "한성백제박물관(漢城百濟博物館, )은 대한민국 서울특별시 송파구 방이동 88-20에 위치한 서울시립박물관이다. 올림픽공원 내부에 위치하고 있으며, 지하3층 지상2층으로 나뉘어져 있다. 상설전시는 서울의 선사·고대문화를 중심으로 백제 탄생 이전과 탄생 후 멸망까지의 내용으로 구성되어 있다. 전시 공간은 지하 2층부터 지상 1층까지이다. 2012년 4월 30일 개관하였고, 총면적은 19,423m 정도이며, 대지면적은 14,894m, 건축면적은 2,901m이다. 관장은(중략)|

위의 데이터의 확장
https://github.com/beir-cellar/beir/wiki/Multilingual-datasets


## (3) XQA-DST
(논문) Zhou, Han, Ignacio Iacobacci and Pasquale Minervini. “XQA-DST: Multi-Domain and Multi-Lingual Dialogue State Tracking.” Findings (2022).

것허브 :  thunlp/XQA: Dataset and baseline for ACL 2019 paper "XQA: A Cross-lingual Open-domain Question Answering Dataset" (github.com)

(questions, answers, and top-10 relevant articles wiki dumps) 
Wikipedia.org에서 Did you know~로 시작하는 페이지 질문을 추출

![did](https://github.com/songys/Multilingual-retrival-task/assets/8298703/ca424cd7-725d-4e85-b11b-bdddd74fa150)



데이터 크기와 포함 언어 (한국어가 없지만 번역 등의 방법으로 구축은 어렵지 않을 듯함)


![xqa](https://github.com/songys/Multilingual-retrival-task/assets/8298703/b4f7d5f1-e400-438f-9dd6-bc77e8a8519d)


(4) Multi-hop Question Answering

(관련 논문 ) Yang, Zhilin, Peng Qi, Saizheng Zhang, Yoshua Bengio, William Cohen, Ruslan Salakhutdinov, and Christopher D. Manning. 2018. HotpotQA: A Dataset for Diverse, Explainable Multi-hop Question Answering. In Proceedings of the 2018 Conference on Empirical Methods in Natural Language Processing, pages 2369–2380, Brussels, Belgium. Association for Computational Linguistics.



![2](https://github.com/songys/Multilingual-retrival-task/assets/8298703/11b0e0a4-1993-4c4a-b56f-156ce622fb37)




참고 문헌 및 자료 링크
Cross-language information retrieval - Wikipedia

ryanzhumich/awesome-clir: A curated list of resources for Cross-lingual Information Retrieval (CLIR). (github.com)

notes/Information Retrieval.md at master · brylevkirill/notes · GitHub

https://paperswithcode.com/sota/zero-shot-text-search-on-beir
Multilingual datasets · beir-cellar/beir Wiki (github.com)
BIG-Bench Hard (BBH; Suzgun et al., 2022) and the Bias  Benchmark  for  QA  (BBQ;   Parrish  et  al., 2022).
SQuAD Data, TriviaQA Data 등


