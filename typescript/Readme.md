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

name:string을 통해서 sting형태로 함수 인자를 받고, Player라는 선언형 정의를 통해서 함수 값을 반환한다.

```tsx
type Player={
	name:string,
	age?:number
}
const playerMaker=(name:string) : Player =>({name})
```

### read only 옵션

⇒ 읽기전용으로 만들기

```tsx
type Plaer = {
	readonly name:String
	age?:number
}
```

⇒ 읽기 전용이라서 변경할 수 없게 됨

```tsx
const numbers : readonly number[] = [1,2,3,4]
number.push(1)
```

⇒ readonly이기 때문에 오류가 나게됨

### Tuple

⇒ 배열을 생성하게 하는데, 최소한의 길이와 특정 위치에 특정 타입이 존재해야함

```tsx
const player : [string, number, boolean] = ["donggil", 23, true]
```

⇒ string, number, boolean형태의 배열을 생성하게 함. 이는 반드시 3가지의 값을 가져야함. 항상 정해진 갯수의 요소를 가져야 하는 배열을 만들 수 있음

API가 이런식으로 배열을 줄 수 있기 때문에 사용하게 될 것임

배열에 접근하여 그 값을 다른 형태로 바꾸려고 해도 오류가 발생하게됨

앞에 readonly를 붙일 수도 있음

```tsx
const player : readonly [string, number, boolean] = ["donggil", 23, true]
```

type에는 undefined와 null, any가 올 수 있음 

any →  모든 값이 올 수 있음, 즉 typescript에서 벗어나기 위해 사용함 

입력하지 않으면 형식을 유추해서 제한이 걸리게 됨

### TypeScript만의 자료형

1. unknown : 변수의 자료형을 모르는 경우
    
    먼저 체크가 되어야 사용가능함
    
    이렇게 사용해야 오류가 나지 않음
    
    ```tsx
    let a:unknown ;
    if (typeof a==="number"){
    	let b = a+1;
    }
    if (typeof a==="String"){
    	let b=a.toUpperCase();
    }
    ```
    
2. void : return 하지 않는 경우
    
    ```tsx
    function hello(){
    	console.log('x');
    }
    ```
    
    ⇒ void형태, 즉 return하지 않는 경우
    
3. never : 함수가 절대 return되지 않을때 사용하고, 오류를 return 하는 경우에 사용함
    
    ```tsx
    function Hello() :never{
    	throw new Error('xxx')
    }
    ```
    
    쓰는 예시
    
    ```tsx
    function hello(name:string|number){
    	if(typeof name==="String"){
    	name
    	} else if(type name ==="number"){
    		name
    	}
    	else{
    		name
    	}
    ```
    
    마지막 else → never를 return함 ⇒ 함수가 제대로 실행되지 않는경우, 즉 예외를 throw하거나 프로그램 실행 종료를 의미함
    
    ### Call signiture
    
    ⇒ 함수 위에 마우스를 올렸을 때 보게 되는 것
    
    함수의 타입과 함수의 인자, 반환 타입을 알려줌
    
    위와 아래는 같은 형태, 결과를 갖는다.
    
    ```tsx
    function add(a:number, b:number){
    	return a+b;
    }
    const add(a:number, b:number) => a+b;
    ```
    
    ```tsx
    type Add = (a:number, b:number) => number;
    const add:Add = (a,b) => a+b;
    ```
    
    ### Overloading
    
    ⇒ 함수가 여러개의 call signiture를 가진 경우 발생함
    
    ```tsx
    type Add = {
    	(a:number, b:number) : number
    	(a:number, b:string) : number
    }
    const add : Add=(a,b) =>{
    	if(typeof b==="string") return a
    	return a+b
    }
    ```
    
    이런 식으로 사용할 때 발생하기때문에 함수에서 예외 처리를 해주어야함
    
    파라미터가 옵션일때
    
    ```tsx
    type Add = {
    	(a:number, b:number) : number
    	(a : number , b:number, c:number) :number,
    }
    const add:Add = (a,b,c?:number) => {
    	if(c) return a+b+c
    	return a+b
    }
    add(1,2)
    add(1,2,3)
    ```