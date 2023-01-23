Object vs Data Container
---
#### Objects
* Objects hide internals and expose public API
* Contain business logic
* Uses abstractions over concretion 
#### Data Containers
* No API
* Public internals/ properties
* Store and transport data
* Concretion only

Polymorphism
Objects with same methods that change behavior based on how they are created

Clean Classes
* Should be small and focused
* Single responsibility - Related to ONE thing

Cohesion - How much are the class methods using the class properties
classes should be highly cohesive 


Law of Demeter - Tell not ask

Avoid `this.customer.lastPurchaseDate`
Principle of least knowledge - Do not depend on the internals of "strangers" other objects you don't know

Code in methods may only access direct internals ( props )
- the object it belongs to
- object that stored in properties of that object
- objects that are received by that object
- objects that are created by that object
