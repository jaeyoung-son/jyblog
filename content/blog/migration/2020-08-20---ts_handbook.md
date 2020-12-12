---
title: '나만의 타입스크립트 핸드북'
date: 2020-08-20 12:21:13
category: 'typescript'
draft: false
---

TS 핸드북을 따라가며 고수가 되기.

## 기본타입

프로그램이 유용하려면 숫자, 문자열, 구조체, 불리언 값과 같은 간단한 데이터 단위가 필요하다. 타입스크립트는 자바스크립트와
거의 동일한 데이터 타입을 지원하며 열거 타입을 사용하여 더 편하게 사용이 가능함.

#### 숫자 number

자바스크립트처럼 타입스크립트의 모든 숫자는 부동 소수값이다. 부동 소우에는
number라는 타입이 붙혀진다. 16진수, 10진수와 더불어 ECMAScript 2015에 소개된 2진수, 8진수 리터럴도 지원한다.

```ts
let demical: number = 1
let hex: number = 0xf00d
let binary: number = 0b1010
```

#### 문자열 string

웹 페이지를 자바스크립트로 만들 때 기본적으로 텍스트 데이터를 다루는 작업이 필요하다.
다른 언어들처럼 타입스크립트에서 텍스트 데이터 타입을 string으로 표현한다.
큰따옴표나 작은따옴표를 문자열 데이터를 감싸는데 사용한다.

```ts
let color: string = 'red'
```

또한 템플릿 문자열을 사용하여 여러줄에 걸쳐 문자열을 작성할 수 있으며 표현식도
포함 시킬 수 있다. 백틱으로 문자를 감싼다.

```ts
let fullName: string = `jaeyoung son`
let age: number = 26
let sentence: string = `hello my name is ${fullName}, i am ${age} years old`
```

#### 배열 array

배열은 두 가지 방법으로 쓸 수 있다. 첫 번째 방법은, 배열 요소를 나타내는 타입 뒤에 []를 쓰는것이다.

```ts
let list: number[] = [1, 2, 3]
```

다음은 제너릭 배열 타입을 쓰는것이다.

```ts
let list: Array<number> = [1, 2, 3]
```

#### 튜플 tuple

튜플 타입을 사용하면 요소의 타입과 개수가 고정된 배열을 표현할 수 있다. 단 요소들의 타입이 모두 같을
필요는 없다. 예) number,string이 쌍으로 있는 값을 나타낼때

```ts
let x: [string, number]

x = ['hi', 4] // 성공
x = [4, 'hi'] // error
```

정해진 인덱스에 위치한 요소에 접근하면 해당 타입이 나타난다.

```ts
console.log(x[0.substring(1)]) // 성공
console.log(x[1].substring(1)) // number에는 substring이 없다는 에러
x[3] = 'hoho' // 에러 [string,number] 타입에는 프로퍼티 3이 없다.
```

#### 열거 enum

자바스크립트의 표준 자료형 집합과 사용하면 도움이 될만한 데이터 형은 enum이다. enum은 값의 집합에 더 나은 이름을
붙여줄 수 있다. 라고 하는데 설명만 들어서는 와닿지 않는다. 예제를 한번 살펴보자.

```ts
enum Color {
  Red,
  Green,
<<<<<<< HEAD
  Blue,
=======
  Blue
>>>>>>> 3f1b45b0... 기본 데이터타입 추가
}
let c: Color = Color.Green;
```

중괄호로 감싸져 Color라는 타입을 정하고 약간 객체와 생김새가 비슷하다.  
기본적으로 enum은 0부터 시작하여 멤버들의 번호를 매긴다. 멤버 중 하나의 값을 수동으로 설정하여 번호를 바꿀 수 있다.
예를 들어 0대신 1부터 시작하게 번호를 바꿔줌.
<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > 5caf73d7... any 내용 추가

```ts
enum Color {
  Red = 1,
  Green,
<<<<<<< HEAD
  Blue,
=======
  Blue
>>>>>>> 5caf73d7... any 내용 추가
}
let c: Color = Color.Green;
```

모든 값을 수동으로 설정이 가능함.

