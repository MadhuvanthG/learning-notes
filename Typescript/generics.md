#Pre-requisites
Knowing the basics of generics - go through https://www.typescriptlang.org/docs/handbook/generics.html

#What we will focus on here
**Examples would be React specific**
**How can you use generics to preserve/propagate type information**
**Generic constraints and why they are useful**

##Type variable

Consider a function which wraps the given operation in a promise
resolves with the output of the function
```
function makeAsync(operation: Function): Promise<any> {
    return new Promise((resolve, rejct) => {
        resolve(operation())
    })
}
```

Now, let's call the function
Notice that we have no information of the type of the resolved value
```
const operation = () => ["Hello", "World"]
makeAsync(operation).then(value => console.log(value))
```

Generics would be useful here
Introduce a Type variable T that represents the type of the resolved value of the operation
Allows your makeAsync function to wrap operations that could resolve to a variety of types
```
function makeGenericAsync<T>(operation: Function): Promise<T> {
    return new Promise((resolve, rejct) => {
        resolve(operation())
    })
}
```

Wrapping your operation with the new function (makeGenericAsync) 
allows you to preserve the type of the resolved value
```
const operation = () => ["Hello", "World"]
makeGenericAsync<Array<string>>(operation).then(value => console.log(value))
```


 