---
layout: single
title:  "C++ STL priority_queue 우선순위 큐 사용법"
excerpt: "C++ stl을 사용한 우선순위 큐 내용정리. How to use priority_queue in C++ stl"
header:
  teaser:
search: true
permalink: /algorithm/use-priorityqueue/
categories: 
  - Algorithm
tags:
  - algorithm
  - priority
  - queue
  - C++
date:  2021-05-27 22:04:00 +0900
last_modified_at: 2022-05-11 17:25:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

# 0. 우선순위 큐(Priority Queue)란?

**(이 글은 Queue와 Vector의 개념을 알고오셔야합니다!)**

Priority Queue는 Container의 한 종류이며

일반 queue는 First In First Out(선입선출)인 것에 반해

설정된 우선순위에 따라 가장 Top을 유지하고 먼저 Out(pop) 된다.

내부적으로 Heap의 자료구조를 갖고있다.

# 1. 기본사용법

## 1.1 헤더

우선순위 큐는 다음과 같이 queue를 include 하여 사용한다.

``` c++
#include <queue>
```

## 1.2 멤버 함수

가볍게 보면 queue와 사용법이 비슷하다고 볼 수 있다.

[멤버 함수]
  - empty
  - size
  - top
  - push
  - emplace
  - pop
  - swap

[멤버함수 설명]
1. empty : priorityqueue가 비어있는지 확인한다.
2. size : priorityqueue의 크기를 확인한다.
3. top : priorityqueue 내부의 제일 우선순위의 값을 보여준다.
4. push : priorityqueue에 값을 삽입한다.
5. emplace : priorityqueue에 구조를 삽입한다.
6. pop : priorityqueue에서 제일 우선순위의 값을 제거한다.
7. swap : 두개의 priorityqueue를 swap한다.(내부를 서로 바꾼다.)

### 1.2.1 push와 emplace의 차이
{: .text-center}

밑에서 더 설명하겠지만, 간단히 말하면 우선순위 큐의 경우 구조체를 많이 삽입하게 된다.

push의 경우 단순히 queue에 값을 넣어준다. 다시 말하면 오브젝트로 제작후 삽입해야하므로 불필요한 복사가 많이 일어난다.

emplace의 경우 오브젝트를 생성하지 않고 바로 값을 넣는다. 즉, copy와 constructor가 합쳐진 것이라 볼 수 있다.

아래 예시코드로 두개의 사용법과 차이점을 자세히 살펴보자.