```ts
enum Color {
  Red = 1,
  Green = 2,
<<<<<<< HEAD
  Blue = 4,
=======
  Blue = 4
>>>>>>> 5caf73d7... any 내용 추가
}
```

유용한 기능 중 하나는 매겨진 값을 사용해 enum 멤버의 이름을 알아낼 수 있다.

```ts
enum Color {
  Red = 1,
  Green,
<<<<<<< HEAD
  Blue,
=======
  Blue
>>>>>>> 5caf73d7... any 내용 추가
}
let colorName: string = Color[2];

console.log(colorName); // 'Green'
```

생소한 느낌도 있고, 어떠한 상황에 효율적일지 아직 잘 모르겠다 예제를 더 찾아보던가 해야겠다.

#### Any

어플리케이션 만들 때, 알지 못하는 타입을 표현해야 할 수도 있다. 이 값들은 동적인 콘텐츠에서 올 수 있다.
이를 위해 any타입을 사용할 수 있다.

```ts
let notSure: any = 4
notSure = 'string string'
notSure = false

let list: any[] = [1, true, 'anything']
```

<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > db9acdce... 데이터 타입 추가 never null undef

#### void

void는 어떤 타입도 존재할 수 없음을 나타내기에, any의 반대 타입이다. 보통 함수에서 반환값이 없을때
반환 타입을 표현하기 위해 쓴다.

```ts
function warnUser(): void {
  console.lo('warning message')
}
```

#### null , Undefined

타입스크립트는 undefined와 null 둘 다 각각 자신의 타입 이름으로 undefined, null로 사용한다.

```ts
let u: undefined = undefined
let n: null = null
```

#### never

never는 절대 발생할 수 없는 타입을 나타낸다. 예로, never는 함수 표현식이나 화살표 함수 표현식에서 항상
오류를 발생시키거나 절대 반환하지 않는 타입으로 쓰인다. 변수 또한 타입 가드에 의해 아무 타입도 얻지 못하게 좁혀지면
never타입을 얻게 될 수 있다.

```ts
function error(msg: string): never {
  throw new Error(msg)
}
```

#### 객체 object

object는 원시 타입이 아닌 타입을 나타낸다. number, string, boolean, bigint, symbol, null, 또는 undefined가 아닌 나머지를 의미한다.

```ts
declare function create(0: object | null): void;

create({ prop: 0}); //성공
create(null); // 성공

create(42) // error
create('string') // error
```

<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > c88a5088... 타입 단언 추가

#### 타입 단언 type assertions

가끔 타입스크립트보다 개발자가 값에 대해 더 잘 알고있을떄가 있다. 대개 이런경우는 어떤 엔티티의 실제 타입이 현재 타입보다
더 구체적일때 발생한다. 쉽게말해서 "날 믿어, 난 내가 뭘 하고 있는지 알아"라고 말해주는것,
타입단언에는 두가지 형태가 잇다. 하나는 angle-bracket문법

```ts
let somValue: any = 'string'

let strLength: number = (<string>somValue).length
```

다른 하나는 as 문법

```ts
let someValue: any = 'string'

let strLength: number = (someValue as string).length
```

어떤 것을 사용할지는 선호에 따른 선택이나. JSX와 함께 사용할떄는 as 스타일의 단언만 허용된다.

## 인터페이스

타입스크립트의 핵심 원칙 중 하나는 타입 검사가 값의 형태에 초점을 맞추고 있다는 것이다.
이를 '덕 타이핑(duck typing)' 혹은 구조적 서브타이핑 이라고 한다. 타입스크립트에서 이런 타입들의
이름을 짓는 역할을 하고 코드 안의 계약을 정의하는 것뿐만 아니라 프로젝트 외부에서 사용하는 코드의 계약을 정의하는 강력한 방법이다.

```ts
function printLabel(labelObj: { label: string }) {
  console.log(labelObj.label)
}

let myObj = { size: 10, label: 'size 10 obj' }
pritntLabel(myobj)
```

타입 검사는 printLabel 호출을 확인한다. printLabel 함수는 string타입 label을 갖는 객체를 하나의 매개변수로 가진다. 실제로 더 많은 프로퍼티를 가지고 있지만,
컴파일러는 최소한 필요한 프로퍼티가 있는지와 타입이 잘 맞는지만 검사한다.  
인터페이스로 다시 작성

