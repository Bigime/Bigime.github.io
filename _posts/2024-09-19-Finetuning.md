## pre-training된 모델로 예시 만들기

1. 트랜스포머 다운받기
   ```
pip install transformers
   ```

![image](https://github.com/user-attachments/assets/958f3c29-f484-486a-85eb-0dc50aabcb0c)

2. 토크나이저와 gpt2 모델 가져오기
   
   ```from transformers import GPT2LMHeadModel,PreTrainedTokenizerFast```

3. 토크나이저 불러와서 사용자 지정 토큰 추가하기

```
tokenizer = PreTrainedTokenizerFast.from_pretrained("skt/kogpt2-base-v2")

tokenizer.add_special_tokens({'pad_token': '<pad>','bos_token':'<bos>','eos_token': '<eos>'})
#tokenizer에서 지정한 토큰에 한해서 special token으로 할당 가능
tokenizer.add_tokens(['<bot>'])
#tokenizer에서 지정한 토큰이 아닐경우 따로 지정해줌. 모두 토큰으로 분리됨
model = GPT2LMHeadModel.from_pretrained("skt/kogpt2-base-v2")
model.resize_token_embeddings(len(tokenizer))
#tokenizer에 추가해줬기 때문에 tokenizer에 있는 어휘사전 재조정

```

4. 예시

```

print(tokenizer.decode(model.generate(**tokenizer('옛날옛적에 ',return_tensors='pt'))[0]))

```
tokenizer로 옛날옛적에 토큰화 한 뒤에 pre-training된 모델로 문장 생성후 출력
![image](https://github.com/user-attachments/assets/d238cab4-595f-4dd7-a8a9-5e7747be5705)

   
## 데이터셋 구성하기

```
from torch.utils.data import Dataset
import pandas as pd
import numpy as np
```
```
df = pd.read_csv('story_mapping+character+order.csv');df
# 행마다 대괄호 [ ] 제거
df['scene_content1'] = df['scene_content'].str.replace(r"[\[\]]", "", regex=True)
df
```

![image](https://github.com/user-attachments/assets/6eda734d-9214-4b49-b62b-f4d2f7a4a517)

```
class NovelData(Dataset):
    def __init__(self, tokenizer):
        self.data = df
        self.X = []
        for i in self.data['scene_content1']:
          self.X.append(i)

        for idx, i in enumerate(self.X):
          try:
            self.X[idx] = "<bos>" +i + '<bot>: ' +self.X[idx+1]+'<eos>'
            # 사용자가 i를 입력하면 bot이 다음 문장 출력하는 형태
          except:
            break

        self.X = self.X[:50000]
        # 데이터셋 개수 정하기

        print(self.X[0])
        #예시로 잘 작동하는지 확인

        self.tokenizer = tokenizer

        self.X_encoded = tokenizer(self.X,padding=True)
        self.input_ids = self.X_encoded['input_ids']
        self.attention_mask = self.X_encoded['attention_mask']



    def __len__(self):
        return len(self.X)

    def __getitem__(self, idx):
        return torch.tensor(self.input_ids[idx]), torch.tensor(self.attention_mask[idx])
#텐서로 변환하지 않으면 list는 shape이 없다고 오류 뜸
```

token_type_ids이 경우 문장 구분용으로 0과 1로 구분되며 gpt 기반 모델에선 쓰이지 않음
attention_mask의 경우 padding과 구분하기 위함이며 padding이 아닌경우 1로 표기되고 아닌경우 0으로 표기됨
```
tokenizer('안녕 오늘 어때?')
```
![image](https://github.com/user-attachments/assets/ceb8bf12-deea-4ded-8eb7-5379e527c7ba)


#### noveldataset을 구현하면 다음과 같이 입력한다. '<'bos'>' + 문장  + '<'bot'>': + 문장
```
noveldata = NovelData(tokenizer)
#문장 구성이 잘 되어있는지 확인용
```
```
 '<bos>''저희 집의 방보다 훨씬 작은데 괜찮으세요?', '딴 소리 말고, 얼른 보여줘요.', 'C003를 바라본다.'
'<bot>: 'C008이 낡은 수첩을 보여준다.', 'C003가 수첩을 본다.', '말도 안 돼. 이게 왜 여기에 있어? 이게 화타의 유물이란 말이에요?' '<eos>'
```

## 학습
```
from torch.optim import Adam
from torch.utils.data import DataLoader
import torch
```

```
#훈련시키기
def train(noveldata, model, optim):
    epochs = 3

    for i in range(epochs):
        
        for X, a in noveldata:
            optim.zero_grad()
            X = X.to(device)
            a = a.to(device)
            loss = model(X, attention_mask=a, labels=X).loss
            loss.backward()
            optim.step()
        print('epoch finished')
    torch.save(model.state_dict(), 'model_state.pt')
    print(infer("안녕 뭐해?"))


# 예측하기
def infer(inp):
    inp = "<bos>"+inp+" <bot>: "
    inp = tokenizer(inp, return_tensors="pt")
    X = inp["input_ids"].to(device)
    a = inp["attention_mask"].to(device)
    output = model.generate(X, attention_mask=a )
    output = tokenizer.decode(output[0])
    return output

device = "cuda" if torch.cuda.is_available() else "mps" if torch.backends.mps.is_available() else "cpu"

model = model.to(device)

noveldata = NovelData(tokenizer)
noveldata = DataLoader(noveldata, batch_size=16)

model.train()

optim = Adam(model.parameters(), lr=1e-5)

print("training...")

train(noveldata,model,optim)
```
output이 출력될 때 앞에 input과 토큰들이 같이 출력이 되기 때문에 이를 수정해야함

