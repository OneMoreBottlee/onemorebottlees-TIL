# Ex 1

### 문제

```typescript
export type User = unknown;

export const users: unknown[] = [
    {
        name: 'Max Mustermann',
        age: 25,
        occupation: 'Chimney sweep'
    },
    {
        name: 'Kate Müller',
        age: 23,
        occupation: 'Astronaut'
    }
];

export function logPerson(user: unknown) {
    console.log(` - ${user.name}, ${user.age}`);
}

console.log('Users:');
users.forEach(logPerson);

```



### 해결

변수 users를 보고 User에 들어갈 타입을 작성한다.

users는 name, age, occupation을 키로 문자열, 숫자 타입을 값으로 갖는 객체의 배열 형태이다.

따라서 User 타입은 다음과 같다.

```typescript
export type User = {
    name: string,
    age: number,
    occupation: string
};

export const users: User[] = [
    {
        name: 'Max Mustermann',
        age: 25,
        occupation: 'Chimney sweep'
    },
    {
        name: 'Kate Müller',
        age: 23,
        occupation: 'Astronaut'
    }
];

export function logPerson(user: User) {
    console.log(` - ${user.name}, ${user.age}`);
}

console.log('Users:');
users.forEach(logPerson);
```

