---
layout: post
title: "📚 알고리즘 기말고사 정리"
description: "2019년 2학기 전공 알고리즘 기말고사 공부 정리"
date: 2020-03-13
feature_image: images/thumbnail_algorithm.png
tags: [CNU, Algorithm, Java, Final]
---
# 기말고사

# 10

알고리즘 : 다음 조건 만족해야함 **(입력, 출력, 명확성, 유한성, 유효성)**

- 입력 : 외부 입력이 주어짐
- 출력 : 결과가 출력됨
- 명확성 : 신뢰할 수 있는 작업
- 유한성 : 유한한 개수를 실행해야함 → 무한하면 알고리즘이라 할 수 없음 OS같은거
- 유효성 : 짧은 시간 내에 수행될 수 있어야 함 → 복잡하여 너무 긴시간이 필요한건 유효하다고 할 수 없음.

분석 : 추정하는 것

측정 : 실제로 측정하는 것 (추정과 측정의 차이)

추정의 결과로 복잡도가 나온다. 

공간복잡도 : 실행을 마칠 때 까지의 메모리 소모 추정치

고정된 크기 : 코드 자체의 크기, 컴파일시부터 정해진 변수상수의 크기

**가변적 크기** : 입출력의 크기, 재귀시 필요한 공간 등 실행 시점에서 정해지는 크기들

시간복잡도 : 실행에 필요한 시간 추정치

**어짜피 소요 시간은 컴파일러/언어/환경마다 다 다르다.** 

따라서 연산의 개수를 통해 추정함 → n이 중요

입력의 크기와 개수가 모두 시간복잡도에 영향을 줄 수 있따. 대표적으로 크기는 factorial 연산, 개수는 정렬연산 

이외에도 다양한 요인들이 복잡도에 영향을 줄 수 있다. 퀵정렬에서 pivot 선정 방식 등..

그래서 시간복잡도는 3가지경우를 고려하는데 최선, 최악, 평균의경우 3가지임.

이렇게 간단해보이지만 사실 스텝 수를 새는건 쉬운문제가 아니다. 정확할수가없음......

상한 : **big-O** → ex) 5n+4의상황에서 O(n) 어짜피 상한. 존나큰 경우이므로 상수 무쓸모

상한은 가능한 작을수록 정확하기때문에 이렇게하는게 맞아용 **걍 최고차항**만 똑 떼어서 생각!

O(1), O(log n), O(n), O(n log n), O(n^2), O(n^3), O(2^n)

하한 : **big-Omega** → ex) 3n+3에서 최소 1이상은 걸리므로 1이라고 말할수도 있다 근데 1이라고말하면 너무부정확하니까 최소 n은 걸린다고 말한다. 상한과달리 가능한 크게말하면되는거

이것도 쉽게보면 걍 최고차항 똑떼서 생각한다고 볼 수도 있긴할듯(다그런건아닌거같음)

평균 : **big-Theta** 잘안쓴다. 그냥 big-Oh가 최고

# 11

stay ahead : 매 스텝마다 최적의 해답을 찾아가는것.  ex)interval scheduling

exchange argument approach : 그리디알고리즘을 통해 찾아진 솔루션으로 점진적으로 변환시키는것. ex)huffman code

최단거리찾기나 최소비용확장트리같은것도 그리디임

**interval scheduling = stay ahead
huffman code = exchange argument**

### interval scheduling

작업 j가 sj에서시작해서 fj에 끝난다고할때 서로 overlap되지않는 작업들은 compatible하다.

상호간에 compatible한 job들의 최대 하위집합을 찾아내는것이 interval scheduling의 목표 많은 수의 job들을 챙기는것이 이득

그리디하게 접근하는방법이 몇몇개가 있다.

- 일찍시작하는놈 : 시작하는 시간이 증가하는 순으로 보자 → 빠르게 시작할수록 좋다
- 일찍끝나는놈 : 끝나는시간이 증가하는 순으로 보자 → 빠르게 끝날수록 좋다
- 가장짧은인터벌 : 작업의 진행시간이 가장짧은 순으로 보자 → 작업시간이 짧을수록 좋다
- 가장적은분쟁 : 다른 작업과 겹치는 수를 증가하는 순으로 보자 → 안겹칠수록 좋다

