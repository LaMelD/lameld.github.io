---
title: "1. 웹 개발 기초 지식"
date: 2026-09-17
weight: 1
tags: [nodejs, nestjs, typescript, decorator, event-loop]
description: "Node.js의 단일 스레드 논블로킹 I/O와 이벤트 루프 6단계, 타입스크립트의 타입 시스템, 데커레이터의 종류와 합성 규칙을 정리한다."
---

## 1.1. `Node.js`

### 1.1.1. 단일 스레드에서 구동되는 논블로킹 I/O 이벤트 기반 비동기 방식

- 멀티스레딩
    - 여러 작업 요청이 한꺼번에 들어올 때, 각 작업을 처리하기 위한 스레드를 만들고 할당하는 방식
    - 속도가 빠르다.
    - 공유 자원의 관리가 힘들다.
    - 스레드가 늘어남에 따라 메모리를 소모하여 메모리 관리가 중요하다.
- `Node.js`
    - 어플리케이션 단에서는 단일 스레드에서 작업을 처리
    - 백그라운드에서는 스레드 풀을 구성해 작업을 처리
    - 웹 서버를 운용할 때는 CPU 코어를 순산해서 관리하므로 실제 작업은 여러 개의 코어에서 별개로 처리
    - 들어온 작업을 앞의 작업이 끝날 때까지 기다리지 않고(non-blocking) 비동기로 처리
    - 입력은 하나의 스레드에서 받지만 순서대로 처리하지 않고 먼저 처리된 결과를 이벤트로 반환해주는 방식 사용

## 1.2. 이벤트 루프

- 이벤트 루프는 시스템 커널에서 가능한 작업이 있다면 그 작업을 커널에 이관한다.
- `Javascript`가 단일 스레드 임에도 불구하고 `Node.js`가 논블로킹 I/O 작업을 수행할 수 있도록 해주는 핵심 기능이다.
- 이벤트 루프의 6 단계
    1. 타이머 단계
    2. 대기 콜백 단계
    3. 유휴, 준비 단계
    4. 폴 단계 - 수신: 연결, 데이터 등
    5. 체크 단계
    6. 종료 콜백 단계

### 1.2.1. 타이머 단계

- 타이머 단계의 큐에는 `setTimeout`이나 `setInterval`과 같은 함수를 통해 만들어진 타이머들을 큐에 넣고 실행한다.
- `now - registeredTime ≥ delta`인 타이머들이 큐에 들어간다.
- `delta`는 `setTimeout(() => {}, delta)`와 같이 타이머가 등록된 시각에 얼만큼 시간이 흐른 후 동작해야 하는지를 나타내는 값이다.

### 1.2.2. 대기 콜백 단계(pending callback phase)

- 대기 단계의 큐에 들어 있는 콜백들은 현재 돌고 있는 루프 이전의 작업에서 큐에 들어온 콜백이다.
- TCP 핸들러 내에서 비동기의 쓰기 작업을 한다면, TCP 통신과 쓰기 작업이 끝난 후 해당 작업의 콜백이 큐에 들어간다. 또 에러 핸들러 콜백도 큐로 들어오게 된다.
- 타이머 단계를 거쳐 대기 콜백 단계에 들어오면, 이전 작업들의 콜백이 `pending_queue`에서 대기 중인지 검사한다. 만약 실행 대기 중이라면 시스템 실행 한도에 도달할 때까지 꺼내어 실행한다.

### 1.2.3. 유휴, 준비 단계(idle, prepare phase)

- 유휴 단계는 틱마다 실행된다.
- 준비 단계는 매 폴링 직전에 실행된다.
- `Node.js`의 내부 동작을 위한 것이라고만 알고 있자.

### 1.2.4. 폴 단계(poll phase)

