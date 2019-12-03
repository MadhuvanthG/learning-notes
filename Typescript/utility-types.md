Typescript provides some utility types that lets you transform 
a defined type (ex. an interface) into another type 

https://www.typescriptlang.org/docs/handbook/utility-types.html

# Example

Consider an interface that's already available in your application
to represent type of the result of a fetch() call
```
interface Result<T> {
    data: T
    status: Boolean
    url: URL
    headers: Headers
}
```

Now, you would like to perform another async operation (ex. reading contents of a static file) 
that could only return `data` and `status` and store the result in a constant `staticData`

You could define the type of `staticData` by picking them from `Result` type
```
const staticData: Pick<Result<any>, "data" | "status"> = {
    data: "",
    status: true
}
```

This is equivalent to defining a new type with just the `data` and `status` properties
```
interface StaticResult<T> {
    data: T
    status: Boolean
}
const staticData: StaticResult<any> = {
    data: "",
    status: true
}
```

However, if in future in your application, you decide to change all your async operations 
to define `status` as an enumeration rather than a Boolean. With different types to represent
results, you would be forced to change it everywhere.

Should you use the utility type `Pick`, you just have to change it in the parent type (in our case, it's `Result`)

#### What is Pick or how is it defined?

Pick is just a type alias that looks like this
https://github.com/Microsoft/TypeScript/blob/4ff71ecb98ccbd882feb1738b0c6f1cc93c2ea66/src/lib/es5.d.ts#L1425
```
type Pick<T, K extends keyof T> = {
    [P in K]: T[P]
}
```

Drilling this type alias down
* New type alias Pick accepts two generic types (T, K)
* T represents the type you want to pick from (in our case, Result<T>)
* K represents the keys or properties you want to pick from T (in our example above, `data` & `status`)
* K should be one or more of keys defined in T. Hence, the constraint `extends keyof T`
* Definition of the type alias says - For every property in K, derive its type from T (i.e. T[P])

There are other utility types provided by Typescript - 
see https://www.typescriptlang.org/docs/handbook/utility-types.html 
and much more in this package - https://github.com/piotrwitek/utility-types


# Useful materials
More utility types - https://github.com/piotrwitek/utility-types
