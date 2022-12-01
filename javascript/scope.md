# 스코프 체인

## 스코프 (scope)
> 변수에 접근할 수 있는 범위, 유효 범위

### var과 let, const

#### var
> 함수 스코프(bunction Scope)의 변수

생성된 함수 내부에서만 사용할 수 있다. 만약 함수 내부에서 생성되지 않은 경우 전역 스코프.

#### let, const
> 블록 스코프(block scope)의 변수

함수 스코프만 스코프로 처리되는 var과 달리 블록 스코프에서는 모든 블록이 스코프가 된다.

### 스코프의 예시

자바스크립트는 기본적으로 함수 단위로 스코프가 생성된다.

코드

```jaavscript
var c = 4;
function outer() {
  var a = 1;
  console.log(a);
  var b = 3;
  inner();

  function inner() {
    var a = 2;
    console.log(a);
    console.log(b);
    console.log(c);
  }
}

outer();
```

결과

```javascript
1
2
3
4
```

1. var a = 1;의 경우 outer 함수의 스코프에 속한다. 따라서 console.log(a);가 outer에서 실행될 경우 그대로 1을 출력한다.


2. var a = 2;의 경우 inner 함수의 스코프에 속한다. 따라서 console.log(a);가 inner에서 실행될 경우 outer의 1이 아닌 inner에서 선언된 2가 출력된다.


3. var b = 3;의 경우 outer 함수의 스코프에 속한다. inner함수의 스코프 내에서 b를 발견하지 못할 경우 outer함수의 스코프에서 b를 찾는다. outer에서 발견될 b를 출력하기에 3이 출력된다.


4. var c = 4;의 경우 함수 외부의 스코프에 위치하지만, 해당하는 스코프 내에서 값을 찾지 못하면 자바스크립트는  계속해서 위의 스코프로 올라가기 때문에 함수 밖에 있는 c를 출력한다.

## 참고

- https://darrengwon.tistory.com/102

- https://eun-ng.tistory.com/16