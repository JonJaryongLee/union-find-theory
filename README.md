# 5. Union-Find

# 1. Idea

열 명의 사람들이 파티에 참석했으며, 각 사람의 생일을 나타낸 배열은 다음과 같다.

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 8 | 2 | 2 | 8 | 3 | 3 | 12 | 12 | 6 | 8 |

생일이 동일한 사람을 그룹으로 묶어서 다음과 같이 표현하고 싶다.

![Untitled](5%20Union-Find%205f3c5372777641d5b10d9a3c4d9d1072/Untitled.png)

이렇게 각 집합별로 묶어둘 경우, 다음 두 가지 상황의 값을 빠르게 구할 수 있을 것이다.

1. 두 사람이 같은 그룹에 속해있는지
2. 특정 사람이 어느 그룹에 속해있는지

이럴 때 사용하는 것이 Union-Find 자료구조다. 단, C++ 에선 집합을 직접적으로 표현하는 자료구조는 존재하지 않으므로 배열과 재귀로 구현할 것이다.

아이디어는 다음과 같다.

1. 먼저, 각 인덱스의 부모를 의미하는 `parent` 배열을 선언하며, 맨 처음엔 각 인덱스와 값을 동일하게 둔다.
    
    ```cpp
    int parent[10] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
    ```
    
    이것은 열 명의 사람이 모두 다른 생일을 가지고 있다는 뜻이다. 즉, 각 사람은 각각의 집합을 이루고 있다. 그래프 이론으로 확장하기 위해, 각각의 사람을 "노드" 로 표현하겠다.
    
2. Union
    
    이중루프를 사용해 데이터셋을 순회하면서, 값이 같은 노드를 발견하면 하나의 집합으로 묶는다. 이것을 Union 이라고 표현하며, C++ 에선 union 이 키워드이므로 `setUnion()` 함수로 구현할 것이다.
    
    ```cpp
    // 생일이 같은 사람들끼리 집합을 만듭니다.
    for (int i = 0; i < 10; i++)
    {
        for (int j = i + 1; j < 10; j++)
        {
            if (birthdays[i] == birthdays[j])
            {
                setUnion(i, j);
            }
        }
    }
    ```
    
3. Find
    
    각 노드가 어떤 집합에 속하는지 알려준다. 이것을 Find 라고 하며, `find()` 함수로 구현할 것이다.
    
    ```cpp
    // 각 사람이 어떤 집합에 속하는지 출력합니다.
    for (int i = 0; i < 10; i++)
    {
        cout << i << "는" << find(i) << "그룹에 속해있습니다." << endl;
    }
    ```
    

이런 자료구조를 Union 과 Find 를 합쳐서 Union-Find 라고 부른다.

# 2. 진행과정

먼저, 어떤 식으로 진행되는지 그 흐름을 살펴보자.

맨 처음엔, 주어진 `birthdays` 와 초기화된  `parent` 는 다음과 같다.

 

birthdays

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 8 | 2 | 2 | 8 | 3 | 3 | 12 | 12 | 6 | 8 |

parent

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |

여기서, parent 의 idx 와 val 이 동일하다는 건, 맨 처음엔 각각의 노드가 각각의 집합을 가지고 있다는 뜻이라고 했다. 즉, 각 사람의 생일이 모두 다르다고 가정한 것이다.

이제 다음 이중루프가 실행될 것이다.

```cpp
// 생일이 같은 사람들끼리 집합을 만듭니다.
for (int i = 0; i < 10; i++)
{
    for (int j = i + 1; j < 10; j++)
    {
        if (birthdays[i] == birthdays[j])
        {
            setUnion(i, j);
        }
    }
}
```

생일이 같은 사람이 맨 처음 등장하는 상황은 언제인가? 

birthdays

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 8 | 2 | 2 | 8 | 3 | 3 | 12 | 12 | 6 | 8 |

0번 노드가 `i` , 3번 노드가 `j` 일 경우다. 이 때, Union 즉 `merge()` 함수가 실행된다.

`merge()` 는 둘을 같은 집합으로 묶을 것이다. 그런데, C++ 에선 집합이라는 자료구조가 딱히 없지 않은가? 대체 둘을 어떻게 같은 집합으로 묶을 수 있을까?

이를 위해 `parent` 배열이 필요하다. 3번 인덱스를 다음과 같이 변경한다.

parent

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 0 | 1 | 2 | 3 ⇒ 0 | 4 | 5 | 6 | 7 | 8 | 9 |

이렇게 할 경우, 3번 인덱스의 부모는 0이라는 뜻이 된다. 즉, `parent` 는 그래프의 구조를 나타내기 위한 배열이다. 부모에 대한 정보를 담고 있기 때문에 `parent` 라고 네이밍한 것이다.

이렇게 표현함으로서, 둘은 더 이상 별개의 집합이 아니라 같은 집합에 속하게 된다.

![Untitled](5%20Union-Find%205f3c5372777641d5b10d9a3c4d9d1072/Untitled%201.png)

