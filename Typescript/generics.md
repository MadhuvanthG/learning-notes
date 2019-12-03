## Type variable

What is a type variable?

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



## Interfaces with Generic types
Similar to functions, interfaces can also have generic types
A simple usecase for that-

Let's say you define an interface to represent result of an async API call
```
interface Result {
    data: any
    isOperationSuccess: boolean
}
```

And this is your function that does the API call
```
async function performRequest(endpoint: string, options: { params: object }): Promise<Result> {
    // Fetches the result of endpoint
    const data = await fetch(endpoint).then(response => response.json())

    // Populates the data and status (isOperationSuccess) and returns
    return {
        data: data,
        isOperationSuccess: true
    }
}
```

Now we want to call a `/getPersons` endpoint (a hypothetical example) 
that we know will return a value of type Person
```
interface Person {
    firstName: string
    lastName: string
}

performRequest("getPersons", {params: {id: "1"}}).then((result: Result) => result.data.????)
```

We fetch response of `/getPersons` endpoint using `performRequest`, but
TS can't infer the type of the resolved value (i.e. `result.data`) to be `Person`
That means, we can NOT confidently code (aka Typescript can't provide code completion options which can really be useful)
result.data.firstName or result.data.lastName

How do we define what is the expected type of `data` when a given endpoint is called?
Using Generic interfaces. Look at the modified signature of the interface and function below.
Note- Generic type on an interface is visible to all properties of the interface

```
interface Result<T> {
    data: T
    status: boolean
}
```

```
async function performRequest<T>(endpoint: string, options: { params: object }): Promise<Result<T>> {
    const data = await fetch(endpoint).then<T>(response => response.json())

    return {
        data: data,
        status: true
    }
}
```

Call the performRequest now and there you could see, TS can infer the type of `result` to be `Result<Person>`
```
performRequest<Person>("getPersons", {params: {id: "1"}}).then((result: Result<Person>) => console.log(result.data.firstName))
```
Here we have explicitly annotated the type, but you don't have to.

**Caveat**: At Runtime, it could be different based on how the endpoint works but Typescript can't really 
save you from that. Because well, it's compile time checking.
In this case, what it still gives you though is better readability 
and documentation leading to easy maintenance as well




# Useful materials
React-Typescript cheatsheet - https://github.com/typescript-cheatsheets/react-typescript-cheatsheet




 