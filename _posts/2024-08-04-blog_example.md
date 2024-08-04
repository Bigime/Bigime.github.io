---
layout: post
title:  "Practicing markdown"
---

This is a title
===
가장 큰 제목을 담당한다.

This is a subtitle 
----
부제목이라고 생각하면 된다.

# This is a H1. Use control+Shift+m to toggle the tab key moving focus.

Alternatively, use esc then tab to move to the next interactive element on the page.
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
코드 넣기 전과 후에 ```를 넣고 코드 넣기 전에 코드 작성 언어를 넣어주면 된다.