birthdays

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 0 | 2 | 2 | 0 | 3 | 3 | 12 | 12 | 6 | 8 |

다음, 0번 노드가 i , 9번 노드가 j 일 경우, `setUnion()` 함수가 실행된다.

어떻게 하는게 좋을까? 9번 노드의 부모를 0번으로 갱신하면 될 것이다.

parent

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 0 | 1 | 2 | 0 | 4 | 5 | 6 | 7 | 8 | 9 ⇒ 0 |

그래프는 다음과 같이 표현될 것이며, 해당 그래프의 모든 노드는 같은 집합에 속하게 된다.

![Untitled](5%20Union-Find%205f3c5372777641d5b10d9a3c4d9d1072/Untitled%202.png)

그런데, 어떤 두 개의 노드가 같은 그룹에 속해있으면 그 루트도 동일할수밖에 없다. 이 역할을 하는 함수가 바로 Union-Find 에서의 `find()` 함수다.

즉, `find()` 함수는 그래프의 루트 노드를 찾아서 반환하는 역할을 하게 된다. `setUnion()` 내부에서 `find()` 함수가 쓰일 것이며, `main()` 에서 `find()` 함수를 별도로 사용하기도 한다.

최종적으로 `setUnion()` 함수가 끝났을 땐 다음과 같은 `parent` 배열이 완성될 것이다. `birthday` 와 비교해보고, 제시된 그래프와도 비교해보자.

특히 `parent` 에서 idx 와 val 이 똑같은 노드들이 확인될 것인데, 이것이 바로 집합의 대표 역할을 하는 루트 노드를 의미한다.

birthdays

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 8 | 2 | 2 | 8 | 3 | 3 | 12 | 12 | 6 | 8 |

parent

| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| val | 0 | 1 | 1 | 0 | 4 | 4 | 6 | 6 | 8 | 0 |

![Untitled](5%20Union-Find%205f3c5372777641d5b10d9a3c4d9d1072/Untitled%203.png)

# 3. 구현

위 과정을 코드로 구현해보면 다음과 같다.

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>

using namespace std;

// 부모 노드
int parent[10];
// 각 사람의 생일을 나타내는 배열
int birthdays[10] = { 8, 2, 2, 8, 3, 3, 12, 12, 6, 8 };

int find(int tar)
{
    if (tar == parent[tar])
        return tar;

    return find(parent[tar]);
}

void setUnion(int a, int b)
{
    int t1 = find(a);
    int t2 = find(b);

    if (t1 == t2)
        return;

    parent[t2] = t1;
}

int main()
{
    for (int i = 0; i < 10; i++)
    {
        parent[i] = i;
    }

    // 생일이 같은 사람들끼리 집합을 만듭니다.
    for (int i = 0; i < 10; i++)
    {
        for (int j = i + 1; j < 10; j++)
        {
            if (birthdays[i] == birthdays[j])
            {
                setUnion(i, j);
            }
        }
    }

    // 각 사람이 어떤 집합에 속하는지 출력합니다.
    for (int i = 0; i < 10; i++)
    {
        cout << i << "는" << find(i) << "그룹에 속해있습니다." << endl;
    }

    return 0;
}
```

각 부분별로 살펴보자.

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>

using namespace std;

// 부모 노드
int parent[10];
// 각 사람의 생일을 나타내는 배열
int birthdays[10] = { 8, 2, 2, 8, 3, 3, 12, 12, 6, 8 };
```

그래프에서 각 노드의 부모 정보를 담고 있는 `parent` 와, 각 사람의 생일을 나타내는 `birthdays` 를 정의한다.

`main()` 부터 보면,

```cpp
int main()
{
    for (int i = 0; i < 10; i++)
    {
        parent[i] = i;
    }
```

부모 노드를 맨 처음엔 idx == val 로 세팅한다. 이것은 각각의 사람의 생일이 서로 다름을 의미한다.

```cpp
// 생일이 같은 사람들끼리 집합을 만듭니다.
for (int i = 0; i < 10; i++)
{
    for (int j = i + 1; j < 10; j++)
    {
        if (birthdays[i] == birthdays[j])
        {
            setUnion(i, j);
        }
    }
}
```

`parent` 배열 세팅 부분이다. 생일이 같은 사람일 경우, Union-Find 의 Union 에 해당하는 `setUnion()` 을 실행해 같은 집합으로 만든다.

```cpp
// 각 사람이 어떤 집합에 속하는지 출력합니다.
for (int i = 0; i < 10; i++)
{
    cout << i << "는" << find(i) << "그룹에 속해있습니다." << endl;
}
```

Union-Find 의 Find 에 해당하는 `find()` 함수 활용 부분이다. 우리의 과정대로라면, 각 집합에서의 최댓값이 출력될 것이다.

제공된 `birthdays` 와 비교해서, 올바른 결과가 나왔는지 확인해보자.

