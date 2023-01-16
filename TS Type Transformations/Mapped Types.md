# Mapped Types

# Map over a union to create and object
```ts
type RoutesObject = {
  [R in Route]: R
}
```
The [R in Route] part looks similar to an index, but it isn't.

Instead, it's saying to map over the Route union extract each member into a private variable called R and make it the property's value.

When hovering over RoutesObject, we can see that the Route union's string literals are now the keys and values of the object:

```ts
// on hover
type RoutesObject = {
  "/": "/"
  "/about": "/about"
  "/admin": "/admin"
  "/admin/users": "/admin/users"
}
```
### More Mapped Type Examples
Here are a few more experiments and examples to demonstrate how mapped types work.

###Assigning Values to string
We could assign the values to be strings instead:

```ts
type RoutesObject = {
  [R in Route]: string
}
```
This would create keys using each member of the Route union. However, using string no longer enforces that the values are the members of the union.

### Manually Passing a Union
We can also manually pass in a union instead of using Route:

```ts
type RoutesObject = {
  [R in "/" | "wow"]: R
}```
Now hovering over RoutesObject shows us this result:

```ts
{
  "/": "/",
  "wow": "wow"
}
```
### Passing additional options
We can also pass additional options such as undefined:

```ts
type RoutesObject = {
  [R in Route]: R | undefined
}
```
Pretty much any union can be mapped over and have an object created from it!


# Map Over the Keys of an Object

The solution to this challenge contains a similar structure to the [R in Route] that we used in the previous challenge:

```ts
type AttributeGetters = {
  [K in keyof Attributes]: () => Attributes[K]
}
```
The same general procedure is there as well:

The left side declares what our key is, and the right side declares what we'll do with that key.

Steps to the Solution
---
For the sake of clarity, let's simplify the right side of the solution to just have K:

Functionally, the this is the same as the previous exercise, where K is used as both the key and the value:

```ts
type AttributeGetters = {
  [K in keyof Attributes]: K
}
```
In order to access the Attribute key's value, we need to use an indexed access type:

```ts
type AttributeGetters = {
  [K in keyof Attributes]: Attributes[K]
}
```
Now we have an object with the same keys and values as Attributes.

However, the challenge was to return getter functions.

In order to do this, we just need to add that structure:

```ts
type AttributeGetters = {
  [K in keyof Attributes]: () => Attributes[K]
}
```


# Remapping Object Keys in a Mapped Type


```ts
type AttributeGetters = {
  [K in keyof Attributes as `get${Capitalize<K>}`]: () => Attributes[K]
}
```
Let's break it down.

The as keyword
This is the first appearance of the as keyword in this workshop.

You might have seen it used like const num = 1 as number, where it is attached to a runtime variable. Annoyingly, TypeScript treats the as keyword differently in different contexts.

Here, we are using as as a key mapper. This gives us access to the original key from Attributes, while also allowing us to use it in a template literal.

Here we use a template literal to add the get prefix and the Capitalize string utility type to remap the key:
`get${Capitalize<K>}` 
If we didn't capitalize the first letter of K we would end up with getname instead of getName.

Experiments
There are a couple of experiments to demonstrate more about how this technique works.

Remap Multiple Properties to One Key
The keys can be changed to anything that we'd like. For example, we could set every key to "wow":

```ts
type AttributeGetters = {
  [K in keyof Attributes as "wow"]: () => Attributes[K]
}
```
Remapping multiple properties to the same key has an interesting effect.

Hovering over AttributeGetters, we can see that we are now getting a function that can return multiple types:

```ts
// on hover
type AttributeGetters = {
  wow: () => string | number
}
```
Manually Passing a Union
Similarly to what we saw before, we can manually pass in a union instead of using keyof and as:

```ts
type AttributeGetters = {
  [K in
    | "firstName"
    | "lastName"
    | "age" as `get${Capitalize<K>}`]: () => Attributes[K]
}
```

# Selective Remapping with Conditional Types and Template Literals
It’s best to start simple when doing key remapping.

We’ll start this solution by mapping over the keys of the generic T to give us back the object that we passed in:

```ts
type OnlyIdKeys<T> = {
  [K in keyof T]: T[K]
}
```
The T[K] will probably not change, since whatever we put in there will correspond to the values of what we pass in.

In order to keep things clean, we’ll create a helper type called SearchForId that will use a string template to look for "id" or "Id" that is surrounded by strings of any length:


```ts
type SearchForId = `${string}${"id" | "Id"}${string}` 
```
Now we can use a conditional type inside of the mapped type.

For every key of the object, we’ll check if it extends SearchById. If it does, it will be included. Otherwise, the never type will be used.

By using never as the else case, any keys that don’t extend SearchById will not be included in the OnlyIdKeys object:

```ts
type OnlyIdKeys<T> = {
  [K in keyof T as K extends SearchForId ? K : never]: T[K]
}
```
This technique lets us restrict the keys which are mapped back into the object, while still keeping hold of K to index into the object. Very cool.


# Union to an Object

Solution 1: Creating a Union
One way to solve this problem is based on creating a union of all of the possible route values, which is the discriminator.

Here's what this this solution looks like:

```ts
type RoutesObject = {
  [R in Route["route"]]: Extract<Route, { route: R }>["search"]
}
```
In the code above, we map over the Route["route"] union. Each key is set to the value of route, which is then assigned to R.

We then use Extract to get out only the route that matches the current R value.

