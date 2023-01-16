# Factory Pattern

### Description
The factory is a creational design pattern meant to abstract the creation of objects away from the client code. Typically, a factory would expose static class methods to act as an interface for creating new concrete objects.  This is valuable when creating many objects with the same base class type and can be overridden easily to create different objects with the same base class. 

#### Uses

#### Example
``` javascript
export interface IThing {
  name: string;
  someMethod(): void
}


class ConcreteThing1 impliments IThing {
  name: string;
  someMethod() {}
}
  
class ConcreteThing1 impliments IThing {
  name: string;
  someMethod() {}
}
  
class ThingFactory {
  
  create(thing: string, name: string) {
    if (thing === 'type1') {
      return new ConcreteThing1(name);
    }else if (thing === 'type1') {
      return new ConcreteThing2(name);
    }
    someOtherLogic(){};
  }
