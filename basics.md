### Introduction to TypeScript basics

#### Types

There are two kinds of types present in typescript, they are <strong>Primitive Types</strong> and <strong>Object Types</strong>

##### Primitive Types

The primitive types available in TS are basically all the primitive types available in JS with one added type called void. The different types are:

1. number
2. boolean
3. string
4. null
5. undefined
6. void (not present in JS)
7. symbol
8. unknown (something we're not sure about maybe something that comes from an external API and could be anything)
9. any - bad bad type, don't use!

##### Object Types

These are any types that either developers create or the ones that the language gives us. like Classes, Functions, Objects and Arrays etc.

Special Object Types

1. Tuples
2. Enums

#### Type Annotations & Type Inference

<strong>Type Annotations</strong> are to tell TS what type we're working with. On the other hand <strong>Type Inference</strong> is when TS identifies and understands which type we're using by looking at the assigned value (this only works when we add the value in the same line) without us having to explicitely specify the type using annotations.

```typescript
//primitive types
let num : number = 5;
const name: string = 'Preetham';

//built in objects
let now: Date = new Date();

//arrays
let colors: string[] = ['hello', 'world];

//ES6 Classes
class Person {
    name: string;
    constructor(name){
        this.name = name;
    }
 }

const person: Person = new Person('Stuti');

//Object Literals
const coordinate: {x: number; y: number } = {x: 10, y:20};

//functions
const logNumber: (i: number) => void = (i: number) => {
    console.log(i);
}
```

#### The any Type

The any type means TS has no idea what the type is, it could be anything. However, avoid using the any type at all costs to initialize variables.

#### Annotations vs Inference

While we can depend on inference in general, but we want to use Type Annotations explicitely in one of the below 3 case:

1. When we use a function built-in or otherwise that returns an 'any' type
2. Delayed Initialization .i.e. When we declare a variable but don't initialize it in the same line
3. When types cannot be inferred correctly.

Example:

```typescript
/*
In the below example, by default we want the numberAboveZero variable to be false, 
however whenever we come across a number that is greater than 0 in the numbers array, 
we assign that number to the numberAboveZero variable. Instead of depending TS' 
type inference we can use a pipe operator to specify via annotation that numberAboveZero 
can either be a number or a boolean
*/

let numbers = [-10, -1, 12];
let numberAboveZero: boolean | number = false;

for (let number of numbers) {
  if (nuber > 0) numberAboveZero = numbers[i];
}
```

#### Type Inference around Functions

Type Inference only works with the return values from functions. We still need to annotate the parameters.

```typescript
//Inferred Return Type
const add = (a: number, b: number) => a + b;

//Annotated Return Type
const add: (a: number, b: number) => number = (a: number, b: number) => a + b;
```

<strong>Anonymous Functions</strong> also follow the same rules, also Function declarations and Function expressions.

#### void & never

```typescript
//void
const logger: (message: string) => void = (message: string): void => {
  console.log(message);
};

//never
const throwError = (message: string): never => {
  throw new Error(messsage);
};
```

#### Destructuring Objects with Type Annotations

```typescript
const profile = {
  name: 'alex',
  age: 20,
  coords: {
    lat: 0,
    lng: 15,
  },
  setAge(age: number): void {
    this.age = age;
  },
};

//destructure age
//Annotating not required, but this is how it's done
const { age }: { age: number } = profile;
//destructuring lat and lng
const {
  coords: { lat, lng },
}: { coords: { lat: number; lng: number } } = profile;
```

#### Types with Arrays

```typescript
//Need annotation when array is empty at inilitiazation
const carManufacturers: string[] = [];

//Can depend on inference when array has data at initialization
const dates = [new Date(), new Date()];

//Same for 2D Arrays
//need annotations
const carsByMake: string[][] = [];

//can depend on inference
const carsByMake2 = [['f150'], ['corolla'], ['swift']];
```

We can depend on type inference while trying to pull out elements from a string using pop(), element selection by index etc. as TS already knows the type of data present in Arrays.

##### Corner Case

Arrays can still hold multiple types of values by using pipes

```typescript
const importantDates: (Date | string | number)[] = [new Date(), 1, 'hello'];
```

#### Tuples

Tuples are similar to arrays. They allow us to express an array with a fixed number of elements who's types and sequence are known but values may change.

In general, it's better to use Objects instead of Tuples in most cases

```typescript
//We add the annotation to specify that this isn't an array but is a tuple
const pepsi: [string, boolean, number] = ['brown', true, 40];

//Alternatively, we can also use a Type Alias for the annotation

type Drink = [string, boolean, number];

const sprite: Drink = ['clear', false, 0];
```

#### ENUMS

enum stands for enumeration. They're similar to enums from Java and other programming languages. They're just sets of numeric values but with better names.

```typescript
enum Color {
  Red, //0
  Green, //1
  Blue, //2
}
let c: Color = Color.Green; //will give us 1

//By default enums are based, but we can change that by manually setting the first element's value as 1 using the = assignment operator
enum Color {
  Red = 1,
  Green,
  Blue,
}

let c: Color = Color.Green; //will give us 2

//Alternatively we can also have all elements various different numbers assigned.
enum Color {
  Red = 1000,
  Green = 15,
  Blue = 100,
}
```

#### Interfaces

TS interfaces are similar to what we have in other strongly typed programming languages such as Java and C#.

```typescript
interface Vehicle {
  name: string;
  year: number;
  broken: boolean;
}

const oldCivic = {
  name: 'civic',
  year: 2000,
  broken: true,
};

const printVehicle = (vehicle: Vehicle): void => {
  console.log(`Name: ${vehicle.name}, Year: ${vehicle.year}`);
};

printVehicle(oldCivic); //This works due to the interface, otherwise we'd have to annotate with an object literal in the method def
```

<strong>Important Note:</strong>We can have both primitive types as well as builtin-types and or objects, functions, types inside interfaces, for example:

```typescript
interface Reportable {
  summary(): string;
}

const oldCivic = {
  name: 'civic',
  year: new Date(),
  broken: true,
  summary(): string {
    return `Name: ${this.name}`;
  },
};

const printSummary = (item: Reportable): void => {
  console.log(item.summary());
};

printSummary(oldCivic);
```

#### Classes

Classes in TS are the same as ES6 classes. Everything that applies to ES6 classes applies to TS classes. However TS provides us access modifiers:

- <strong>public</strong> - can be called anywhere
- <strong>private</strong> - can be called by other methods in the class
- <strong>protected</strong> - can be called by methods from same class or child chasses

```typescript
class Vehicle {
  drive(): void {
    console.log('chugga chugga');
  }

  vehicle = new Vehicle();
  //or
  vehicle: Vehicle = new Vehicle();
}
```