```ts
interface LabeledValue {
  label: string
}

function printLabel(labeledObj: LabeledValue) {
  console.log(labeledObj.label)
}

let myObj = { size: 10, label: 'Size 10 Object' }
printLabel(myObj)
```

타입 검사는 프로퍼티들의 순서를 요구하지 않는다. 단지 인터페이스가 요구하는 프로퍼티들이
존재하는지와 프로퍼티들이 요구하는 타입을 가졌는지만 확인한다.
<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > f2e6c217... 프로퍼티 내용 업로드

#### 선택적 프로퍼티 optional properties

인터페이스의 모든 프로퍼티가 필요한것은 아니다. 어떤 조건에서만 존재하거나 아에 없을 수도 있다.

```ts
interface SquareConfig {
  color?: string
  width?: number
}

function createSquare(config: SquareConfig): { color: string; area: number } {
  let newSquare = { color: 'white', area: 100 }
  if (config.color) {
    newSquare.color = config.color
  }
  if (config.width) {
    newSquare.area = config.width * config.width
  }

  return newSquare
}

let mySquare = createSquare({ color: 'black' })
```

#### 읽기전용 프로퍼티 Readonly Properties

일부 프로퍼티들은 객체가 처음 생성될 때만 수정 가능해야한다. 프로퍼티 이름앞에 readonly를 넣어서 이를 지정할 수 있다

```ts
interface Point {
  readonly x: number
  readonly y: number
}
```

객체 리터럴을 할당하여 Point를 생성하면 할당 후 x,y를 수정할 수 없다.

```ts
let p1: Point = { x: 10, y: 20 }
p1.x = 5 // error
```

타입스크립트에서는 모든 변경 메서드가 제거된 Array<T>와 동일한 ReadonlyArray<T>타입을 제공한다.
생성 후에 배열을 변경하지 않음을 보장할 수 있다.
<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > 6aa8a626... 함수 타입 추가

```ts
let a: number[] = [1, 2, 3, 4]
let ro: ReadonlyArray<number> = 0
ro[0] = 12 // error
ro.push(5) // error
a = ro // readonly는 변경가능한 number[]에 할당할 수 없다
```

프로퍼티에 설정할때와 다른점은 대문자로 시작한다. 또한 제너럴타입으로 지정해준다.  
추가 프로퍼티가 있음을 확신할때, 문자열 인덱스 서명(string index signatuer)을 추가해주면 된다.
SquareConfig가 color, width프로퍼티를 갖고 있고, 다른 프로퍼티를 가질 수 있다면

```ts
interface SquareConfig {
  color?: string
  width?: number
  [propName: string]: any
}
```

#### 함수타입 Function Types

인터페이스는 자바스크립츠 객체가 가질 수 있는 넓은 범위의 형태를 기술할 수 있다. 프로퍼티로 객체를 기술하는것 외에,
인터페이스는 함수 타입을 설명할 수 있다. 인터페이스로 함수 타입을 기술하기 위해,
인터페이스에 호출 서명(call signature)을 전달한다.

```ts
interface SearchFunc {
  (source: string, subString: string): boolean
}
```

한번 정의되면, 함수 타입 인터페이스는 다른 인터페이스처럼 사용이 가능하다.

```ts
let mySearch: SearchFunc
mySearch = function(source: string, subString: string) {
  let result = source.search(substring)
  return result > -1
}
```

#### 인덱서블 타입 Indexable Types

인터페이스로 함수 타입을 설명하는 방법과 유사하게 a[10] 이나 agemap['daniel'] 처럼 타입을
인덱스로 기술하 수 있다. 인덱서블 타입은 인덱싱 할때 해당 반환 유형과 함꼐 객체를 인덱싱하는데 사용할 수 있는 타입을 기술하는
인덱스 시그니쳐를 가지고 있다.
<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > 323cbb7b... 함수 타입 추가

```ts
interface StringArray {
  [index: number]: string
}

let myArray: StringArray
myArray = ['jy', 'kiki']

let myStr: string = myArray[0]
```

