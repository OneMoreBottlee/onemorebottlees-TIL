# Ex 8

### 문제

```typescript
/*

Intro:

    Project grew and we ended up in a situation with
    some users starting to have more influence.
    Therefore, we decided to create a new person type
    called PowerUser which is supposed to combine
    everything User and Admin have.

Exercise:

    Define type PowerUser which should have all fields
    from both User and Admin (except for type),
    and also have type 'powerUser' without duplicating
    all the fields in the code.

*/

interface User {
    type: 'user';
    name: string;
    age: number;
    occupation: string;
}

interface Admin {
    type: 'admin';
    name: string;
    age: number;
    role: string;
}

type PowerUser = unknown;

export type Person = User | Admin | PowerUser;

export const persons: Person[] = [
    { type: 'user', name: 'Max Mustermann', age: 25, occupation: 'Chimney sweep' },
    { type: 'admin', name: 'Jane Doe', age: 32, role: 'Administrator' },
    { type: 'user', name: 'Kate Müller', age: 23, occupation: 'Astronaut' },
    { type: 'admin', name: 'Bruce Willis', age: 64, role: 'World saver' },
    {
        type: 'powerUser',
        name: 'Nikki Stone',
        age: 45,
        role: 'Moderator',
        occupation: 'Cat groomer'
    }
];

function isAdmin(person: Person): person is Admin {
    return person.type === 'admin';
}

function isUser(person: Person): person is User {
    return person.type === 'user';
}

function isPowerUser(person: Person): person is PowerUser {
    return person.type === 'powerUser';
}

export function logPerson(person: Person) {
    let additionalInformation: string = '';
    if (isAdmin(person)) {
        additionalInformation = person.role;
    }
    if (isUser(person)) {
        additionalInformation = person.occupation;
    }
    if (isPowerUser(person)) {
        additionalInformation = `${person.role}, ${person.occupation}`;
    }
    console.log(`${person.name}, ${person.age}, ${additionalInformation}`);
}

console.log('Admins:');
persons.filter(isAdmin).forEach(logPerson);

console.log();

console.log('Users:');
persons.filter(isUser).forEach(logPerson);

console.log();

console.log('Power users:');
persons.filter(isPowerUser).forEach(logPerson);

// In case you are stuck:
// https://www.typescriptlang.org/docs/handbook/utility-types.html
// https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types

```



### 풀이

User와 Admin 타입을 합친 PowerUser 타입을 생성해 정의해야 한다.

처음에는 &를 활용해봤는데

```typescript
type PowerUser = User & Admin
```

타입 PowerUser의 프로퍼티인 type = "powerUser"여야 했기에 실패했다.

그래서 다음과 같이 type을 설정하고, Pick을 활용해 User와 Admin에서 필요한 프로퍼티를 가져와 합성했다.

```typescript
type PowerUser = {type: 'powerUser'} & Pick<User, 'name' | 'age' | 'occupation' > & Pick<Admin, 'role'>
```



\*\*

정답을 보니 Pick으로 쓸 프로퍼티를 가져오는게 아니라 안 쓰는 프로퍼티를 지우는 방법을 사용했다.

```typescript
type PowerUser = Omit<User, 'type'> & Omit<Admin, 'type'> & {
    type: 'powerUser'
};
```

이 방법이 더 깔끔한 방법이었다.



### 참고

Pick\<Type, 'property'> - 타입에서 특정 프로퍼티를 뽑아 타입을 구성한다.

Omit\<Type, 'property'> - 타입에서 특정 프로퍼티를 지운 타입을 구성한다.

[참고](https://im-developer.tistory.com/209)

