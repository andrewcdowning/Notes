# Singleton Pattern

### Description:
A singleton is an object that is created one time.  The singleton pattern creates only one instance and provides a global access point to the object.  Use with caution.

#### Practical uses:
You can use to set aws clients like boto3.Resource('s3') and reuse the client across lambda invocations as long as it is outside the event handler.  

#### Example:

``` javascript

class SingletonResource {
  private static instance: Singleton;
  
  /**
   * the constructor should be private in langagues that support it
   * In langagues that dont it needs to stay hidden or documented not to 
   * invoke the constructor directly
   */
  private construtor(){};
  
  /**
   * returns the instance if it exists else creates the instance
   */
  public static getInstance(): Singleton {
      if (!Singleton.instance) {
          Singleton.instance = new Singleton();
      }

      return Singleton.instance;
  }
   /**
   * This method should impliment any actions you need to take on the instance
   */
   public logicToPerfomOnInstance() {
     ...
   }
}

```