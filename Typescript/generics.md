## Type variable

Consider a function which wraps the given operation in a promise
And returns the Promise
```
function makeAsync(operation: Function): Promise<any> {
    return new Promise((resolve, rejct) => {
        resolve(operation())
    })
}
```

Now, let's call the function
Notice that we have no information of the type of the resolved value
Even though operation was defined by us
```
const operation = () => {
    // can be any async operation
    return ["Hello", "World"]
}
makeAsync(operation).then(resolvedValue => console.log(resolvedValue))
```

Generics would be useful here to define & preserve the type of the resolved value
Introduce a Type variable T that represents the type of the resolved value of the operation
Allows your makeAsync function to wrap operations that could resolve to a variety of types
```
function makeGenericAsync<T>(operation: Function): Promise<T> {
    return new Promise((resolve, rejct) => {
        resolve(operation())
    })
}
```

Wrapping your operation with the new generic function (makeGenericAsync) 
allows you to preserve the type of the resolved value
```
const operation = () => ["Hello", "World"]
makeGenericAsync<Array<string>>(operation).then(resolvedValue => console.log(resolvedValue))
```

# Useful materials
React-Typescript cheatsheet - https://github.com/typescript-cheatsheets/react-typescript-cheatsheet




 