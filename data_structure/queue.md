# Queue

> 선입선출 형태의 (FIFO: First In First Out)의 자료구조

## Queue의 특징

1. 선형 자료구조이다.

2. 삽입과 삭제 연산이 반대 방향에서 이루어진다.

3. 가장 먼저 들어간 요소가 가장 먼저 나온다(FIFO).

## Queue의 주요 연산

1. enqueue(): 큐의 오른쪽 끝에서 요소를 넣는다.

2. dequeue(): 큐의 왼쪽에서 자료를 반환하고 삭제한다.

## Queue의 구현

```python
class Node:
    # Linked_list
    def __init__(self, item):
        self.item = item
        self.next = None

class Queue:
    # Queue using linked list
    def __init__(self):
        self.front = None
        self.rear = None

    # Enqueue
    def enqueue(self, val):
        new_node = Node(val)

        if not self.front:
            self.front = self.rear = new_node
        else:
            self.rear.next = new_node
            self.rear = self.rear.next

    # Dequeue
    def dequeue(self):
        if not self.front:
            return "queue is empty"
        else:
            val = self.front.item
            self.front = self.front.next
            return val
```

## Python의 deque모듈

앞 뒤 모두에서 O(1)에 요소를 추가하고 제거할 수 있는 deque 모듈을 사용한다.

### deque의 사용법

```python
from collections import deque

Q = deque([])
```

내부에 리스트[]를 선언해주어야 오류 없이 사용할 수 있다.


1. append(item): 오른쪽 끝에 삽입 - O(1)

2. appendleft(item): 왼쪽 끝에 삽입 - O(1)

3. pop(): 오른쪽 끝에서 반환하고 삭제 - O(1)

4. popleft(): 왼쪽 끝에서 반환하고 삭제 - O(1)

5. extend(array): 주어진 array를 오른쪽에 추가 - O(k)

6. extendleft(array): 주어진 array를 왼쪽에 추가 - O(k)

7. remove(item)L 해당 item을 찾아서 삭제 - O(n)

8. rotate(): 해당 숫자만큼 회전 (+시계방향 -반시계방향) - O(k)