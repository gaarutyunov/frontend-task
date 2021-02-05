## Декораторы

### Написать декоратор, который можно использовать для валидации объекта

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
