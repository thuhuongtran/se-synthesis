## Angular
### What is Angular
Angular is a development platform built on Typescript.
Angular platform includes:
- A component-based framework
- Integrated libraries
- Tools to develop, build, test, update your code.
### Essentials - Angular core ideas
#### Components
A component is a building blocks.
A component includes:
- A Typescript class with `@Component()`
- css, html files
```angularjs
import { Component } from '@angular/core';

@Component({
  selector: 'hello-world',
  template: `
    <h2>Hello World</h2>
    <p>This is my first component!</p>
  `
})
export class HelloWorldComponent {
  // The code in this class drives the component's behavior.
}
```
```angular2html
<hello-world></hello-world>
```
#### Templates
Every component has an HTML template that declares how that component renders.
Angular automatically updates the rendered DOM when your component's state changes.

Dynamic text
```angular2html
<p>{{ message }}</p>
```
Angular also supports property bindings, to help you set values for properties and attributes of HTML elements.
```angular2html
<p
  [id]="sayHelloId"
  [style.color]="fontColor">
  You can set my color in the component!
</p>
```
Declare event listeners to listen for and respond to user actions:
```angular2html
<button
  type="button"
  [disabled]="canClick"
  (click)="sayMessage()">
  Trigger alert message
</button>
```
#### Dependency injection
Declare dependency injection service class to use its functions in other classes.

Add `@Injectable` annotation in the service class
```angularjs
import { Injectable } from '@angular/core';

@Injectable({providedIn: 'root'})
export class Logger {
  writeCount(count: number) {
    console.warn(count);
  }
}
```
Add `private logger: Logger` to the constructor.
```angularjs
import { Component } from '@angular/core';
import { Logger } from '../logger.service';

@Component({
  selector: 'hello-world-di',
  templateUrl: './hello-world-di.component.html'
})
export class HelloWorldDependencyInjectionComponent  {
  count = 0;

  constructor(private logger: Logger) { }

  onLogMe() {
    this.logger.writeCount(this.count);
    this.count++;
  }
}
```
#### Angular CLI
The Angular CLI is the fastest, straightforward, and recommended way to develop Angular applications.

- `ng build`	Compiles an Angular app into an output directory.
- `ng serve`	Builds and serves your application, rebuilding on file changes.
- `ng generate`	Generates or modifies files based on a schematic.
- `ng test`	Runs unit tests on a given project.
- `ng e2e`	Builds and serves an Angular application, then runs end-to-end tests.
#### Angular libraries
- Router
- Forms
- HttpClient
- Animations
- PWA
- Schematics
#### Deploying
```shell
npm install -g @angular/cli
```
**Run locally**
```shell
npm install
ng serve
```
**Build**
```shell
ng build
```
### Angular in details
#### Components
**Lifecycle**

A component instance has a lifecycle that starts when Angular instantiates the component class and renders the component view along with its child views.

The lifecycle ends when Angular destroys the component instance and removes its rendered template from the DOM. 

**Lifecycle sequence**

1. `ngOnChanges()`:  Respond when Angular sets or resets data-bound input properties. Called  and whenever one or more data-bound input properties change.

2. `ngOnInit()`: Initialize the directive or component after Angular first displays the data-bound properties and sets the directive or component's input properties. Called once, after the first `ngOnChanges()`

3. `ngDoCheck()`: Called immediately after `ngOnChanges()` on every change detection run, and immediately after `ngOnInit()` on the first run. Detect and act upon changes that Angular can't or won't detect on its own. 
4. `ngAfterContentInit()`: Called once after the first `ngDoCheck()`. Respond after Angular projects external content into the component's view.
5. `ngAfterContentChecked()`: Called after `ngAfterContentInit()` and every subsequent `ngDoCheck()`.
6. `ngAfterViewInit()`: Respond after Angular initializes the component's views and child views. Called once after the first `ngAfterContentChecked()`.
7. `ngAfterViewChecked()`: Respond after Angular checks the component's views and child views.
8. `ngOnDestroy()`: Cleanup just before Angular destroys the directive or component.

**Sharing data between parents and childs**

Using `@Input()` and `@Output()`
#### Template syntax
**Text interpolation**

By default, interpolation uses the double curly brace ({{ and }}) characters as delimiters.
```angular2html
<p>{{title}}</p>
```

**Template statements**
To respond to user events.

Event in parentheses equals to function name in quotes
```angular2html
(event)="statement"
```

**Property binding**

To bind to an element's property, enclose it in square bracket `([])` characters.

```angular2html
<img alt="item" [src]="itemImageUrl">
```

**Two-way binding**

Use two-way binding to listen for events and update values simultaneously between parent and child components.

Angular's two-way binding syntax is a combination of square brackets and parentheses, `[()]`.

```angular2html
<app-sizer [(size)]="fontSizePx"></app-sizer>
```

#### Built-in attribute directives
- `NgClass`	Adds and removes a set of CSS classes.
- `NgStyle`	Adds and removes a set of HTML styles.
- `NgModel`	Adds two-way data binding to an HTML form element.

#### Routing library
Defining a basic route

1. Import `RouterModule` and `Routes` into your routing module.

```angularjs
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router'; // CLI imports router

const routes: Routes = []; // sets up routes constant where you define your routes

// configures NgModule imports and exports
@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

2. Define your routes in your `Routes` array.
```angularjs
const routes: Routes = [
  { path: 'first-component', component: FirstComponent },
  { path: 'second-component', component: SecondComponent },
];
```

3. Add your routes to your application.
```angular2html
 <li><a routerLink="/first-component" routerLinkActive="active">First Component</a></li>
```

#### HttpClient library
Communicating with backend services using HTTP.

Before you can use `HttpClient`, you need to import the Angular `HttpClientModule`

#### Testing library
Setup testing
```shell
ng test
```
Find out how much code you're testing
```shell
ng test --no-watch --code-coverage
```

#### Angular Internationalization library
Localization is the process of building versions of your project for different locales. The localization process includes the following actions.

- Extract text for translation into different languages
- Format data for a specific locale