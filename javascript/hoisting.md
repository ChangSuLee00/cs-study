# 호이스팅

> 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것.

호이스팅은 hoist(감아 올리다) + ing의 결합으로, 코드가 실행되기 이전에 함수와 변수가 해당 스코프의 최상단으로 끌어 올려져 있는 것 '처럼' 동작하는 현상을 의미한다.

## 호이스팅의 예시

```javascript
console.log(x); // undefined

var x;
x = 10;

console.log(x); // 10

function hoist() {
  console.log(y); // undefined

  var y;
  y = 20;

  console.log(y); // 20
}

hoist();
```

위와 같은 코드를 실행시키면

```javascript
undefined;
10;
undefined;
20;
```

위와 같은 결과가 출력된다.

첫번째 console.log(x); 를 출력했을때 문맥상으로 아직 var x; 이전이기에  
'x가 선언되지 않았다'는 뜻의 ReferenceError: x is not defined가 뜰 것 같지만  
'(x가 선언되었으나) x의 값이 정의되지 않았다' 라는 뜻의 undefined가 출력된다.

<details markdown="1">
<summary>함수의 스코프 확인</summary>

```javascript
console.log(y);

function hoist() {
  var y;
  y = 20;
}

hoist();
```

위와같은 코드 작성시 ReferenceError: y is not defined 에러가 발생한다.

호이스팅이 '해당 스코프의' 최상단에 끌어올려진 것 처럼 동작한다는 것을 알 수 있다.

</details>

---

## 호이스팅이 발생하는 이유

코드가 일반적인 생각과 다르게 작동하는 이유는
자바스크립트 엔진이 '소스코드의 평가'와 '소스코드의 실행'을 나누어 처리하기 때문이다.

소스코드의 평가와 실행을 살펴보기 이전에 호이스팅이 일어나는 중요한 요인중 하나인 실행 컨텍스트를 알아보기로 하자.

### 실행 컨텍스트 (Execution context)

실행 컨텍스트는 실행할 코드에 제공할 환경 정보를 모아놓은 객체이다.  
다른 말로 하자면 코드가 실행되기 위한 환경이라고 할 수 있다.

실행 컨텍스트는 콜 스택(FILA)의 형태로 스택이 쌓이고 빠져나가는 알고리즘을 통해 전체 코드의 환경과 순서를 보장한다.

![context_stack](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/Context%20stack.png?raw=true)

(실행 컨텍스트 콜스택의 예시)

이때 실행컨텍스트 내부의 Lexical Environment에는 Environment Record 라는 것이 존재한다.

이 Environment Record (환경 레코드)는
[key: value]의 형태로 식별자와 식별자에 바인딩 된 값을 기록해두는 객체이다.

### 소스코드의 평가

소스코드의 평가 단계에서 JS 엔진은 전역 실행 컨텍스트를 생성하고 콜 스택에 넣는다.  
이후 전체 코드를 스캔하여 변수와 함수 등의 선언문만 먼저 실행한 뒤 생성된 변수나 함수 식별자를 Environment Record에 등록한다.

```javascript
var hoist;
hoist = 10;
```

위와 같은 코드가 소스코드의 평가 단계에 있을 때는 선언문만이 먼저 실행되고 할당문은 실행되지 않았기 때문에 undefined로 Environment Record에 등록되어 값이 저장된다.  
(var 키워드로 변수를 선언한 경우 선언과 초기화가 동시에 진행된다.)

![ER_undefined](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/ER_undefined.png?raw=true)

### 소스코드의 실행

소스코드의 평가 과정이 끝나면 소스코드의 실행이 시작된다. 이때 var hoist;는 이미 실행되었기 때문에 hoist = 10;이라는 할당문만이 실행되게 된다. 할당하려는 변수가 선언된 변수라면 할당의 결과를 Environment Record에 기록한다.

