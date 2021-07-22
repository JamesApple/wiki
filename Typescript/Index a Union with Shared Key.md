Given a list of items in a [discriminated
union](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#discriminated-unions)
it's common to need to be able to index the type by its discriminator

# Example

Given these classes:

```typescript
class User {
		type = 'USER' as const

		userId: string
}

class Admin {
		type = 'ADMIN' as const

		adminId: string
}

```

If we have a container class we may want to provide a method like so:

```typescript
type AppUser = User | Admin

class Person {
	private value: AppUser

	maybe<K extends this['value']['type']>(key: K)/** What Type? */
	{
		if(this.value.type === key) {
			return this.value
		}
		return null
	}
}
```

In order to properly type the return value we will need to index the `Person` type to look like:
```typescript
type IndexedAppUser = {
	USER: User,
	ADMIN: Admin
}
```

Given that background we can index any set of data that shares a string key with a generic like:

```typescript
// UnionMapped<T, TK extends StringKeys<T>>
type IndexedAppUser = UnionMapped<AppUser, 'type'>
```

To define this we need to be able to:
1. "Iterate" through the values of `TK` in `T`
2. Extract the union member that has `TK` matching `T` for this iteration


`Tuple` creates an object of `{ type: 'USER' }` from the input `Tuple<'type', 'USER'>`

```typescript
type Tuple<K extends string, V> = { [key in K]: V }
```


`StringKeys` returns only keys that have a value assignable to `string`.

```typescript
type StringKeys<T> = {
  [K in keyof T]: T[K] extends string ? K : never
}[keyof T]
```


`String` only allows values assignable to `string`. This is important as union
types cannot be used to index an object directly.

```typescript
type String<T> = T extends string ? T :	never
```

`UnionMapped` is the final helper that will create the union

```typescript
```
