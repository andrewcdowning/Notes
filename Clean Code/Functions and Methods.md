# Functions and Methods

### Function Parts
- Functions should be small and do one thing
- calling should be easy
- body should be readable
- minimize number of params
- none 1 or 2 is best, 3 to 7 avoid 

### Refactoring

- 2 or more params
  - consider a map or object as an input
- avoid output arguments as input

### levels of abstraction

Functions should do work one level of abstraction below there name
- different operations or level of abstraction, high level to low level
Functions can initiate multiple functions 

### When to split 
- extract code that works on the same functionality
- requires more interpritation than surround code.
- DRY - Don't repeat yourself
- Don't over do it
  - avoid useless abstraction
  - be reasonable and use common sense
  - being granular may not always lead to more readable code
#### Don't split if
- just renaming
- the new function will take longer than reading the extracted code
- can not produce a reasonable name for the extracted code