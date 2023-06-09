# 02-Typescript

## 2.typescript

### 학습 키워드

* REPL
* TypeScript
  * interface vs Type
  * 타입 추론
  * Union Type vs Intersection Type
* Optional Parameter

\


현실적으로 TypeScript를 쓰는 가장 큰 이유는 vscode 자동완성 + 실시간 오류 검사

오래된 라이브러리의 경우 d.ts 파일만 따로 패키지로 제공된다.

### 타입 별칭 (type alias)

똑같은 타입을 한 번 이상 재사용하거나 다른 이름으로 부르고 싶은 경우에 사용한다.

타입을 위한 이름이다.

```js
type Point = {
  x: number,
  y: number,
};

// 앞서 사용한 예제와 동일한 코드입니다
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
```

객체 타입뿐이 아닌 모든 타입에 대해 새로운 이름을 부여할 수 있다.

```js
// ex)
type ID = number | string;
```

단지 별칭으로, 동일 타입에 대해 각기 구별되는 '여러 버전'을 만드는게 아니다.

별도로 이름 붙인 타입을 새로 작성하는 것.

```js
type UserInputSanitizedString = string;

function sanitizeInput(str: string): UserInputSanitizedString {
  return sanitize(str);
}

// 보안 처리를 마친 입력을 생성
let userInput = sanitizeInput(getInput());

// 물론 새로운 문자열을 다시 대입할 수도 있습니다
userInput = 'new input';
```

\


### interface

객체 타입을 만드는 또 다른 방법.

```js
interface Point {
  x: number;
  y: number;
}

function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
```

\


### 타입 별칭과 인터페이스의 차이점

비슷 하지만 뚜렷한 차이는,

타입은 새 프로퍼티를 추가하도록 개방될 수 없고,

**인터페이스의 경우 확장될 수 있다는 점**입니다.

```JS
interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean
}

const bear = getBear()
bear.name
bear.honey
```

type도 Bear 타입을 만들 수 있지만

프로퍼티가 추가되는 extends의 개념이 아니고 교집합을 이용한 방식이라서

확장 혹은 개방이 될 수는 없다고 표현한 것 같다.

```JS
type Animal = {
  name: string
}

type Bear = Animal & {
  honey: Boolean
}

const bear = getBear();
bear.name;
bear.honey;
```

**기존의 인터페이스에 새 필드를 추가하기**

```js
interface Window {
  title: string;
}

interface Window {
  ts: TypeScriptAPI;
}

const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {});
```

**타입은 생성된 뒤에는 달라질 수 없다**

```js
type Window = {
  title: string,
};

type Window = {
  ts: TypeScriptAPI,
};

// Error: Duplicate identifier 'Window'.
```

언제 어떤걸 써야할지 잘 모르겠다면 우선 interface 쓰고 문제가 발생할때 type 쓰라함.

\


정해진 값으로 타입을 지정할 수도 있다. 이런 타입은 Union에서 유용하게 쓰인다.

```ts
let category: 'food';

category = 'food';
```

배열보다 깐깐한 Tuple 타입이 있다.

```js
let anythings: any[];

anythings = ['hp', 256];

let pair: [string, number];

pair = ['hp', 256];
```

\


### Union Type

여러 타입 중 하나.

```js
type bool = true | false;

let flag: bool;

flag = true;

flag = false;

flag = 3; // 에러남
```

매개변수 제한할때 매우 유용

```js
type Category = 'food' | 'toy' | 'bag';

function fetchProducts({ category }: { category: Category }) {
  console.log(`Fetch ${category}`);
}
```

* [React Types](https://github.com/facebook/react/blob/main/packages/shared/ReactTypes.js)

기본값을 잡아줄 수 있다.

```js
function greeting(name: string = 'world'): string {
  return `Hello, ${name}`;
}
```

매개변수가 오브젝트일 경우

```js
function greeting({ name, age }: { name: string, age?: number }): string {
  return age ? `${name} (${age})` : name;
}
```

→ 이렇게 쓰면 ts-node에선 아쉽게도 해석 불가.

아래처럼 한 줄로 붙여서 쓰거나, type 등으로 따로 잡아주면 된다.

```js
function greeting({ name, age }: { name: string, age?: number }): string {
  return age ? `${name} (${age})` : name;
}

//

// 더 깔끔
type Human = {
  name: string,
  age?: number,
};

function greeting({ name, age }: Human): string {
  return age ? `${name} (${age})` : name;
}

greeting();

greeting({ name: '홍길동' });

greeting({ name: '홍길동', age: 13 });
```

\


### Intersection Type

* 교집합

```js
type Combined = { a: number } & { b: string };
type Conflicting = { a: number } & { a: string };

Combined 은 마치 하나의 객체 리터럴 타입으로 작성된 것 처럼 a 와 b 두 개의 속성을 가짐.

교집합과 유니언은 재귀적인 케이스에서 충돌을 일으켜서 Conflicting을 일으킴.
```

* Intersection Types

interfaces를 사용하면 다른 유형을 확장하여 새 유형을 구축할 수 있다.

```js
interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;

function draw(circle: Colorful & Circle) {
  console.log(`Color was ${circle.color}`);
  console.log(`Radius was ${circle.radius}`);
}

// okay
draw({ color: 'blue', radius: 42 });

// oops
draw({ color: 'red', raidus: 42 }); // 오타 ㅋㅋ
```

\


### Generics

재사용 가능한 컴포넌트를 구축하는데 사용하는 도구.

현재의 데이터와 미래의 데이터 모두를 다룰 수 있는 컴포넌트를 만든다.

단일 타입이 아닌 다양한 타입에서 작동하는 컴포넌트를 작성할 수 있다.

\


```js

identity 함수는 인수로 무엇이 오던 그대로 반환하는 함수.

제네릭이 없다면 함수에 특정 타입을 줘야한다.

function identity(arg: number): number {
  return arg;
}

//or
function identity(arg: any): any {
  return arg;
}
```

any는 어떤 타입이든 받을 수 있지만 반환하는 값의 타입을 잃는다.

number값을 넘겨도 any 타입이 반환된다는 의미.

우리는 무엇이 반환되는지 표시하기 위해 인수의 타입을 `캡처`할 방법이 필요합니다.

값이 아닌 타입에 적용되는 타입 변수를 사용할 것입니다.

```js
function identity<Type>(arg: Type): Type {
  return arg;
}
```

Type라는 타입 변수를 추가했다.

Type은 유저가 준 인수의 타입을 `캡처`하고 이 정보를 나중에 사용할 수 있게 한다.

여기서는 반환 타입으로 다시 사용.

이 버전의 identity 함수는 타입을 불문하고 동작하므로 `제네릭`이라 할 수 있습니다.

any를 쓰는 것과는 다르게 인수와 반환 타입에 number를 사용한 첫 번째 identity 함수만큼 정확.

(즉, 어떤 정보도 잃지 않는다)

\


[더 좋은 타입스크립트 프로그래머로 만드는 11가지 팁](https://velog.io/@lky5697/11-tips-that-help-you-become-a-better-typescript-programmer)
