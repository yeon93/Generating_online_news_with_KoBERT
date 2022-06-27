# 신문 원문 PDF 파일로부터 온라인 서비스화가 가능한 형태로 뉴스기사 생성하기
NEXTLAB과의 기업협업 프로젝트

## 프로젝트 기획 배경
OCR 기술을 이용해 PDF에서 텍스트 추출 시 신문 원문 형태의 특성으로 인해  
디지털화된 뉴스 기사를 생성하는 것이 쉽지 않음  
![image](https://user-images.githubusercontent.com/88722429/175818388-84f3b942-e716-4bab-a7b4-292199a061c1.png)

자연어처리 기술을 이용해 이런 문제점을 해결할 수 있을 것으로 기대  
> 필요한 알고리즘들은?  
1. 한글 띄어쓰기 검사  
신문 원문 형태의 특성 상, 단락 폭이 좁아서 줄바꿈이 자주 일어나기 때문에  
원래 띄워서 써야 할 부분과 그렇지 않은 부분이 혼동될 가능성이 높음
2. 단락 연결 여부 검사  
동일 뉴스 기사에 포함된 단락을 동일 뉴스로 맞게 처리했는지 확인
3. 뉴스 카테고리 분류  
생활, 사회, 스포츠, 정치, 경제, 연예, IT/과학, 미용/건강, 문화 총 9가지 카테고리로 분류



## Data
+ 학습용 : [국립국어원 모두의 말뭉치 - 신문 말뭉치 2020](https://corpus.korean.go.kr/)
+ 검증용 : [서울신문의 PDF](https://github.com/yeonok93/CP2/files/8357499/01100611.20170102000010100.PDF)
+ 참고자료
  + [SKTBrain/KoBERT](https://github.com/SKTBrain/KoBERT/)
  + [monologg/KoBERT-Transformers](https://github.com/monologg/KoBERT-Transformers)
  + [Transformers와 Tensorflow를 활용한 BERT Fine-tuning](https://velog.io/@jaehyeong/Fine-tuning-Bert-using-Transformers-and-TensorFlow)
  + https://wikidocs.net/92961
  + https://wandb.ai/wandb_fc/korean/reports/Weights-Biases-Pytorch-Lightning---VmlldzozNzAxOTg
  + https://www.slideshare.net/TaekyoonChoi/taekyoon-choi-pycon
  + https://huggingface.co/docs/transformers/v4.17.0/en/index
  + https://linuxtut.com/en/9f37631561379ca06ee5/
  + https://velog.io/@seolini43/KOBERT%EB%A1%9C-%EB%8B%A4%EC%A4%91-%EB%B6%84%EB%A5%98-%EB%AA%A8%EB%8D%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-%ED%8C%8C%EC%9D%B4%EC%8D%ACColab
  + https://notebook.community/huggingface/pytorch-transformers/notebooks/02-transformers
-----


## 1. 한글 띄어쓰기 검사
+ 데이터 전처리
  + ['o','『', '(', '┌','│', '└', 'ㄴ', '┌','├','◎', '[', '■', 'ㄱ', '-', '.', '<']로 시작하는 문장 제거
  + ['쪽', '-', '>', ']']로 끝나는 문장 제거
  + [')']
+ KoBERT를 이용한 모델 생성
+ 결과
  + test_acc : val_loss = 0.0308, val_f1 = 0.9300
+ 문제점
  + 한글의 띄어쓰기는 매우 다양한 요소에 의해 결정되고, 한국인들도 완벽하게 구사하지 못하는만큼,   
    대량의 데이터를 딥러닝 모델에 학습시켜야만 성능 향상에 도움이 될 것이라고 생각
  + 하지만, 장비의 한계로 인해 모두의 말뭉치 총 535만여 개의 데이터 중 20만개만 사용



## 2. 단락 연결 여부 검사 
아이디어 1. 
+ 2개 단락의 유사도를 파악하기 위해 BertForNextSentencePrediction 모델 이용
+ 실제로 전후 관계인 단락(0, True)과 실제로는 연결되지 않지만 랜덤으로 이어붙인 단락(1, False)을 학습
+ 학습된 모델에 2개 단락을 입력했을 때 True/False를 잘 판별해낸다면 단락 유사도 파악에 사용할 수 있을 것으로 예상
  + 파라미터 : klue/bert-base, epoch=2
  + test_acc = 0.991667

아이디어 2.(진행중)
+ 문장 연결성을 파악하기 위해 BertForMaskedLM 모델 이용
+ 한 문장이 두 개의 단락으로 끊어진 경우, 끊어진 부분을 마스킹해 모델에 입력
+ 모델의 예측값 5개 안에 실제값이 존재할 확률이 높다면 두 단락이 매끄럽게 이어지는게 맞는지 모델로 판단할 수 있을 것으로 예상

## 3. 뉴스 카테고리 분류
+ 데이터셋 : 모두의 말뭉치에서 기사 본문, 카테고리를 추출해 BertClassifier, KoBERT에 학습
+ 결과
  + val_acc = 0.8025
+ 문제점
  + 모두의 말뭉치에서 제공된 카테고리와 각 언론사별 카테고리 내용이 조금씩 달라 분류에 어려움이 있음  
![image](https://user-images.githubusercontent.com/88722429/175820115-0818305f-4128-4100-a9f3-67360295dafd.png)



## 더 진행해야 할 점
+ 한글 띄어쓰기 검사 : OCR 결과물을 이용해 성능 테스트, PyKoSpacing과 비교해보기
+ 단락 연결 여부 검사 : 아이디어 2 마무리, 모두의말뭉치 데이터를 이용해 Fine Tuning

---

참고자료
+ https://github.com/Doheon/NewsClassification-KoBERT
+ [PyTorch Lightning](https://pytorch-lightning.readthedocs.io/en/latest/)
+ [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 01. 데이터 준비](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_01/)
+ [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 02. 데이터 전처리](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_02/)
+ [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 03. 모델](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_03/)
+ [BERT를 이용한 한국어 띄어쓰기 모델 만들기 - 04. 학습](https://bhchoi.github.io/post/nlp/dev/bert_korean_spacing_04/)
