# TypeScript

## 1. Типизация

```typescript
// Тип действия
export interface Action<T extends string, P> {
    // Тип действия
    type: T;
    // Тип передаваемого пэйлоуда 
    payload: P;
}

// Сущность с уникальным идентификатором
export interface Entity {
    id: number;
}

// Доступные действия с сущностью
export enum Actions {
    CREATE = 'CREATE',
    READ = 'READ',
    UPDATE = 'UPDATE',
    DELETE = 'DELETE',
}

// Ассоциативный массив со списком доступных действий
export interface ActionMap<T extends Entity> {
    create: Action<Actions.CREATE, T>;
    update: Action<Actions.UPDATE, T>;
    read: Action<Actions.READ, number>;
    delete: Action<Actions.DELETE, number>;
}
```

### 1. Создать интерфейс Person, который наследуется от Entity, у которого одно строковое поле name

### 2. Создать тип, наследуясь от которого, в интерфейс добавляются методы следующих типов

```typescript
// Тип функции в которую передается пэйлоуд
export interface PayloadDispatcherFn<T extends string, P> {
    (type: T, payload: P): void;
}

// Тип функции, в которую передается только тип действия
export interface EmptyDispatcherFn<T extends string> {
    (type: T): void;
}
```

### 3. Теперь нужно создать новый интерфейс Task, который так же наследуется от Entity и имеет поле person: Person

### 4. У тебя добавились действия с объектом, нужно создать интерфейс, в котором помимо действий из ActionMap есть другие действия

```typescript
export enum AdditionalActions {
    UPSERT = 'UPSERT',
    READ_ALL = 'READ_ALL',
    REMOVE_ALL = 'REMOVE_ALL',
}

export interface AdditionalActionMap<T extends Entity> {
    upsert: Action<AdditionalActions.UPSERT, T>;
    readAll: Action<AdditionalActions.READ_ALL, undefined>;
    reset: Action<AdditionalActions.REMOVE_ALL, undefined>;
}
```

## 2. Декораторы

### 1. Написать декоратор, который можно использовать для валидации объекта

```typescript
// Интерфейс с валидаторами
export interface Validators {
    required?: boolean;
}

// Интерфейс самого декоратора
export interface Validate<T extends Object> {
    (validators: Validators): Decorator<T>;
}

// Интерфейс для функции-декоратора
export interface Decorator<T extends Object> {
    (target: T, property: string): void
}

export class Person {
    @Validate({
        required: true
    })
    public name?: string;

    constructor(name?: string) {
        this.name = name;
    }
}

const person = new Person();

console.log('Is invalid? ', Validator.validate(person)); // Is invalid? true

person.name = 'Test name';

console.log('Is invalid? ', Validator.validate(person)); // Is invalid? false
```