단, 이미 선택된거랑 compatible한걸 골라야함. 선택될 작업의 s가 마지막에 선택된 작업의 f보다 커야한다.

근데 위에서 제시한 그리디한 방법들이 다 통하는거는 아니다.. 모두 예외가있음.

{% include image_caption.html imageurl="/images/algorithm_final_2019-12-08__1.41.27.png" title="images" caption="" %}

**earliest finish time이 반례없이 optimal하다. 시간복잡도 O(n log n)**

"stay ahead" 방식으로.. 모순을 통해 증명한다.

만약 greedy하게 선택된 k개의 job과 실제 optimal한 m개의 job이 있다고 생각하자. 근데 greedy는 optimal하지 않다고 가정하자 일단. r번째까지 optimal한것과 optimal하지 않다고 가정된 greedy한것이 같다고 가정할 때, optimal 한것의 r + 1번째과 greedy한것의 r + 1이 같지 못할 수 없다. 즉 같을 수 밖에 없다. 그럼 optimal한건데 가정이 optimal하지 않다였으므로 모순된다. 즉 greedy한것이 optimal하지 않다는 가정이 모순되므로 greedy한것은 optimal 하다.

{% include image_caption.html imageurl="/images/algorithm_final_2019-11-26__8.13.39.png" title="images" caption="" %}

만에하나 r+1에서 greedy하게 찾은것보다 optimal하게 찾은게있다면 뭔가잘못된거다.

### 정렬된 리스트 merge

최소한의 비교를 하는 방법은? ex) 20개리스트와 30개리스트, 50개리스트를 합칠때

20개와 30개를 합친다(비교50회) 그리고 50개와 50개를 합친다 (비교 100회) → 총 150회 

20개와 50개를 합친다(비교 70회) 그리고 70개와 30개를 합친다 (비교 100회) → 총 170회

30개와 50개를 합친다 (비교 80회) 그리고 80개와 20개를 합친다 (비교 100회) → 총 180회

분명 같은 리스트들을 하나로 합친건데도 비교의 회수에 차이가 발생한다. 가장 비교가 적어야 성능이 좋은거다. 비교가 적게 하는방법은?

각 리스트의 크기가 리프노드가 되어서 배치됨. 올라갈떄마다 더해질거기때문. 각 노드의 값들이 올라오는데 거치는 path마다 비교회수가 그만큼 늘기 때문에, **리스트의 크기와 그 리스트에 도달하는 path의 거리를 곱한값을 다 더한게 비교의 회수가됨. ← 허프만의 기본 개념**

{% include image_caption.html imageurl="/images/algorithm_final_2019-11-26__8.13.22.png" title="images" caption="" %}

### 허프만코드

문자열을 0과 1의 시퀀스로 만들어 메시지를 구성하는 비트의 수가 최소한이 되도록 한다. 물론 구별 가능해야함. 

길이가 고정된 인코딩에서는 n개 심볼의 표시에 log2n비트가 필요함. 고정되어있음.. 그래서 공간적인 활용도가 좋지못함 그래서 변수의 길이를 갖는 인코딩 방식도 있는데, 대표적으로 모스코드가 있다. 근데 이런 코드의 문제점은 구분이 안되면 알 수가 없음. 그래서 모스코드도 각 문자별로 구분해주는 쉬는시간이있다.(pause) 그러다보니 사실상 0과 1이아니라 그냥 3개를 쓴다고 봐야할정도. 정말 그냥 0과 1로만 하려면??

등장하는 코드는 고정(prefix code)되어있다. 만약 동일한게 나오면 그건 다른여지가없이 바로 "그거"다

