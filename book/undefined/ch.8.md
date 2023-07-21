---
description: 📘러닝 타입스크립트 클래스 정리
---

# Ch.8 클래스



### 8.1 클래스 메서드

타입스크립트는 독립 함수 이해와 동일한 방식으로 메서드를 이해한다. 매개변수 타입에 타입이나 기본값을 지정하지 않으면 기본타입으로 any 가 지정된다. 메서드를 호출하려면 허용 가능한 수의 인수가 필요하고, 재귀 함수가 아니라면 대부분 반환 타입을 유추 가능하다.&#x20;

```typescript
class Greeter {
    greet(name: string){
        console.log(`${name}, do your stuff !`)
    }
}

new Greeter().greet("Son")
new Greeter().greet() // Expected 1 arguments, but got 0.(2554)
```

클래스 생성자 constructor는 매개변수와 관련해 전형적인 클래스 메서드처럼 취급된다.&#x20;

```typescript
class Test {
    constructor(name: string){
        console.log(`${name} in tot`)
    }
}

new Test("Son")
new Test() // Expected 1 arguments, but got 0.(2554)
```



### 8.2 클래스 속성

클래스에 대한 기본적인내용들...

타입스크립트에서 클래스의 속성을 읽거나 쓰려면 명시적으로 선언해야 한다. 클래스 속성은 인터페이스와 동일한 구문을 사용해 선언한다. 클래스 속성 이름 뒤에는 선택적으로 타입 애너테이션이 붙는다.

```typescript
class Trip {
    destination: string;

    constructor(destination: string){
        this.destination = destination
        console.log(`We're going ${this.destination}`)
        this.noneistent = destination // Property 'noneistent' does not exist on type 'Trip'.(2339)
    }
}
```

명시적으로 클래스 속성을 선언하면 타입스크립트가 빠르게 이해할 수 있다.



#### 8.2.1 함수 속성

자바스크립트에는 클래스 멤버를 호출 가능한 함수로 선언하는 두가지 방법이 있다.

```typescript
class Exam {
    myMethod(){}
    myProperty = () => {}
}
```

메서드로 접근하는 방식과 프로퍼티로 접근하는 방식 2가지인데,

메서드 접근 방식은 함수를 클래스 프로토타입에 할당하여 모든 클래스 인스턴스에서 동일한 함수 정의를 사용하는 방식이고,

프로퍼티 접근 방식은 인스턴스당 새로운 함수가 생성되어 새로운 함수를 생성하는 시간과 메모리 비용 측면에서 유용한 방식이다.



#### 8.2.2 초기화 검사

엄격한 컴파일러 설정이 활성화된 상태에서 타입스크립트는 undefined 타입으로 선언된 각 속성이 생성자에서 할당되었는지 확인한다. 이와 같은 엄격한 초기화 검사는 클래스 속성에 값을 미할당하는 실수를 예방할 수 있다.

대부분의 경우 엄격한 초기화 검사가 유용하지만 필요하지 않은 경우도 있다. 이 때 이름 뒤에 !를 추가해 검사를 비활성화하면 타입스크립트에서 속성이 사용되기 이전에 undefined 값을 할당한다.



#### 8.2.3 선택적 속성

이름 뒤에 ?를 추가해 선택적 속성을 선언할 수 있다. | undefined를 포함하는 유니언 타입과 거의 동일하게 작동한다.



#### 8.2.4 읽기 전용 속성

readonly 키워드를 추가해 읽기 전용으로 선언할 수 있다.



### 8.3 타입으로서의 클래스

타입 시스템에서 클래스는 상대적으로 독특하다. 클래스 선언이 런타임 값과 타입 에너테이션에서 사용 가능한 타입을 모두 생성한다.



### 8.4 클래스와 인터페이스

타입스크립트는 클래스 이름 뒤에 implements 키워드와 인터페이스 이름을 추가함으로써 클래스의 해당 인스턴스가 인터페이스를 준수한다고 선언할 수 있다.

```typescript
interface Test {
    name: string;
    study(hours: number): void
}

class Student implements Test {
    name: string;

    constructor(name: string){
        this.name = name
    }

    study(hours: number){
        for(let i = 0; i < hours; i++){
            console.log("... studying ...")
        }
    }
}

// Class 'Slacker' incorrectly implements interface 'Test'.
//  Type 'Slacker' is missing the following properties from type 'Test': name, study(2420)
class Slacker implements Test{
    // Test의 속성들을 추가해야 한다.
}
```



#### 8.4.1 다중 인터페이스 구현

타입스크립트의 클래스는 다중 인터페이스를 구현해 선언할 수 있다. 클래스에 구현된 인터페이스 목록은 인터페이스 이름 사이에 쉼표를 넣고, 개수 제한 없이 인터페이스를 사용할 수 있다.

```typescript
interface Graded {
    grades: number[]
}

interface Reporter {
    report: () => string
}

class ReportCard implements Graded, Reporter {
    grades: number[]

    constructor(grades: number[]){
        this.grades = grades
    }

    report(){
        return this.grades.join(", ")
    }
}

