## Typescript 
TypeScript is JavaScript for application-scale development.
TypeScript is just JavaScript, compiled to JavaScript. TypeScript supports other JS libraries.

JavaScript wasn’t even able to fulfill the requirement of an Object-Oriented Programming language. So TypeScript was created by the development team to bridge of succeeding at the enterprise level as a server-side technology.

### TypeScript and Object Orientation
TypeScript is Object-Oriented JavaScript that follows real-world modelling.

### Data types
- number
- string
- boolean
- void
- null
- undefined

### Variables

`var name:string = ”mary”`

`var name:string;`

`var name = ”mary”`

`var name;

### Functions
```typescript
function function_name (param1[:type], param2[:type], param3[:type])
```
```typescript
function addNumbers(...nums:number[]) {  }
addNumbers(1,2,3)
addNumbers(10,10,10,10,10)
```
#### Anonymous Function
````typescript
var res = function( [arguments] ) { ... }
````
#### Lambda Functions
```typescript
( [param1, parma2,…param n] )=>statement;
```
#### Array
```typescript
var array_name[:datatype]; //declaration
array_name = [val1,val2,valn..] //initialization
```
#### Tuple
At times, there might be a need to store a collection of values of varied types.
```typescript
var tuple_name = [value1,value2,value3,…value n]
```
#### Union
Union types are a powerful way to express a value that can be one of the several types.
```typescript
Type1|Type2|Type3
var val:string|number
val = 12 
```
#### Interface
```typescript
interface interface_name {
}
interface IPerson {
    firstName:string,
    lastName:string,
    sayHi: ()=>string
}

var customer:IPerson = {
    firstName:"Tom",
    lastName:"Hanks",
    sayHi: ():string =>{return "Hi there"}
} 
```
#### Class
A class in terms of OOP is a blueprint for creating objects
```typescript
class class_name {
//class scope
}
class Car {
    //field 
    engine:string;

    //constructor 
    constructor(engine:string) {
        this.engine = engine
    }

    //function 
    disp():void {
        console.log("Engine is  :   "+this.engine)
    }
}
```
#### Object
```typescript
var object_name = {
key1: “value1”, //scalar value
key2: “value”,
key3: function() {
//functions
},
key4:[“content1”, “content2”] //collection
};
```
#### Namespace
A namespace is a way to logically group related code.
```typescript
namespace SomeNameSpaceName { 
   export interface ISomeInterfaceName {      }  
   export class SomeClassName {      }  
}
```