따라서 암호화한걸 복호화할때 순차적으로 읽어들이면서 알맞는게 나오면 그게 바로 그거다. 그냥 의심의 여지가 없다. 남은 비트 없을때까지 위과정을 반복하면 huffmancode를 decode할 수 있게 되는 것.

바이너리 트리로 표현할 때는 유니크한 path를 갖도록 해줘야하므로 모든 알파벳들은 트리의 leaf에 대응된다. 그리고 root로부터 각 알파벳(리프노드)으로의 path가 좌측으로가냐 우측으로가냐를 토대로 0과1의 시퀀스를 만들어낸다. 리프가 아닌 노드가 모두 자식을 가지게되면 꽉찬것.

앞서 정렬된 리스트 2개 정렬**(optimal 2-way merge)** 와 동일한 방식이다. 각 문자가 등장하는 회수가 리스트의 길이랑 대응되기때문. 즉 사이즈가 작을수록 밑으로가면 이득

{% include image_caption.html imageurl="/images/algorithm_final_2019-11-26__9.51.45.png" title="images" caption="" %}

따라서 optimal 2-way merge랑 동일하게 시간복잡도는 O(n log n)이된다!

증명은 다들 그리디가 옵티멀하지 않음을 강조하는듯. **그리디 하지 않은 방법으로했을때 적어도 같거나 안좋은걸 증명한다.**

# 12

divide and conquer 분할정복

문제를 작은 부분으로 나누고 그 부분들을 재귀적으로 해결한다. 그리고 각 부분의 해결을 합쳐 하나의 솔루션으로 만들어낸다. 결과적으로 브루트포스면 O(n^2)걸릴걸 O(n log n)으로 가능함

## merge sort

반으로쪼갠다 O(1)

쪼개진걸 각각 정렬한다 2T(n/2) = 2O(n/2 log n/2) = O(n log  n)

**비교할수 있을 정도로 작은 조각으로 계속 나눈다. 두개씩 비교할 수 있게33**

T(n)은 재귀의 시간복잡도를 나타냄. n이 깊이? 인거같음 걍 O표기법으로바꾸면 O(n log n)이 된다. 

{% include image_caption.html imageurl="/images/algorithm_final_2019-11-26__10.29.56.png" title="images" caption="" %}

정렬된쪼가리를 합친다(merge) O(n)

퀵소트는 divide하는 방식으로인해 divide가 느리지만 merge는 divide는 빠름(무조건중간)

merge는 임시 영역을 통해서 이뤄지는데 굳이 이렇게 안하고싶으면 안하는 방법도 있긴하다.. 

### Inversion counting

랭킹예제 : 나는 i가 j보다 랭킹이 높은데, 쟤는 j가 i보다 랭킹이 높을 때 i랑 j가 역전됐다고함 브루트포스로 비교해서 찾는방법이 있지만 시간복잡도 O(n^2)임...

이것도 나눠서생각하자 이말이야~

{% include image_caption.html imageurl="/images/algorithm_final_2019-11-26__11.07.12.png" title="images" caption="" %}

이걸 다시 하나로 합쳐서 또 돌린다. (sort and count + sort and count + merge and count)

기본적으론 merge sort랑 같다. 

**나눠서 각 나눠진 부분에서 inversion을 구하고 (구했으므로 한쪽은 정렬되어있다고 봐도 무방) 각 파트별로 합쳐진다고 했을 때 inversion을 또 구한다.** 

## closest pair

평면에 주어진 n개의 점에서 가장 작은 거리를 갖는 쌍을 찾기 

모든 쌍들을 다 검사하면 됨. 시간복잡도 O(n^2) 가능?ㅋㅋ

해결법 : 대강 n/2점들로 나뉘도록 평면을 잘라서 각 쪼가리에서 제일 짧은 거리를 찾고 그 둘을 비교해서 더 짧은걸 찾음. 그럼 merge할 때 merge된 사이에서 그 짧은 길이보다 짧은게 있는지 찾으면 됨 

{% include image_caption.html imageurl="/images/algorithm_final_2019-11-26__11.36.17.png" title="images" caption="" %}

