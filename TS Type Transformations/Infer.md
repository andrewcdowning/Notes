# Infer

### Infer

```ts
import { Equal, Expect } from "../helpers/type-utils";

// Non infer solution
type GetDataValue<T> = T extends { data: any } ? T["data"] : never; 
// Infer solution
type GetDataValue<T> = T extends {data: infer TDATA} ? TDATA: never;

// Infer is inferining type for 'data' and returning it

type tests = [
  Expect<Equal<GetDataValue<{ data: "hello" }>, "hello">>,
  Expect<Equal<GetDataValue<{ data: { name: "hello" } }>, { name: "hello" }>>,
  Expect<
    Equal<
      GetDataValue<{ data: { name: "hello"; age: 20 } }>,
      { name: "hello"; age: 20 }
    >
  >,
];

```

The infer in T extends { data: infer TData } says "Whatever is passed in to the data key, infer its type".

Then, the infer declares TData for the true branch. If we try and access TData in the 'false' branch, we won't be able to. In other words, the TData variable is only defined for one branch.

GetDataValue in Action
To check our work, we can create a new Example type that passes {data: 1} to GetDataValue:

```ts
type Example = GetDataValue<{ data: 1 }>;
type Example = GetDataValue<{ data: 1 }>; 
```
This would extract the 1.

If we change GetDataValue to return 1 or undefined:

```ts
type GetDataValue = T extends { data: infer TData } ? TData | undefined : never;
type GetDataValue = T extends { data: infer TData }
  ?  TData | undefined
  : never;
```
Our Example type would show 1 | undefined when hovered over.


#### Infer arguments

Problem 
```ts
import { Equal, Expect } from "../helpers/type-utils";

interface MyComplexInterface<Event, Context, Name, Point> {
  getEvent: () => Event;
  getContext: () => Context;
  getName: () => Name;
  getPoint: () => Point;
}

type Example = MyComplexInterface<
  "click",
  "window",
  "my-event",
  { x: 12; y: 14 }
>;

type GetPoint<T> = unknown;

type tests = [Expect<Equal<GetPoint<Example>, { x: 12; y: 14 }>>];
```

Instead of tying into the structure of the interface, we should instead just look at its public declaration because it is less likely to change.

```ts
MyComplexInterface<any, any, any, any>
  ```
We can add infer to any of the slots in the declaration (where "slot" is anything between angle brackets). In this case, all of the slots above are any.

Because Point was the fourth argument, we can replace the corresponding slot with TPoint then return it for the matching conditional branch:
```ts
type GetPoint<T> = T extends MyComplexInterface<any, any, any, infer TPoint> ? TPoint : never;
type GetPoint<T> = T extends MyComplexInterface<any, any, any, infer TPoint>
  ? TPoint
  : never;
```
This approach gives us all of the benefits of being able to extract out all the members of the type arguments without needing to dive deep into the element itself to understand its structure.

You could add infer for the other slots, too, but in this case GetPoint is only interested in TPoint.

Being able to extract type arguments to another type helper is another really cool feature of infer.


### Infer type literals

```ts
import { Equal, Expect } from "../helpers/type-utils";

type Names = [
  "Matt Pocock",
  "Jimi Hendrix",
  "Eric Clapton",
  "John Mayer",
  "BB King",
];

// Solution 1
type GetSurname<T extends string> = S.Split<T, " ">[1] 

// Infer solution
type GetSurname<T> = T extends `${infer First} ${infer Last}`? Last: never;

type tests = [
  Expect<Equal<GetSurname<Names[0]>, "Pocock">>,
  Expect<Equal<GetSurname<Names[1]>, "Hendrix">>,
  Expect<Equal<GetSurname<Names[2]>, "Clapton">>,
  Expect<Equal<GetSurname<Names[3]>, "Mayer">>,
  Expect<Equal<GetSurname<Names[4]>, "King">>,
];
```
The better solution is to use infers inside of a template literal:

```ts
type GetSurname<T> = T extends `${infer First} ${infer Last}` ? Last : never 
```
Here, we use the conditional check to check that T is a string with two strings with a space between, then infer the strings in those slots.

Note that since the challenge only asked for the last name, we don't really need to use infer First.

We could just use:
```ts
`${string} ${infer Last}`
```
Pattern matching with template literals defines a clear capture group while providing more semantic meaning to others reading our code.


