# Linked list
> 참조(link)로 이어져 있는 리스트 자료구조

## Linked list의 특징

1. Array와 같이 선형적 자료구조이다.

2. Array와 달리 data와 link로 구성되어 있어 배열 중간에 삽입하거나 삭제하는 것이 용이하다.

3. Array보다 데이터 접근이 느리다 (Array는 메모리에 연속적으로 존재하기 때문에 O(1)에 접근이 가능하지만 linked list는 link를 따라 해당 데이터를 찾아야 한다.)

## Linked list의 주요 연산

1. 삽입 및 삭제
Array는 배열의 마지막에 데이터를 추가하는 경우를 제외하고 O(n)의 성능을 갖는 반면 linked list는 O(1)의 성능으로 데이터를 삽입, 삭제할 수 있다.

2. 탐색
Array는 O(1)의 성능으로 인덱스의 데이터에 접근할 수 있지만 linked list는 모든 노드를 차례대로 순회하며 해당 데이터에 접근해야 한다(O(n)).

## Linked list의 구현

```python
class Node:
    def __init__(self, data=None)
        self.__data = data
        self.__prev = None
        self.__next = None
        # data, prev, next 초기화

    @property
    def data(self):
        return self.__data
    
    @data_setter(self, data)
        self.__data = data

    @property
    def prev(self):
        return self.__prev

    @prev.setter
    def prev(self, pr):
        self.__prev = pr

    @property
    def next(self):
        return self.__next

    @next.setter
    def next(self, ne):
        self.__next = ne
```

- data저장: __data  
- 이전 노드 참조: __prev  
- 다음 노드 참조: __next

```python
class linkedList:
    def __init__(self):
        self.head = Node()
        self.tail = Node()

        self.head.next = self.tail
        self.tail.next = self.head
```

- node로 linked list를 구현
- head와 tail로 처음과 끝을 지정한다

## Linked list의 주요 기능 구현

### 삽입

```python
def add_after(self, data, node):
    # 지정 node의 다음에 새로운 노드 값 추가
    new_node = Node(data)
    # (1) 새로 추가할 노드(new_node)

    new_node.next = node.next
    # (2) new_node에 link될 다음 노드 값 = 지정 node의 next에 있는 노드
    new_node.prev = node
    # (3) new_node에 link될 이전 노드 값 = 지정 node

    node.next.prev = new_node
    # (4) 지정 node의 다음 노드값에 link될 이전 node = new_node
    node.next = new_node
    # (5) 지정 node에 link될 다음 노드값 = new_node
```

![linked_list](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/linked_list.png?raw=true)

### 삭제

```python
def delete(self, node):
    node.prev.next = node.next
    # (1) ...
    node.next.prev = node.prev
    # (2) ...
```

![linked_list2](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/linked_list2.png?raw=true)

링크가 끊긴 노드는 가비지 컬렉션이 제거한다.

### 탐색

```python
def search(self, target):
    x = self.head.next
    # linkedlist의 처음부터 탐색 시작

    while node.next:
        # node의 next가 존재하지 않을때까지 탐색

        if x.data == target:
            return x
            # 만약 x의 data가 target에 해당하면 x 출력
        x = x.next
        # 아니라면 x.next로 이동

    return None
    # 찾지 못한다면 Node 리턴
```