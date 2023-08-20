# JavaScript Logging

This is an informal proposal for a new logging framework in JavaScript. 
JavaScript needs a logging framework with more features than `console`.
There are already good choices for logging frameworks, but the choice is the problem.
There's no obvious choice. It's unlikely for a library to use a logging framework because it's an extra dependency
and it may not be what the consuming application uses.

![xkcd comic](https://imgs.xkcd.com/comics/standards.png)

I think the only solution to a new built in logging framework.
I'm not sure if this should be a ECMAScript proposal, WHATWG, or something else.

## What's wrong with `console`?

### Cannot redirect console to a file, CloudWatch, or another sink.

Server side JavaScript can redirect stdio to a file or logging service but that can't write debug logs to a file while showing information on stdout.
Browsers cannot redirect console logs

### Cannot filter log levels

Browsers may support filtering the console's level, but server side runtimes like JavaScript, Bun and Deno don't.

### Cannot filter by module  or logger

`console` is a global and logs are not scoped to files, modules, or contexts.

### Structured logging

### Formatting and additional context

## What's good about console?

### Always available
### Zero dependnecies

## Outline

 - seperate logging frontend and backend
   - frontend is the log statements
   - backend writes the logs somewhere
     - > is filtering handled by the backend or the core?
 - The runtime provides a reasonable backend
   - server side runtimes can log to stdio
   - browsers can log to their consoles
   - runtimes may provide additional backends, like http or files
   - libraries can implement additional backends
 - support most features of popular logging frameworks
   - existing frameworks could be replaced with shims so any code can use the new backend


## Front end api

### Getting a log instance
> TODO how do you get a logger?

### Writing logs

```typescript
log.info`hello ${name}`;
```

This could be logged as a normal string template `hello everett1992` or structured logging
could preserve the template and args.

> Unfortunatly we can't get the variable names so we cannot record `{name: 'everett1992'}` best we can do is
> `{ message: "hello {}", ["everett1992"]}`. This would be harder to search the log index for logs for a given name.
>
> We could recommend `hello ${{name}}` but that's unusual.
>
> We could use the replacement values as lookup keys in context. Also getting into unusual teritory.
> ```
> log.info({name})`hello ${"name"}`
> ``````

Logging with a tagged template supports readable log calls while supporing structured logging. A console backend may convert the template arguments to strings, but a database backend could save the template and values seperately. 

```typescript
using child = log.child({ a: 'property' })

using _ = log.context({ a: 'property' })
```