```cpp
int birthdays[10] = {8, 2, 2, 8, 3, 3, 12, 12, 6, 8};
```

```
0는0그룹에 속해있습니다.
1는1그룹에 속해있습니다.
2는1그룹에 속해있습니다.
3는0그룹에 속해있습니다.
4는4그룹에 속해있습니다.
5는4그룹에 속해있습니다.
6는6그룹에 속해있습니다.
7는6그룹에 속해있습니다.
8는8그룹에 속해있습니다.
9는0그룹에 속해있습니다.
```

다음, `find()` 와 `setUnion()` 함수를 함께 보자.

```cpp
int find(int tar)
{
    if (tar == parent[tar])
        return tar;

    return find(parent[tar]);
}

void setUnion(int a, int b)
{
    int t1 = find(a);
    int t2 = find(b);

    if (t1 == t2)
        return;

    parent[t2] = t1;
}
```

`find()` 함수는 노드를 하나 받은 후, 루트가 나올 때까지 부모 노드를 추적해서 올라간다.

만약, idx == val 이라면 루트 노드이므로 찾은 값을 반환한다.

이 값이 집합의 대표값이 되며, 두 노드를 각각 find 했을 때 대표값이 같다면 두 노드는 같은 집합에 속해 있다는 뜻이다.

`setUnion()` 함수는 같은 집합으로 묶을 두 개의 노드를 `a`, `b` 로 받고, 루트 노드를 찾아 `t1`, `t2` 를 갱신한다.

둘의 루트 노드가 같다면 같은 집합에 속한 노드라는 뜻이므로 더 이상의 작업이 필요없다.

다를 경우, `t2` 의 부모 노드는 `t1` 으로 저장해주어 그래프 연결관계로 만들고, 같은 집합으로 묶는다.

우선 여기까지가 Union-Find 를 잘 이해하는 첫번째 단계다. 이해 안 되는 부분이 있다면 여기까지 다시 복습해보자.

이제 최적화 기법을 배워보자.

# 4. 최적화 - Path Compression

`find()` 함수는 `parent` 를 따라가서 루트를 찾을 때까지 계속 올라갈 수밖에 없는 구조다.

그런데, 그럴 필요 없이 `parent` 에 루트 노드를 배정해버리면 `find()` 는 중간 과정을 생략하고 상수 시간으로 루트를 찾게 된다.

이 최적화 기법을 경로 압축 (Path Compression) 이라고 한다.

기존

```cpp
int find(int tar)
{
    if (tar == parent[tar])
        return tar;

    return find(parent[tar]);
}
```

Path Compression

```cpp
int find(int tar)
{
    if (tar == parent[tar])
        return tar;

    // Path Compression
    int ret = find(parent[tar]);
    parent[tar] = ret;
    return ret;
}
```

이 경우, 처음 `setUnion()` 시에만 루트 노드를 재귀적으로 찾고, `main()` 에서 `find()` 사용 시엔 재귀를 딱 한번만 사용해 루트를 찾게 된다.

Q. 생일 예제는 어차피 높이가 2단계까지밖에 가지 않아서 Path Compression 이 딱히 필요하지 않을 것 같다.

A. 그렇다. 생일 예제는 Union-Find 를 설명하는 가장 대표적인 예제다. 하지만 훨씬 복잡한 상황에서 Union-Find 사용 시엔 Path Compression 이 그 힘을 발휘한다. 아래의 경우,

 

![Untitled](5%20Union-Find%205f3c5372777641d5b10d9a3c4d9d1072/Untitled%204.png)

이런 식으로 트리가 만들어졌다고 가정하면 `setUnion()` 과정 이후 `parent` 의 인덱스 0, 9, 3, 6, 7 은 모두 7 이므로 훨씬 효율적이다. 

# 5. 완성코드

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>

using namespace std;

// 부모 노드
int parent[10];
// 각 사람의 생일을 나타내는 배열
int birthdays[10] = { 8, 2, 2, 8, 3, 3, 12, 12, 6, 8 };

int find(int tar)
{
    if (tar == parent[tar])
        return tar;

    // Path Compression
    int ret = find(parent[tar]);
    parent[tar] = ret;
    return ret;
}

void setUnion(int a, int b)
{
    int t1 = find(a);
    int t2 = find(b);

    if (t1 == t2)
        return;

    parent[t2] = t1;
}

int main()
{
    for (int i = 0; i < 10; i++)
    {
        parent[i] = i;
    }

    // 생일이 같은 사람들끼리 집합을 만듭니다.
    for (int i = 0; i < 10; i++)
    {
        for (int j = i + 1; j < 10; j++)
        {
            if (birthdays[i] == birthdays[j])
            {
                setUnion(i, j);
            }
        }
    }

    // 각 사람이 어떤 집합에 속하는지 출력합니다.
    for (int i = 0; i < 10; i++)
    {
        cout << i << "는" << find(i) << "그룹에 속해있습니다." << endl;
    }

    return 0;
}
```