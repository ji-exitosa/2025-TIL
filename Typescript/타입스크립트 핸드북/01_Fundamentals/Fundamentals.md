## 01. 기본타입

#### 문자열(string)

```ts
let str: string = 'hi';
```

---
#### 숫자(number)

```ts
let num: number = 10;
```

---
#### 진위(boolean)

```ts
let isLoggedIn: boolean = false;
```

---
#### 객체(object)

```ts
let user: object = { name: 'capt', age: 100 };
```

- 인터페이스로 선언 방법
- 타입 별칭 사용 방법

---
#### 배열(array)

- 일반적인 사용법  
```ts
let arr: number[] = [1,2,3];
```

- 제네릭(`Array<number>`) 사용법
```ts
let arr: Array<number> = [1,2,3];
```

---
#### 튜플(tuple)
- 배열의 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열 형식

- 일반적인 사용법
```ts
let arr: [string, number] = ['hi', 10];
```

- 단, 아래와 같이 정의하지 않은 타입이나 인덱스로 접근할 경우 오류 발생
```ts
arr[1].concat('!'); // Error, 'number' does not have 'concat'
arr[5] = 'hello'; // Error, Property '5' does not exist on type '[string, number]'.
```

---
#### 이넘(enum)
- 특정 값(상수)들의 집합
- 불변한 상수의 열거를 위해 `enum`은 선언한 값을 외부에서도 변경할 수 없다
- 양방향 매핑
	- const로 선언시 단방향 매핑

- 일반적인 사용법 (index로 접근 가능)
```ts
enum Weather { 
  sun, 
  cloud, 
  rain 
}
let today_weather1: number = Weather.sun; // 0 출력, type:number
let today_weather2: number = Weather['sun']; // 0 출력, type:number
let today_weather3: string = Weather[0]; // 'sun' 출력, type:string
let today_weather4: string = Weather['0']; // 'sun' 출력, type:string
```

- index 지정 가능
```ts
enum Weather { 
  sun = 10, 
  cloud, // 자동으로 11 set 됨
  rain = 5 
}

let today_weather1: number = Weather.sun; // 10 출력, type:number
let today_weather2: number = Weather['sun']; // 10 출력, type:number
let today_weather3: string = Weather[1]; // undefined 출력, type:undefined
let today_weather4: string = Weather['1']; // undefined 출력, type:undefined
let today_weather5: string = Weather[5]; // 'rain' 출력, type:string
let today_weather6: string = Weather['5']; // 'rain' 출력, type:string
let today_weather6: number = Weather.cloud; // 11 출력, type:number
```

- index 지정 (휴먼에러)
```ts
enum Weather { 
  sun = 1, 
  cloud, // 자동으로 2 set 됨. 중복으로 인해 index로 접근시에는 접근 불가...
  rain = 2 
}

let today_weather1: number = Weather['sun']; // 1 출력, type:number
let today_weather2: number = Weather['cloud']; // 2 출력, type:number
let today_weather3: number = Weather['rain']; // 2 출력, type:number
let today_weather4: string = Weather[1]; // sun 출력, type:string
let today_weather5: string = Weather[2]; // rain 출력, type:string
```

- 단방향 매핑 case1) const enum
```ts
const enum Weather { 
  sun = 1, 
  cloud, // 자동으로 2 set 됨
  rain = 2 
}

let today_weather1: number = Weather['sun']; // 1 출력, type:number
let today_weather2: number = Weather['cloud']; // 2 출력, type:number
let today_weather3: number = Weather['rain']; // 2 출력, type:number
let today_weather4: string = Weather[1]; // 오류발생
let today_weather5: string = Weather[2]; // 오류발생
```

- 단방향 매핑 case1) 문자열 지정
```ts
enum Weather { // 문자로 지정할 경우 모든 항목에 다 지정해줘야 함.
  sun = 's', 
  cloud = 'c', 
  rain = 'r' 
}

let today_weather1: number = Weather['sun']; // 's' 출력, type:string
let today_weather4: string = Weather['s']; // Error, '"s"' 형식의 식을 'typeof Weather' 인덱스 형식에 사용할 수 없으므로 요소에 암시적으로 'any' 형식이 있습니다. 'typeof Weather' 형식에 's' 속성이 없습니다. 
```


---
#### any
- 타입 모를 때 (사용 지양)

- 일반적인 사용법
```ts
let str: any = 'hi';
let num: any = 10;
let arr: any = ['a', 2, true];
```

---
#### void
- 반환 값이 없는 함수의 반환 타입

- 일반적인 사용법
```ts
function printSomething(): void {
  console.log('sth');
}

function returnNothing(): void {
  return;
}
```

