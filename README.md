## 프로젝트 목표
+ 신문 원문 PDF 파일로부터 온라인 서비스화가 가능한 형태로 뉴스기사 생성하기
+ PDF에서 텍스트 추출 시 신문 원문 형태의 특징으로 인해 디지털화된 뉴스 기사를 생성하는 것이 쉽지 않은데,  
 자연어처리 기술을 이용해 이런 문제점을 해결할 수 있을 것으로 기대
  + 1. 뉴스 카테고리 분류(=>IT/과학, 문화, 경제, 연예, 건강, 라이프, 정치, 사회, 스포츠)
  + 2. 한글 띄어쓰기 검사(=>신문 원문 형태의 특성 상, 단락 폭이 좁아서 줄바꿈이 자주 일어남)
  + 3. 단락 연결 여부 검사(=>동일 뉴스 기사에 포함된 단락이 맞는지)



## Data
+ 학습용 : [국립국어원 모두의 말뭉치 - 신문 말뭉치 2020](https://corpus.korean.go.kr/)
+ 검증용 : [서울신문의 PDF](https://github.com/yeonok93/CP2/files/8357499/01100611.20170102000010100.PDF)


##
+ 출처
  + [SKTBrain/KoBERT](https://github.com/SKTBrain/KoBERT/)
  + [monologg/KoBERT-Transformers](https://github.com/monologg/KoBERT-Transformers)
  + [Transformers와 Tensorflow를 활용한 BERT Fine-tuning](https://velog.io/@jaehyeong/Fine-tuning-Bert-using-Transformers-and-TensorFlow)
-----

## 1. 뉴스 카테고리 분류
+ 데이터셋 : 모두의 말뭉치에서 총 5,352,250개의 문장 추출
+  

+ 출처
  + https://github.com/Doheon/NewsClassification-KoBERT
  + [PyTorch Lightning](https://pytorch-lightning.readthedocs.io/en/latest/)
  + [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 01. 데이터 준비](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_01/)
  + [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 02. 데이터 전처리](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_02/)
  + [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 03. 모델](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_03/)
  + [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 04. 학습](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_04/)

## 2. 한글 띄어쓰기 검사
+ Bert를 이용한 모델 생성, PyKoSpacing과 성능 비교
+ 
+ 출처
  + https://wikidocs.net/92961

## 3. 단락 연결 여부 검사 
