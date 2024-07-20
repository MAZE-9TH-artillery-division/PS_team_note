# 예선 9번. "격자 위의 ELICE" 

< 문제 >

엘리스는 _N_ * _N_ 격자 모양의 미로에 갇혀버렸다! 

<br/>
<br/>  
가장 왼쪽 위 칸의 좌표는 ( 1 , 1 ) 이고 가장 오른쪽 아래 칸의 좌표는 ( _N_ , _N_ )이다. 

<br/>
<br/>
엘리스는 위대한 마법사이기 때문에 미로 위에 흩어져 있는 글자들을 순서대로 모아 단어 **ELICE**를 만든다면, 그 자리에서 즉시 순간이동 마법을 이용해 미로를 탈출할 수 있다고 한다. 

<br/>
<br/>
엘리스는 현재 ( 1 , 1 )에 위치해 있다. 

<br/>
<br/>
모든 격자에는 양의 정수가 쓰여져 있다. 몇몇 칸에는 글자가 놓여 있을 수 있다. 

<br/>
<br/>
엘리스가 있는 칸에 글자가 놓여 있는 경우, 원한다면 그 글자를 얻을 수 있다. 

<br/>
<br/>
글자를 얻는다면, 다시 이 칸에 도달해도 다시 한 번 글자를 얻을 수는 없다. 

<br/>
<br/>
이렇게 모은 글자를 얻은 순서대로 이었을 때, 단어 ELICE가 된다면 순간이동 마법을 사용할 수 있다. 

<br/>
<br/>

엘리스가 어떠한 격자 칸에서 다른 격자 칸으로 이동하고 싶다면, **상하좌우** 한 방향을 골라 인접한 격자 칸으로 이동할 수 있다. 

<br/>
<br/>
(단 미로를 벗어날 수는 없다.) 

<br/>
<br/>
엘리스가 어떤 칸에서 인접한 칸으로 이동할 때, 두 칸 위에 쓰여 있는 정수의 합만큼의 시간이 걸린다. 

<br/>
<br/>

예를 들어 아래 예제1과 같이 3이 쓰여 있는 ( 1 , 2 ) 격자에서 4가 쓰여 있는 ( 1 , 3 ) 격자로 이동하려면 7의 단위 시간이 필요하다. 

<br/>
<br/>

미로에는 정확히 2개의 **E**와, 1개의 **L**, 1개의 **I**, 1개의 **C**문자가 존재한다. 엘리스가 순간이동 마법을 사용해 미로를 탈출하는 최소 단위 시간을 알려주자. 

<br/> 
<br/> 
<br/> 


< 입력 >  (  제한 시간 : 7초  )

 - 첫째 줄에 정수 _N_ 이 주어진다. 
   
   $$ 3 \, \le N \le 1,000 $$ 

 - 둘째 줄부터 $N + 1$번째 줄까지 $a_{i, j}$ 이고, 격자 $(i , j)$에 쓰여있는 정수를 의미한다.
   
   $$ 1 \le a_{i,j} \le 1,000$$ 

 - $N + 2$ 번째 줄부터 $N+6$번째 줄까지는 정수 $r, c$가 주어진다.

   - 각 줄에 주어지는 정보는, 격자 $(r, c)$에 순서대로 글자 E, L, I, C, E가 놓여있음을 의미한다. 
    
    $$1 \, \le r, c \le  \, N $$ 

    - $N + 2$번째 줄에 입력된 위치의 E와 $N + 6$번째 줄에 입력된 위치의 E는 프로그램 내에서 동일한 글자로 취급한다. 

<br/>
<br/>
<br/> 

< 출력 > 

 - 첫째 줄에 엘리스가 미로를 탈출하는 최소 단위 시간을 출력한다. 

<br/>
<br/>
<br/> 

< 입력 예시 >

    3
    2 3 4
    1 4 3
    1 1 1
    1 1
    2 1
    3 1
    3 2
    3 3

<br/> 
<br/>
<br/> 

< 출력 예시 >  

    9

<br/> 
<br/> 
<br/> 

< 해설 >

지문이 참 길다 그쵸? 

이 문제는 전형적인 Dijkstra 문제인데, 아직 Dijkstra Algorithm이 무엇인지 모른다면 먼저 공부하고 와서 해설을 볼 것! 

말은 복잡하게 되어 있지만 결국 주어진 상황을 요약해보면 아래와 같아. 

<br/>

 - 이동할 때에는 현재 칸 + 이동하려는 칸 만큼의 비용이 든다.  
    
    - 이때 $a_{i,j}$가 항상 양수이므로 비용 역시 양수이고, 이러한 점으로부터 Dijkstra를 사용해볼 법하다고 판단 가능! 
  <br/>
  <br/> 
  - 특히 지금 구해주고 싶은 것은 엘리스가 탈출하는 데에 필요한 "최소 비용"이니 최단 거리 알고리즘들을 생각해볼 수 있고, 최단 거리 + 가중치 양수면 Dijkstra라고 거의 확신해줄 수 있지. 실제로 거의 이런 과정을 통해 Dijkstra를 사용해야겠다고 생각했어.    