class Empty implements Graded, Reporter {}
// Class 'Empty' incorrectly implements interface 'Graded'.
//   Property 'grades' is missing in type 'Empty' but required in type 'Graded'.(2420)
// Class 'Empty' incorrectly implements interface 'Reporter'.
//   Property 'report' is missing in type 'Empty' but required in type 'Reporter'.(2420)
```



두 개의 충돌하는 인터페이스를 구현하는 클래스를 선언하려하면 타입 오류가 발생한다.

```typescript
interface AgeIsNumber {
    age: number
}

interface AgeIsString {
    age: string
}

class AsNumber implements AgeIsNumber, AgeIsString {
    age = 0;
    // Property 'age' in type 'AsNumber' is not assignable to the same property in base type 'AgeIsString'.
    //   Type 'number' is not assignable to type 'string'.(2416)
}
```

두 인터페이스가 다른 객체 형태를 표현하는 경우에는 동일 클래스로 구현하지 않아야 한다.



### 8.5 클래스 확장

타입스크립트는 클래스를 확장하거나 하위 클래스를 만드는 자바스크립트 개념에 타입 검사를 추가한다. 기본 클래스에 선언된 모든 메서드나 속성은 파생 클래스라고 하는 하위 클래스에서 사용 가능하다.

```typescript
class Teacher {
    teach() {
        console.log('Teach')
    }
}

class StudentTeacher extends Teacher {
    learn() {
        console.log("Learn")
    }
}

const teacher = new StudentTeacher()
teacher.teach()
teacher.learn()
```



#### 8.5.1 할당 가능성 확장

파생 인터페이스가 기본 인터페이스를 확장하는 것과 마찬가지로 하위 클래스도 기본 클래스의 멤버를 상속한다. 하위 클래스의 인스턴스는 기본 클래스의 모든 멤버를 가지므로 기본 클래스의 인스턴스가 필요한 모든 곳에서 사용 가능하다. 만약 기본 클래스에 하위 클래스가 가지고 있는 멤버가 없다면 더 구체적인 하위 클래스가 필요할 때 사용 불가능하다.

```typescript
class Lesson {
    subject: string;

    constructor(subject: string){
        this.subject = subject
    }
}

class OnlineLesson extends Lesson {
    url: string;

    constructor(subject: string, url: string){
        super(subject)
        this.url = url
    }
}

let lesson: Lesson
lesson = new Lesson("coding")
lesson = new OnlineLesson("coding", "naver.com")

let online: OnlineLesson
online = new Lesson("coding") // Property 'url' is missing in type 'Lesson' but required in type 'OnlineLesson'.(2741)
online = new OnlineLesson("coding", "naver.com")
```

타입스크립트의 구조적 타입에 따라 하위 클래스의 모든 멤버가 동일 타입의 기본 클래스에 이미 존재하는 경우 기본 클래스의 인스턴스를 하위 클래스 대신 사용할 수 있다.



#### 8.5.2 재정의된 생성자

타입스크립트에서 하위 클래스는 자체 생성자를 정의할 필요 없다. 암묵적으로 기본 클래스의 생성자를 사용하기 때문이다.

자바스크립트에서 하위 클래스가 자체 생성자를 선언하면 super 키워드를 통해 기본 클래스 생성자를 호출해야 한다. 하위 클래스 생성자는 기본 클래스에서의 필요 여부와 상관없이 모든 매개 변수를 선언 가능하다. 타입 검사기는 기본 클래스 생성자를 호출할 때 올바른 매개변수를 사용하는지 확인한다.

자바스크립트 규칙에 따르면 하위 클래스의 생성자는 this 또는 super에 접근하기 전 반드시 기본 클래스의 생성자를 호출해야 한다. 따라서 타입스크립트는 super()를 호출하기 전 this 또는 super에 접근하려는 경우 타입 오류를 보고한다.



#### 8.5.3 재정의된 메서드

하위 클래스의 메서드가 기본 클래스의 메서드에 할당될 수 있는 한 하위 클래스는 기본 클래스와 동일한 이름으로 새 메서드를 다시 선언할 수 있다. 기본 클래스를 사용하는 모든 곳에 하위 클래스를 사용할 수 있으므로 새 메서드의 타입도 기본 메서드 대신 사용할 수 있어야 한다.



#### 8.5.4 재정의된 속성

하위 클래스는 새 타입을 기본 클래스의 타입에 할당할 수 있는 한 동일한 이름으로 기본 클래스의 속성을 명시적으로 다시 선언할 수 있다. 재정의된 메서드와 마찬가지로 하위 클래스는 기본 클래스와 구조적으로 일치해야 한다.

속성을 다시 선언하는 대부분의 하위 클래스는 해당 속성을 유니언 타입의 더 구체적인 하위 집합으로 만들거나 기본 클래스 속성 타입에서 확장되는 타입으로 만든다.

```typescript
class Assignment {
    grade?: number
}

class GradedAssignment extends Assignment {
    grade : number

    constructor (grade: number){
        super()
        this.grade = grade
    }
}
```

유니언 타입의 허용된 값 집합을 확장할 수는 없다. 확장한다면 하위 클래스 속성은 더 이상 기본 클래스 속성 타입에 할당할 수 없다.

```typescript
class NumericGrade {
    value = 0
}