### Infer return type from async function
Here’s what the solution looks like:

```ts 
type InferPropsFromServerSideFunction<T> = T extends () => Promise<{ props: infer P }> ? P : never
```


First, we want T to extend a function that returns a Promise.

Inside the Promise is an object that will contain the props. Then we infer those props P. Then we return P or never if there aren’t any.

This is very cool - you can create any structure you like, and stick an infer inside it to only infer the element you care about.

This pattern is useful when working with functions where you want to extract something but you might not have access to its internals or you don’t want to declare a type annotation for.

### Infer from several possible function shapes

The first solution is to write a long conditional where you infer the result from each possible function:
```ts
type GetParserResult<T> = T extends {
  parse: () => infer TResult
}
  ? TResult
  : T extends () => infer TResult
  ? TResult
  : T extends {
      extract: () => infer TResult
    }
  ? TResult
  : never
```
This makes the tests pass, but it's a fairly ugly solution.

Solution 2 - Use a Union Type
The preferred solution is to use a union type instead.

We can say that T extends either an object with parse, an object with extract, or a function. Each of these branches has its own infer TResult.

If T extends any of the types, infer its TResult, otherwise return never:

```ts
type GetParserResult<T> = T extends
  | {
      parse: () => infer TResult
    }
  | {
      extract: () => infer TResult
    }
  | (() => infer TResult)
  ? TResult
  : never
```
Using a union type gives us the same expressiveness as the ternary solution, but is much more readable.


### Using a Generic to avoid distributive conditional types
For reference, here is the failing code from the problem:

type Fruit = "apple" | "banana" | "orange" type AppleOrBanana = Fruit extends "apple" | "banana" ? Fruit : never
type Fruit = "apple" | "banana" | "orange"

type AppleOrBanana = Fruit extends "apple" | "banana" ? Fruit : never
As mentioned, there are two ways to solve this challenge.

Using a Generic
The distributed conditional types problem can be solved by using a generic.

Consider the solution code below:

type GetAppleOrBanana<T> = T extends "apple" | "banana" ? T : never type AppleOrBanana = GetAppleOrBanana<Fruit>
type GetAppleOrBanana<T> = T extends "apple" | "banana" ? T : never

type AppleOrBanana = GetAppleOrBanana<Fruit>
We create a new GetAppleOrBanana that accepts T. If T extends apple or banana, return T. Otherwise, return never.

Then we update AppleOrBanana to be GetAppleOrBanana, passing in Fruit.

This technique will work even when changing the Fruit type to include other options.

It works because when you pull a union into a generic like T, the members of the union distribute across it.

In other words, T will become each individual member of the union, and the conditional type will iterate over each one.

Check Your Understanding
Read the following code, and think through what is happening with T:

type Fruit = "apple" | "banana" | "orange"; type GetAppleOrBanana<T> = T extends "apple" | "banana" ? T : never
type Fruit = "apple" | "banana" | "orange";
type GetAppleOrBanana<T> = T extends "apple" | "banana" ? T : never
This line of code checks if T is either "apple" or "banana", and will return it if it is.

Think of it as iterating through the members of the union type to find what matches.

Now compare that solution to the original problem:

type Fruit = "apple" | "banana" | "orange"; type AppleOrBanana = Fruit extends "apple" | "banana" ? Fruit : never;
type Fruit = "apple" | "banana" | "orange";

type AppleOrBanana = Fruit extends "apple" | "banana" ? Fruit : never;
In the problem code, we're considering the entire Fruit union itself.

Because "apple" | "banana" | "orange" is not the same as "apple" | "banana", the code will return never.

This issue can really catch you off-guard if you aren't aware of it!

Solving with infer
It's also possible to solve this problem without a generic context:

type AppleOrBanana = Fruit extends infer T ? T extends "apple" | "banana" ? T : never : never
type AppleOrBanana = Fruit extends infer T
  ? T extends "apple" | "banana"
    ? T
    : never
  : never
Here we create a conditional type with Fruit extends infer T.

This infers what's inside of Fruit, and treats it as an iterable because it is now within a generic context!

With T acting as the iterable, the individual members of Fruit are checked similarly to the solution above.

While this behaviour is useful, you'll usually be able to wrap things in a type helper and have it just work.