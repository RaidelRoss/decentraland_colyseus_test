[Decentraland](https://docs.decentraland.org/) + [Colyseus](https://github.com/colyseus)

fixed error 'Awaited' in (es2015.iterable.ts)

```
lib.es2015.iterable.d.ts (227,59): Cannot find name 'Awaited'
```

--------------------------------------------------------
Solution:

```
declare global {

/**
 * Recursively unwraps the "awaited type" of a type. Non-promise "thenables" should resolve to `never`. This emulates the behavior of `await`.
 */
type Awaited<T> =
    T extends null | undefined ? T : // special case for `null | undefined` when not in `--strictNullChecks` mode
        T extends object & { then(onfulfilled: infer F): any } ? // `await` only unwraps object types with a callable `then`. Non-object types are not unwrapped
            F extends ((value: infer V, ...args: any) => any) ? // if the argument to `then` is callable, extracts the first argument
                Awaited<V> : // recursively unwrap the value
                never : // the argument to `then` was not callable
        T; // non-object or non-thenable

}

export { };
```

https://github.com/colyseus/decentraland/issues/2