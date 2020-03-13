---
layout: post
title: "📚 알고리즘 중간고사 정리"
description: "2019년 2학기 전공 알고리즘 중간고사 공부 정리"
date: 2020-03-13
feature_image: images/thumbnail_algorithm.png
tags: [CNU, Algorithm, Java, Mid]
---
# 중간고사 준비

# 1

directed graph는 n개의 vertex를 가질 때 n(n-1)개의 edge를 가질 수 있다. 방향성이 없으니 모든 vertex가 자신을 ㅈ[외한 다른 vertex로 가는 edge를 모두 count

undirected graph는 n개의 vertex를 갖는 그래프는 최대 n(n-1)/2개의 edge를 가질 수 있다. 방향성이 없기때문에 중복되는 거 고려해서 2로나눠줌.

서브그래프 : 한 그래프에 대해 그래프의 모든 vertex와 그를 이을 수 있는 수준의 edge들을 가지고있는 다른 그래프

경로 : edge를 통해 연결 되는 vertex의 연속

사이클 : 경로의 시작과 끝이 같으면 사이클 

acyclic : 사이클이 없는 그래프

심플 : 경로상에 방문했던곳을 재방문하지 않으면 심플

강하게 연결된 요소 : 오가는게 모두 있으면 강하게 연결된것

degree : indegree 들어오는 edge 개수, outdegree 나가는 edge 개수
디그리 다 더해서 반나누면 edge수가 된다. 왜냐 indegree는 곧 outdegree이고 그 역도 성립하기 때문에..

matrix graph의 공간 복잡도는 O(n^2), 시간복답도는 O(n^2)

# 2-1

find의 시간복잡도가 O(n^2)임. 따라서 최적화해 줄 필요가 있다.

weighting rule : union해줄 떄 합치는 두 set의 크기를 비교해서 union 해주자. 확률적으로 크기가 큰 set가 길이가 길 가능성이 큼. 길이가 긴 set에 짧은 set을 union해주는편이 좋다
```java
    public void union(int aMemberA, int aMemberB){
    	int rootOfA = this.find(aMemberA);
    	int rootOfB = this.find(aMemberB);
    	int sizeOfA = this.sizeOfSetFor(rootOfA);
    	int sizeOfB = this.sizeOfSetFor(rootOfB);
    	if(sizeOfA < sizeOfB){
    		this.setParentOf(rootOfA, rootOfB);
    		this.setSizeOf(rootOfB, sizeOfA + sizeOfB);
    	}
    	else{
    		this.setParentOf(rootOfB, rootOfA);
    		this.setSizeOf(rootOfA, sizeOfA + sizeOfB);
    	}
    }
```
collapsing rule : find시에 찾아지는 모든 vertex의 부모를 root로 만들어서 높이를 낮추는것.
```java
    public int find(int aMember){
    	int rootCandidate = aMember;
    	while(rootCandidate < this.graph().numberOfVertices() && this.parentDoesExist(aMember)){
    		rootCandidate = this.parentOf(rootCandidate);
    	}
    	int root = rootCandidate;
    	int child = aMember;
    	int parent = this.parentOf(child);
    	if(parent >= 0){
    		while(parent != root){
    			this.setParentOf(child, root);
    			child = parent;
    			parent = parentOf(child);
    		}
    	}
    	return root;
    }
```
# 2-2

AdjacencyList의 모든 Edge를 확인하기 시간복잡도 = O(n + e) → 상수 복잡도
Adjacency List 공간복잡도 O(n+e), 시간 복잡도 O(n+e)

모든 vertex의 degree들을 확인한다.

DFS : Depth First Search 깊이 우선 탐색 : 방금 확인한 vertex 중 방문 안한 다른 vertex로 이동, 모두 방문했다면 왔던길로 돌아감 adjacencyList의 경우 O(e), matrix의 경우 O(n^2)

BFS : Breadth First Search 너비 우선 탐색 : 시작 vertex를 큐에 넣는다. 큐를 pop시키고 pop된 vertex의 인접 vertex를 모두 add, pop → 인접모두add, pop→인접 add반복해서 결과적으로 큐에 남은 원소가 없을 때 까지 반복한다. 그럼 모든 vertex를 순회한 것.

색칠하기에서는 BFS를 이용, 확장 트리에서는 DFS사용

# 3

spanning tree, minimum cost spanning tree, greedy algorithm

spanning tree : 모든 vertex가 하나의 set에 위치한 (component 하나로 이뤄진) 트리

어떠한 컴포넌트의 스패닝트리는 그 컴포넌트(트리)의 서브트리이다.

DFS랑 BFS를 통해 찾아지는게 결론적으로 확장 트리라고 볼 수 있음. 

**즉 한번 간곳은 다시 안가는, 그래프에서 트리를 만들어낸게 스패닝트리. 근데 그 과정을 DFS로하냐 BFS로하냐의 차이가 있는 것. 그렇게 찾아져서 남게된 edge를 tree edge, 사용하지 않게된 edge를 back edge 라고 한다. 당연 tree edge + back edge는 기존 graph의 edge가 된다.**

back edge를 사용하지 않는 이유는 그 edge를 쓰면 방문했던 vertex를 재 방문 하게 되기 때문이다. 다른 말로하면 해당 edge는 cycle을 만들어내는 edge라는 의미가 된다. 

minimum cost spanning tree : 최소 비용 확장 트리 : 최소의 비용을 갖는 경로로 이뤄진 확장 트리← 어떻게 찾을것인가? kruskal , prim's sollin's 가 있지만 kruskal을 다룬다. 

주어진 상황에서 가장 적은 cost를 가진 edge를 타고 가며 spanning tree를 만든다. 즉 주어진 상황에서 최적의 선택을 하는 그리디 알고리즘이다. 이렇게 하고나면 모든 edge의 cost 합이 최소라는건 보장할 수 없지만, 최소한 그 순간순간에는 최선의 선택을 했다고 볼 수 있다.

kruskal algorithm : 각 vertex는 한번 씩 방문한다. n개 vertex가 있으면 n-1개의 edge만을 뽑는다. 일단 모든 edge를 cost 적은순으로 최소우선순위 큐에 넣고 큐에서 빼면서 spanning tree를 만들지 않는 선에서만 받아주면 됨. ← 사이클을 만들지 않도록 

time complexity of Kruskal Algorithm = O(e log e)

inplement : 인터페이스를 상속받을 때 ← 안에 있는걸 필수적으로 정의해줘야함 인터페이스에서는 기능을 정의하지 않는다. 
extend : 클래스(추상 클래스 포함)를 상속받을 때 ← 그냥 쓸거는 그냥 쓰면되고 수정하고싶으면 오버라이드해주면 된다.

클래스 : 가져다 쓰던지 오버라이드하던지 아무튼 쓰면된다!
인터페이스 : 선언된 함수를 무조건 구현해야해!
추상클래스 : 가져다 쓰던지 오버라이드 하던지! 단, abstract인건 무조건 구현해야해!

# 4

shortest path : 다익스트라 알고리즘

Weighted Directed Edge로 구성이 된 그래프에서 시작점이 주어질 때 최단경로를 찾는다. 다음 vertex가 정해졌을때 최단경로를 모두 업데이트해서 그 중 최고의 선택을 한다. 즉 매 순간 최상의 선택을 하는 그리디알고리즘의 일종이라고 볼 수 있다.

음의 비용을 갖는 edge는 있을 수 없다. 그렇게되면 그 edge를 계속 통과하면 계속해서 cost가 줄어들테니까. edge가 없음을 표기하기위해서는 무한히 큰값을 갖는다고 가정한다. 

매번 vertex를 옮겨갈때마다 옮겨온 vertex에서 갈때의 경로의 길이와 이미 구해놨던 경로 중 뭐가 더 최단경로인지를 비교해서 최단경로를 업데이트 해줘야한다. 

time complexity of Dijkstra's Algorithm = O(n^2)

Transitive Closures : 이행폐쇄

A→B B→C 이면 A→C이다. (이행관계) Closure : 집합에 행위를 가해 나온 결과물을 집합에 넣을 때 어느 수준이 되면 더이상 새로운것이 나오지 않는다 그 상태를 폐쇄상태라고 한다.

더이상 새로운 경로가 없을 때 까지 모든 경로를 찾는 것

Transitive Closure : A+

Reflexive Transitive Closure : A*

# 5

Activity On Vertex Network에서 vertex는 일종의 행위를 의미한다. edge는 순서를 의미한다. 따라서 A→B이면 A의 후에 B가 실행되는, 즉 A가 B의 선행자, B가 A의 후행자가 된다.(predecessor, successor) 직접적으로 바로 연결된 predecessor는 immediate predecessor라고 부른다. 

cartesian product. 두 관계의 요소를 모두 곱해서 만들어낸 새로운 관계 집합

partial order : irreflexive (reflexive 한 요소 없음), transitive (3단논법) 두가지를 만족

DAG : Directed Acyclic Graph 사이클을 만들지 않는 유향그래프 partial order를 나타내기에 유리하다. 사이클이 없어야함!(Acyclic : 사이클 읎다!) 실제로 partial 하진 않다. 모두 transitive하게 만들지 않기 때문. 근데 굳이 transitive는 안만들어줘도 걍 다 있다고 가정하고 해주면 됨.  transitive closure를 이용해서 해결 가능

모든 partial order는 DAG로 표현해줄 수 있지만 역은 성립하지 않는다. 가정이 끼어있기 때문..
partial order는 DAG로 표현할 수 있다.
DAG의 transitive cloosure는 partial order이/다. 
ex) R1 = DAG, R2= Partial order 이면 R1은 R2에 속하고 R1을 transitive closure는 R2와 같다.
A^+ : A의 transitive closure. A의 모든 vertex가 transitive하게 된 것.

