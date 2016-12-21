### How to install Typescript
>TypeScript를 시작하기 전에 Node.js와 Npm을 설치해야 한다. 다음 [링크](https://docs.npmjs.com/getting-started/installing-node)를 통해 Node.js를 설치할 수 있다.

TypeScript를 설치하는 가장 쉬운 방법은 npm을 통해 설치하는 것이다. 다음 명령어를 통해 설치한다.
```shell
npm install -g typescript
```

설치가 완료되면, 아래와 같은 명령어를 입력 후 제대로 설치됐는지 확인한다.
```shell
tsc -v
```
----------------------
### Compiling to Javascript
TypeScript는 .ts 파일(또는 JSX를 위해 .tsx 확장자를 쓰기도 한다.)로 작성된다. 브라우저에서는 이 ts 파일을 직접 사용하지 못하기 때문에 아래의 방법을 활용해 Javascript로 변환(Transpile or Compile? 확인해야함)해야 한다.
- tsc 사용(typescript를 설치할 때 같이 설치된다.)
- Visual Studio 혹은 플러그인을 지원해주는 IDE 사용
- gulp 혹은 grunt 같은 task runner 사용

다음 커맨드는 ts 파일을 js 파일로 변환하는 명령어다. 이름이 같은 Javascript 파일이 있으면 덮어 쓴다.
```shell
tsc main.ts
tsc main.ts worker.ts //여러 파일 동시에 변환
tsc *.ts
```
tsconfig.json 파일로 여러 컴파일 옵션을 줄 수 있다. 관련 내용은 [공식 문서](http://www.typescriptlang.org/docs/handbook/tsconfig-json.html)에서 확인 가능하다.

----------------------
### Static Typing
TypeScript의 가장 큰 특징은 정적 타이핑을 지원한다는 점이다. 즉, 변수 타입을 선언할 수 있으며 기존의 Java나 C처럼 컴파일러가 변수 형이 제대로 선언되어 있는지 확인 가능하다. 만약 타입 선언이 생략되면 코드에서 자동으로 유추한다.
다음 예제처럼 모든 변수나 함수 인자에 타입을 선언할 수 있다.

```typescript
var burger: string = 'hamburger',     // String 
    calories: number = 300,           // Numeric
    tasty: boolean = true;            // Boolean

// Alternatively, you can omit the type declaration:
// var burger = 'hamburger';

// The function expects a string and an integer.
// It doesn't return anything so the type of the function itself is void.

function speak(food: string, energy: number): void {
  console.log("Our " + food + " has " + energy + " calories.");
}

speak(burger, calories);
```

위 코드를 Javascript로 컴파일하면 아래와 같은 코드가 생성된다.
```javascript
// JavaScript code from the above TS example.

var burger = 'hamburger',
    calories = 300, 
    tasty = true; 

function speak(food, energy) {
    console.log("Our " + food + " has " + energy + " calories.");
}

speak(burger, calories);
```

만약 typescript에서 다음 코드처럼 오류가 있을 경우, tsc로 컴파일 시 error 메세지를 출력한다.
```typescript
// The given type is boolean, the provided value is a string.
var tasty: boolean = "I haven't tried it yet";
```

```shell
main.ts(1,5): error TS2322: Type 'string' is not assignable to type 'boolean'.
```

```typescript
function speak(food: string, energy: number): void{
  console.log("Our " + food + " has " + energy + " calories.");
}

// Arguments don't match the function parameters.
speak("tripple cheesburger", "a ton of");
```

```shell
main.ts(5,30): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```

Typescript가 지원하는 type은 다음과 같다.
- boolean: true or false, 0이나 1을 사용하면 컴파일 에러
- number: int와 float를 구분하지 않는다.
- string: single quotes ‘ ‘ 와 double quotes “”를 구분하지 않는다.
- array: 배열
- tuple: array완 달리 다른 타입을 한 tuple에 선언할 수 있다.
```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
```

- enum : C와 같은 enum 타입
- any: 말그대로 anything. 변수 타입이 변할 수 있다
```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a Boolean
```
- void : 함수에서 아무것도 리턴하지 않을 때 사용된다.
- null and undefined: typescript에서 undefined와 null 각기 다른 타입이다.
```typescript
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```
