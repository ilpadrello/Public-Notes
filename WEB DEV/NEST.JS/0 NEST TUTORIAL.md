Using Nest.js is a good idea because it let you use the power of a framework and help you not write bad code.
Nest.js is actually prepared to be worked in TS and  not plain JS. The framework will use decorators and other stuff that exists only in TS.

## NEST CLI
Nest CLI Tool is a key part of the framework letting you create stuff easily without having to remember how to name stuff etc.

### Install the CLI : 
`npm install -g @nestjs/cli`

### Creating the project
`nest new your-project-name`
This will create a new project with all the boilerplate files inside.

## DECORATORS
QuickAnswer: Decorator add information about the class, method, property you want to create: for example you can use `@Controller` to specify that is a controller or `@Injectable`, `@Get`, `@Body`, `@Param`
Decorators are very important part of the framework, they serve a specific purpose, but before diving in the "how to use them" let's just remember the "what are them".

**Decorators** are functions that are called at runtime, once per decorator, with metadata about the class/method/function/property attached to the decorator.

For example, a class decorator is called only when the class is evaluated, this happens once not every time you create an instance.

Here's an example : 

```typescript
function Log(target: any, key: string, descriptor: PropertyDescriptor) {  
	const original = descriptor.value;  
	descriptor.value = function (...args: any[]) {  
		console.log(`Calling ${key} with`, args);  
		return original.apply(this, args);  
	};  
}  
  
class Example {  
	@Log  
	sayHello(name: string) {  
	console.log(`Hello ${name}`);  
	}  
}  
  
const e = new Example();  
e.sayHello('Alice');  
// Console:  
// Calling sayHello with ['Alice']  
// Hello Alice
```

If you don't see a use for the moment is absolutely normal, the motivation for creating them was to answer the question : 
“How can frameworks add functionality like routing, dependency injection, validation, logging, etc., **without forcing developers to write tons of boilerplate code**?"
#### Why call them “decorators”?
Originally, “decorator” in programming meant: **something that adds behavior or metadata to a function, class, or object without changing the original code directly**.
Python has decorators too, which “wrap” a function, Typescript borrowed the term, because decorators **decorate a class/method/property with extra behavior or metadata**.

> Yes, in NestJS they’re way more important than just metadata—they often **change the runtime behavior** (like injecting services, defining routes, etc.). But conceptually, they still “decorate” the class/method with additional capabilities.

### NEST Decorators
In NestJS decorators are very important because they let the framework understand what is the class meant to do and do some magic behind the scene.
For example you can specify if a class is a `@Controller`, you can specify if a method if for a `@Get` or `@Post` request etc.
The framework than will know how to act accordingly and will Dependency Inject the needed stuff etc.

Here's an example : 

```Typescript
@Module({
	imports: [],
	controllers: [AppController],
	providers: [AppService],
})
export class AppModule {}
# The module decorator is applyed to the AppModule class
```

## MODULES
Nest.JS strongly recommend that you organize your code with modules. 
A **Module** is a way to organize your application into boundaries
Every `NestJS` app has only one `rootModule` and that module can use other modules, that can also use other modules as well

Each module will encapsulate a set of related capabilities, and it can be a particular feature or a domain.

For example, in a nest nest app, `AppModule` is the `rootModule` and we know this because is the class that is used to create the app.
We can then create a lot of other modules, like a `configModule` that will be in charge of the configuration etc.

### Create a new Module using CLI
Let's create a new config module: You create a new module using the CLI to make it easy.
`nest generate module config`

This will create a new folder called `config` in the `src` folder with inside a `config.module.ts` and also will add the imports necessary in the root module (``app.module.ts``)

```typescript
import { ConfigModule } from './config/config.module';

@Module({
	imports: [ConfigModule],
	controllers: [AppController],
	providers: [AppService],
})
export class AppModule {}
```

**IMPORTANT** : The import statement is where you define how modules are organized.
For example, you can remove the import from the `app.module.ts` to use it from within another module that you may need. This will create a tree graph of uses of the modules.

**IMPORTANT** : Modules can be only classes.

## CONTROLLERS
As for pretty much any standard JS application, `NestJS` uses controllers to handle HTTP requests. 
A controller is simply a class that is decorated with the `@controller` decorator, and that is why when creating a new project, the `app.controller.ts` file has a `@controller` decorator inside : 

```typescript
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
	constructor(private readonly appService: AppService) {}
	@Get()
	getHello(): string {
		return this.appService.getHello();
	}
}
```

Of course you can generate a new controller using the CLI : 
`nest generate controller hb`

## PROVIDERS
Nearly everything in `NestJS` is a provider.
A **Provider** is the most fundamental concept in NestJS. It is an umbrella term for anything that can be **injected** as a dependency.

## SERVICES
A Service is just the most common _type_ of Provider. When you run `nest g service`, it generates a class with the `@Injectable()` decorator. It’s called a Service because its job is to handle business logic.