- 폴 단계에서는 새로운 I/O 이벤트를 가져와서 관련 콜백을 수행한다.
- 소켓 연결과 같은 새로운 커넥션을 맺거나 파일 읽기와 같이 데이터 처리를 받아들이게 된다.
- `watch_queue`를 갖고 있다.
- `watch_queue`가 비어 있지 않다면 큐가 비거나 시스템 실행 한도에 다다를 때까지 동기적으로 모든 콜백을 실행한다.
- 큐가 비게 되면 `Node.js`는 곧바로 다음 단계를 이동하지 않고 `check_queue`, `pending_queue`, `closing_callback_queue`에 남은 작업이 있는지 검사한 다음 작업이 있다면 다음 단계로 이동한다.
- 큐가 모두 비어서 해야할 작업이 없다면 잠시 대기를 하게 된다.
    - 타이머 최소 힙의 첫 번째 타이머를 꺼내어 지금 실행할 수 있는 상태라면 그 시간만큼 대기한 후 다음 단계로 이동한다.
    - 바로 타이머 단계로 넘어간다고 해도 어차피 첫 번째 타이머를 수행할 시간이 되지 않았기 때문에 이벤트 루프를 한 번 더 돌아야 하기 때문이다.

### 1.2.5. 체크 단계(check phase)

- `setImmediate`의 콜백만을 위한 단계
- 큐가 비거나 시스템 실행 한도에 도달할 때까지 콜백을 수행

### 1.2.6. 종료 콜백 단계(close callback phase)

- `socket.on('close', () => {})`과 같은 close나 destroy 이벤트 타입의 콜백이 처리된다.
- 이벤트 루프는 종료 콜백 단계를 마치고 나면 다음 루프에서 처리해야 하는 작업이 남아 있는지 검사한다.
- 작업이 남아 있다면 타이머 단계부터 한 번 더 루프를 돌게 되고, 아니라면 루프를 종료

- `nextTickQueue`는 `process.nextTick()` API의 콜백들을 가지고 있으며, `microTaskQueue`는 `resolve`된 `Promise`의 콜백을 가지고 있다.
- 이 두 개의 큐는 기술적으로 이벤트 루프의 일부가 아니다.
- 이 두 큐에 들어 있는 콜백은 단계를 넘어가는 과정에서 먼저 실행된다.
- `nextTickQueue`가 `microTaskQueue`보다 높은 우선순위를 가지고 있다.

## 1.3. 타입스크립트(Typescript)

마이크로소프트에서 개발한 언어로 자바스크립트 코드에 타입 시스템을 도입하여 런타임에 에러가 발생할 가능성이 있는 코드를 정적 프로그램 분석(`static program analysis`)으로 찾아준다.

### 1.3.1. 변수 선언

```typescript
[선언 키워드] [변수명]: [타입]
const var1: number = 11
```

### 1.3.2. 타입

- 원시 타입

| 타입 | 설명 | 할당 가능한 값 |
|---|---|---|
| boolean | 참 또는 거짓을 나타내는 타입 | `true`, `false` |
| null | 값이 없음을 나타내는 타입 | `null` |
| undefined | 정의되지 않은 값을 나타내는 타입 | `undefined` |
| number | 숫자를 나타내는 타입 | `1`, `3.14`, `-7` 등 |
| bigint | 큰 정수를 나타내는 타입 | `123456789n`, `-987654321n` 등 |
| string | 문자열을 나타내는 타입 | `"hello"`, `'world'` 등 |
| symbol | 고유하고 변경 불가능한 값을 나타내는 타입 | `Symbol('description')` |

- 객체 타입
    - 속성을 가지고 있는 데이터 컬렉션이다.
    - C 언어의 구조체와 유사하다.
    ```typescript
    const dexter = {
        name: 'Dexter Han',
        age: 21,
        hobby: ['Movie', 'Billiards'],
    }
    ```
    - 내장 객체 : `Date`, `Array`, `Map`, `WeakMap`, `set`, `WeakSet`, `JSON`, …
- 함수 타입
    - 자바스크립트는 함수를 변수에 할당하거나 다른 함수의 인수로 전달할 수 있다.
    - 함수의 결과로 반환할수도 있다.
    - 이러한 특징을 일급 함수(first-class function)라고 한다.
    ```typescript
    typeof function === 'function'
    ```
- any / unknown / never
    - any : 어떤 타입의 값도 받을 수 있는 타입이지만 런타임 오류를 일으킬 가능성이 있다.
    - unknown : 어떤 타입도 할당 가능하지만 다른 변수에 할당 또는 사용할 때 타입을 강제하도록 한다.
    - never : 어떤 값도 할당할 수 없으며, 함수의 리턴 타입으로 지정하면 함수가 어떤 값도 반환하지 않겠다는 것을 뜻한다. 특정 타입의 값을 할당받지 못하도록 할 수 있다.

