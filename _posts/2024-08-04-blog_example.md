---
layout: post
title:  "Practicing markdown"
---

This is a title
===
제목을 쓰고 아래줄에 ===를 써주면 된다. =의 갯수는 상관 없다.

This is a subtitle 
----
부제목을 쓰고 아래 줄에 ---를 써주면 된다. -의 갯수는 상관 없다.

#### This is a H1. Use control+Shift+m to toggle the tab key moving focus.

##### Alternatively, use esc then tab to move to the next interactive element on the page.
#하나가 글씨 크기가 가장 크고 #의 수가 늘어날수록 
## This is a H2
### This is a H3
#### This is a H4
##### This is a H5
###### This is a H6

#하나가 글씨 크기가 가장 크고 #의 수가 늘어날수록 글씨 크기가 감소한다.

1.  첫번째 순서의 글
2.  두번째 순서의 글
3.  세번째 순서의 글


#### 코드 블럭을 넣는 방법



```
import sys
input = sys.stdin.readline

n = int(input())
a=list(map(int, input().split()))
pack = [0]+a
dp = [0]+a

for i in range(1, n+1):
    for j in range(1, i+1):
        dp[i] = min(dp[i], dp[i-j]+pack[j])
print(dp[-1])

#11052과 마찬가지로 dp배열과 pack배열을 따로 두고 하나는 누적하는 용도로 하나는 추가하는 용도로 사용
```
코드 넣기 전과 후에 ```를 넣어주고 해당 코드를 사이에 넣어주면 된다.


#### 마크다운 이미지 넣는 법

1. **내 컴퓨터에 있는 이미지의 경우**
내 컴퓨터에 있는 이미지의 경우 복사/붙여넣기 하면 되는 듯 하다.

<img width="897" alt="스크린샷 2023-10-02 090633" src="https://github.com/user-attachments/assets/6bd59db8-a1c6-4b87-9c52-e72340af7251">

코드에서 width='897'이라고 되어있는 부분이 크기를 나타낸다.


<img width="600" alt="스크린샷 2023-10-02 090633" src="https://github.com/user-attachments/assets/c83defa8-ad3b-4bbc-9de5-4efccd859a80">

width=600으로 바꿨을 때의 이미지이다.

2. **사이트에서 이미지를 가져올 경우**

``` 
![텍스트](링크주소)와 같은 양식으로 써준다. 
```

예시

![example](https://search.pstatic.net/common/?src=http%3A%2F%2Fblogfiles.naver.net%2FMjAyMDA5MjNfMjEz%2FMDAxNjAwODI0NDQ1MjA4.CdeKN46wqiJiH5AKk6xFWynLmZFFS7fZB7gW2Ok2NfEg.iwICis3OIrb-5qawNx9PC1gMwneqL7AzYjvDQQEnAOkg.PNG.meetple%2Fe6wR4WnZXR.png&type=sc960_832)

만약에 가로 세로 길이를 조정하고 싶다면 다음과 같은 형식을 따른다
```
<img src="이미지 URL" width="가로 사이즈" height="세로 사이즈">

```

예시
<img src="https://search.pstatic.net/common/?src=http%3A%2F%2Fblogfiles.naver.net%2FMjAyMDA5MjNfMjEz%2FMDAxNjAwODI0NDQ1MjA4.CdeKN46wqiJiH5AKk6xFWynLmZFFS7fZB7gW2Ok2NfEg.iwICis3OIrb-5qawNx9PC1gMwneqL7AzYjvDQQEnAOkg.PNG.meetple%2Fe6wR4WnZXR.png&type=sc960_832" width="300" height="400">


width='300', height='400'으로 설정했다.


#### 표 삽입 방법

표위에 빈줄 하나가 있어야 한다.

|제목|제목|
|내용내용|내용내용|