<br/>
<br/> 

 Dijkstra Algorithm 기반으로 그냥 _N_ * _N_ 배열을 돌면서 글자 ELICE를 모으면 되는데, 

 배열 위에 있는 두 'E'는 서로 같은 것으로 취급한다고 하였으니 가능한 경로는 아래의 두 가지야. ( 편의를 위해 각각의 E를 E1, E2라 하자 )

<br/> 


 1.  **(1, 1) $\rightarrow$ E1 $\rightarrow$ L $\rightarrow$ I $\rightarrow$ C $\rightarrow$ E2** <br/><br/>
 2.  **(1, 1) $\rightarrow$ E2 $\rightarrow$ L $\rightarrow$ I $\rightarrow$ C $\rightarrow$ E1** 

<br/>
<br/> 

이때 제한 시간이 7초로 상당히 넉넉하고 _N_ 도 1,000 이하니까, 그냥 각각의 경우를 모두 Dijkstra로 계산한 뒤에 더 작은 값을 출력해주면 돼. 

```python

from heapq import heappush, heappop # Dijkstra를 위한 우선순위 큐. 
import sys 
input = sys.stdin.readline 

n = int(input()) 
board = [] 
for _ in range(n): 
    board.append(list(map(int, input().split(' ')))) 
# 일단 격자 정보를 받아서 저장. 

target =[(0, 0)] 
for _ in range(5): 
    r, c = map(int, input().split(' ')) 
    target.append((r - 1, c - 1))
    # 인덱스 고려해서 1씩 빼주자.  

# E, L, I, C, E 좌표 받기. 
# (0, 0)은 출발지를 위해서 넣어둔 것. 자세한 건 아래 코드를 확인해! 


# E1 - L - I - C - E 계산. 

ans = 0 
for i in range(5): 

    queue = [(0, target[i][0], target[i][1])] 
    # 처음 출발을 target[i]로 둔다. 
    # 이걸 위해서 위에서 target = [(0, 0)]으로 선언했던 것. 
    
    dist = [[float('inf') for _ in range(n)] for _ in range(n)] 
    visited = [[False for _ in range(n)] for _ in range(n)] 
    dist[target[i][0]][target[i][1]] = 0 

    # 위의 세 줄은 전형적인 Dijkstra 세팅. 
    tr, tc = target[i + 1] # 원하는 목적지의 좌표. 

    while(queue): 
        d, r, c = heappop(queue) 

        if(visited[r][c]): 
            continue # 이미 방문한 점이면 skip
        elif(r == tr and c == tc): 
            # 목적지에 도착했다면 
            ans += d  
            break 

        visited[r][c] = True 

        nr = [r - 1, r + 1, r, r]
        nc = [c, c, c - 1, c + 1] 

        for j in range(4): 
            if(nr[j] < 0 or nr[j] >= n or nc[j] < 0 or nc[j] >= n):
                continue # 배열을 벗어나는 경우는 생각 X. 

            w = board[r][c] + board[nr[j]][nc[j]] # 이동 비용 계산. 
            if(not visited[nr[j]][nc[j]] and d + w < dist[nr[j]][nc[j]]):
                dist[nr[j]][nc[j]] = d + w 
                heappush(queue, (d + w, nr[j], nc[j]))  


# 이 다음에는 E1과 E2의 자리를 바꿔준 뒤에 다시 한 번 위 과정을 반복. 
target[1], target[5] = target[5], target[1]

temp = 0
for i in range(5): 

    queue = [(0, target[i][0], target[i][1])]
    
    dist = [[float('inf') for _ in range(n)] for _ in range(n)] 
    visited = [[False for _ in range(n)] for _ in range(n)] 
    dist[target[i][0]][target[i][1]] = 0 

    tr, tc = target[i + 1] 
    while(queue): 
        d, r, c = heappop(queue) 

        if(visited[r][c]): 
            continue
        elif(r == tr and c == tc):
            temp += d # 이 부분만 바꿔주면 돼!  
            break 

        visited[r][c] = True 

        nr = [r - 1, r + 1, r, r]
        nc = [c, c, c - 1, c + 1] 

        for j in range(4): 
            if(nr[j] < 0 or nr[j] >= n or nc[j] < 0 or nc[j] >= n):
                continue 

            w = board[r][c] + board[nr[j]][nc[j]]
            if(not visited[nr[j]][nc[j]] and d + w < dist[nr[j]][nc[j]]):
                dist[nr[j]][nc[j]] = d + w 
                heappush(queue, (d + w, nr[j], nc[j]))

print(min(ans, temp)) 

```