### 1.3.3. 타입 정의하기

- 변수에 객체를 바로 할당하지 않고 interface로 선언

```typescript
interface User {
    name: string;
    age: number;
}

const user: User = {
    name: 'Dexter',
    age: 21,
}
```

```typescript
class User {
    constructor(name: string, age: number) { }
}

const user: User = new User('Dexter', 21);

// MyUser 타입은 기존 User 타입을 그대로 사용하지만
// 본인이 사용하는 서비스에 맞는 이름으로 바꾼 것
type MyUser = User;
```

### 1.3.4. 타입 구성하기

- 덕 타이핑(`duck typing`) : 변수에 어떠한 타입의 값도 할당할 수 있다.
- 유니언 타입

```typescript
// parameter를 다양하게 받을 수 있음
function getLength(obj: string | string[]) {
    return obj.length;
}

// 변수가 가질 수 있는 값을 제한할 수 있음
type Status = 'Ready' | 'Waiting';

enum Status {
    READY = 'Ready',
    WAITING = 'Waiting',
}
```

- 제네릭 타입(`generic type`)

```typescript
function identity(arg: any): any {
    return arg;
}

function identity<T>(arg: T): T {
    return arg;
}
```

## 1.4. 데커레이터(Decorator)

```typescript
function deco(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log('데커레이터가 평가됨');
}

class TestClass {
    @deco
    test() {
        console.log('함수 호출됨');
    }
}

const t = new TestClass();
t.test();

// 데커레이터가 평가됨
// 함수 호출됨
```

```typescript
function deco(value: string) {
    console.log('데커레이터가 평가됨');
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log(value);
    }
}

class TestClass {
    @deco('HELLO')
    test() {
        console.log('함수 호출됨');
    }
}

const t = new TestClass();
t.test();

// 데커레이터가 평가됨
// HELLO
// 함수 호출됨
```

### 1.4.1. 데커레이터 합성

```typescript
@f
@g
test
// f(g(x)) 와 비슷한 형태로 결과를 낸다.
```

- 합성의 단계
    - 각 데커레이터의 표현은 위에서 아래로 평가(`evaluate`)
    - 결과는 아래에서 위로 함수로 호출(`call`)

```typescript
function first() {
    console.log("first(): factory evaluated");
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("first(): called");
    }
}

function second() {
    console.log("second(): factory evaluated");
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("second(): called");
    }
}

class ExampleClass {
    @first()
    @second()
    method() {
        console.log('method is called');
    }
}

const example = new ExampleClass();
example.method();

// first(): factory evaluate
// second(): factory evaluate
// second(): called
// first(): called
// method is called
```

### 1.4.2. 클래스 데커레이터

- 클래스의 생성자에 적용되어 클래스 정의를 읽거나 수정할 수 있다.
- 선언 파일과 선언 클래스(`declare class`) 내에서는 사용할 수 없다.

```typescript
// 클래스 데커레이터 팩터리
// 생성자 타입에 의해 new 키워드와 함께 어떠한 형식의 인수들도 받아들일 수 있는 타입
// 상속받는 제네릭 타입 T를 가지는 생성자를 팩터리 메서드의 인수로 전달하고 있다.
function reportableClassDecorator<T extends {new (...args: any[]): {} }>(constructor: T) {
    // 생성자를 리턴하는 함수여야한다.
    return class extends constructor {
        // reportingURL이라는 속성을 추가한다.
        reportingURL = "http://www.example.com";
    };
}

@reportableClassDecorator
class BugReport {
    type = 'report';
    title: string;
    constructor(t: string) {
        this.title = t;
    }
}

const bug = new BugReport("Needs dark mode");
console.log(bug);

// {type:'report', title:'Needs dark mode', reportingURL:'http://www.example.com'}
```

### 1.4.3. 메서드 데커레이터

- 선언 파일, 오버로드 메서드, 선언 클래스에 사용할 수 없다.
- 세 개의 인수를 가진다.
    - 정적 멤버가 속한 클래스의 생성자 함수이거나 인스턴스 멤버에 대한 클래스의 프로토타입
    - 멤버의 이름
    - 멤버의 속성 설명자, `PropertyDescriptor` 타입을 가짐
