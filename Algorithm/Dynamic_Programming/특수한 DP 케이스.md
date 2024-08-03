# 특수 유형 : DP의 시간 복잡도 줄이기. (with Binary Search)

- 관련 백준 문제 : 

  [19623번 : 회의실 배정 4](https://www.acmicpc.net/problem/19623) (골드 3)

  [12738번 : 가장 긴 증가하는 부분 수열 3](https://www.acmicpc.net/problem/12738) (골드 2)  


<br/><br/>

몇몇 DP 문제들의 경우 점화식으로부터 얻어지는 자연스러운 시간 복잡도보다 더 효율적으로 해결할 수 있어. Segment Tree, Convex Hull Trick (CHT) 등 여러 가지 방식들도 있긴 하지만, 이 문서에서는 Binary Search (이분 탐색)을 이용한 최적화를 살펴볼 거야.

<br/><br/>
우선 백준 19623번 "회의실 배정 4" 문제를 살펴보면서 어떻게 풀 수 있을지 생각해보자. 

![오류 발생](/Algorithm/Images/19623.JPG)

![오류 발생](/Algorithm/Images/19623_2.JPG) 
<br/><br/>

이 문제를 푸는 Naive한 방법 중 하나는, dp 배열을 선언한 뒤에 아래의 반복문을 통해 정답을 출력하는 거야. 

```python

# 각 회의의 정보는 (a, b, c)의 형태로 arr 배열에 저장해두었다고 하자. 

dp = [0 for _ in range(n)]
ans = float('-inf')
for i in range(0, n, 1):
    dp[i] = arr[i][2]
    for j in range(0, i, 1): 
        if(arr[j][1] <= arr[i][0]): 
            dp[i] = max(dp[i], dp[j] + arr[i][2])
    
    ans = max(ans, dp[i]) 

print(ans)   
```

정석적인 $O$( $n^2$ ) DP에 해당하지만, 아쉽게도 위 코드를 제출하면 TLE를 받아. 

시간 제한이 1초인데도 n의 범위가 $10^{5}$ 까지 가능하기 때문이지. <br/><br/>


따라서 시간 복잡도를 줄일 수 있는 방법을 생각해봐야 하는데... 

생각해보면 위 반복문에서 변수 $j$가 하는 주된 역할은 $arr[j][1]$과 
$arr[i][0]$의 비교니까, $arr$을 정렬하고 이분탐색을 적용하면 시간 복잡도를 줄일 수 있어.<br/><br/>

즉, 아래의 과정을 통해서 $DP[i]$의 값을 갱신해주고 싶다는 거지.  

1. $arr$을 정렬한 뒤 이분 탐색으로 $arr[j][1] \leq arr[i][0]$ 를 만족하는 최대의 $j$ 값을 찾는다.

2. $DP[i] = DP[j] + arr[i][2]$ 로 갱신해준다. 


<br/><br/>
다만 위의 방법에는 허점이 있어. 

$arr$을 정렬했다고 하면 $arr[j - 1][1] \leq arr[j][1] \leq arr[i][0]$ 일텐데, 

이때 $DP[j - 1]$의 값이 $DP[j]$ 값보다 크다면, 단순히 $DP[i] = DP[j] + arr[i][2]$만으로는 올바른 값을 구할 수 없겠지?  <br/><br/><br/>

따라서 정확히는 다음과 같이 갱신해줘야 해. 

$$DP[i] = max(DP[i - 1], DP[j] + arr[i][2])$$ 

<br/><br/>

 위와 같이 갱신해주면 $DP[j - 1] \gt DP[j]$ 같은 상황이 발생하지 않게 되고, 굳이 ans 변수를 사용하지 않고도 편하게 정답을 $DP[n - 1]$ 혹은 $DP[-1]$로 출력해줄 수 있어. 

 ```python 

# 전체 코드. 

import sys 
input = sys.stdin.readline 

n = int(input()) 

arr = [] 
for _ in range(n): 
    s, e, p = map(int, input().split(' '))
    arr.append((s, e, p)) 
arr.sort(key = lambda x : x[1])

dp = [0 for _ in range(n)]
for i in range(0, n, 1): 
    l, r = 0, i - 1 # 이분 탐색 초기 범위. 
    while(l <= r): 
        middle = (l + r) // 2 

        if(arr[middle][1] <= arr[i][0]): 
            l = middle + 1 
        else: 
            r = middle - 1 

    # 여기 도달 --> r이 우리가 원하는 j 값. 
    # 이 부분이 잘 이해가 안 간다면 이분탐색 응용 문제들을 공부하고 올 것! 

    dp[i] = max(dp[i - 1], dp[r] + arr[i][2]) 

print(dp[-1]) 
 ```

참고 :  전체 시간 복잡도는 $O(n) + O(n\,\, log \,n) + O(n \,\, log \,n) = O(n \,\, log\, n)$ 