Topological sort : 위상 정렬. partial order가 주어지면 그를 선형적인 순서로 정렬해주는 것.

immediate predecessor가 없는 vertex로부터 시작해야 할 것. 확인하는 vertex를 제거, 그 vertex와 연결된 모든 엣지를 확인, 그 중에 한 vertex를 선택, 이 과정을 모든 vertex가 빠져나올 때 까지 반복한다. 

adjacency graph로 구현할 때 각 vertex의 immediate predecessor를 기록하도록 한다. 그리고 그 값이 0인 vertex를 찾아 이용하도록 한다. 

partial order는 irreflexive 하며 transitive한 관계인데 이는 DAG로 표현될 수 있음
DAG는 transitive하진 않다. irreflexive하다 즉 no cycle인 그래프로 서로 모두 연결되어있음. 여기에 transitive하다는 속성만 더해주면 partial order가 되기땀시롱, partial order는 dag로 표현해줄 수 있음.

**time complexity of topological sort is O(n+e)**

# 6

관계도 일종의 집합으로 볼 수 있다. S의 cartesian product SXS는 S에대해 관계를 갖는다

S = {a, b, c}
SXS = {<a, a>, <a, b>, <a, c>, <b, a>, <b, b>, <b, c>, <c, a>, <c, b>, <c, c>}
R = {<a, a>, <a, b>, <a, c>, <b, b>, <b, c>, <c, c>}

