# JavaScript Logging

This is an informal proposal for a new logging framework in JavaScript. 
JavaScript needs a logging framework with more features than `console`.
There are already good choices for logging frameworks, but the choice is the problem.
There's no obvious choice. It's unlikely for a library to use a logging framework because it's an extra dependency
and it may not be what the consuming application uses.

![xkcd comic](https://imgs.xkcd.com/comics/standards.png)

I think the only solution to a new built in logging framework.
I'm not sure if this should be a ECMAScript proposal, WHATWG, or something else.

# What's wrong with `console`?

## Cannot redirect console to a file, CloudWatch, or another sink.

Server side JavaScript can redirect stdio to a file or logging service but that can't write debug logs to a file while showing information on stdout.
Browsers cannot redirect console logs

## Cannot filter log levels

Browsers may support filtering the console's level, but server side runtimes like JavaScript, Bun and Deno don't.

## Cannot filter by module  or logger

`console` is a global and logs are not scoped to files, modules, or contexts.

## Structured logging

## Formatting and additional context

# What's good about console?

## Always available
## Zero dependnecies

# Outline

 - seperate logging frontend and backend
   - frontend is the log statements
   - backend writes the logs somewhere
 - provide reasonable backends
   - server side runtimes can log to stdio
   - browsers can log to their consoles
   - runtimes may provide additional backends, like http or files
   - libraries can implement additional backends
 - support most features of popular logging frameworks
   - existing frameworks could be replaced with shims so any code can use the new backend
