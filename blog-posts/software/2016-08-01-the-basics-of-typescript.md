# The Basics of TypeScript

### Looking Forward to New Challenges

Recently, I started working for a new client who uses [TypeScript](https://www.typescriptlang.org/) for both client and server side development. I had previously worked with [Coffeescript](http://coffeescript.org/) and learning how to use TypeScript was something that I was definitely looking forward to do. There has been a lot of buzz about TypeScript in the JavaScript community; [Angular 2](https://angular.io/) is actually written in TypeScript (even though you don’t have to use it to write Angular 2 applications).

In this blogpost, my goal is to give you a brief introduction to TypeScript and hopefully convince you to use it for your next big project.

### Why TypeScript?

[TypeScript is a typed superset of JavaScript that compiles to plain JavaScript](https://www.typescriptlang.org/) and can be run in any web browser or in a server in a NodeJS application. TypeScript offers support for the latest and evolving JavaScript features, including those from ECMAScript 2015. Since TypeScript is a superset of Javascript, it is extremely similar and it is very easy for JavaScript developers to learn how to use it. Static typing makes code safer as it helps to create applications with fewer bugs, make more predictable code, and most of the time easier to debug. TypeScript also enables faster development as IDEs and text editors that support Typescript allow to examine methods and properties in custom types. As if all of that wasn't  just enough, the TypeScript compilation step catches all kinds of errors before they reach runtime and break something.

### Static Typing and Functions

TypeScript allows to define the type of variables and functions and the compiler will make sure they aren't assigned to other types in the application (if you are using and IDE or text editor with support for TypeScript, you will be warned immediately as you are writing your code):

#### Variable declaration example:
``` ts
  // 'dayOfTheWeek' must be of type string
  let dayOfTheWeek: string = 'Monday';

  // Warning: Assigned expression type number is not assignable to type string
  dayOfTheWeek = 1;
```

#### Function declaration example:
``` ts
  // 'sumAllNumbers' takes an array of numbers and must return a number
  function sumAllNumbers(numbers: number[]): number {
    return numbers.reduce((prev: number, curr: number) => prev + curr);
  }
  // 'numbers' is an array of numbers
  const numbers: number[] = [1, 2, 3];
  const totalSum: number = sumAllNumbers(numbers);
  // TypeScript supports many ES6 features including template literals:
  console.log(`The total sum is ${totalSum}`); // The total sum is 6
```

As another option, you can leave out the type declaration and TypeScript will infer it for you and check if any further declaration follow the initial type of the variable. TypeScript types are only useful during development and later on completely removed when compiled to Javascript.

### Interfaces and Classes
TypeScript allows to define interfaces which are the equivalent of a contract of what shape will an object have. Interfaces are useful for defining function parameters, return types, and implementing classes. A class that implements an interface must provide the code for all of the required methods and properties of the interface. Interfaces definition are completely removed when compiled to Javascript; these are only useful in the development stage.

In the following example, I show a simple snippet of how to use both interfaces and classes with TypeScript:

``` ts
// Enums are useful for defining a finite
// number of values that can be used
enum Gender {Male, Female};

// Whoever uses this interface must implement
// all of its properties and methods
interface IAnimal {
  name: string
  gender: Gender
  age: number
  whoAmI: () => void
}

// The 'Person' class implements the
// 'IAnimal' interface
class Person implements IAnimal {

  name: string;
  gender: Gender;
  age: number;

  constructor(name: string, gender: Gender, age: number) {
    this.name = name;
    this.gender = gender;
    this.age = age;
  }

  public whoAmI(): void {
    console.log(`${this.name} - ${Gender[this.gender]} - ${this.age}`);
  }
}

let aPerson = new Person('Diego Castillo', Gender.Male, 24);

aPerson.whoAmI(); // Diego Castillo - Male - 24
```

As you might have noticed, the syntax is very similar to classes in ECMAScript 2015, except that types make them more robust.

### Type Definitions
Type definitions are declaration files to make JavaScript libraries compatible with TypeScript. Type definition files have the extension ``.d.ts`` and mainly consist of interfaces that help the TypeScript compiler type check the application code. These files do not contain any implementation details of the library they define the types for.

[DefinitelyType](https://github.com/DefinitelyTyped/DefinitelyTyped) is probably the largest repository of type definitions right now, but there are others such as NPM packages.

Type definitions can be installed manually, but for larger projects it is recommended to use a type definitions manager tool. A very popular tool for managing type definitions nowadays is [typings](https://github.com/typings/typings). Typings can resolve type definitions to the Typings Registry, GitHub, NPM, Bower, HTTP and local files. Here's a quick example of how to use the typings library:

``` bash
# Search for the type definitions of a particular library
typings search react-dom

# Install one of the definitions found (dt = DefinitelyType)
typings install --global --save dt~react-dom
```

As you can see, installing and managing type definitions is very easy and similar to managing packages with NPM.

### Conclusion
I hope this short blogpost has given you enough information to be excited about trying out TypeScript. I expect TypeScript usage to keep increasing, as current large and complex applications need more tools like this which enforce developers to follow consistent rules all across an application. Finally, I you are interested in learning more about TypeScript, I highly recommend the course
[TypeScript In-depth](https://www.pluralsight.com/courses/typescript-in-depth) from Pluralsight (this course has been incredible helpful to me while learning more about TypeScript).
