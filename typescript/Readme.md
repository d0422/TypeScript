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
    
    ### Generic사용하기(다형성)
    
    ```tsx
    tye SuperPrint={
    	<TypePlaceholder>{arr : Typeplaceholder[]):void
    }
    const superPrint : SuperPrint = (arr) => {
    	arr.forEach(i=>console.log(i))
    }
    superPrint([1,2,3,4])
    ```
    
    ⇒ generic을 사용하면 일일히 type을 모두 입력할 필요가 없다.
    
    위의 <TypePlaceholder>에는 대신에 다른 단어가 올 수 도 있다.
    
    ⇒ Generic은 요구한대로 call signiture를 작성해줌
    
    ⇒ any와는 다르다! any와는! → any는 타입스크립트에서 벗어나는 것
    
    ```tsx
    type Player<E>={
    	name:string
    	extraInfo:E
    }
    const nico :Player<null>={
    	name:"nico",
    	extraInfo : null
    }
    ```
    
    ### Class
    
    ```tsx
    class Player{
    	constructor{
    		private firstName :string
    		private lastName : string
    		public nickname :string -> 얘는 다르게 사용 가능
    	}{}
    }
    ```
    
    원래라면 클래스의 객체는 this.firstname형태로 선언되어야 하지만, typescript에서는 그러지 않아도 된다.
    
    ⇒ js로 컴파일 된 결과
    
    ```jsx
    class Player{
    	constructor{
    		this.firstName= firstName;
    		this.lastName=lastName;
    	}
    }
    ```
    
    ### 추상 클래스
    
    ```tsx
    class Player{
    	constructor{
    		private firstName :string
    		private lastName : string
    		public nickname :string
    	}{}
    }
    class Player extends User{
    }
    ```
    
    ⇒ extends 추상클래스 이름 형태로 선언하며, 인스턴스를 새로 만들수는 없다.
    
    ### 추상 메소드
    
    ```tsx
    class Player{
    	constructor{
    		private firstName :string
    		private lastName : string
    		public nickname :string
    	}{}
    	getFullName(){
    		return `${this.firstName} ${this.lastName}`
    }
    }
    class Player extends User{
    }
    const nico = new Player("nico","last,"니코");
    nico.getFullName();
    ```
    
    만약 getFullName앞에 private를 붙여준다면 위코드의 nico.getFullName은 작동하지 않는다.
    
    추상메소드를 만들려면, 메소드를 클래스 안에서 작성하지 않아도 됨
    
    추상클래스에서는 추상메소드를 만들 수 있으나, 직접 구현해서는 안되고, call signiture만 적어둬야 함
    
    ```tsx
    class Player{
    	constructor{
    		private firstName :string
    		private lastName : string
    		public nickname :string
    	}{}
    	abstract getNickName() : void
    	getFullName(){
    		return `${this.firstName} ${this.lastName}`
    	}
    }
    class Player extends User{
    }
    const nico = new Player("nico","last,"니코");
    nico.getFullName();
    ```
    
    property를 private로 선언하면, 상속받아도 접근되지 않는다.
    
    따라서 protected로 선언한다.
    
    ```tsx
    abstract class User{
    	constructor{
    		protected firstName :string
    		protected lastName : string
    		protected nickname :string
    	}{}
    	abstract getNickName() : void
    	getFullName(){
    		return `${this.firstName} ${this.lastName}`
    	}
    
    }
    class Player extends User{
    }
    const nico = new Player("nico","last,"니코");
    nico.getFullName();
    ```
    
    protected로 선언하면 상속받아서 메소드로 사용할 수 있다.
    
    ```tsx
    type Words={
    	[key:string]: string
    }
    
    class Dict{
    	private words: Words;
    }
    let dict:Words={
    	"potato:"food"
    }
    
    ```
    
    [키 이름 : 형태] :형태 
    
    ⇒ 객체 선언시  type 지정
    
    ### Readonly
    
    ```tsx
    class Word{
    	constructor(
    		public readonly term : string,
    		public readonly def : string,
    	){}
    }
    ```
    
    ⇒ 읽을 수는 있지만 변경할 수 없음(JS에서는 보이지 않음)
    
    ### 특수한 type 선언하기
    
    ```tsx
    type Team="red"|"blue"|"yellow"
    type Player={
    	nickname:string,
    	team:Team,
    	health:number
    }
    ```
    
    ⇒ Team은 red, blue, yellow만 가능함
    
    ### interface
    
    ⇒ 오브젝트 모양을 설정을 위해 사용함
    
    ```tsx
    interface Player{
    	nickname:string,
    	team:Team,
    	health:number
    }
    ```
    
    interface는 오직 객체의 모양을 설정해 주기 위한 것으로, type 키워드가 더 많은 것을 할 수 있음
    
    interface는 상속이 가능함
    
    ```tsx
    interface User extends Player{
    }
    const nico : User{
    	nickname:"nico"
    }
    ```
    
    type으로도 가능함
    
    ```tsx
    type User={
    	name : string
    }
    type Player=User&{
    }
    ```
    
    interface를 n번쓴다고 해도, ts가 합쳐준다.
    
    그러나 type은 한번만 사용가능하다.
    
    ### 추상클래스
    
    ```tsx
    abstract class User{
    	constructor(
    		protected firstName:string,
    		protected lastName:string,
    	){}
    	abstract sayHi(name:string):string
    	abstract frullName():string
    }
    class Player extends User{
    	fullName(){
    		return `${this.firstName} ${this.lastName}`
    }
    	sayHi(name:string){
    		return `Hello ${name}. My name is ${this.fullname()}
    	}
    ```
    
    ⇒ js에 추상클래스란 존재하지 않는다.
    
    인터페이스는 가벼움. 인터페이스는 js로 컴파일되지 않고 사라짐
    
    ```tsx
    interface User{
    	firstName:string,
    	lastName:string,
    	sayHi(name:string) : string
    	fullName():string
    }
    class Player implements User{
    	constructor(){
    		public firstName:string,
    		public lastName:string	
    	}{}
    	fullName(){
    		return `${this.firstName} ${this.lastName}`
    }
    	sayHi(name:string){
    		return `Hello ${name}. My name is ${this.fullname()}
    	}
    }
    ```
    
    ⇒ js로 번역(컴파일)되지 않고, 가볍게 선언이 가능함 똑같이 선언됨
    
    다만 interface를 상속할때는 public으로 선언해야만 함
    
    ## 복습
    
    ### Type
    
    1. type [이름] = {[내용]}
    2. 내용은 형태가 될 수 있으나 **특정 내용**일 수도 있다.
    3. type의 상속은 type type2=type1&{}의 형태로 이루어진다.
    4. type은 여러번 선언할 수 없다.
    
    ### Interface
    
    1. interface [이름]{[내용]}
    2. interface의 상속은 interface interface2 extends interface1
    3. intercace는 여러번 선언하면 합쳐진다.
    
    ### 클래스 상속
    
    ```tsx
    class User implements typeorinterface{
    	constructor(
    		public firstName:string
    		){}
    }
    ```