---
#### never 
- 🧐 어떻게 사용하는지 아직 이해 못함 🧐
- 절대 발생하지 않는 값
- 함수가 반복문이나 에러 핸들링으로 인해 함수의 끝에 절대 도달하지 않는 경우 사용

- 일반적인 사용법
```ts
function printSomething(): void {
  console.log('sth');
}

function returnNothing(): void {
  return;
}
```



## 02. 함수

```ts
function sum(a: number, b: number): number {
  return a + b;
}
```

#### 인자/매개변수(parameter) 타입

- 일반적인 사용법  
	- `(a: number, b:number)`

- 일부 인자 선택적 기입
	- `(a: number, b?:number)`
		- `sum(10)` 형태로 넘길 때, 호출부에서 에러 발생 X
		- 하지만, `10+undefined` 이므로 결과값 `NaN` 
	- `(a: number, b = '100')`
		- `sum(10, undefined)` // 110
		- `sum(10)` // 110

- 인자로 배열 받기
	- ES6+ Rest 문법 적용
		```ts
		function sum(a: number, ...nums: number[]): number {
		  const totalOfNums = 0;
		  for (let key in nums) {
			totalOfNums += nums[key];
		  }
		  return a + totalOfNums;
		}
		```

	- 예전 문법 적용
		```ts
		function sum(a: number): number {
		  let totalOfNums = 0;
		
		  for (let i = 1; i < arguments.length; i++) {
			totalOfNums += arguments[i] as number; 
			// arguments는 any 타입이므로 number로 캐스팅 필요
		  }
		
		  return a - totalOfNums;
		}
		```

	- 호출 방식 동일
		```ts
		console.log(sum(10, 20, 30, 40)); // -80
		```

---
#### 반환(return) 타입
- `function 함수명(매개변수) : number` 형태로 작성
- 아무것도 반환하지 않을 경우 `void` 사용
#### 구조 타입