아무리길어봐야 나눈 선 기준으로 좌우 델타만큼 벗어나지 않음. 그리고 이 델타 밴드 안에서(분리선의x좌표까지의 거리가 delta보다 작은 점들) 가장 가까운 점을 찾기위해서 y로 정렬을 시킨다. 이렇게 정렬시킨 후에는 아무리 가까운 점이 주변에 많아봐야 최대 7개가 있다. 따라서 y로 정렬시킨 상태에서 근처의 7개에 대해서 모두 조사한다. 

# 13

Greedy : 해법을 점차 찾아간다. 매 상황 최적의 선택

Divide and conquer : 문제를 작은 하위문제로 나누어 해결한뒤 솔루션을 합친다

Dynamic programming : 하위 문제의 연속으로 만든다. 작은문제에 살을붙어 큰문제를 해결해나간다. 

일반적으로 재귀 프로그램은 시간복잡도가 쓰레기다. 피보나치수열 구하는 프로그램의 시간복잡도는 지수적이다. 

그렇게 재귀적으로 동작하면 사실 생각보다 **이미 계산된 값을 다시 계산하는 중복 계산이 굉장히 많이 일어난다.** 

따라서 중복계산만 막아줘도 엄청나게 시간이 회복됨. 선형적인 시간복잡도가된다. 
```java
    public int fibo(int n ){
    	for(int i = 0; i < n; i++){
    			memoizedFibo[i] = -1;
    	}
    
    	memoizedFibo[0] = 0;
    	memoizedFibo[1] = 1;
    	return fiboByMemoization(n);
    }
    
    private int fiboByMemoization(int n){
    	if(memoizedFibo[n] < 0){
    		memoizedFibo[n] = fiboByMemoization(n - 2) + fiboByMemoization(n - 1);
    	}
    	return memoizedFibo[n];
    } 
```
### weighted interval scheduling

기존에 interval scheduling과 유사하지만 각 job에 weight이 있다. 단순히 안겹치는걸로 끝이 아니라, 겹치지 않으면서 최대의 weight 총 합을 만드는 것이 목표다.

기존 interval scheduling은 각 job이 끝나는 시간으로 greedy한 알고리즘을 쓰면 간단히 해결됐음. 그건 weighted에서 모든 job의 weight이 동일하다고 가정할때와 결과가 같다. 

기존 방식이 안통하는건 너무 자명하다. 일찍끝나는건 weight이 1이고 늦게끝나는건 999라고 가정해보자....

{% include image_caption.html imageurl="/images/algorithm_final_2019-12-01__1.16.28.png" title="images" caption="" %}

job들을 기존처럼 끝나는 시간 순으로 정렬할 때 p(j)를  j번째 job과 compatible한 가장 가까운 job의 번호를 i라 하면 j는 i보다 작은 job들과 compatible하다. 

{% include image_caption.html imageurl="/images/algorithm_final_2019-12-01__1.18.56.png" title="images" caption="" %}

예를들면 p(8)은 5번째 job부터 호환이되고, p(7)은 3번째부터 호환이된다. 

j번째 까지의 솔루션중 가장 optimal한걸 opt(j)를 통해 알 수 있다고 하자. 

opt(8)이 8번째를 무조건 선택한다고 가정하면, 8까지의 최적의 선택은 opt(5) + 8번째 job이 될 것이다. 

그럼 만약에 opt(8)이 8번째를 선택 안한다고 하면..? 그럼 간단하게 7을 선택한다고 다시 가정한다. **또 아니면 6을 고르면되니까**

{% include image_caption.html imageurl="/images/algorithm_final_2019-12-01__1.26.27.png" title="images" caption="" %}

이렇게 풀면 되는데 이걸 재귀로 풀면 지수적으로 꼬인다. 이미 한계산 또하고 또하고..삽질함

그래서 memoization을 도입한다.

n개의 최적 정답을 저장할 공간을 마련해서 계산됐던 값들은 저장, 계산된값이 있으면 가져다쓰고 없으면 계산해서 저장하도록 하여 중복계산을 막는다. 

