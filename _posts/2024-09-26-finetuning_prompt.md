---
title:  "챗봇 프롬프트 만들기 및 랭체인 개념 학습"
---

밑의 정리는 모두의 AI라는 유튜브의 Langchian 강의를 통해 작성되었습니다.

Langchain이 왜 필요한가?
---

1. 정보접근 제한 --> Vectorstore 기반 정보 탐색 or Agent 활용한 검색 결합
2. 토큰 제한     --> TextSplitter를 활용한 문서 분할
3. 환각 현상     --> 주어진 문서에 대해서만 답하도록 Prompt 입력

Langchain의 구조
----
1. LLM : 초거대 언어모델
ex) GPT-3,GPT-4,LLAMA,PALM-2

3. Prompts : 초거대 언어모델에게 지시하는 명령문
ex) Prompt Templates, Chat Prompt Template, Example Selectors, Output Parsers

4. Index : LLM이 문서를 쉽게 탐색할 수 있도록 구조화 하는 모듈
ex) 

5. Memory : 채팅 이력을 기억하도록 하여, 이를 기반으로 대화가 가능하도록 하는 모듈
ex) ConversationBufferMemory, Entity Memory, Conversation Knowledge Graph Memory

6. Chain : LLM 사슬을 형성하여, 연속적인 LLM 호출이 가능하도록 하는 핵심 구성 요소
   promt를 한 번만 생성하는 것이 아니라 여러번 호출을 통해 연속적으로 프롬프트 생성
ex) LLM Chain, Question Answering, Summarization, Retrieval Question/Answering,..

7. Agents : LLM이 기존 Prompt Template으로 수행할 수 없는 작업을 가능케하는 모듈
ex) Custom Agent, Custom MultiAction Agent, Conversation Agent

Langchain 예시
---

```
import os
import time

from langchain.chat_models import ChatOpenAI
from langchain.schema import HumanMessage, SystemMessage, AIMessage

os.environ['OPENAI_API_KEY']='USER_SECRET_KEY'

chat = ChatOpenAI(model_name = "text-davinci-003",temperature=0)

messages = [SystemMessage(content="You are a helpful assistant that translates English to Korean"),
            HumanMessage(content="I love langchain")]

time.sleep(3)

response = chat.invoke(messages)
print(response)

```

나의 생각

System Message를 통해 Chat gpt에게 역할을 부여하여, 대화의 맥락을 설정하는 메세지
이를 활용하여 등장인물을 인식시키고 스토리 전체의 맥락을 이해시킬 수 없는건가?


파인튜닝 프롬프트 작성하기
---

처음에는 사용자가 작성한 문장에 대해서만 예측을 진행하려고 했으나 사용자가 작성한 문장에 대해서만 
예측하면 소설을 쓰는 데 필요한 맥락을 파악하기 어렵다고 판단하여 전 문장들도 같이 포함해서 예측하도록 함 


