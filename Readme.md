# TypeScript
### 왜 사용하는가?

⇒ **타입 안정성**이 있는 MS에서 만든 프론트 엔드 언어

작성한 문서가 javascript로 변환됨 → 타입스크립트가 에러를 탐지하면 javascript로 컴파일되지 않음

```tsx
let a="hello"
a=1
```

오류 발생

Typescript 변수형 명시방식

```tsx
let b : boolean  = false
let c : number[] = []
```

⇒ 타입 추론이 작동하지 않으므로, 명시적 표현은 최소화 하는 것이 좋음

### 변수형

1. number
2. string
3. boolean
4. 각 변수형의 array

### Object에 대한 Type 선언

```tsx
const player {
	name : string;
	age: number;
} = {
	name:"nico";
	age : 23;
}
```

만약 age가 없는 경우도 있다면?

```tsx
const player {
	name : string;
	age**?**: number;
} = {
	name:"nico";
	age : 23;
}
```

⇒ age 형태가 number|undefined 로 선언된다.

 

### Type Alias

```tsx
type Player={
	name:string,
	age?:number
}
const playerNico : Player ={
	name :"nico"
}
const player2 : Player ={
	name :"nicto"
}
```

### 함수 반환형 정의

```tsx
type Player={
	name:string,
	age?:number
}
function playerMaker(name:string) : Player{
	return {
		name
	}
}	
```

name:string을 통해서 함수 인자를 받고, Player라는 선언형 정의를 통해서 함수 값을 반환한다.

```tsx
type Player={
	name:string,
	age?:number
}
const playerMaker=(name:string) : Player =>({name})
```# TypeScript
