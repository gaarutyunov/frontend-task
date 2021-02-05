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

### 4. Добавились действия с объектом, нужно создать интерфейс, в котором помимо действий из ActionMap есть другие действия

```typescript
export enum AdditionalActions {
    UPSERT = 'UPSERT',
    READ_ALL = 'READ_ALL',
    REMOVE_ALL = 'REMOVE_ALL',
}

export interface AdditionalActionMap<T extends Entity> {
    upsert: Action<AdditionalActions.UPSERT, T>;
    readAll: Action<AdditionalActions.READ_ALL, undefined>;
    removeAll: Action<AdditionalActions.REMOVE_ALL, undefined>;
}
```

В результаты ожидается получить интерфейс, на который IDE будет реагировать следующим образом:

![Screenshot](https://github.com/gaarutyunov/frontend-task/edit/master/Screenshot 2021-02-05 at 10.31.11.png)
