# Stack

> 후입선출 형태의 (LIFO: Last In First Out)의 자료구조

## Stack의 특징

1. 선형 자료구조이다.

2. 삽입과 삭제가 한 방향에서 이루어진다

3. 따라서 가장 나중에 들어간 요소가 가장 마지막에 나오는 구조를 가지고 있다(LIFO).

## Stack의 주요 연산

1. top() 스택의 가장 위에 있는 요소를 제거하지 않고 출력한다.

2. push(): 스택의 위에서부터 원소를 삽입한다.

3. pop(): 스택의 가장 위에 있는 원소를 제거하면서 반환한다.

## Stack의 구현

파이썬의 기본 list를 사용하면 매우 쉽게 구현이 가능하다.

```python
class Stack:
    
    # Stack using list
    def __init__(self):
        self.content = []
        
    # Length
    def __len__(self):
        return len(self.content)
    
    # tush()
    def push(self, item):
        self.content.append(item)
        
    # top()
    def pop(self, item):
        self.content.pop(-1)
        
    # top()
    def top(self):
        if self.content:
           return self.content[-1]
        else:
            print("Stack underflow")
```

## Stack의 연산 속도

데이터 삽입(push): O(1)  
데이터 삭제(pop): O(1)  
top 조회(top): O(1)  
top이외의 데이터 조회: O(n)