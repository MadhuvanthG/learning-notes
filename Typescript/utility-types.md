Typescript provides some utility types that lets you transform 
a defined type (ex. an interface) into another type 

# Example
#### TODO: Don't like the examples, change it to something more useful
#### TODO: Also change comments to be descriptive

Consider a interface
```
interface PersonAPIResponse {
    name: string
    gender: string
    personalInfo?: string[]
}
```

Creating a const with only name of PersonAPIResponse
```
const PersonName: Pick<PersonAPIResponse, "name"> = {
    name: "NewName"
}
```

Try to recreate the alias Pick
Type aliases with generics is what you need
This iterates over every property provided in the second generic type variable
And defines its type from same as your original type T
See implementation - https://github.com/Microsoft/TypeScript/blob/4ff71ecb98ccbd882feb1738b0c6f1cc93c2ea66/src/lib/es5.d.ts#L1425
```
type PickDup<T, K extends keyof T> = {
    [Prop in K]: T[Prop]
}
```

Using PickDup now
```
const PersonGender: PickDup<PersonAPIResponse, "gender"> = {
    gender: "female"
}
```

Your API is unstable; so you want every type in PersonAPIResponse to be optional for now
`type SafePersonAPIResponse = Partial<PersonAPIResponse>`

```
const safeResult: SafePersonAPIResponse = {
    name: "OptionalName"
}
```

Try to recreate the alias Partial
Iterates over every property in T
Add `?` modifier to make it optional in the new type
type PartialDup<T> = {
    [P in keyof T]?: T[P]
}

#### TODO
Let's now try to create something that is not provided out of the box by Typescript
I want to create a type that has only the non-optional properties from a given type


# Useful materials
More utility types - https://github.com/piotrwitek/utility-types