참고 및 출처 : [https://lhy0.tistory.com/3](https://lhy0.tistory.com/3)

``` c++
#include <queue> // 우선순위 큐를 위한 라이브러리
#include <iostream> // cout사용을 위한 라이브러리
#include <utility> // pair를 위한 라이브러리
using namespace std; 
    
int main() { 
    // pair를 요소로 갖는 priority_queue를 선언
    priority_queue< pair<char, int> > pqueue; 
        
    // pair의 오브젝트를 만들지 않고 바로 값을 push한다
    pqueue.emplace('a', 24); 
        
    // 아래의 주석처리된 명령어는 오류가 난다.
    // pqueue.push('b', 25);     
        
    // push를 사용할 때에는 pair를 만들어 준 다음에 넣어야 한다. 이 과정에서 불필요한 복사가 일어난다.
    pqueue.push(make_pair('b', 25));
        
    // 우선순위 큐 출력부분
    while (!pqueue.empty()) { 
        pair p =  pqueue.top(); 
        cout << p.first << " "
             << p.second << endl; 
        pqueue.pop(); 
    } 

    // 출력 결과
    // b 25
    // a 24
    return 0; 
} 
```

### 1.2.2 쉬운 예시
{: .text-center}

간단하게 우선순위큐 사용방법은 아래와 같이 쓸 수 있다.

``` c++
#include <iostream>
#include <queue>
using namespace std;

int main() {
  // 우선순위 큐 선언
  priority_queue<int> pq;

  // 우선순위 큐에 값을 넣는다.
  pq.push(5);
  pq.push(3);
  pq.push(6);
  pq.push(1);
  pq.push(2);

  cout << pq.size() << endl;
  // 출력결과 
  // 5

  // 우선순위 큐 출력
  // 큐가 비어있지 않다면 무한반복 비어있다면 반복문을 실행하지 않는다
  while(!pq.empty()) {
    // 큐의 제일 앞의 부분의 값 가져오기
    int temp = pq.top();
    cout << temp;
    // 큐의 제일 앞의 부분 빼기
    pq.pop();
  }

  // 출력결과
  // 65321
  return 0;
}
```

넣는 순서와 상관없이 큰 값이 제일 먼저 나오게 된다.

---

# 2. 심화사용법

큰값이 나오는 것이 아닌 작은 값 순서로 배치되게 하려면 어떻게 해야할까.
혹은 여러값을 큐에 넣고 싶을 때는 어떻게 해야할까.

## 2.1 우선순위 큐 활용

우선순위 큐를 활용하기 위해서는 선언을 다음과 같이 해야함을 알아두자.

```c++
#include<queue>

priority_queue<자료형, 구현체, 비교연산자> pq;
```

1. 자료형 : int, double 등의 기본자료형 뿐만 아니라 구조체, 클래스 등 다양하게 사용할 수 있다.
2. 구현체 : 보통 vector<자료형> 으로 구현한다. (stl에서 힙을 구현하기 좋은 자료형이면 다 가능하다고 한다. vector를 include하지 않아도 동작한다.)
3. 비교연산자 : 비교를 위한 기준을 알려준다.

### 2.1.1 작은수 우선
{: .text-center}

위의 선언방법을 이용하여 우선순위 큐를 큰값이 아닌 작은 값으로 배치되게 하려면 어떻게 해야할까.

#### 2.1.1.1 기본 비교연산자
{: .text-center}

비교연산자에 greater<자료형> 을 사용하면 작은수부터 출력된다
(큰수부터 출력하려면 less<자료형> 이다.)

```c++
#include <queue>
#include <iostream>
using namespace std;

int main() {
  // 우선순위 큐를 <자료형, 구현체, 비교연산자> 를 이용하여 선언한다.
  // 비교연산자에 greater<int>를 사용하여 int가 작은값이 우선한다.
  priority_queue <int, vector<int>, greater<int> > pq;
  
  pq.push(5);
  pq.push(3);
  pq.push(6);
  pq.push(7);

  while(!pq.empty()) {
    int temp = pq.top();
    cout << temp;
    pq.pop();
  }
  // 출력결과
  // 3567
  return 0;
}
```


#### 2.1.1.2 비교연산자 활용
{: .text-center}

```c++
#include <queue>
#include <iostream>
using namespace std;

// 비교연산 선언
// int형인 a와 b가 있을때
// 반환에 a > b 를 입력하면 작은수부터 출력한다.
// (반대로 a < b 를 입력하면 큰수부터 출력한다.) 
struct cmp {
  bool operator()(int a, int b) {
    return a > b;
  }
};

int main() {
  priority_queue <int, vector<int>, cmp> pq;
  
  pq.push(5);
  pq.push(3);
  pq.push(6);
  pq.push(7);

  while(!pq.empty()) {
    int temp = pq.top();
    cout << temp;
    pq.pop();
  }
  // 출력결과
  // 3567
  return 0;
}
```

### 2.1.2 구조체 활용
{: .text-center}

우선순위 큐를 제대로 활용하기 위해 보통 다음과 같이 구조체를(다른 컨테이너의 방법으로 사용해도 된다) 이용하여 많이 사용한다.

``` c++
#include <queue>
#include <vector>
#include <iostream>
using namespace std;

//사용할 구조체
struct s{
  int i;
  char c;
  
  //생성자
  s(int num, char alpha) : i(num), c(alpha) {}
};

// 비교를 위한 기준
struct cmp {
  bool operator()(s a, s b) {
    // int형의 값이 같을 경우
    if(a.i == b.i) {
      // char형의 값이 큰 것이 우선하도록한다.
      return a.c < b.c;
    }
    // int형의 값이 작은 것이 우선하도록한다.
    return a.i > b.i;
  }
};

int main() {
  //우선순위큐를 선언할때 <자료형, 구현체, 비교연산자> 로 선언한다.
  priority_queue <s, vector<s>, cmp> pq;
  
  //생성자를 이용한 구조체 입력
  pq.push(s(3, 'a'));
  pq.push(s(4, 'b'));
  pq.push(s(5, 'y'));
  pq.push(s(3, 'z'));
  pq.push(s(100, 'z'));

  while(!pq.empty()) {
    s temp = pq.top();
    cout << temp.i << " " << temp.c << endl;
    pq.pop();
  }
// 출력결과
// 3 z
// 3 a
// 4 b
// 5 y
// 100 z
  return 0;
}
```
출력결과를 보면 숫자의 경우 작은 수가 우선하고 숫자가 같을경우 알파벳이 큰 것이 우선되도록 되어있다.(수 뿐만 아니라 다른 자료형도 크기 비교가 가능하다)

# 3. 예제

## 3.1 백준 1966 프린터큐

**문제**

[백준 - 1966 프린터 큐](https://www.acmicpc.net/problem/1966) (https://www.acmicpc.net/problem/1966)

---

**내가 쓴 답 풀이**

[풀이](https://github.com/jungeu1509/algorithm_Baekjoon/blob/master/2020/1966(Printer_queue).cpp) (https://github.com/jungeu1509/algorithm_Baekjoon/blob/master/2020/1966(Printer_queue).cpp)

---

**참고 풀이**

[Easy is Perfect - [백준] 1966번 C/C++ 풀이 _ 프린터 큐](http://melonicedlatte.com/algorithm/2018/02/18/230705.html) (http://melonicedlatte.com/algorithm/2018/02/18/230705.html)


# 4. 참고 및 출처

1. [cplusplus - priority_queue reference](http://www.cplusplus.com/reference/queue/priority_queue/swap/) (http://www.cplusplus.com/reference/queue/priority_queue/swap/)
2. [lhy0 - priority_queue emplace() vs push()](https://lhy0.tistory.com/3) (https://lhy0.tistory.com/3)
3. [프로그래밍 - [C++ STL] Priority_queue 사용법](https://kbj96.tistory.com/15) (https://kbj96.tistory.com/15)
4. [geeksforgeeks - priority_queue emplace() in C++ STL](https://www.geeksforgeeks.org/priority_queue-emplace-in-cpp-stl/) (https://www.geeksforgeeks.org/priority_queue-emplace-in-cpp-stl/)
5. [구사과 - STL priority queue 활용법](https://koosaga.com/9) (https://koosaga.com/9)
   