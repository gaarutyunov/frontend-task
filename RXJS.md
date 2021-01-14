# RxJs

## 1. Операторы

### 1. Написать оператор с кэшированием

```typescript
// Функция, которая получает объект
export interface Get<T> {
    (id: number): Observable<T>;
}

// Оператор функция
export interface CacheFn<R> {
    (project: Get<R>): OperatorFunction<number, R>;
}

// Источник данных (для упрощения обычный BehaviorSubject)
export const subject = new BehaviorSubject<Record<number, Person>>({
    1: {
        id: 1,
        name: 'First'
    },
    2: {
        id: 2,
        name: 'Second'
    },
    3: {
        id: 3,
        name: 'Third'
    },
    4: {
        id: 4,
        name: 'Fourth'
    }
});

// Функция для получения данных
export function get(id: number): Observable<Person | undefined> {
    return subject.pipe(map(x => x[id]))
}

// При вызове оператора
of(1, 1, 1).pipe(cache(get)).subscribe(console.log);
// Вывод 
// { id: 1, name: 'First' }
// Returning from cache (задается внутри оператора, чтобы показать, что выводится из кэша)
// { id: 1, name: 'First' }
// Returning from cache
// { id: 1, name: 'First' }
```