![ER_defined](https://github.com/ChangSuLee00/CS-study/blob/main/pictures/ER_defined.png?raw=true)

### 호이스팅의 이유

결론적으로 호이스팅이 발생하는 이유는 소스코드의 평가에 의해 선언문이 미리 실행되어 Environment Record에 undefined라는 값이 기록되고, 그 값을 할당문 이전에 불러올 수 있기 때문이다.

---

## var vs let, const

### var

var 키워드로 변수를 선언하면 선언과 동시에 초기화가 된다.

```javascript
console.log(hoist); // undefined

var hoist;
console.log(hoist); // undefined

hoist = 10;
console.log(hoist); // 10
```

(위와 같이 선언을 하면 암묵적으로 undefined 값이 바인딩된다)

### let, const

하지만 let 키워드로 변수를 선언하면 var과 다르게 선언과 동시에 초기화가 되지 않기 때문에 변수 선언문이 실행되기 전까지는 값을 읽어올 수 없다.

```javascript
console.log(hoist); // ReferenceError: hoist is not defined

let hoist; // 초기화 단계(Initialization phase)
console.log(hoist); // undefined

hoist = 10; // 할당 단계(Assignment phase)
console.log(hoist); // 10
```

(let hoist; 이전에는 바인딩된 값이 없기 때문에 값을 불러오려고 하면 ReferenceError가 발생한다.)

const 키워드는 선언과 동시에 초기화를 해야하며 그 이전에 호출할 시 let과 마찬가지로 ReferenceError가 발생한다.

이와 같이 스코프의 처음(선언 단계)부터 초기화 지점(초기화 단계)까지 변수를 참조할 수 없는 구간을 Temporal Dead Zone(TDZ) 라고 부른다.

---

## 함수에서의 호이스팅

### 함수 표현식

함수 표현식(Function Expression)이란 변수에 할당되는 값이 함수 리터럴인 문이다.

```javascript
hoist(); // TypeError

var hoist = function () {
  console.log("hoist?");
};
```

var 키워드에 함수를 담아 실행하려고 하면 var에 이미 담겨있는 undefined는 함수가 호출될 수 없기 때문에 TypeError가 발생한다.

```javascript
hoist(); // ReferenceError

let hoist = function () {
  console.log("hoist?");
};
```

let 키워드에 함수를 담아 실행하려고 하면 기록되어 있는 값이 없기 때문에 ReferenceError가 발생한다.

결론적으로 함수 표현식으로 함수를 정의하면 함수 호이스팅이 아닌 변수 호이스팅이 발생하게 된다.

### 함수 선언식

함수 선언문(Function Declaration)이란 지정된 매개변수를 갖고, 변수에 별도로 할당 되지 않는 함수를 정의하는 식이다.

```javascript
hoist(); // hoisted!

function hoist() {
  console.log("hoisted!");
}
```

함수 표현식과 다르게 함수 선언식으로 함수를 정의하면 선언과 동시에 완성된 함수 객체를 생성하여 EnvoronmentRecord에 기록한다.

이처럼 함수 선언식은 선언과 동시에 함수가 생성되기 때문에 선언문 이전에 사용할 수 있다.  
(그렇기 때문에 함수 선언문 대신 함수 표현식을 사용할 것이 권장된다.)

---

## 참고

- [10분 테코톡 : 하루의 실행 컨텍스트](https://www.youtube.com/watch?v=EWfujNzSUmw&list=RDCMUC-mOekGSesms0agFntnQang&index=8)

- 자바스크립트 Deep dive

- [호이스팅은 실제로 끌어올려지지 않는다](https://velog.io/@yukyung/%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85%EC%9D%80-%EC%8B%A4%EC%A0%9C%EB%A1%9C-%EB%81%8C%EC%96%B4%EC%98%AC%EB%A0%A4%EC%A7%80%EC%A7%80-%EC%95%8A%EB%8A%94%EB%8B%A4)

- [자바스크립트 실행 컨텍스트 - 환경 레코드](https://roseline.oopy.io/dev/javascript-back-to-the-basic/environment-record)
