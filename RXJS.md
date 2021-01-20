# RxJs

## 1. Операторы

### 1. Написать оператор с кэшированием

```typescript
// Функция, которая получает объект
export interface Get<T> {
    (id: string): Observable<T>;
}

// Оператор функция
export interface CacheFn<R> {
    (project: Get<R>): OperatorFunction<string, R>;
}

// Источник данных (для упрощения обычный BehaviorSubject)
export const subject = new BehaviorSubject<Record<string, Person>>({
    '1': {
        id: 1,
        name: 'First'
    },
    '2': {
        id: 2,
        name: 'Second'
    },
    '3': {
        id: 3,
        name: 'Third'
    },
    '4': {
        id: 4,
        name: 'Fourth'
    }
});

// Функция для получения данных
export function get(id: string): Observable<Person | undefined> {
    return subject.pipe(map(x => x[id]))
}

// При вызове оператора
of('1', '1', '1').pipe(cache(get)).subscribe(console.log);
// Вывод 
// { id: 1, name: 'First' }
// Returning from cache (задается внутри оператора, чтобы показать, что выводится из кэша)
// { id: 1, name: 'First' }
// Returning from cache
// { id: 1, name: 'First' }
```

### 2. Написать last recently used оператор с кэшированием

```typescript
export interface LruItem<T> {
    value: T;
    lastUsed: Date;
}

export interface LruStore<T> {
    [key: string]: LruItem<T>;
}

export interface Lru<T> {
    store: LruStore<T>;
    readonly bucketSize: number;

    has(id: string): boolean;

    get(id: string): LruItem<T> | undefined;

    getValue(id: string): T | undefined;

    set(id: string, value: T): void;

    delete(id: string): void;
}

// При вызове оператора c bucketSize = 2
of('1', '1', '2', '2', '3', '1').pipe(lru(get)).subscribe(console.log);
// Вывод 
// { id: 1, name: 'First' }
// Returning from cache (задается внутри оператора, чтобы показать, что выводится из кэша)
// { id: 1, name: 'First' }
// { id: 2, name: 'Second' }
// Returning from cache
// { id: 2, name: 'Second' }
// { id: 3, name: 'Second' }
// { id: 1, name: 'Second' }
```