그래프와 유사하게 볼 수 있다. 각 요소가 vertex, 관계가 edge랑 같다. 따라서 관계를 나타내는 그래프 G = (V, E)로 나타낼 수 있다. (V : vertex 집합, E : edge, 관계)

**Transitivity : 모든 노드에 대해 X→Y와 Y→Z이면 X→Z인 경우**

**Reflexivity : 모든 노드가 self loop을 가질 때. 즉 X→X edge가 있을 때** 

**Symmetricity : 모든 노드에 대해 X→Y가 있을 때 Y→X도 있을 때**

유용한 관계 : transitivity, reflexivity, symmetricity 모두 만족하는 관계 (동등관계)

**Set과 Realation을 알 때 그로부터 유용한 정보를 뽑아내는데 사용할 수 있다.** 

Equivalance Realation : 동등한 관계. 집합 S가 reflexive, symmetric, transitive하면 동등관계라고 한다. 

S = {a, b, c}
SXS = {<a, a>, <a, b>, <a, c>, <b, a>, <b, b>, <b, c>, <c, a>, <c, b>, <c, c>}
R1 = {<a, a>, <b, b>, <c, c>, <a, b>, <b, c>, <a, c>}
→ Reflexivity : a와 b, c에대해 모두 self loop을 가지기때문에 reflexive하다
→ Transitivity : a→b, b→c, 이고 a→c이므로 transitive하다
→ Symmetric : a→b이지만 b→a는 없으므로 reflexive하지 않다
⇒ R1은 동등관계가 아니다.
R2 = {<a, a>, <b, b>, <c, c>, <a, b>, <b, c>, <a, c>, <b, a>, <c, b>, <c, a>}
→ Reflexivity : a, b, c가 모두 자기 자신으로의 self loop을 가지기 때문에 reflexive
→ Transitivity : a→b, b→c, c→a, a→c, c→b, b→c, b→a 이므로 transitive 하다
→ symmetricity : a→b, b→a, b→c, c→b, a→c, c→a 모두 있다.  Symmetric하다
⇒ R2는 동등관계이다.