- 메서드 데커레이터가 값을 반환한다면 이는 해당 메서드의 속성 설명자가 된다.

```typescript
function HandleError() {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log(target);
        console.log(propertyKey);
        console.log(descriptor);

        const method = descriptor.value;

        descriptor.value = function() {
            try {
                method();
            } catch (e) {
                console.log(e);
            }
        }
    };
}

class Greeter {
    @HandleError()
    hello() {
        throw new Error('테스트 에러');
    }
}

const t = new Greeter();
t.hello();

interface PropertyDescriptor {
    configurable?: boolean; // 속성의 정의를 수정할 수 있는지 여부
    enumerable?: boolean;   // 열거형인지 여부
    value?: any;            // 속성 값
    writable?: boolean;     // 수정 가능 여부
    get?(): any;            // getter
    set?(v: any): void;     // setter
}
```

### 1.4.4. 접근자 데커레이터

```typescript
function Enumerable(enumerable: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.enumerable = enumerable
    };
}

class Person {
    constructor(private name: string) {}

    @Enumerable(true)
    get getName() {
        return this.name;
    }

    // set은 열거할 수 없다.
    @Enumerable(false)
    set setName(name: string) {
        this.name = name;
    }
}

const person = new Person('Dexter');
for (let key in person) {
    console.log(`${key}: ${person[key]}`);
}

// name: Dexter
// getName: Dexter
```

### 1.4.5. 속성 데커레이터

- 아래 두 개의 인수를 가지는 함수
    - 정적 멤버가 속한 클래스의 생성자 함수이거나 인스턴스 멤버에 대한 클래스의 프로토타입
    - 멤버의 이름

```typescript
function format(formatString: string) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        let value = target[propertyKey];
        function getter() {
            return `${formatString} ${value}`;
        }

        function setter(newVal: string) {
            value = newVal;
        }

        return {
            get: getter,
            set: setter,
            enumerable: true,
            configurable: true,
        };
    };
}

class Greeter {
    @format('Hello')
    greeting: string;
}

const t = new Greeter();
t.greeting = 'World';
console.log(t.greeting);
```

### 1.4.6. 매개변수 데커레이터

- 3가지 인수와 함께 호출된다.
    - 정적 멤버가 속한 클래스의 생성자 함수이거나 인스턴스 멤버에 대한 클래스의 프로토타입
    - 멤버의 이름
    - 매개변수가 함수에서 몇 번째 위치에 선언되었는지를 나타내는 인덱스

```typescript
import { BadRequestException } from '@nestjs/common'

function MinLength(min: number) {
    return function (target: any, propertyKey: string, parameterIndex: number) {
        target.validators = {
            minLength: function (args: string[]) {
                return args[parameterIndex].length >= min;
            }
        };
    };
}

function Validate(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const method = descriptor.value;

    descriptor.value = function(...args) {
        Object.keys(target.validators).forEach(key => {
            if (!target.validators[key](args)) {
                throw new BadRequestException();
            }
        });
        method.apply(this, args);
    };
}

class User {
    private name: string;

    @Validate
    setName(@MinLength(3) name: string) {
        this.name = name;
    }
}

const t = new User();
t.setName('Dexter');
console.log('-----------------');
t.setName('De'); // 3 보다 길이가 작기 때문에 예외 발생
```

### 1.4.7. 데커레이터 요약

| 데커레이터 | 역할 | 호출 시 전달되는 인수 | 선언 불가능한 위치 |
|---|---|---|---|
| 클래스 데커레이터 | 클래스의 정의를 읽거나 수정 | `constructor` | d.ts 파일, declare 클래스 |
| 메서드 데커레이터 | 메서드의 정의를 읽거나 수정 | `target`, `propertyKey`, `propertyDescriptor` | d.ts 파일, declare 클래스, 오버로드 메서드 |
| 접근자 데커레이터 | 접근자의 정의를 읽거나 수정 | `target`, `propertyKey`, `propertyDescriptor` | d.ts 파일, declare 클래스 |
| 속성 데커레이터 | 속성의 정의를 읽음 | `target`, `propertyKey` | d.ts 파일, declare 클래스 |
| 매개변수 데커레이터 | 매개변수의 정의를 읽음 | `target`, `propertyKey`, `parameterIndex` | d.ts 파일, declare 클래스 |
