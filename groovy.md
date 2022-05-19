## Groovy
Apache Groovy is an object oriented and Java syntax compatible programming language built for the Java platform. Groovy source code gets compiled into Java Bytecode so it can run on any platform that has JRE is installed.

### Advantages
- Groovy is an agile and dynamic language
- Groovy is simple for Java developers since the syntax for Java and Groovy are very similar.
- Seamlessly integration with all existing Java objects and libraries
- You can use it as much or as little as you like with Java apps

Basic syntax, data types, variables are as same as Java

### Methods
```groovy
def methodName() { 
   //Method code 
}
def someMethod(parameter1, parameter2 = 0, parameter3 = 0) {
    // Method code goes here 
} 
```
### Range
```groovy
 def rint = 1..10; 
```
### List
```groovy
[11, 12, 13, 14]
[‘Groovy’, 21, 2.11]
```
### Maps
```groovy
[‘TopicName’ : ‘Lists’, ‘Author’ : ‘Raghav’]
```
### Regex
```groovy
def regex = ~'Groovy'
```
### Class
```groovy
class Student {
   int StudentID;
   String StudentName;
	
   static void main(String[] args) {
      Student st = new Student();
      st.StudentID = 1;
      st.StudentName = "Joe"     
   } 
}
```
### Traits
Traits are a structural construct of the language which allow
- Composition of behaviors.
- Runtime implementation of interfaces.
```groovy
trait Marks {
   void DisplayMarks() {
      println("Marks1");
   } 
} 

trait Total extends Marks {
   void DisplayMarks() {
      println("Total");
   } 
}  

class Student implements Total {
   int StudentID 
}
```
### Closure
A closure is a short anonymous block of code.

```groovy
def clos = {param->println "Hello ${param}"};
```
### Annotations
Annotations are mainly used for the following reasons
- Information for the compiler 
- Compile-time and deployment-time processing 
- Runtime processing 
### Shell
The Groovy shell known as groovysh can be easily used to evaluate groovy expressions, define classes and run simple programs.