동등한 관계로부터 동등한 클래스들을 뽑아낸다. 그것이 동등클래스

동등 관계의 그래프로부터 동등한 클래스를 뽑아낸다. 이 집합은 동등관계로 이어져있기 때문에 vertex간의 인접성은 곧 vertex간의 동등관계를 나타낸다. 따라서 A→B이면 A와 B는 동등관계에 있는 동등클래스, B→C이면 A와 B, C는 동등관계에 있는 동등클래스가 된다. 따라서 한 vertex의 인접 vertex에대해, 그리고 그 인접한 vertex들의 인접한 vertex들에 대해 검사를 해주어야 한다. 이렇게 인접성이 성립하는 vertex들을 모아놓은것이 동등 클래스가 된다. 이미 동등클래스가 찾아진거에 도달하게되면 동등클래스 찾기를 멈춘다. 

만약 다른 동등클래스랑 이어진다면..? 어떻게

**list 이용 시 O(n+e) 의 시간복잡도/공간복잡도**

# 6-2

순차 탐색 : 이미 정렬 되어있는 자료를 말 그대로 순차적으로 하나하나 다 확인하는거 ← 어느 새월에 하누?

배열의 마지막에 찾으려던 값을 넣어놓고 그냥 반복돌려서 마지막 전에 찾아지면 있는거 마지막에 찾아지면 없어서 마지막에 인위적으로 넣어논거 찾아진걸로 하면 미미하지만 시간적 이득을 얻을 수 있다. 이걸 sentinel이라고 함. **O(N)의 시간복잡도를 갖기때문에 존나게느려요~**

이진 탐색 : 이미 정렬 되어있는 자료에서 중간값을 딱 정해놓고 탐색해주자 중간보다 작으면 중간 아랫범위에서 다시 이진검색, 크면 중간 윗부분에서 이진검색 이걸 반복. 최악의 경우 log n번을 비교해야한다. (한번 비교에 2배수씩 비교가 되니까..) 따라서 **O(logN)의 시간 복잡도**를 갖는다.

interpolation search : 찾을 값을 이용해 어디서부터 찾아 나갈지를 결정한다는 아이디어..

리스트 검증 : 리스트가 같은지 아닌지를 확인. 정렬되어있지 않다면 모든 요소를 훑어야하니 O(NM)의 시간복잡도를 갖는다. 정렬시켜서 하는편이 빠름 왜냐면 정렬시키면 O(NlogN + MlogM)인데 정렬이 됐기 때문에 비교는 요소의 개수만큼 시간을 더 소비함 O(N + M) 따라서 전체적으로 O(NlogN + MlogM + N + M)의 복잡도를 가지므로 아무리 커봐야 **O(max(NlogN, MlogM))의 시간복잡도를 갖는다.** 

# 7

정렬 : 대부분의 컴퓨팅 시간은 데이터를 정렬하는데 사용된다. 그래서 정렬 알고리즘 좋은걸 찾아내는게 매우 중요한데, 안타깝게도 모든 경우에대해 최적의 속도를 보장하는 정렬 알고리즘은 없다. 경우에 따라 크기에 따라 정렬 알고리즘의 효율은 천차만별이 된다. 

삽입정렬 : 매번 삽입하면서 앞이랑 비교해서 앞으로 보내주거나 나비두거나 반복하는거. 배열의 범위를 벗어나지 않게 하기위해 sentinel을 사용하기도 한다. 맨 앞자리를 음의 무한대로 설정해주는 방식. 근데 이런것보다 걍 최소값을 찾아서 미리 박아놓고 시작하는 방법도 있다. 

입력이 알아서 정렬돼서 들어오면 최고. 비교했다 아니네.. 비교했다 아니네.. 이거만 반복해주면 되니 최상의 경우다. 그치만 입력이 완전히 뒤바껴서 들어오면 최악. 매번 비교이동 비교 이동 비교 이동 계속 해줘야함.

정렬해야하는 대상이 20개언저리 정도일 때 사용하면 효율 적이다. 순서에 맞지 않은 요소가 적을수록 효율적인데 정렬 대상이 20개 밑에서는 어짜피 그게 적을테니까

LOO(자리에 맞게 정렬되지 않은거)가 k개일때 삽입+비교 O(N), 정렬해야하는경우 O(KN) 따라서 O(KN + N) = O((K + 1)N)의 시간복잡도를 갖는다. K가 적을수록 당연히 좋겠지?
K가 0이면 O(N), **K가 n이면 O(N^2)까지도 간다.** 