Finally, we use an indexed access type to get the search property off of the matching object:

```ts
type RoutesObject = {
  [R in Route["route"]]: Extract<Route, { route: R }>["search"]
}
```
We do end up with the correct solution, but it's too verbose.

Solution 2: Using as
A cleaner solution is to iterate over Route itself, and remap the keys using the as clause.

As seen in the previous exercise, this means that we have access to the entire route object instead of only the route property.

This means we can use indexed access to get the search property directly off of R without needing to do any kind of extraction!

Here's what this solution looks like:

```ts
type RoutesObject = {
  [R in Route as R["route"]]: R["search"]
}
```
Experiment: Removing the as clause
Let's try removing the as clause from the RoutesObject solution above:

```ts
type RoutesObject = {
  [R in Route]: R["search"]
}
```
This throws us an interesting error:

```
Type 'Route' is not assignable to type 'string | number | symbol'. Type '{ route: "/"; search: { page: string; perPage: string; }; }' is not assignable to type 'string | number | symbol'. 
```
This is saying that you can't just have whatever you want as the key. You must use either a string, number, or symbol as a key in TypeScript.

When we use as, we are able to assign R to whatever we want, while ensuring that the key will be set to a valid type.

This is a great technique for manipulating discriminated unions into new objects!



# Create a Union of Tuples by Reindexing a Mapped Type

In order to create a union of tuples, we need to extract the values of the object that is currently being created:

```ts
  email: ["email", string]
  firstName: ["firstName", string]
  lastName: ["lastName", string]
}
```
The solution is to re-index the type we started with by appending [keyof Values] to the end of it.

Before:

```ts
type ValuesAsUnionOfTuples = {
  [K in keyof Values]: [K, Values[K]]
}
```
After:
```ts
type ValuesAsUnionOfTuples = {
  [K in keyof Values]: [K, Values[K]]
}[keyof Values]
```
With [keyof Values] appended, we are creating an intermediary object that contains the values we want, but then remapping over it using its key.

This results in the union of tuples we wanted:

`['email', string] | ['firstName', string] | ['lastName', string]`, 




# Map an Object to a Union of Template Literals

We know that we need to start by using a mapped type to transform the object.

Since we are working with an object, we can use the keyof operator to get the keys and use them in the value:
```ts
type TransformedFruit = {
  [K in keyof FruitMap]: K
}
```
Hovering over the type declaration, we can see that our mapped type has the fruit as keys and values as we've seen in previous exercises:
```ts
// on hover
type TransformedFruit = {
  apple: "apple"
  banana: "banana"
  orange: "orange"
}
```
At this point we'll want to create the intermediary template literal representations. Like before, we'll use the key to access the values off of FruitMap via indexed access:
```ts
type TransformedFruit = {
  [K in keyof FruitMap]: `${K}:${FruitMap[K]}`
}
```
Now when we hover over TransformedFruit, we can see an object containing the template literals we want:
```ts
// on hover
type TransformedFruit = {
  apple: `apple:red`
  banana: "banana:yellow"
  orange: "orange:orange"
}
```
Now that we've reached an intermediary representation, we have all the pieces we need to transform this into the union we want.

We can append another mapped type containing the keys to the end of TransformedFruit. This will transform the object into a union:
```ts
type TransformedFruit = {
  [K in keyof FruitMap]: `${K}:${FruitMap[K]}`
}[keyof FruitMap]
```
Hovering over TransformedFruit now, we see that we have the union the challenge was asking for:

```ts
// on hover
type TransformedFruit = `apple:red` | `banana:yellow` | `orange:orange`
```
This technique is important to learn - it's extremely flexible and gives us a really solid mental model for iterating over unions.


# Iteratively Map and Remap to Transform Types

First, we need to transform a discriminated union into a union of template literals.

We can do this by using a mapped type to get each member of the union:

```ts
type TransformedFruit = {
  [F in Fruit as F["name"]]: F
}
```
Remember we can't actually use F as the key, since it must be a symbol, number, or string!

To work around this, we need to use the as clause to set the key to the fruit's name property value:

```ts
type TransformedFruit = {
  [F in Fruit as F["name"]]: F
}
```
With this in place, TransformedFruit now results in this object:

```ts
// on hover
type TransformedFruit = {
  apple: {
    name: "apple"
    color: "red"
  }
  banana: {
    name: "banana"
    color: "yellow"
  }
  orange: {
    name: "orange"
    color: "orange"
  }
}
```
All the pieces are in place to allow us to create the template literals.

To assemble the template literal, we'll use indexed access to get the properties we need off of F:

```ts
type TransformedFruit = {
  [F in Fruit as F["name"]]: `${F["name"]}:${F["color"]}`
}
```
Now when we hover over TransformedFruit, we can see our results:

```ts
// on hover
type TransformedFruit = {
  apple: "apple:red"
  banana: "banana:yellow"
  orange: "orange:orange"
}
```
The only thing left to do now is to take the name discriminator and remap the object with it by appending it at the end:

```ts
type TransformedFruit = {
  [F in Fruit as F["name"]]: `${F["name"]}:${F["color"]}`
}[Fruit["name"]]
```
We now are extracting each property of the object into a union:

```ts
// on hover
type TransformedFruit = "apple:red" | "banana:yellow" | "orange:orange"
```
This pattern of using iterators to create an object, remapping it to a different object, and then iterating over its values is extremely powerful!