위 인덱스 서명은 StringArray 가 number로 색인화(indexed)되면
string을 반환할 것을 나타낸다. 인덱스 서명을 지원하는 타입에는 문자열과 숫자 두 가지 타입이 있다.

## 함수

타입스크립트 함수는 자바스크립트와 마찬가지로 기명 함수와 익명 함수로 만들 수 있다.

#### 함수 타입

```ts
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function(x: number, y number): number {
    return x + y
}
```

각 파라미터와 함수 자신의 반환될 타입을 정해줄 수 있다. 타입스크립트는 반환 문을 보고
반환 타입을 파악할 수 있으므로 반환 타입을 생략할 수 있다.

#### 타입 작성

```ts
let myAdd: (x: number, y: number) => number = function(
  x: number,
  y: number
): number {
  return x + y
}
```

매개변수들의 타입과 반환 타입 사이에 화살표를 써서 반환 타입을 분명히 할 수 있다. 함수 푝에 필요한 부분이다.
값을 반환하지 않는다면 void를 써서 표시한다.
<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > c1595bf1... 제너릭 추가

#### 선택적 매개변수와 기본 매개변수

타입스크립트에서는 모든 매개변수가 함수에 필요하다고 가정한다. 이것이 null이나 undefined를 줄수 없다는걸
의미하는것은 아니다. 다만 함수가 호출될 때, 컴파일러는 각 매개변수에 대해 사용자가 값을 제공했는지 검사한다.

```ts
function buildName(firstName: string, lastName: string) {
  return firstName + lastName
}

let result1 = buildName('jaeyoung') // 적은 매개변수
let result2 = buildName('jaeyoung', 'son', 'yeah') // 많은 매개변수
let result3 = buildName('jaeyoung', 'son')
```

#### 나머지 매개변수

필수, 선택적 매개변수는 한번에 하나의 매개변수만 가지고 이야기한다. 때로는 다수의 매개변수를 그룹지어 작업하기를
원하거나, 함수가 최종적으로 얼마나 많은 매개변수를 취할지 모를 때도 있다. 자바스크립트에서는
모든 함수 내부에 위치한 arguments라는 변수를 사용해 직접 인자를 가지고 작업할 수 있다.

```ts
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + ' ' + restOfName.join(' ')
}

let name = buildName('재영', '손', '안녕하세요', '하하')
```

나머지 매개변수는 선택적 매개변수들의 수를 무한으로 취급한다. 나머지 매개변수로 인자들을 넘겨줄 때는
원하는 만큼 넘겨 줄 수 있다. 반대로 넘겨주지 않을 수도 있다.

## 제너릭 Generics

C#이나 자바 같은 언어에서 재사용 가능한 컴포넌트를 생성하는 도구상자의 주요 도구 중 하나는
제너릭이다. 즉, 단일 타입이 아닌 다양한 타입에서 작동하는 컴포넌트를 작성할 수 있다.

```ts
function identity(arg: any): any {
  return arg
}
```

제너릭이 없다면 어느타입이 인자로 들어올지 모른다면 any를 사용해야한다. 어떤 타입을 반환하는지
정보를 잃게된다.  
대신에 무엇이 반환되는지 표시하기 위해 인수의 타입을 캡쳐할 방법이 필요한데 여기서 값이 아닌 타입에 적용되는 타입 변수를
사용한다.
<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > 877b26ae... 제너릭 타입 변수 추가

```ts
function identity<T>(arg: T): T {
  return arg
}
```

identity함수에 T라는 타입 변수를 추가했다. T는 유저가 준 인수의 타입을 캡쳐하고 반환 타입으로 다시 사용한다.  
호출하는 방법은 두 가지가 있다. 첫 번쨰 방법은 함수에 타입 인수를 포함한 모든 인수를 전달하는 방법.

```ts
let output = identity<string>('myString') // 출력 string
```

두 번쨰 방법은 일반적인 방법으로 타입 인수 추론을 사용한다 즉 우리가 전달하는 인수에 따라서
컴파일러가 T의 값을 자동으로 정하게 한다. 인자에 들어간 값 ---> T가됨

```ts
let output = identity('myString')
```

#### 제너릭 타입 변수 작업

위의 예제에 arg의 길이를 로그에 찍는다면?