class VagueGrade extends NumericGrade {
    value = Math.random() > 0.5 ? 1 : "..."

    // Property 'value' in type 'VagueGrade' is not assignable to the same property in base type 'NumericGrade'.
    // Type 'string | number' is not assignable to type 'number'.
    //  Type 'string' is not assignable to type 'number'.(2416)
}

const instance: NumericGrade = new VagueGrade()
instance.value
```



### 8.6 추상 클래스

때로는 일부 메서드의 구현을 선언하지 않고, 대신 하위 클래스의 해당 메서드 제공을 예상하고 기본 클래스를 만드는 방법이 유용할 수 있다. 추상화하려는 클래스 이름과 메서드 앞에 타입스크립트의 abstract 키워드를 추가한다. 이러한 추상화 메서드 선언은 추상화 기본 클래스에서 메서드의 본문 제공을 건너뛰고, 대신 인터페이스와 동일한 방식으로 선언된다.

```typescript
abstract class School {
    readonly name: string

    constructor(name: string){
        this.name = name
    }

    abstract getStudentTypes(): string[]
}

class Preschool extends School {
    getStudentTypes() {
        return ["prescholler"]
    }
}

class Absence extends School {}
// Non-abstract class 'Absence' does not implement all abstract members of 'School'(18052)
// input.tsx(8, 14): Non-abstract class 'Absence' does not implement inherited abstract member 'getStudentTypes' from class 'School'.
```

구현이 존재한다고 가정할 수 있는 일부 메서드에 대한 정의가 없기에 추상 클래스를 직접 인스턴스화 할 수 없다. 추상 클래스가 아닌 클래스만 인스턴스화 가능하다.

추상 클래스는 클래스의 세부 사항이 채워질 거라 예상되는 프레임워크에서 자주 사용된다. 클래스는 값이 클래스를 준수해야 함을 나타내는 타입 애너테이션으로 사용할 수 있다. 그러나 새 인스턴스를 생성하려면 하위 클래스를 사용해야 한다.



### 8.7 멤버 접근성

자바스크립트에서는 클래스 멤버 앞에 # 을 추가해 private 클래스 멤버임을 나타낸다. private 클래스 멤버는 해당 클래스 인스턴스에서만 접근 가능하다. 자바스크립트 런타임은 클래스 외부 코드 영역에서 private 메서드나 속성에 접근하려면 오류를 발생시킴으로써 프라이버시를 강화한다.

타입스크립트의 클래스 지원은 자바스크립트의 # 프라이버시 보다 먼저 만들어졌다. 또한 타입스크립트는 private 클래스 멤버를 지원하지만, 타입 시스템에만 존재하는 클래스 메서드와 속성에 대해 조금 더 미묘한 프라이버시 정의 집합을 허용한다. 타입스크립트의 멤버 접근성(가시성 visibility)은 클래스 멤버 선언 이름 앞에 다음 키워드 중 하나를 추가해 만든다.

* public 모든 곳에서 누구나 접근 가능
* protected 클래스 내부 또는 하위 클래스에서만 접근 가능
* private 클래스 내부에서만 접근 가능

이 키워드는 순수하게 타입 시스템 내에서만 존재한다.



타입 스크립트의 멤버 접근성은 타입 시스템에서만 존재하는 반면, 자바스크립트의 private 선언은 런타임에도 존재한다는 점이 차이점이다. protected 또는 private로 선언된 타입스크립트 클래스 멤버는 명시적으로 또는 암묵적으로 public으로 선언된 것 처럼 동일한 자바스크립트 코드로 컴파일된다. 자바스크립트 런타임에서는 # private 필드만 진정한 private이다.

접근성 제한자는 readonly와 함께 표시할 수 있다. readonly와 명시적 접근성 키워드로 멤버를 선언하려면 접근성 키워드를 먼저 적어야 한다.



#### 8.7.1 정적 필드 제한자

자바스크립트는 static 키워드를 사용해 클래스 자체에서 멤버를 선언한다. 자바스크립트는 static 키워드를 단독 사용하거나 readonly와 접근성 키워드를 함께 사용할 수 있도록 지원한다.

접근성 키워드 + static + readonly 키워드의 순으로 작성한다.

```typescript
class Question {
    protected static readonly answer: "bash"
    protected static readonly prompt = "WWEFWEF"
    
    guess(getAnswer: (prompt: string) => string) {
        const answer = getAnswer(Question.prompt)

        if(answer === Question.answer){
            console.log("Answer !")
        }else{
            console.log("Try again...")
        }
    }
}
```



### 8.8 마치며

* 클래스 메서드 및 속성 선언과 사용법
* 읽기 전용 또는 선택적 속성으로 표시하기
* 타입 애너테이션에서 타입으로 클래스 이름 사용하기
* 클래스 인스턴스 형태를 적용하기 위한 인터페이스 구현하기
* 하위 클래스에 대한 할당 가능성과 재정의 규칙으로 클래스 확장하기
* abstract 클래스와 abstract 메서드로 표시하기
* 클래스 필드에 타입 시스템 제한자 추가하기