---
#### this
- 🧐 아직 이해 못함 🧐
[javascript에서의 this 이해하기](https://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/)
[typescript에서의 this 이해하기](https://joshua1988.github.io/ts/guide/functions.html#this)

---




## 03. 인터페이스

- 특징
	- 타입을 정의할 때 사용
		- 객체의 스펙(속성과 속성의 타입)
		- 함수의 파라미터
		- 함수의 스펙(파라미터, 반환 타입 등)
		- 배열과 객체를 접근하는 방식
		- 클래스
	- 객체 선언과 관련된 타입 체킹, 좀 더 엄밀한 속성 검사를 진행
	- 인터페이스 내 변수에 값을 '할당' 할 때는 처음에 선언해둔 변수만 할당 가능
	- 인터페이스로 형 체크를 하는 함수 내에 객체를 넣어서 보낼 경우,
	  선언해둔 변수 외 값이 포함되어도 상관 없음. (어차피 interface에 정의된 값만 갖다 씀)

- 일반적인 사용법
	- Interface 적용 전
	```ts
	let person = { name: 'Capt', age: 28 };
	
	function logAge(obj: { age: number }) {
	  console.log(obj.age); // 28
	}
	logAge(person); // 28
	```
	-  Interface 적용 후
	```ts
	interface personAge {
	  name: string;
	  age: number;
	}
	
	function logAge(obj: personAge) {
	  console.log(obj.age);
	}
	//personAge에 해당하는 값만 다 들어있으면 더 많아도 됨
	let person = { name: 'Capt', age: 28, gender:'F'}; 
	// let person = { age: 28, gender:'F'}; // 오류발생
	logAge(person);
	```

- 인터페이스를 인자로 받아 사용할 때, 
	- `인터페이스의 속성 갯수`와 `인자로 받는 객체의 속성 갯수` 일치하지 않아도 됨
	- 정의된 `속성`, `타입`의 조건만 만족한다면, 인자로 넘기는 객체 속성이 더 많아도 됨
	- 인터페이스에 선언된 속성 순서 지키지 않아도 됨

- 옵션 속성
	- 일반적인 사용법
	```ts
	interface 인터페이스_이름 {
	  속성?: 타입;
	}
	```

	```ts
	interface personAge {
	  name?: string;
	  age: number;
	}
	
	function logAge(obj: personAge) {
	  console.log(obj.age);
	}
	
	let person1 = { age: 28, gender:'F'};
	logAge(person);
	```


- 읽기 전용 속성
	- 객체를 처음 생성할 때만 값을 할당하고 그 이후에는 변경할 수 없는 속성
	- 일반적인 사용법
	```ts
	interface CraftBeer {
	  readonly brand: string;
	}
	
	let myBeer: CraftBeer = {
	  brand: 'Belgian Monk'
	};
	
	myBeer.brand = 'Korean Carpenter'; // error!
	```

- 읽기 전용 배열
	- `ReadonlyArray<T>` 타입 사용시, 읽기 전용 배열을 생성
	- 일반적인 사용법
	```ts
	let arr: ReadonlyArray<number> = [1,2,3];
	arr.splice(0,1); // error
	arr.push(4); // error
	arr[0] = 100; // error
	```

- 타입추론 관련
	 ![[Pasted image 20250118211122.png|300]]
	 ![[Pasted image 20250118211156.png|300]]
	-  일반적인 사용법
		```ts
	interface personAge {
	  name?: string;
	  age: number
	  [propName: string]: any; // 이 속성 추가시, 인터페이스에 정의하지 않은 속성 추가 가능
	}
	
	function logAge(obj: personAge) {
	  console.log(obj.age);
	}
	
	let person = { agee: 28}; // age 오탈자 발생, 
	// error: Object literal may only specify known properties, but 'agee' does not exist in type 'personAge'. Did you mean to write 'age'?
	logAge(person as personAge); // 타입 추론을 무시? 
	```

- 함수 타입
	- 함수의 타입을 정의할 때에도 사용
	- 일반적인 사용법
	```ts
	interface login {
	  (username: string, password: string): boolean;
	}
	
	let loginUser: login;
	loginUser = function(id: string, pw: string) {
	  console.log('로그인 했습니다');
	  return true;
	}
	```

- 클래스 타입
	- C#이나 자바처럼 `클래스가 일정 조건을 만족`하도록 타입 규칙을 정할 수 있음
	- 일반적인 사용법
	```ts
	interface CraftBeer {
	  beerName: string;
	  nameBeer(beer: string): void;
	}
	
	class myBeer implements CraftBeer {
	  beerName: string = 'Baby Guinness';
	  nameBeer(b: string) {
	    this.beerName = b;
	  }
	  constructor() {}
	}
	```

- 인터페이스 확장
	- 클래스와 마찬가지로, 인터페이스도 인터페이스 간 확장 가능
	- 일반적인 사용법
	```ts
	interface Person {
	  name: string;
	}
	interface Drinker {
	  drink: string;
	}
	interface Developer1 extends Person {
	  skill: string;
	}
	interface Developer2 extends Person, Drinker {
	  skill: string;
	}
	let fe1 = {} as Developer1;
	fe1.name = 'josh';
	fe1.skill = 'TypeScript';
	
	let fe2 = {} as Developer2;
	fe2.name = 'josh';
	fe2.skill = 'TypeScript';
	fe2.drink = 'Beer';
	```

- 하이브리드 타입
	- ex) 함수 타입이면서 객체 타입을 정의할 수 있는 인터페이스
	- 일반적인 사용법
	````ts
	interface CraftBeer {
	  (beer: string): string; // 콜러블 시그니처(함수처럼 호출할 수 있는 객체) 
	  // 즉, CraftBeer인터페이스를 함수의 형태로 잡는다면, `beer`라는 문자열을 인수로 받아 문자열을 반환하는 함수로 동작해야 함. 해당 타입으로 선언된 객체가 함수처럼 호출될 때의 매개변수와 반환값을 정의한 것
	  brand: string; // 속성
	  brew(): void; // method, 반환값이 없는(`void`) 메서드
	}
	
	// myBeer라는 함수의 반환타입으로 CraftBeer를 지정한 것
	// 즉, myBeer 함수는 `CraftBeer` 타입의 객체를 반환한다
	function myBeer(): CraftBeer {
	  // (beer: string) 시그니처를 가진 익명 함수
	  // 함수와 객체를 결합하여 나중에 동적으로 속성과 메서드를 추가하기 위해서 '강제캐스팅'
	  let my = (function(beer: string) {}) as CraftBeer; 
	  
	  my.brand = 'Beer Kitchen';
	  my.brew = function() {};
	  return my;
	}
	
	let brewedBeer = myBeer();
	brewedBeer('My First Beer'); 
	// 함수처럼 호출될 수 있는 객체이므로 호출 시 콜러블 시그니처가 실행 됨
	brewedBeer.brand = 'Pangyo Craft';
	brewedBeer.brew();
	````


## 04. 이넘

- [기본타입] - [이넘]에서 한번 다뤘었음

- 정의
	- 특정 값들의 집합을 의미하는 자료형

- 지원 타입
	- 문자형 이넘
	- 숫자형 이넘

- 일반적인 사용법
```ts
나이키
아디다스
뉴발란스
```

#### 숫자형 이넘
- 작용방식
	- 초기 값을 주지 않으면, 0부터 차례로 1씩 증가
	- 직접 할당시, 초기값부터 차례로 1씩 증가
- 일반적인 사용법
```ts
enum Direction {
  Up, // 0
  Down, // 1
  Left= 5, // 5
  Right // 6
}
```

```ts
enum Response {
  No = 0,
  Yes = 1,
}

function respond(recipient: string, message: Response): void {
  // ...
}

respond("Captain Pangyo", Response.Yes);
```

#### 문자형 이넘
- 작용방식
	- 초기값을 전부 다 지정해줘야함
	- auto-incrementing 없음

- 일반적인 사용법
```ts
enum Direction {
    Up = "u",
    Down = "d",
    Left = "l",
    Right = "r",
}
```

#### 복합 이넘(Heterogeneous Enums)
- 작용방식
	- 이넘에 문자와 숫자를 혼합하여 생성
	- 권고하지 않음
	- 가급적 같은 타입으로 이루어진 이넘을 사용하기!

#### 런타임 시점에서의 이넘 특징
- 런타임 시점에서는 객체로 변환
- 실제 객체 형태로 존재
```ts
enum Variables { // object로 인식되기 때문에
  X, Y, Z
}

function getX(obj: { X: number }) { // input을 object로 받는 함수에 
  return obj.X;
}

getX(Variables); // 인자값으로 넣을 수 있음
// 출력 결과는 0
```

- enum 코드 컴파일 결과
```ts
// `Variables` 이넘을 컴파일 한 결과
var Variables;
(function (Variables) {
    Variables[Variables["X"] = 0] = "X";
    Variables[Variables["Y"] = 1] = "Y";
    Variables[Variables["Z"] = 2] = "Z";
})(Variables || (Variables = {}));
```

#### 컴파일 시점에서의 이넘 특징
- `keyof`를 사용할 때 주의
- `keyof`를 사용해야 되는 상황에서는 대신 `keyof typeof` 사용하기
	- `typeof`
		- 런타임에 사용되는 `자바스크립트 객체의 타입 정보`를 `TypeScript 타입 시스템으로` 가져오는 역할
		- 일반적인 사용법
		```ts
		const LogLevel = { ERROR: 0, WARN: 1, INFO: 2, DEBUG: 3, };
		
		type LogLevelType = typeof LogLevel;
		
		// LogLevelType은 아래와 같은 타입으로 확장됩니다:
		{ 
			ERROR: number; 
			WARN: number; 
			INFO: number; 
			DEBUG: number; 
		}
		```

	- `keyof`
		- 객체 타입에서 키 이름만 추출하여 문자열 리터럴 유니온 타입으로 반환
		- 일반적인 사용법
		```ts
		type LogLevelStrings = keyof typeof LogLevel;
		
		// keyof typeof LogLevel은 아래와 같이 확장됩니다:
		"ERROR" | "WARN" | "INFO" | "DEBUG";
		```

	- `keyof typeof`
		- 특정 객체의 키 집합을 타입으로 추출**하는 방식
```ts
enum LogLevel {
  ERROR, WARN, INFO, DEBUG
}

// 'ERROR' | 'WARN' | 'INFO' | 'DEBUG';
type LogLevelStrings = keyof typeof LogLevel;

function printImportant(key: LogLevelStrings, message: string) {
    const num = LogLevel[key];
    if (num <= LogLevel.WARN) {
       console.log('Log level key is: ', key); // 'ERROR'
       console.log('Log level value is: ', num); // '0'
       console.log('Log level message is: ', message); //'This is a message'
    }
}
printImportant('ERROR', 'This is a message');
```

#### 리버스 매핑(Reverse Mapping)
- 숫자형 이넘에만 존재하는 특징
- 키(key)로 값(value)를 얻을 수 있고 값(value)로 키(key)를 얻을 수 있음
```ts
enum Enum {
  A
}
let a = Enum.A; // 키로 값을 획득 하기
let keyName = Enum[a]; // 값으로 키를 획득 하기
``` 


## 05. 연산자를 이용한 타입 정의

#### Union Type
- 자바스크립트의 `OR` 연산자(`||`)와 같이 `A이거나 B이다`라는 의미의 타입

- 일반적인 사용법
```ts
function logText(text: string | number) {
  // 파라미터 `text`에는 문자열 타입이나 숫자 타입이 모두 올 수 있음
  // ...
}
```

- 장점
	- `any` 사용시, 자바스크립트로 작성하는 것처럼 동작
	- 유니온 타입 사용시, 타입스크립트의 이점을 살릴 수 있음
```ts
// any를 사용하는 경우
function getAge(age: any) {
  age.toFixed(); // 에러 발생
  // age의 타입이 any로 추론되기 때문에, 숫자 관련된 API를 작성할 때 코드가 자동 완성되지 않는다.
  return age;
}

// 유니온 타입을 사용하는 경우
function getAge(age: number | string) {
  if (typeof age === 'number') {
    age.toFixed(); // 정상 동작
    // age의 타입이 `number`로 추론되기 때문에 숫자 관련된 API를 쉽게 자동완성 할 수 있다.
    return age;
  }
  if (typeof age === 'string') {
    return age;
  }
  return new TypeError('age must be number or string');
}
```

#### 인터섹션 타입(Intersection Type)
- 여러 타입을 모두 만족하는 하나의 타입

- 일반적인 사용법
	- `Person` 인터페이스의 타입 정의와 `Developer` 인터페이스의 타입 정의를 `&` 연산자를 이용
```ts
interface Person {
  name: string;
  age: number;
}
interface Developer {
  name: string;
  skill: number;
}
type Capt = Person & Developer;

// Capt는 아래와 같은 타입으로 확장됩니다:
{
  name: string;
  age: number;
  skill: string;
}
```

- 주의할 점
	- 유니온 타입 : AND 조건 (공통되는 부분) 
	- 인터섹션 : OR 조건 (두 영역 모두)
	```ts
	interface Person {
	  name: string;
	  age: number;
	}
	interface Developer {
	  name: string;
	  skill: string;
	}
	// 함수 호출 시점에 `Person` 타입이 올지 `Developer` 타입이 올지 모르니까, 
	// 어느 타입이 들어오든 간에 오류가 안 나는 방향으로 타입을 추론
	function introduce(someone: Person | Developer) {
	  someone.name; // O 정상 동작
	  someone.age; // X 타입 오류
	  someone.skill; // X 타입 오류
	}
	```


## 06. 클래스

#### readonly
- `constructor()` 함수에 초기 값 설정 로직을 넣어줘야 함
- 한번 설정된 이후로 변경 불가능

- 일반적인 사용법
```ts
class Developer {
    readonly name: string;
    constructor(theName: string) {
        this.name = theName;
    }
}

// Developer라는 클래스의 생성자를 호출해서 name을 'john'으로 초기화하고, 
// 새로운 인스턴스를 생성한 뒤에 변수 john에 저장한다.
let john = new Developer("John");
john.name = "John"; // error! name is readonly.
```

```ts
class Developer {
  readonly name: string;
  constructor(readonly name: string) {
  }
}
```

#### Accessor
- 클래스로 생성한 객체일 경우, 특정 속성의 접근과 할당에 대해 제어 가능

- 일반적인 사용법
```ts
class Developer {
  private name: string;
  
  get name(): string {
    return this.name;
  }

  // `get`만 선언하고 `set`을 선언하지 않는 경우에는 자동으로 `readonly`로 인식
  // 단, `get`만 선언할 경우, 
  // 직접 초기값 설정 원하면 constructor 정의 필요
  // 혹은 클래스 내 직접 초기값 설정 (set, constructor 불필요)
  set name(newValue: string) {
    if (newValue && newValue.length > 5) {
      throw new Error('이름이 너무 깁니다');
    }
    this.name = newValue;
  }
}
const josh = new Developer();
josh.name = 'Josh Bolton'; // Error
josh.name = 'Josh';
```


#### 추상 클래스(Abstract Class)
- 인터페이스와 비슷한 역할을 하면서도 조금 다름
- 추상 클래스는 공통적인 기능과 설계를 정의하기 위한 "틀"로 사용됨
- 추상 클래스는 특정 클래스의 상속 대상이 되는 클래스
	- 좀 더 상위 레벨에서 속성, 메서드의 모양을 정의
	- 변수에 직접 생성할 수는 없음

- 일반적인 사용법
```ts
abstract class Developer {
  abstract coding(): void; // 'abstract'가 붙으면 상속 받은 클래스에서 무조건 구현해야 함
  drink(): void {
    console.log('drink sth');
  }
}

class FrontEndDeveloper extends Developer {
  coding(): void {
    // Developer 클래스를 상속 받은 클래스에서 무조건 정의해야 하는 메서드
    console.log('develop web');
  }
  design(): void {
    console.log('design web');
  }
}
const dev = new Developer(); // error: cannot create an instance of an abstract class
const josh = new FrontEndDeveloper();
josh.coding(); // develop web
josh.drink(); // drink sth
josh.design(); // design web
```


## 07. 제네릭
- 한가지 타입보다 여러 가지 타입에서 동작하는 컴포넌트를 생성하는데 사용
#### 정의 및 예시
- 정의
	- 타입을 마치 함수의 파라미터처럼 사용하는 것

- 주의
	- 이넘(enum)과 네임스페이스(namespace)는 제네릭으로 생성할 수 없음

- 제네릭 미사용
```ts
function getText(text) {
  return text;
}

getText('hi'); // 'hi'
getText(10); // 10
getText(true); // true
```

- 제네릭 사용
	- 제네릭 기본 문법이 적용된 형태
	- 함수를 호출할 때 아래와 같이 함수 안에서 사용할 타입을 넘겨줄 수 있음

	- 인자와 반환값 타입이 동일한 경우
	```ts
	function getText<T>(text: T): T {
	  return text;
	}
	const a = getText<string>('hi'); // a,b 둘 다 동일하게 동작
	const b = getText('hi'); // 가독성이 좋기 때문에 흔하게 사용
	// 복잡한 코드에서 두 번째 코드로 타입 추정이 되지 않는다면 첫 번째 방법을 사용
	const c = getText<number>(10);
	const d = getText<boolean>(true);
	```

	- 인자와 반환값 타입이 다른 경우
	```ts
	function transform<Input, Output>(input: Input, transformFn: (value: Input) => Output): Output {
	  return transformFn(input);
	}
	
	// 사용 예
	const result = transform<number, string>(42, (num) => `Number: ${num}`);
	console.log(result); // "Number: 42"
	```

#### 제네릭 타입 변수
- 컴파일러에서 인자에 타입을 넣어달라는 경고 발생
	```ts
	// 아래와 같은 코드에서, text로 number가 들어와도 호출부에서는 오류 발생 안함.
	// 하지만 함수 선언부 중 text.length에서 에러가 발생.
	function logText<T>(text: T): T {
	  // text에 .length가 있다는 단서가 없어서, 에러 발생
	  console.log(text.length); // Error: T doesn't have .length
	  return text;
	}
	```

- 배열 형태로 받을 경우, length 사용 가능
	```ts
	function logText<T>(text: T[]): T[] {
	  console.log(text.length); // 제네릭 타입이 배열이기 때문에 `length` 허용
	  return text;
	}
	```

	```ts
	function logText<T>(text: Array<T>): Array<T> {
	  console.log(text.length);
	  return text;
	}
	```

#### 제네릭 타입 (제네릭 인터페이스)
- 🧐 아직 이해 못함 🧐
- 일반적인 사용법
	```ts
	function logText<T>(text: T): T {
	  return text;
	}
	// #1
	let str: <T>(text: T) => T = logText;
	// #2
	let str: {<T>(text: T): T} = logText;
	```

	```ts
	interface GenericLogTextFn {
	  <T>(text: T): T;
	}
	function logText<T>(text: T): T {
	  return text;
	}
	let myString: GenericLogTextFn = logText; // Okay
	```

- 인터페이스에 인자 타입을 강조하고 싶다면,
	```ts
	interface GenericLogTextFn<T> {
	  (text: T): T;
	}
	function logText<T>(text: T): T {
	  return text;
	}
	let myString: GenericLogTextFn<string> = logText;
	```

#### 제네릭 클래스
- 일반적인 사용법
		- 클래스 이름 오른쪽에 `<T>`를 붙여줌
		- 해당 클래스로 인스턴스를 생성할 때 타입에 어떤 값이 들어갈 지 지정
	```ts
	class GenericMath<T> {
	  pi: T;
	  sum: (x: T, y: T) => T;
	}
	
	let math = new GenericMath<number>();
	```

#### 제네릭 제약 조건
- 제네릭 함수에 어느 정도 타입 힌트를 줄 수 있는 방법

- 일반적인 사용법
```ts
interface LengthWise {
  length: number;
}

// `length`에 대해 동작하는 인자만 넘겨받을 수 있다?
function logText<T extends LengthWise>(text: T): T {
  console.log(text.length);
  return text;
}

logText(10); // Error, 숫자 타입에는 `length`가 존재하지 않으므로 오류 발생
logText({ length: 0, value: 'hi' }); // `text.length` 코드는 객체의 속성 접근과 같이 동작하므로 오류 없음
```

#### 객체의 속성을 제약하는 방법
- 두 객체를 비교할 때도 제네릭 제약 조건을 사용 가능
```ts
function getProperty<T, O extends keyof T>(obj: T, key: O) {
  return obj[key];  
}
let obj = { a: 1, b: 2, c: 3 };

getProperty(obj, "a"); // okay
getProperty(obj, "z"); // error: "z"는 "a", "b", "c" 속성에 해당하지 않습니다.
```


## 08. 타입 추론(Type Inference)
- 타입스크립트가 코드를 해석해 나가는 동작

#### 타입 추론의 기본 (Best Common Type)
- 몇 개의 표현식(코드)을 바탕으로 타입 추론
- 가장 근접한 타입: [Best Common Type](https://www.typescriptlang.org/docs/handbook/type-inference.html)

#### 문맥상의 타이핑 (Contextual Typing)
- 코드의 위치(문맥)를 기준으로 문맥상의 타이핑(타입 결정) 실행
- 일반적인 사용법
```ts
window.onmousedown = function(mouseEvent) {
  console.log(mouseEvent.button);   //<- OK, onmousedown에 button 있을거라고 추론
  console.log(mouseEvent.kangaroo); //<- Error!
};
```

```ts
window.onscroll = function(uiEvent) {
  console.log(uiEvent.button); //<- Error!, onscroll에 button 없을거라고 추론
}
```

```ts
const handler = function(uiEvent) {
  console.log(uiEvent.button); //<- OK, handler에는 버튼이 있는지 없는지 타입 추정 불가
}
```
- 단, 위 케이스에서 `--noImplicitAny` 옵션을 사용시, 에러 발생

#### 타입체킹
- 지향점
	-  타입 체크는 값의 형태에 기반하여 이루어져야 함
	- Duck Typing : 
		- `객체의 변수` 및 `메서드의 집합`이 객체의 타입을 결정
		- `동적 타이핑`의 한 종류 
	- Structural Subtyping : 
		- 객체의 `실제 구조`나 `정의`에 따라 타입을 결정

## 09. 타입 호환(Type Compatibility)
- 의미
	- 특정 타입이 다른 타입에 잘 맞는가
	- 명시적타입 지정보다 **코드의 구조 관점에서 타입 지정**

- 일반적인 사용법
```ts
interface Ironman {
  name: string;
}

class Avengers {
  name: string;
}

let i: Ironman;
i = new Avengers(); // OK, because of structural typing
// C#이나 Java였다면 위 코드에서 에러 발생
// `Avengers` 클래스가 명시적으로 `Ironman` 인터페이스를 상속받아 구현하지 않았기 때문
```

#### 구조적 타이핑(structural typing)
- 코드 구조 관점에서 타입이 서로 호환되는지의 여부를 판단
- 일반적인 사용법
```ts
interface Avengers {
  name: string;
}

// case 1
let hero: Avengers;
// 타입스크립트가 추론한 타입은 { name: string; location: string; }
let capt = { name: "Captain", location: "Pangyo" };
hero = capt;

// case 2
function assemble(a: Avengers) {
  console.log("어벤져스 모여라", a.name);
}
// 위에서 정의한 capt 변수, 타입은 { name: string; location: string; }
assemble(capt);
```

#### Soundness란?
- 들리지 않는다(it is said to not be sound)
- 컴파일 시점에 타입을 추론할 수 없는 특정 타입은 일단 안전하다고 본다.

#### Enum 타입 호환 주의사항
- 이넘 타입
	- `number` 타입 호환 됨
	- 이넘 타입끼리는 호환되지 않음

```ts
enum Status { Ready, Waiting };
enum Color { Red, Blue, Green };

let status = Status.Ready;
status = Color.Green;  // Error
```


#### Class 타입 호환 주의 사항
- 🧐 아직 이해 못함 ... 테스트 하니까 결과가 다르게 나온다 🧐
- 클래스 타입끼리 비교할 때 
	- 스태틱 멤버(static member)와 생성자(constructor)를 제외하고 비교
	- 속성만 비교
```ts
class Hulk {
	static teamName: string = "Avengers";
  handSize: number;
  constructor(name: string, numHand: number) { } // 제외하고 비교
}

class Captain {
  handSize: number;
  constructor(numHand: number) { } // 제외하고 비교
}

let x: Hulk = new Hulk('xx',2);
let y: Captain = new Captain(1);

a = s;  // OK
s = a;  // OK
```

#### Generics
- 타입 인자 `<T>`가 속성에 할당 되었는지를 기준
	- 속성(member 변수)이 없기 때문에, `x`와 `y`는 같은 타입으로 간주된다
	```ts
	interface Empty<T> {}
	let x: Empty<number> = {};
	let y: Empty<string> = {};
	
	x = y;  // OK, because y matches structure of x
	```

	- 제네릭의 타입 인자가 속성에 할당될 경우에는, 서로 다른 타입으로 간주됨
	- 인터페이스 `NotEmpty`에 넘긴 제네릭 타입`<T>`이 `data` 속성에 할당되었음
		- `x`와 `y`는 서로 다른 타입으로 간주
	```ts
	interface NotEmpty<T> {
	  data: T;
	}
	let x: NotEmpty<number>;
	let y: NotEmpty<string>;
	
	x = y;  // Error, because x and y are not compatible
	```


## 10. 타입 별칭 (Type Aliases)
- 특정 타입이나 인터페이스를 참조할 수 있는 타입 변수
```ts
// string 타입을 사용할 때
const name: string = 'capt';

// 타입 별칭 사용
type MyName = string;
const name: MyName = 'capt';

// `interface` 레벨의 복잡한 타입에도 별칭 부여 가능
type Developer = {
  name: string;
  skill: string;
}

// 타입 별칭에 제네릭 사용
type User<T> = {
  name: T
}

```

- 특징
	- 새로운 타입 값을 하나 생성하는 것 🙅🏻‍♀️
	- 정의한 타입에 대해 나중에 쉽게 참고할 수 있게 이름을 부여 🙆🏻‍♀️ 

- type vs interface 차이점
	- 확장 가능 / 불가능 여부
	- `type` 보다는 `interface`로 선언해서 사용하는 것을 추천

![[Pasted image 20250119004906.png|200]]*인터페이스로 선언한 타입을 프리뷰로 확인한 결과*
![[Pasted image 20250119004906.png|200]]*타입 별칭으로 선언한 타입을 프리뷰로 확인한 결과*



## 11. 타입 단언(Type Assertion)
- 해당 타입에 대해 확신이 있을 때 사용하는 타입 지정 방식
- 타입 캐스팅과 비슷한 개념
- 타입스크립트 컴파일 할 때 특별히 타입을 체크하지 않고, 데이터의 구조도 신경쓰지 않음
#### as
- `as` 키워드를 이용해서 정의

- 일반적인 사용법
	- 타입 표기 방식을 이용해 `name` 이라는 변수의 타입은 `string` 이라고 정의
	```ts
	const name: string = 'Capt';
	```
	- 타입 단언 적용
	```ts
	const name = 'Capt' as string;
	```
	-> `name` 변수의 정보를 확인해 보면 동일하게 `string`으로 추론됨

- 사용하면 좋은 케이스
	- 타입스크립트 컴파일러보다 개발자가 더 해당 타입을 잘 알고 있을 때
	```ts
	// app.js
	const capt = {};
	capt.name = '캡틴';
	capt.age = 100;
	```

	- 타입 표기 방식으로 타입 정의하려고 하면 에러 발생
		- `capt` 변수가 정의되는 시점에서 `name`, `age` 등의 속성이 정의되지 않음
		```ts
		interface Hero {
		  name: string;
		  age: number;
		}
		
		const capt: Hero = {}; // X. 오류 발생
		capt.name = '캡틴';
		capt.age = 100;
		```
		- 해결방법 1
		```ts
		interface Hero {
		  name: string;
		  age: number;
		}
		
		const capt: Hero = {
		  name: '캡틴',
		  age: 100
		};
		```
		- 해결방법 2
		```ts
		interface Hero {
		  name: string;
		  age: number;
		}
		
		const capt = {} as Hero; // 오류 없음
		capt.name = '캡틴';
		capt.age = 100;
		```


## 12. 타입 가드(TypeGuard)
- 여러 개의 타입 중 원하는 타입으로 타입을 걸러냄
- 여러 개의 타입 중 하나의 타입으로 타입을 좁힘

- 일반적인 사용법
```ts
type Age = 'string' | 'number' // 문자열 또는 숫자가 될 수 있다

function getAge1(age: Age) { // age는 getAge1() 함수 안에서 문자열 타입이거나 숫자 타입
  age.length; // age 파라미터의 문자열 길이를 구하려고 하면 에러가 발생
}

function getAge2(age: Age) {
  // 여러 개의 타입 중 내가 원하는 타입으로 좁히거나 걸러냄 (타입가드)
  if (typeof age === 'string') {
    age.length; // 정상 동작
  }
}
```

#### 타입 가드 연산자
- `typeof` 
- `instanceof`

#### 커스텀 타입 가드 함수
- `is`를 이용해서 타입 가드 역할을 하는 함수 생성 가능
- 특정 시점에 원하는 타입으로 추론되도록 타입을 걸러낼 수 있음

- 일반적인 사용법
```ts
// 문자열이거나 숫자인 타입을 받아서 문자열 타입으로 좁혀주는 커스텀 타입 가드 함수
function isString(age: string | number): age is string {
  return typeof age === 'string';
}

function getAge(age: string | number) {
  // "이 함수의 return 값이 true면, age는 string이라는 의미"라고 컴퓨터한테 알려주는 방법이
  // is string
  if (isString(age)) { 
    // 이 블록에서 age의 타입은 문자열로 추론됨
    age.length;
  }
}
```
- `getAge()` 함수 안에서 `typeof` 라는 연산자 대신에 `isString()` 커스텀 타입 가드 함수 적용
	- if 문 안에서 `age`의 타입은 동일하게 문자열로 인식
- 위 코드는 굳이 커스텀 타입 가드 함수를 이용하지 않고 `typeof`로만 해도 됨
	- 커스텀 타입 가드 함수는 여러 개의 객체 타입을 하나의 타입으로 좁힐 때 위력을 발휘함

