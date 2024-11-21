# NestJS Interview Questions & Answers

### Table of Contents

<!-- TOC_START -->
| No. | Questions |
| --- | --------- |
| 1 | [What is NestJS](#what-is-nestjs) |
| 2 | [What is under the hood of NestJS](#what-is-under-the-hood-of-nestjs) |
| 3 | [Project structure in NestJS](#project-structure-in-nestjs) |
| 4 | [Controllers in NestJS](#controllers-in-nestjs) |
| 5 | [How to create a CRUD controller with validation built-in in NestJS](#how-to-create-a-crud-controller-with-validation-built-in-in-nestjs) |
| 6 | [Routing in NestJS](#routing-in-in-nestjs) |
| 7 | [What is @Get decorator?](#what-is-get-decorator) |

<!-- TOC_END -->

<!-- QUESTIONS_START -->
1. ### What is NestJS

Nest (NestJS) is a framework for building efficient, scalable Node.js server-side applications. It uses progressive JavaScript, is built with and fully supports TypeScript (yet still enables developers to code in pure JavaScript) and combines elements of OOP (Object Oriented Programming), FP (Functional Programming), and FRP (Functional Reactive Programming).

**[⬆ Back to Top](#table-of-contents)**

2. ### What is under the hood of NestJS
Under the hood, Nest makes use of robust HTTP Server frameworks like Express (the default) and optionally can be configured to use Fastify as well!
Nest provides a level of abstraction above these common Node.js frameworks (Express/Fastify), but also exposes their APIs directly to the developer. This gives developers the freedom to use the myriad of third-party modules which are available for the underlying platform.

**[⬆ Back to Top](#table-of-contents)**

3. ### Project structure in NestJS
The project-name directory will be created, node modules and a few other boilerplate files will be installed, and a src/ directory will be created and populated with several core files.

```app.controller.ts``` - A basic controller with a single route.

```app.controller.spec.ts``` - The unit tests for the controller.

```app.module.ts``` - The root module of the application.

```app.service.ts``` - A basic service with a single method.

```main.ts``` - The entry file of the application which uses the core function NestFactory to create a Nest application instance.

**[⬆ Back to Top](#table-of-contents)**

4. ### Controllers in NestJS

Controllers are responsible for handling incoming requests and returning responses to the client.

A controller's purpose is to receive specific requests for the application. The routing mechanism controls which controller receives which requests. Frequently, each controller has more than one route, and different routes can perform different actions.

In order to create a basic controller, we use classes and decorators. Decorators associate classes with required metadata and enable Nest to create a routing map (tie requests to the corresponding controllers).

**[⬆ Back to Top](#table-of-contents)**

5. ### How to create a CRUD controller with validation built-in in NestJS?

 You may use the CLI's CRUD generator: ```nest g resource [name]```
 
   **[⬆ Back to Top](#table-of-contents)**
   
6. ### Routing in NestJS

In the following example we'll use the @Controller() decorator, which is required to define a basic controller. We'll specify an optional route path prefix of cats. Using a path prefix in a @Controller() decorator allows us to easily group a set of related routes, and minimize repetitive code. For example, we may choose to group a set of routes that manage interactions with a cat entity under the route /cats. In that case, we could specify the path prefix cats in the @Controller() decorator so that we don't have to repeat that portion of the path for each route in the file.

```javascript
import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
}
```

To create a controller using the CLI, simply execute the ```$ nest g controller [name]``` command.

   **[⬆ Back to Top](#table-of-contents)**

7. ### What is @Get decorator?

The @Get() HTTP request method decorator before the findAll() method tells Nest to create a handler for a specific endpoint for HTTP requests. The endpoint corresponds to the HTTP request method (GET in this case) and the route path. What is the route path? The route path for a handler is determined by concatenating the (optional) prefix declared for the controller, and any path specified in the method's decorator. Since we've declared a prefix for every route ( cats), and haven't added any path information in the decorator, Nest will map GET /cats requests to this handler. As mentioned, the path includes both the optional controller path prefix and any path string declared in the request method decorator. For example, a path prefix of cats combined with the decorator @Get('breed') would produce a route mapping for requests like GET /cats/breed.

   **[⬆ Back to Top](#table-of-contents)**


<!-- QUESTIONS_END -->