```ts
function loggingIdentity<T>(arg: T): T {
  console.log(arg.length) // error T에는 length가 없다.
  return arg
}
```

어느 곳에서도 arg가 length가 있다고 명시되어 있지 않기때문이다.
이 함수를 사용하는 사용자는 .length가 없는 number를 전달할 수도 있다.

```ts
function loggingIdentity<T>(arg: T[]): T[] {
  console.log(arg.length)
  return arg
}
```

위의 예를 보면 타입 매개변수 T와 T배열인 인수 arg를 취하고, T배열을 반환한다.
라고 읽을 수 있다. 만약 number배열을 넘기면 T가 number에 바인딩되므로 함수는 number배열을 얻게된다.
<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > cf2b4edd... 제너릭 내용 추가

#### 제너릭 타입 Generic Types

제너릭 함수의 타입은 함수 선언과 유사하게 타입 매개변수가 먼저 나열되는, 비 제너릭 함수의 타입과 비슷하다.

```ts
function identity<T>(arg: T): T {
  return arg
}

let myIdentity: <T>(arg: T) => T = identity
```

제너릭 인터페이스

```ts
interface GenericIdentityFn {
  <T>(arg: T): T
}

function identity<T>(arg: T): T {
  return arg
}

let myIdentity: GenericIdentityFn = identity
```

#### 제너릭 제약조건 Generic Constraints

이전 length에 접근하는 예제에서 arg의 프로퍼티 .length에 접근하기를 원했지만, 컴파일러는 모든
타입에서 .length프로퍼티를 가짐을 증명할 수 없으므로 경고했다.
any와 모든 타입에서 동작하는 대신, .length프로퍼티가 있는 any와 모든 타입들에서 작동하는 것으로 제한하고
싶을 수 있다. 최소한 .length가 있어야 한다면 T가 무엇이 될 수 있는지에 대한 제약 조건을
나열해야 한다. 제약사항을 extends 키워드로 표현한 인터페이스를 이용해 명시한다.

```ts
interface Lengthwise {
  length: number
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
  cosole.log(arg.length)
  return arg
}

loggingIdentity(3) // error number는 .length가 없다
loggingIdentity({ length: 10, value: 3 })
```

<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > 205d8d79... 타입 별칭 추가

#### 제너릭 제약조건에서 타입 매개변수 사용

다른 타입 매개변수로 제한된 타입 매개변수를 선언할 수 있다. 이름이 있는 객체에서 프로퍼티를 가져오고
싶은 경우를 예로 실수로 obj에 존재하지 않는 프로퍼티를 가져오지 않도록 하기 위해 두가지 타입에 제약조건을 둔다.

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key]
}

let x = { a: 1, b: 2, c: 3, d: 4 }

getProperty(x, 'a') // 1
getProperty(x, 'm') // error m은 a,b,c,d에 해당되지 않는다.
```

제너릭을 보면 K라는 타입 매개변수는 T의 keyof로 T라는 객체의 키들로 타입을 정해준것같다. 그리고 매개변수
key에 해당 K타입을 넣어준다.

## 타입 별칭 Type Aliases

타입 별칭은 타입의 새로운 이름을 만든다. 타입 별칭은 떄때로 인터페이스와 유사하지만,
원시값, 유니언, 튜플 그리고 손으로 작성해야 하는 다른 타입의 이름을 지을 수 있다.

```ts
type Name = string
type nameResolver = () => string
type NameOrResolver = Name | NameResolver
function getName(n: NameOrResolver): Name {
  if (typeof n === 'string') {
    return n
  } else {
    return n()
  }
}
```

인터페이스처럼 타입 별칭은 제너릭이 될 수 있다. 타입 매개변수를 추가하고 별칭 선언의 오른쪽에 사용한다.

```ts
type Container<T> = { value: T }
```

<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > 815ff091... 인터페이스와 타입별칭

프로퍼티 안에서 자기 자신을 참조하는 타입 별칭을 가질 수 있다.

```ts
type Tree<T> = {
  value: T
  left: Tree<T>
  right: Tree<T>
}
```

교차 타입과 같이 사용하면 놀라운 타입을 만들 수 잇다.

```ts
type LinkedList<T> = T & { next: LinkedList<T> }

