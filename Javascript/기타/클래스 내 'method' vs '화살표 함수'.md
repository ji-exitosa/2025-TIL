
### 1. 클래스 메서드와 프로토타입

- 클래스 메서드(`classMethod()`)는 `prototype`에 정의됨
- 화살표 함수는 인스턴스에 직접 연결
	- 모든 클래스 인스턴스가 *해당 메서드를 공유*한다는 의미
	- 메모리 효율적
	- 모든 객체가 *동일한 메서드 참조*

#### 예시
```ts
class Handler {
  handle() {
    console.log("This is a class method.");
  }
}

const obj1 = new Handler();
const obj2 = new Handler();

console.log(obj1.handle === obj2.handle); // true (공유 메서드)

```

- `handle` 메서드는 `Handler.prototype`에 정의되어 있음
- `obj1.handle`과 `obj2.handle`은 동일한 메모리 위치 참조
- 메모리 절약

- method 여러 객체에서 재사용

---

### 2. 화살표 함수로 정의된 method

- 화살표 함수는 클래스의 *인스턴스 자체에 할당*
- 클래스의 각 인스턴스가 *독립적인 화살표 함수 버전을 갖게 됨*
- 프로토타입에 정의되지 않고 *인스턴스의 속성으로 동작*

```ts
class Handler {
  handle = () => {
    console.log("This is an arrow function.");
  };
}

const obj1 = new Handler();
const obj2 = new Handler();

console.log(obj1.handle === obj2.handle); // false (독립적인 함수)

```

- `handle`은 각 객체의 **인스턴스**에 생성됨
- `obj1.handle`과 `obj2.handle`은 서로 다른 메모리 위치 참조
- 각 인스턴스마다 새로운 함수가 생성 -> 메모리 사용 ⬆️

- 화살표 함수는 선언 시점의 컨텍스트 캡처
    - 화살표 함수는 자신만의 `this`를 가지지 않고, **정의된 위치의 `this` 상속**
    - 클래스 내에서 화살표 함수는 **인스턴스의 컨텍스트(현재 객체)** 캡처

- 화살표 함수는 인스턴스에 직접 연결
    - 화살표 함수는 **인스턴스의 속성**으로 추가됨
	    - ↔️ 클래스 메서드(`classMethod()`)는 `prototype`에 정의됨
    - 따라서 해당 화살표 함수는 인스턴스와 항상 동일한 `this`를 참조

- 왜 화살표 함수의 `this`는 변하지 않을까?
	- 화살표 함수는 `this`를 자신이 선언된 컨텍스트에서 고정
		- 화살표 함수가 클래스에서 정의되면, `this`는 클래스의 인스턴스를 참조
		- 화살표 함수를 다른 곳으로 전달해도, `this`는 여전히 선언 당시의 인스턴스를 가리킴

---

### 차이점 요약

| 특징           | 클래스 메서드                    | 화살표 함수                       |
| ------------ | -------------------------- | ---------------------------- |
| **생성 위치**    | `prototype` (공유)           | 인스턴스 자체                      |
| **메모리 사용**   | 메서드가 모든 객체에서 공유됨           | 각 객체마다 새로운 함수 생성             |
| **this 바인딩** | 호출 시점에 바인딩 필요              | 선언 시점에 `this`가 고정됨           |
| **사용 목적**    | 메모리 효율적, 대부분의 일반 메서드       | `this` 고정이 필요한 이벤트 핸들러       |
| **예시**       | `Handler.prototype.method` | `instance.method = () => {}` |

- class 내 화살표 함수 vs 일반 메서드
```ts

class Handler {
  name = "Handler Instance";

  // 화살표 함수
  arrowFunction = () => {
    console.log(this.name); // this는 Handler 인스턴스를 참조
  };

  // 일반 메서드
  classMethod() {
    console.log(this.name); // 호출 방식에 따라 this가 달라질 수 있음
  }
}

const handler = new Handler();

// 화살표 함수 호출
handler.arrowFunction(); // "Handler Instance"

// 일반 메서드 호출
handler.classMethod(); // "Handler Instance"

// 일반 메서드를 변수에 할당하고 호출
const method = handler.classMethod;
method(); // 에러 또는 undefined (this가 바인딩되지 않음)

// 화살표 함수를 변수에 할당하고 호출
const arrow = handler.arrowFunction;
arrow(); // "Handler Instance" (this가 고정됨)

```

- 같은 클래스, 다른 this
```ts
class Printer {
  message: string;

  constructor(message: string) {
    this.message = message;
  }

  // 클래스 메서드
  print() {
    console.log(`Message: ${this.message}`);
  }
}

const printer1 = new Printer("Hello from Printer 1");
const printer2 = new Printer("Hello from Printer 2");

// 직접 호출 - `this`는 각각의 인스턴스
printer1.print(); // Message: Hello from Printer 1
printer2.print(); // Message: Hello from Printer 2

// 메서드를 다른 변수에 할당 - `this`가 바뀜
const print1 = printer1.print;
const print2 = printer2.print;

// 호출 시점의 컨텍스트에 따라 `this`가 달라짐
print1(); // Message: undefined (전역 객체의 this 또는 undefined in strict mode)
print2(); // Message: undefined

// 바인딩을 통해 명시적으로 this 설정
const boundPrint1 = printer1.print.bind(printer1);
const boundPrint2 = printer2.print.bind(printer2);

boundPrint1(); // Message: Hello from Printer 1
boundPrint2(); // Message: Hello from Printer 2

```

---

### 어떤 걸 선택해야 하는지?

1. **클래스 메서드**:
    - 대부분의 경우 메서드가 모든 객체에서 공유되는 것이 적합
    - 일반적인 메서드 정의에는 클래스 메서드 사용

2. **화살표 함수**:
    - 이벤트 핸들러나 `this`가 다른 컨텍스트로 바인딩되지 않도록 보장해야 하는 경우 사용
    - ex, React 컴포넌트에서 이벤트 핸들러를 정의할 때.

---

### 정리된 예시

```ts
class Handler {
  // 클래스 메서드
  classMethod() {
    console.log(this);
  }

  // 화살표 함수
  arrowFunction = () => {
    console.log(this);
  };
}

const handler = new Handler();

const classMethod = handler.classMethod;
const arrowFunction = handler.arrowFunction;

// 호출
classMethod(); // undefined or 에러 (this가 바인딩되지 않음)
arrowFunction(); // Handler 객체 (this가 고정됨)
```

 - `this` 바인딩 동작과 메모리 효율성 측면에서 중요한 의미를 가짐