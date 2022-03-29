## 프로젝트 목표
+ 신문 원문 PDF 파일로부터 온라인 서비스화가 가능한 형태로 뉴스기사 생성하기
+ PDF에서 텍스트 추출 시 신문 원문 형태의 특징으로 인해 디지털화된 뉴스 기사를 생성하는 것이 쉽지 않은데,  
 자연어처리 기술을 이용해 이런 문제점을 해결할 수 있을 것으로 기대
  + 1. 뉴스 카테고리 분류(=> 생활, 사회, 스포츠, 정치, 경제, 연예, IT/과학, 미용/건강, 문화)
  + 2. 한글 띄어쓰기 검사(=>신문 원문 형태의 특성 상, 단락 폭이 좁아서 줄바꿈이 자주 일어남)
  + 3. 단락 연결 여부 검사(=>동일 뉴스 기사에 포함된 단락이 맞는지)



## Data
+ 학습용 : [국립국어원 모두의 말뭉치 - 신문 말뭉치 2020](https://corpus.korean.go.kr/)
+ 검증용 : [서울신문의 PDF](https://github.com/yeonok93/CP2/files/8357499/01100611.20170102000010100.PDF)


##
+ 참고자료
  + [SKTBrain/KoBERT](https://github.com/SKTBrain/KoBERT/)
  + [monologg/KoBERT-Transformers](https://github.com/monologg/KoBERT-Transformers)
  + [Transformers와 Tensorflow를 활용한 BERT Fine-tuning](https://velog.io/@jaehyeong/Fine-tuning-Bert-using-Transformers-and-TensorFlow)
-----

## 1. 뉴스 카테고리 분류
+ 데이터셋 : 모두의 말뭉치에서 총 5,352,250개의 문장 추출
+  
+ 결과
 + 파라미터 :
 + train_acc :
 + test_acc :

+ 참고자료
  + https://github.com/Doheon/NewsClassification-KoBERT
  + [PyTorch Lightning](https://pytorch-lightning.readthedocs.io/en/latest/)
  + [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 01. 데이터 준비](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_01/)
  + [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 02. 데이터 전처리](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_02/)
  + [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 03. 모델](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_03/)
  + [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 04. 학습](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_04/)

## 2. 한글 띄어쓰기 검사
+ 데이터셋 : 
  + ['o','『', '(', '┌','│', '└', 'ㄴ', '┌','├','◎', '[', '■', 'ㄱ', '-', '.', '<']로 시작하는 문장 제거
  + ['쪽', '-', '>', ']']로 끝나는 문장 제거
  + [')']
+ Bert를 이용한 모델 생성, PyKoSpacing과 성능 비교
+ 결과
 + 파라미터 :
 + train_acc :
 + test_acc :

+ 참고자료
  + https://wikidocs.net/92961
  + https://wandb.ai/wandb_fc/korean/reports/Weights-Biases-Pytorch-Lightning---VmlldzozNzAxOTg
  + https://www.slideshare.net/TaekyoonChoi/taekyoon-choi-pycon
  + https://huggingface.co/docs/transformers/v4.17.0/en/index
  + https://linuxtut.com/en/9f37631561379ca06ee5/
  + https://velog.io/@seolini43/KOBERT%EB%A1%9C-%EB%8B%A4%EC%A4%91-%EB%B6%84%EB%A5%98-%EB%AA%A8%EB%8D%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-%ED%8C%8C%EC%9D%B4%EC%8D%ACColab
  + https://notebook.community/huggingface/pytorch-transformers/notebooks/02-transformers

## 3. 단락 연결 여부 검사 
+ BertForNextSentencePrediction 모델 이용, 모두의말뭉치 데이터로 Fine Tuning
+ 
 + 파라미터 :
 + train_acc :
 + test_acc :