interface Person {
  name: string
}

const people: LinkedList<Person>
var s = people.name
var s = people.next.name
var s = people.next.next.name
```

#### 인터페이스와 타입 별칭

타입별칭은 인터페이스와 같은 역할을 할 수 있다. 하지만 미묘한 차이가 있다.
한 가지 차이점은 인터페이스는 어디에서나 사용할 수 있는 새로운 이름을 만들 수 있다.
타입 별칭은 새로운 이름을 만들지 못한다.

```ts
type Alias = {num: number}
interface Interface {
    num: number;
}
declare function aliased(arg: Alias): Alias;
declare function interfaced(arg: Interface: Interface;
```

에디터에서 interfaced에 마우스를 올리면 Interface를 반환한다고 보여주지만,
aliased는 객체 리터럴 타입을 반환한다고 보여준다. 이게 새로운 이름을 만들지 못한다는 말인가?? 싶다.. 이전 버전에서는 차이가 꽤 있었지만 최신 버전으로 오면서 큰 차이는 없다고 한다.
<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > f74452d3... jsx as 업로드

## JSX와

기본적으로 JSX를 사용하려면 두 가지 작업을 해야한다.

1. 파일 이름을 .tsx 확장자로 지정한다.
2. jsx 옵션을 활성화 한다.

#### as 연산자

```ts
const foo = <foo>bar
```

타입 단언(type assertion)을 보면 위 코드는 볍ㄴ수 bar가 foo타입이라는 것을 단언한다.
타입스크립트는 꺾쇠 괄호를 사용해 타입을 단언하기 떄문에, JSX구문과 함께 사용할 경우 특정 문법 해석에 문제가 될 수 있다.
따라서 as 라는 대체 연산자를 통해 타입을 단언한다.

```ts
const foo = bar as foo
```

as연산자는 .ts와 .tsx파일 모두 사용할 수 있다.
<<<<<<< HEAD
<<<<<<< HEAD
=======

> > > > > > > a3ee0015... 내장요소 추가

#### 내장 요소 Intrinsic elements

내장 요소는 특수 인터페이스 JSX.IntrinsicElemens에서 조회된다.
기본적으로 이 인터페이스가 지정되지 않으면 그대로 진행되어 내장 요소 타입은 검사되지 않는다.
그러나 이 인터페이스가 있는 경우, 내장 요소의 이름은 JSX.IntrinsicElements인터페이스의 프로퍼티로 조회된다.

```tsx
declare namespace JSX {
    interface IntrinsicElements {
        foo: any
    }
}

<foo />  // 성공
<bar /> // error
```

#### 리액트와 통합하기

```tsx
interface Props {
  foo: string;
}

class MyComponent extends React.Component<Props, {}> {
  render() {
    return <span>{this.props.foo}</span>;
  }
}

<MyComponent foo='bar'> // 성공
<MyCompoenet foo={0}> // error
```

# <<<<<<< HEAD

> > > > > > > # 3f1b45b0... 기본 데이터타입 추가
> > > > > > >
> > > > > > > # 5caf73d7... any 내용 추가
> > > > > > >
> > > > > > > # db9acdce... 데이터 타입 추가 never null undef
> > > > > > >
> > > > > > > # c88a5088... 타입 단언 추가
> > > > > > >
> > > > > > > # f2e6c217... 프로퍼티 내용 업로드
> > > > > > >
> > > > > > > # 6aa8a626... 함수 타입 추가
> > > > > > >
> > > > > > > # 323cbb7b... 함수 타입 추가
> > > > > > >
> > > > > > > # c1595bf1... 제너릭 추가
> > > > > > >
> > > > > > > # 877b26ae... 제너릭 타입 변수 추가
> > > > > > >
> > > > > > > # cf2b4edd... 제너릭 내용 추가
> > > > > > >
> > > > > > > # 205d8d79... 타입 별칭 추가
> > > > > > >
> > > > > > > # 815ff091... 인터페이스와 타입별칭
> > > > > > >
> > > > > > > # f74452d3... jsx as 업로드
> > > > > > >
> > > > > > > a3ee0015... 내장요소 추가