```

import torch
from transformers import PreTrainedTokenizerFast, GPT2LMHeadModel
import re

#토큰들과 토크나이저 설정
Q_TKN = "<usr>"
A_TKN = "<sys>"
SENT = "<unused1>"
EOS = "</s>"
BOS = "</s>"

koGPT2_TOKENIZER = PreTrainedTokenizerFast.from_pretrained(
    "skt/kogpt2-base-v2",
    bos_token=BOS,
    eos_token=EOS,
    unk_token="<unk>",
    pad_token="<pad>",
    mask_token="<unused0>",
)

#토크나이저에 새로운 토큰 추가
#등장인물들 새로운 토큰으로 추가하여 예외적으로 등장인물이 잘려서 토큰화되지 않도록 함

koGPT2_TOKENIZER.add_tokens(charList)


#Load your trained model
#resize_token_embeddings를 명시해줘야 제대로 작동
model_path = "/content/drive/MyDrive/deepdaivproject/only_stage/storymaker_model.pth" # Adjust the path to your saved model
model = GPT2LMHeadModel.from_pretrained("skt/kogpt2-base-v2")
model.resize_token_embeddings(len(koGPT2_TOKENIZER))


#저장된 커스텀 모델경로 복사
checkpoint = torch.load(model_path)
model.load_state_dict(checkpoint["model_state_dict"],strict=False)
model.eval()

#Interaction loop with the chatbot
end = False
with torch.no_grad():
    conversation_history = []
    num=0
    while not end:
        #if문은 처음 사용자가 문장을 입력했을 때
        if num==0:
          q = input("user > ").strip()
          if q == "quit":
              break
          #사용자가 감정+대사 또는 서술 순으로 작성하기 때문에 학습한 패턴대로 토큰을 넣어주기
          위해서 문장을 쪼갬
          q1 = q[0:2]
          q2 = q[3:]
          #학습했던 토큰의 순서대로 넣어줌
          q = Q_TKN+q1+SENT+q2+SENT+A_TKN
          #print(q)
          gen_ids = model.generate(koGPT2_TOKENIZER.encode(q, return_tensors='pt'),
                      max_length=512,
                      pad_token_id=koGPT2_TOKENIZER.pad_token_id,
                      eos_token_id=koGPT2_TOKENIZER.eos_token_id,
                      bos_token_id=koGPT2_TOKENIZER.bos_token_id,
                      use_cache=True,top_p=0.9,temperature=1.0,do_sample=True)
          generated = koGPT2_TOKENIZER.decode(gen_ids[0],)
          #print(generated)

          #<sys>와 </s> 사이의 텍스트 추출
          result = re.search(r"<sys>(.*?)</s>", generated)
           # 결과 출력
          if result:
                generated = result.group(1).strip()
                #<sys>와 </s> 사이의 텍스트만 추출하고 공백 제거
          else:
                print("해당 패턴을 찾을 수 없습니다.")

          #print(generated)
          #1. 불필요한 태그 제거 (<unused1>, <sys>, </s> 등)
          #cleaned_text = re.sub(r"<unused1>|<sys>|</s>|<usr>|</s>", "",generated)

 
          #2. 불필요한 공백 제거할려고 했으나 공백이 있는 것이 토큰화에 유리하기 때문에 배제
          #cleaned_text = cleaned_text.replace(" ", "")
          #print(cleaned_text)
                
          #3 conversation_history에 추가
          #사용자가 처음 문장을 입력했을 때에는 이전 문장이 존재하지 않으므로 현재 문장을 추가만 해준다.
          conversation_history.append(generated)
          #conversation_history.append('안녕하세요')
          #print(conversation_history)
        

          print(generated)
          num+=1
       
        #사용자가 작성한 문장이 처음이 아닐 때
        else:
          q = input("user > ").strip()
          if q == "quit":
             break
          q1 = q[0:2]
          q2 = q[3:]
          #print(q1)
          #print(q2)
          #바로 이전 문장만 기억하게 할 것이기 때문에 pop 함수 사용해서 꺼내고 conversation_history list는 비어있도록 만듬
          q3 = conversation_history.pop()
          q3 = str(q3)
          q = Q_TKN+q1+SENT+q3+q2+SENT+A_TKN
          #print(q)
          gen_ids = model.generate(koGPT2_TOKENIZER.encode(q, return_tensors='pt'),
                      max_length=512,
                      pad_token_id=koGPT2_TOKENIZER.pad_token_id,
                      eos_token_id=koGPT2_TOKENIZER.eos_token_id,
                      bos_token_id=koGPT2_TOKENIZER.bos_token_id,
                      use_cache=True,top_p=0.9,temperature=1.0,top_k=80, repetition_penalty = 2.0,do_sample=True)
          
          generated = koGPT2_TOKENIZER.decode(gen_ids[0],)
          #print(generated)
          #<sys>와 </s> 사이의 텍스트 추출
          result = re.search(r"<sys>(.*)</s>", generated)

          #결과 출력
          if result:
                generated = result.group(1).strip()  # <sys>와 </s> 사이의 텍스트만 추출
          else:
                print("해당 패턴을 찾을 수 없습니다.")

          #<sys>와 </s> 사이의 텍스트만 추출하면 special token 존재하지 않기 때문에 special token 제거과정은 생략
          #generated = re.sub(r"<pad>", r"", generated)
          #generated = re.sub(r"<sys>",r"",generated)
          #generated = re.sub(r"<unused0>",r"",generated)
          #generated = re.sub(r"<unused1>",r"",generated)
          #generated = re.sub(r"</s>",r"",generated)
          a = "Bot > "
          print(a + generated)

          #서사구조 자르기
          #con_generated = generated[0:2]
          #서사구조 자른 문장 conversation_history에 저장
          conversation_history.append(q2+generated)
          #print(conversation_history)
```







