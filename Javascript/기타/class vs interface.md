
## 1. `class`

### 자바스크립트(JavaScript)

- ES6(ECMAScript 2015)부터 도입
- 객체를 생성하는 템플릿, 메서드와 속성을 정의할 수 있
- 기능적 역할: 
	- 인스턴스 생성
	- 프로토타입 기반 상속

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

// "Person 클래스의 생성자(`constructor`)를 호출하여 `'John'`과 `30`이라는 값을 초기값으로 받아 새로운 인스턴스를 생성하고, 이를 변수 `john`에 저장했다."
const john = new Person("John", 30);
john.greet(); // 출력: Hello, my name is John and I am 30 years old.

```

### 타입스크립트(TypeScript)

- JavaScript와 기본적인 기능은 동일
- 추가적으로 `class`의 속성과 메서드에 타입 지정할 수 있음
- 기능적 역할: 
	- 인스턴스 생성
	- 정적(static) 속성/메서드와 상속 지원

```ts
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): void {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

const john = new Person("John", 30);
john.greet(); // 출력: Hello, my name is John and I am 30 years old.

```


---


## 2. `interface`

### 자바스크립트(JavaScript)

- 존재하지 않지만, 객체 리터럴 형태로 구조를 정의하거나 타입 확인 가능
- 자바스크립트는 런타임에 타입 검사를 하지 않기 때문에, 명시적인 인터페이스 개념이 없음

```js
const person = {
  name: "John",
  age: 30,
};

function greet(person) {
  console.log(`Hello, ${person.name}!`);
}

greet(person); // Hello, John!
```

### 타입스크립트(TypeScript)

- `interface`는 TypeScript의 특징적인 기능
- 객체나 클래스의 구조를 정의하는 데 사용
- `interface`는 런타임에는 사라지고, 컴파일 시에만 동작
- 기능적 역할: 
	- 타입 검증(일관된 구조)
	- 코드 가독성 향상
	- 유연한 설계

```ts
interface Person {
  name: string;
  age: number;
  greet(): void;
}

const john: Person = {
  name: "John",
  age: 30,
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  },
};

john.greet(); // 출력: Hello, my name is John
```

---

## 3. `class` & `interface`

- 클래스와 함께 사용 : 

```ts
// 선언부
interface Person {
  name: string;
  age: number;
  greet(): void;
}

// 구현부
class Employee implements Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): void {
    console.log(`Hi, I'm ${this.name}, and I'm ${this.age} years old.`);
  }
}

const emp = new Employee("Alice", 25);
emp.greet(); // 출력: Hi, I'm Alice, and I'm 25 years old.
```

---

## 3. `class`와 `interface`의 차이점 (TypeScript)

|특징|`class`|`interface`|
|---|---|---|
|언어 지원 여부|JavaScript와 TypeScript 모두 지원|TypeScript 전용|
|주요 역할|객체 생성 및 인스턴스 관리|타입 정의 및 구조 명세|
|런타임 동작|런타임에서도 동작|컴파일 시에만 사용, 런타임에는 사라짐|
|구조 정의|속성과 메서드를 정의, 구현 가능|구조만 정의하며 구현은 없음|
|상속|클래스 간 상속(`extends`) 지원|인터페이스 간 상속(`extends`) 지원|
|구현 가능 여부|메서드와 속성을 구현|직접 구현 불가, 클래스에서 구현 필요|

---

### 결론

- JavaScript: `class`는 있지만, `interface`는 없음
- TypeScript: `class`와 `interface`가 모두 있으며, 각각의 역할이 다름
    - `class`: 객체 생성 및 동작 정의
    - `interface`: 타입과 구조를 정의하여 컴파일 타임에 타입 검사 지원