링크드 리스트를 이용해 인덱스 움직이는걸 간소화시키거나 순차탐색대신 이진탐색을 사용하는 등으로 성능을 조금 더 개선할 수 있다..

퀵소트 : 디바이드 앤 컨커! 삽입정렬이 stable한대 반해 얘는 unstable하다 보장을 못해준다? 대부분의 경우 좋은 성능을 뽑아준다. 정렬이 되어있으면 최악임. 피벗을 잡고 그걸 기준으로 왼쪽엔 작은친구들 오른쪽엔 큰 친구들을 넣어준다. 재귀적으로 할 수 있음. 아무거나 피봇으로 정해도 되지만, 다르게 해주는편이 좋긴하다. 가장 많이쓰이는건 맨 왼쪽거를 피봇으로 잡아주는거. 왼쪽에서 toright로 올라가며 오른쪽에서 toleft로 내려가며 pivot이랑 값을 비교하고 서로 바뀌어야하는게 나오면 멈추고 바꿔주고 반복. 그러다 toleft가 toright를 지나치면 그것도 문제가있는거니까 바꿔줌. 

pdf에 정리잘되어있다..그거봐

시간복잡도 : **평균적인 경우 O(NlogN), 최악의 경우 O(N^2)**인데 그런경우는 잘 없음..

공간복잡도 : 최악 O(N) 최적O(1), 평균O(logN). 작은 리스트에 대해서는 반복문을 써주면 공간을 덜 쓸 수 있다. pivot의 위치도 중간값을 잡아줌으로써 최악을 피할 수 있음. 

Sentinel : 오름차순의 경우 가장 큰값을 미리 찾아서 오른쪽 끝에 박아두면 Sentinel로 쓸 수 있따.

# 9

**sorting은 아무리 빨라봐야 NlogN을 못넘는다.** 

데이터 개수가 n개면 최대 n!개의 leaf가 생긴다. logN! + 1만큼의 반복이 필요하다 
height 이 k이면 2^k-1개의 잎을 갖는다.  ⇒ 2^k-1의 잎을 가질때 log를 취해 1을 더하면 height이 된다.
따라서 N!의 잎을 가지면 logN! + 1이 height이된다. 따라서 최소 높이는 logN!이 된다. 
logN! = log N(N-1)(N-2)...3*2*1 이고 이는 최소의 경우 NlogN이된다.

힙 : 입력받을때마다 힙을 만드는경우, 입력시마다 확인해서 소팅 O(NlogN)의 시간복잡도. 평균적으로 O(N)을 갖는다. 

n개의 노드를 갖는 이진트리의 최대 높이는 log(N + 1)이므로  N번의 insert시 최대 O(NlogN)

입력이 아니라 입력받은걸 adjust하는경우 O(N)

입력받은걸 adjust하는게 더 빠를 것 같지만 어짜피 insert할때 추가하는거만 해도 O(logN)이 필요 → 결국엔 O(NlogN)이다.
힙소트는 최악의 경우가 O(NlogN)이고 평균적으로 O(N)이다. 이와 달리 퀵소트는 최악의 경우가 O(N^2)이고 거의 어느 경우에나 평균적으로 O(NlogN)이므로 사실상 퀵소트가 더 빠르게 다양한것에 적용될 수 있다고 볼 수 있다. 

summary : 크기가 20언저리일땐 O(N^2)인 삽입 정렬을, 아니면 걍 퀵소트 때려버리자 O(NlogN) 데이터를 주고받는건 오래걸린다. 가능하면 링크드리스트나 인덱스를 이용하자. 

# 복잡도

matrix graph ⇒ 공간복잡도 O(n^2), 시간복잡도 O(n^2)
List graph ⇒ 공간복잡도 O(n+e), 시간복잡도 O(n+e)
kruskal 알고리즘 ⇒ 시간복잡도 O(e log e)
dijkstra 알고리즘 ⇒ 시간복잡도 O(n^2), 
topological sort 알고리즘 ⇒ 시간복잡도 O(n+e) 
equivalence class 알고리즘 ⇒ 시간복잡도 O(n+e)
순차탐색 ⇒ O(n)
이진탐색 ⇒ O(n log n)
리스트검증 ⇒ O(n^2)
삽입정렬 ⇒ O(n^2)
퀵정렬 ⇒ O(n^2), 평균적으로 O(n log n)
