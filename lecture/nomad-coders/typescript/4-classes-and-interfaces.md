# #4 Classes and Interfaces

### #4.0 Classes

TS로 객체 지향 프로그래밍 하기



```typescript
class Player {
    constructor (
        private firstName: string, // JS 에는 private가 없음 TS에서만 작동
        private lastName: string,
        public nickname: string
    ) {}
}

const nic = new Player("nic", "las", "n")

nic.firstname // TS에서작동 안함 private 이기에
nic.nicname // 작동
```



Abstract - 다른 클래스가 상속받을 수 있는 클래스, 직접 새로운 인스턴스를 생성할 수는 없음\
JS에는 작동하지 않음 Only TS

```typescript
abstract class User {    // 추상 클래스
    constructor (
        private firstName: string,
        private lastName: string,
        public nickname: string
    ) {}
    abstract getNickName(): void // absc- 안에 absc 가능, class에서 반드시 구현해야함
    
    getFullName(){    // 추상 메소드
        return `${this.firstName} ${this.lastName}`
    }
}

class Player extends User {
    ...
}

const nic = new Player("nic", "las", "n")
nic.getFullName() // "nic las"
```



접근 가능 위치

<table><thead><tr><th width="145.66666666666666">구분</th><th width="207">선언한 클래스 내</th><th>상속받은 클래스 내</th><th>인스턴스</th></tr></thead><tbody><tr><td>private</td><td>O</td><td>X</td><td>X</td></tr><tr><td>protected</td><td>O</td><td>O</td><td>X</td></tr><tr><td>public</td><td>O</td><td>O</td><td>O</td></tr></tbody></table>

\=> TS에서 체크하고 JS에서는 보이지 않음



### #4.1 Recap

4.0 복습

```typescript
type Words = {
    [key: string] : string // Words 타입이 string만을 프로퍼티로 갖는 오브젝트
}

class Dic {
    private words: Words
    constructor() {
        this.words = {}
    }
    add(word: Word){ // 클래스를 타입처럼 사용할 수 있다
        if(this.words[word.term] === undefined){
            this.words[word.term] = word.def;
        }
    }
    def(tern: string){
        return this.words[term]
    }
}

class Word {
    constructor() {
        public term: string,
        public def: string
    } {}
}

const kimchi = new Word("kimchi", "한국음식")

const dic = new Dic(kimchi)
dic.add(kimchi)
dic.def("kimchi")
```

\+ mission



### #4.2 Interfaces

타입과 비슷하지만 다름

```typescript
// 타입
// 특정 값만 가지도록 제한 가능
type Team = "MU" | "BM" | "NA"
type Health = 1 | 5 | 10
type Player = {
    nickname: string,
    team: Team,
    health: Health,
}

const kim : Player = {
    nickname: Kim,
    team: BM,
    health: 11 // err : 설정된 타입이 아니기에 에러 발생
}
```



Interface - 오직 한가지 용도로 오브젝트의 모양을 특정하기 위해 사용

```typescript
// 인터페이스
type Team = "MU" | "BM" | "NA"
type Health = 1 | 5 | 10

interface Player {
    nickname: string,
    team: Team,
    health: Health,
}

const kim : Player = {
    nickname: Kim,
    team: BM,
    health: 11 // err : 설정된 타입이 아니기에 에러 발생
}
```



type과 interface는 오브젝트의 모양을 특정하기 위한 용도이므로 거의 같음

```typescript
interface User {
    name: string
}

interface Player extends User {}

const kim : Player = {
    name: "Kim"
}

/////////

type User = {
    name: string
}

type Player = User & {}

const kim : Player = {
    name: "Kim"
}
```



interface는 객체 지향 프로그래밍의 개념을 활용해 디자인되었고, 타입은 더 유연하고 개방적임

```typescript
// Interface 프로퍼티의 축적이 가능함
interface User {name: string}
interface User {lastName: string}
interface User {age: number}

const nico :User = {
    name: "nico",
    lastName: "n",
    age: 10
}
```



### #4.3 Interfaces part Two

인터페이스는 가볍다. 컴파일하면 JS로 변하지 않고 사라지기 때문이다.

```typescript
abstract class User {
    constructor(
        protected firstName: string,
        protected lastName: string
    ) {}
    
    abstract sayHi (name: string):string
    abstract fullName (): string
}

class Player extends User {
    fullName(){
        return `${this.firstName} ${this.lastName}`
    }
    sayHi(name:string){
        return `Hello ${name}. My Name is ${this.fullName()}`
    }
}


// => abstract 를 interface 로 변경
interface User {
    firstName: string,
    lastName: string, 
    sayHi (name: string):string
    fullName (): string
}

class Player implements User { // implements 는 JS 에 사용되지 않음
    constructor(
        public firstName: string,
        public lastName: string
    ){}
    
    fullName(){
        return `${this.firstName} ${this.lastName}`
    }
    sayHi(name:string){
        return `Hello ${name}. My Name is ${this.fullName()}`
    }
}
```

위 두 코드는 같은 내용을 abstract로 생성했을 때와 interface로 생성했을 때의 차이다. 이렇게 보면 큰 차이 없어보이지만, JS로 컴파일한 내용을 살펴보면 불필요한 User class가 사라져있음을 확인할 수 있다. 이만큼 더 가벼워졌다는 의미이다.



<figure><img src="../../../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>



### #4.4 Recap

1. Type과 Interface 는 오브젝트의 모양을 지정하는 동일한 기능을 수행한다.

```typescript
type PlayerA = {
    name: string
}

const playerA: PlayerA = {
    name: "Kim MJ"
}

///

interface BlayerB {
    name: string
}

const playerB: PlayerB = {
    name: "Son HM"
}
```

2. 추가적인 부분에서 차이가 생긴다.

```typescript
type PlayerA = {
    name: string
}

type PlayerAA = PlayerA & {
    lastName: string
}

const playerA: PlayerAA = {
    name: "MJ"
    lastName: "Kim"
}

///
interface PlayerB {
    name: string
}

interface PlayerBB extends PlayerB { // 이런 식으로 상속 가능하다.
    lastName: string
}

interface PlayerBB {    // 이런 식으로 확장 가능하다.
    health: 10
}

const playerB: PlayerB = {
    name: "HM"
    lastName: "Son"
    health: 10
}
```

3. 추상 class를 대체한다.

```typescript
type PlayerA = {
    firstName: string
}

class User implements PlayerA {
    constructor(
        public firstName: string
    ){}
}

interface PlayerB {
    firstName: string
}

class User implements PlayerB {
    constructor(
        public firstName: string
    ){}
}
```

\=> 타입스크립트에 오브젝트의 모양을 알려주고 싶으면 interface, 나머지 상황에서는 type을 사용해라\~



### #4.5 Polymorphism

다형성 - 다른 모양의 코드를 가질 수 있게 하는것. 제네릭(placeholder 타입)을 사용한다.

```typescript
interface SStorage<T> {
    [key: string]: T
}

class LocalStoage<T> {
    private storage : SStorage<T> = {}
    set(key:string, value:T){
        this.storage[key] = value
    }
    remove(key:string){
        delete this.storage[key]
    }
    get(key:string):T {
        return this.storage[key]
    }
    clear(){
        this.storage = {}
    }
}

const stringsStroage = new LocalStorageMstring<string>()
stringsStorage.get("ee")
stringsStorage.set("hello", "world")
stringsStorage.set("hello", 1) // err T is string

const booleansStorage = new LocalStorage<boolean>()
booleansStorage.get("xxx")
stringsStorage.set("hello", "world")
```