그럼 O(n log n)의 시간을 소요한다.  ← 정렬하는데 시간 다 쓰나본데 ㅋㅋㅋㅋ

{% include image_caption.html imageurl="/images/algorithm_final_2019-12-01__10.06.05.png" title="images" caption="" %}

이진 탐색을 이용해서 p()를 수행한다. 
mid의 finish가 target의 start와 같거나 작으면 left를 mid + 1로 옮겨가며, 
mid의 finish가 target의 start보다 크다면 right를 mid -1 로 내려가며 이진탐색하다가
left랑 right가 만나거나 left가 right를 넘어서는 순간이 딱 알맞은거임
이진탐색이니까 O(n log n)

8의 optimal을 찾는거니까 8은 제외하고 1-7에서 탐색 → 5-7탐색 → 5-5탐색 순으로 간다. 

### OBST

최적의 이진트리는 지금까지 높이가 낮은 트리라고 했따. 근데 그건 모든 노드를 탐색할 확률이 같다고 가정했을 때 이야기

트리의 탐색에 실패할 경우에 대해서 고려를 해보아야 한다. 

트리의 검색에 실패할 경우 갈 곳이 없다. 그 갈 곳을 만들어서 외부노드 혹은 실패노드라고 명명해준다. 그럼 오리지널 트리의 노드들은 내부노드라고 할 수 있고, 이 실패노드 혹은 외부노드까지 포함한 트리를 확장이진트리라고 말해줄 수 있다.

외부 노드로의 path의 총 합은, 내부노드개수의 두배에 내부노드의 path합과 같다.

{% include image_caption.html imageurl="/images/algorithm_final_2019-12-01__10.45.39.png" title="images" caption="" %}

E = I + 2n → 노드의 개수는 상수이므로 고정적이고 path인 I를 최대한 줄이는것이 E를 줄이는 방향이다. 따라서 최악의경우는 skewed tree : O(n^2), 최적의 경우는 complete binary tree (O(n log n)가 된다. 

탐색에 성공할확률과 실패할확률을 해당 노드에 도달하는데 필요한 경로의 수와 곱한것의 총 합

따라서 찾을확률과 못찾을확률이 모두 동일하다고 치면 그냥 납작한 트리가 최고다. 

그러나 그렇지 않다면..

{% include image_caption.html imageurl="/images/algorithm_final_2019-12-01__1.58.51.png" title="images" caption="" %}

계산공식은 위와 같다. 근데 여기서 어떻게 트리를 끌어내지?

# 14

그래프의 최단거리를 찾는 Shortest paths 찾기에서 우리는 edge의 값이 음을 가지면 불가능한걸로 했었다. 다익스트라로 하면 불가능하다.  혹은 다른방식, 모든 weight를 양수가되도록 다 더해주고 하는것도 문제가있음.

다익스트라 REVIEW
가장 짧게갈 수 있는걸 선택, 선택된애들로 cost를 업데이트, 업데이트된 cost 중 또 짧게갈 수 있는걸 선택, 이걸 반복. 그리디 알고리즘의 일종

path의 수를  #of nodes - 1로 제한하고 음의 cost가 나오지 않도록 한다면? DP로 해결이 가능하다.

v → t로 k번 거쳐서 가는 최적의 cost가 Opt(k, v)라고 하자

**종이를 확인하도록 하자**

시간복잡도 O(mn), 공간복잡도 O(n^2)

- **OBST** 구하기
- **Huffman** 구하기
- 개념문제 3가지정도
    - ~~interval scheduling(뒤에 weighted가 있기 때문에 안나올 것 같음)~~
    - ~~optimal 2-way merge(huffman code가 있기 때문에 안나올 것 같음~~
    - ~~merge sort(inversion counting이 있어서 안나올것같음)~~
    - inversion counting
    - weighted interval scheduling
    - closet path
    - inversion counting
- Huffman, closest pair, quick sort 등 **실습에서 한문제**
- **응용코딩 문제**
