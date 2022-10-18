# 실행 컨텍스트
> 소스 코드를 실행하는데 필요한 환경을 제공하고 코드 실행 결과를 실제로 관리하는 영역

실행 컨텍스트를 설명하기 위해 다음을 예시로 든다.

```javascript
var v1 = 10;
const c1 = 20;

function f1(a) {

    const c2 = 30;
    b = c2 + 10;

    function f2(b) {
        console.log(b)
    }
    f2(b)
}

f1(100)
```

# 소스코드의 평가
> 실행 컨텍스트를 생성하고 변수, 함수 등 선언문을 먼저 실행 하여 실행 컨텍스트에 등록하는 것

소스코드의 평가 과정에서는 전역코드의 평가가 이루어진다. 이 과정에서는 선언문만 먼저 실행된다.

```javascript
v1,
c1,
f1(), 
f2()

선언됨
```

# 소스코드의 실행
> 소스코드의 평가가 끝나면 소스코드를 실행하여 변수, 함수의 참조를 실행컨텍스트에 등록하는 것

![sourcecode_evaluation01](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/sourcecode_evaluation01.png?raw=true)

먼저 위와 같이 글로벌 컨텍스트부터 순차적으로 callstack에 쌓이게 된다.  
이때 전역변수에 값이 할당되고, 이후 함수가 callstack에 쌓인다.

![sourcecode_evaluation02](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/sourcecode_evaluation02.png?raw=true)

실행 컨택스트 내에는 Lexical Environment라는 것이 생긴다.  
(Lexical Environment란 key:value 형태로 식별자와 식별자에 바인딩 된 값을 기록해두는 객체이다)

### Environment Record

자바스크립트는 전체 코드를 스캔하여 선언문만 먼저 실행된 변수나 함수를 Lexical Environment 내부에 있는 Environment Record에 등록한다.

![sourcecode_evaluation03](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/sourcecode_evaluation03.png?raw=true)

Environment Record는 다시 Object Environment Record와 Declarative Environment Record로 나뉜다.

### Outer Lexical Environment Refrence

Outer Lexical Environment Refrence는 상위 Lexical Environment를 참조하는 포인터이다.  
자세한 내용은 이후 다시 설명한다.

# 실행컨텍스트 스택

![EventLoop](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/EventLoop.png?raw=true)

위 그림(EventLoop)에서 callstack이 실행컨텍스트 스택이다.

![sourcecode_evaluation04](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/sourcecode_evaluation04.png?raw=true)

위의 코드에서 f1(100)이 실행되면 callstack(실행컨텍스트 스택)에 새롭게 f1(실행컨텍스트)이 등록되며 마찬가지로 Lexical Envorinment가 생성된다.

# Outer Lexical Environment Refrence

![sourcecode_evaluation05](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/sourcecode_evaluation05.png?raw=true)

f1()을 실행하면 f2()가 실행되고 f2에는 b와 console, log가 없기 때문에 이를 상위 스코프에서 찾아야 한다. Outer Lexical Environment Refrence는 단방향 스코프체인을 구현해 외부 렉시컬에 대한 참조를 가능하게 한다.


b는 f1의 렉시컬에서 찾을 수 있고, console은 전역 렉시컬의 객체 환경 레코드에서 찾을 수 있고, log는 console객체의 프로토타입 체인을 통해 찾을 수 있다.


위와 같이 함수의 실행이 완료되고 나면 callstack에서 하나씩 빠져나가게 되며, 마지막 함수가 끝나면 실행컨텐스트가 종료된다.

```javascript
var v1 = 10;
const c1 = 20;

function f1(a) {

    const c2 = 30;
    b = c2 + 10;

    function f2(b) {
        console.log(b)
    }
    f2(b)
}

f1(100)
```

실행 결과

```javasvript
40
```

## 참고

- 자바스크립트 Deep dive

- [호이스팅이란 무엇인가?](https://hardworking-everyday.tistory.com/121?category=983473)

- [JavaScript 10강 - 실행 코드로 알아보는 실행컨텍스트 동작 원리](https://www.youtube.com/watch?v=pfQfEwnJHRs)

- [자바스크립트 실행 환경](https://velog.io/@hoo00nn/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%8B%A4%ED%96%89-%ED%99%98%EA%B2%BDExecution-Context)