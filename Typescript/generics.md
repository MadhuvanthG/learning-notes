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

Let's say, we want to write a function that abstracts fetch operation of any given endpoint
At its simplest form, its signature would be something like

```
async function performRequest(endpoint: string, options: { params: string[] }): Promise<any> {
    return fetch(endpoint).then(response => response.json())
}
```
Takes name of an endpoint and options (headers etc..) as arguments
Returns a Promise type

Now we want to call a `/getPersons` endpoint (a hypothetical example) 
that we know will return a value of type Person
```
interface Person {
    firstName: string
    lastName: string
}

performRequest("getPersons", {params: {id: "1"}}).then(person => person.????)
```

We fetch response of `/getPersons` endpoint using `performRequest`, but
TS can't infer the type of the resolved value to be `Person`
That means, we can NOT confidently code (aka Typescript can't provide code completion options which can really be useful)
person.firstName or person.lastName

Because our `performRequest` is meant to work for all endpoints in our application,
it has no way (yet) to specify what would be the type of the resolved value, hence it returns `Promise<any>`

Generics is the answer to this problem. New version of the function takes a generic type
that represents type of value a given endpoint is expected to return
```
async function performRequest<TData>(endpoint: string, options: RequestOptions): Promise<TData> {
    return fetch(endpoint).then<TData>(response => response.json())
}
```
Here we are basically using generics to assert that `getPersons` endpoint would return a JSON object of type `Person`

**Caveat**: At Runtime, it could be different based on how the endpoint works but Typescript can't really 
save you from that. Because well, it's compile time checking.
In this case, what it still gives you though is better readability 
and documentation leading to easy maintenance as well



# Useful materials
React-Typescript cheatsheet - https://github.com/typescript-cheatsheets/react-typescript-cheatsheet




 