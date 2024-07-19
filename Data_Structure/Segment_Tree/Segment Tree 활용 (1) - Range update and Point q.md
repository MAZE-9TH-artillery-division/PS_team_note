# Segment Tree 활용 (1) - Range update and Point query

- 관련 백준 문제 : 
[16975번: 수열과 쿼리 21 (acmicpc.net)](https://www.acmicpc.net/problem/16975) ( **플레 4** )
[7578번: 공장 (acmicpc.net)](https://www.acmicpc.net/problem/7578) ( **플레 5** )

 ** 아직 Segment Tree가 뭔지 모른다면 아래의 문서를 먼저 보고 올 것! 

[세그먼트 트리 (Segment Tree) 기본편](%E1%84%89%E1%85%A6%E1%84%80%E1%85%B3%E1%84%86%E1%85%A5%E1%86%AB%E1%84%90%E1%85%B3%20%E1%84%90%E1%85%B3%E1%84%85%E1%85%B5%20(Segment%20Tree)%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%E1%84%91%E1%85%A7%E1%86%AB%20db099fbf576647b9b7cfaae5f3b9d14f.md)

 기본편에서 다루었던 Segment Tree의 query 함수와 update 함수는 다음과 같은 특징을 가져. 

- update 함수에서 받는 입력값은 단 하나.
- query 함수에서 받는 입력값은 특정 구간.

 즉 update는 실질적으로 배열의 한 값 (한 점)에 대해서만 이루어지고, 반대로 query 처리는 
배열의 여러 값 (구간)에 대해서 이루어진다고 할 수 있어. 

 이때, 보통 위와 같은 특징을 각각 point update / range query 라고 나타내. 

 그런데 때로는 반대로 update를 구간에 대해서 해주고 싶을 때가 있겠지?
하지만 우리가 기본편에서 구현했던 Segment Tree는 update를 구간에 대해 진행해주려고 하면 
구간의 길이를 l, 원본 배열의 길이를 n이라 했을 때 update의 시간복잡도가 O( l * log n )이야. 

 물론 각 변수만 놓고 보면 나쁜 시간 복잡도는 아니지만 update를 상당히 자주해야 하는
상황에서는 실행 시간이 꽤 길어지겠지? 이러한 상황을 해결해보고자 하는 것이 이 문서와 아래
문서의 내용이야. 

[Segment Tree 활용 (2) - Lazy propagation](Segment%20Tree%20%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%20(1)%20-%20Range%20update%20and%20Point%20q%20117e2aa97dec48b5ad2484e156af1255/Segment%20Tree%20%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%20(2)%20-%20Lazy%20propagation%20b4312749f192482bac9f798d55b8fb2d.md)

 굳이 두 문서를 분류한 이유는 Lazy propagation 쪽의 개념 및 구현 난이도가 높아서? 정도로 
알면 될 것 같고, 이 문서에서는 range update & point query를 지원하는 Segment Tree를 
공부해 볼 거야.

* Lazy propagation을 공부하면 range update & range query를 지원하는 Segment Tree를 
 만들 수 있어! 

** 합과 관련된 Segment Tree with range update and point query. 

 Segment Tree 기본편에서 가장 간단한 예시로 구간합을 봤듯이, 여기서도 합과 관련이 있는 간단한 
예시를 살펴볼 거야. 변형할 때에도 원리 자체는 거의 유지가 되니까 아마 아래 예제 정도만 
살펴보면 괜찮을 것 같아. 

예제 : [16975번: 수열과 쿼리 21 (acmicpc.net)](https://www.acmicpc.net/problem/16975) ( **플레 4** )

![1.JPG](Segment%20Tree%20%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%20(1)%20-%20Range%20update%20and%20Point%20q%20117e2aa97dec48b5ad2484e156af1255/1.jpg)

 문제 부분을 읽어보면 정확히 이 문제가 range update & point query를 구현해야 하는 문제임을 
알 수 있어. 구간과 관련한 처리, 잦은 업데이트가 있으니까 Segment Tree를 사용해볼 수 있을 거
같기는 한데… 기본편에서 다뤘던 Segment Tree랑은 느낌이 좀 다르지? 

* 꼭 Segment Tree일 필요는 없지만 이 문서에서는 Segment Tree로 해결한다고 가정할게! 

 우선 이 문제를 풀기 위해서는 이전에 배운 Segment Tree를 개선해야 하는데, 
특히 개선해야 하는 부분을 정리해보면 아래와 같아. 

- n이 고정되었다면, 구간의 길이 l에 대해 update의 시간 복잡도 = O( l )인데 일단 이걸 줄여야 함.
- 대신 여기서는, 위 사항을 위해서 range query를 포기해도 괜찮음.

 우선 위 시간 복잡도 O( l )의 원인을 고민해보자. 
길이가 l인 구간의 각 원소에 대해서 “일일이” update 함수를 실행시켜줘야 하다보니 시간 복잡도가 
O( l )이 되는 거지. 

그래서 생각해볼 수 있는 아이디어 중 하나가 a번째 ~ b번째 구간을 업데이트할 때, 내가 실제로 
업데이트 해줘야 하는 값의 개수를 줄이는 거고, 여기서 고민을 조금 더 해보면 아래의 아이디어를
떠올릴 수 있어

- 계산 방식을 바꿔서, 앞 인덱스 숫자 하나만 바꾸면 그 이후의 모든 인덱스에 대해서 
영향을 주도록 해볼까?

 저 아이디어만 놓고 보면 “??? 근데 그래서 이걸 어떻게 함 ㅋㅋㅋ”라는 생각이 들 수 있지만 
계차수열이라는 것을 이용하면 실제로 저 아이디어를 구현할 수 있어. 

 고등학교 때 공부해봤을지 모르겠는데, 일단 계차수열은 수열 a_n이 주어졌을 때 다음과 같이 
정의되는 수열 b_n이야. 

![계차수열.jpg](Segment%20Tree%20%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%20(1)%20-%20Range%20update%20and%20Point%20q%20117e2aa97dec48b5ad2484e156af1255/%25EA%25B3%2584%25EC%25B0%25A8%25EC%2588%2598%25EC%2597%25B4.jpg)

 그리고 이때 수열 b_n의 첫 번째부터 n 번째 항까지 더해주면, a_n의 값이 나온다는 특징이 있어.

즉, 계차수열의 구간합으로 원래 수열의 값을 얻어낼 수 있다는 뜻이야. 

이제 이 사실을 이용해서 아이디어를 구현해보자.

```python
arr = [1, 3, 6, 10, 15] 
diff = [1, 2, 3, 4, 5] 
new_arr = [] 

value = 0 
for i in range(5): 
	value += diff[i] 
	print(value, end = " ") #이때 출력 결과: 1, 3, 6, 10, 15. arr이 얻어짐을 알 수 있어! 
print() 

#이제 diff의 3번째 원소에 2를 더하고 똑같은 코드를 돌려보자. 
diff[2] += 2 
value = 0 
for i in range(5): 
	value += diff[i] 
	print(value, end = " ") #이때 출력 결과: 1, 3, 8, 12, 17
```

 어때? diff[2]에 2를 더해준 뒤 똑같은 과정으로 arr을 복원해보면 arr[0], arr[1]은 그대로이지만 
arr[2] ~ arr[4]의 값은 기존 값에 2를 더한 것으로 나타남을 알 수 있어. 바꿔준 것은 분명 숫자 
하나 뿐이지만, 실제로 계산할 때에는 구간 단위로 변화가 일어난다는 것이지. 

 Q. 근데 diff[2]를 바꾸면 arr[2]부터 모든 값이 바뀌는 거 아님? 

 A. ㅇㅇ 맞음. 
 

 Q. 그럼 arr[2] ~ arr[3]까지만 바꾸고 싶을 때는 어떻게 함? 

 A. 더해준 만큼을 diff[4] (마지막 인덱스 + 1) 에서 빼주면 되지.  

< 풀이 코드 및 주석 설명 > 

 이제 위 아이디어를 코드로 구현해서 문제를 해결해보자. 

```python
import sys 
input = sys.stdin.readline 

n = int(input())
inform = list(map(int, input().split(' '))) #원본 배열을 입력받는다. 

#-------- 여기부터는 Segment Tree Initialize --------
j = 0 
while(2 ** j < n): 
	j += 1 

cst = 2 ** j 
seg_tree = [0 for _ in range(2 * cst)] 
for i in range(n): #원래는 원본 배열을 옮기지만, 이번에는 계차수열을 옮겨준다. 
	if(i == 0): 
		seg_tree[i + cst] = inform[i] #계차수열의 첫 번째 항 
	else: 
		seg_tree[i + cst] = inform[i] - inform[i - 1] 
		
idx = n - 1 + cst 
while(idx > 1): 
	seg_tree[idx // 2] += seg_tree[idx] #부모 노드 값 계산 
	idx -= 1 

#-------- 여기까지 Initialize 완료 --------

def query(a): #a번째 원소를 출력하는 함수. 2번 쿼리에 해당함. 
	s = cst 
	e = a - 1 + cst
	#계차수열을 사용할 경우 항상 합이 0번 인덱스부터임에 유의하자. 
	summation = 0 
	while(s <= e): 
		if(s % 2 == 1): 
			summation += seg_tree[s] 
		if(e % 2 == 0): 
			summation += seg_tree[e] 
		
		s = (s + 1) // 2 
		e = (e - 1) // 2 
	
	print(summation) 
	return 
	

def update(a, b, c): #a번째 원소부터 b번째 원소까지 c만큼 더해주는 함수. 
	a += cst - 1 #리프 노드의 인덱스로 바꿔주고 
	
	while(a > 0): 
		seg_tree[a] += c 
		a = a // 2 
		
	if(b < n): #여기에 b < n 조건을 거는 이유는 따로 생각해볼 것! 
		b += cst
		while(b > 0): 
			seg_tree[b] -= c 
			b = b // 2 
			
	return 
	

m = int(input()) 
for _ in range(m): 
	q = list(map(int, input().split(' '))) 
	if(len(q) == 2): 
		query(q[1]) 
	else: 
		update(q[1], q[2], q[3]) 

```