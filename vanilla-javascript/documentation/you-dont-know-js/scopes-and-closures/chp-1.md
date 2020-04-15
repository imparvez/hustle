# What is Scope?
How does javascript knows which variables are accessible by any given statement
and how does it handle two variables of the same name?
The answer to questions like these are explained with the help of well defined rules called scope.
---
#### How JS engine process our program before it runs?
Javascript is classified as an **interpreted language** making most of us assumed that JS program are processed in a single, top-down pass.
But JS is parsed/compiled in a separate phase **before execution begins**.
---
Javascript functions are first class values. They can be assigned and parsed like numbers or strings.
But since this function holds and access variable they maintain their **original scope**, no matter where in the program the function are eventually executed.
This is called **Closure**.
---
#### Compiled Vs Interpreted
Code compilation is a set of steps that process the text of your code and turn it into a list of instructions that computer can understand. Over here, whole source code is transformed all at once and those resulting instructions are saved as output that can be later executed.
Interpretation is same as compilation where it transforms your program into machine-understandable instructions. But the processing model is different. Unlike a program being **compiled all at once**, with interpretation the source code is **tranformed line by line** or statement is executed before immediately proceeding to processing the next line of the source code.
---
Program is processed by a compiler in three basic stages:
1. Tokenizing/Lexing: breaking up strings of character into meaningful chunks, called tokens. For Eg: `var a = 2;` will be broken into `var`, `a`, `=`, `2` and `;`
2. Parsing: Taking stream of array of tokens and turning it into tree of nested elements which collectively represents the grammatical structure of the program. This is called an Abstract Syntax Tree(AST). For Eg: the tree for `var a = 2` will start from top level node called `VariableDeclaration` with child node called `Identifier`(whose value is a), and another child called `AssignmentExpression` which itself has a child called `NumericLiteral`(whose value is 2).
3. Code Generation: Taking AST and turning it into executable code. This part varies greatly depending on the language, the platform it's targeting, and other factors.

The JS engine takes just described AST for `var a = 2` and turns in into a set of machine instructions to actually create a variable called `a` and then store value to `a`.


All the JS programs occurs in two phases:
- Parsing/Compilation
- Execution

Few program characteristics you can observe to prove compile-then-execute-approach in Javascript.
- Syntax Errors.
- Early Errors.
- Hoisting.

Example no: 1
```js
var greeting = 'hello';
console.log(greeting);
greeting = ."Hi";
```
No output is produced("Hello" is not printed in console) but instead throws a `SyntaxError` on line `3`.
This error has taken place after `console.log` statement, if JS was executing top-down line by line, one would expect the "Hello" message being printed before the syntax error being thrown. That doesn't happen.

Example no: 2
```js
console.log("Howdy");

saySomething("Hello", "HI");

function saySomething(greeting, greeting) {
    "use strict";
    console.log(greeting);
}
```
line no: 1, being a well formed statement was not printed.
Over here also, `SyntaxError` was thrown before the program is executed giving `Uncaught SyntaxError: Duplicate parameter name not allowed in this context` because "strict mode" was used in `saySomething` function.
This error is not an syntax error as what we saw in Example no: 1 but more of a specification to be thrown as an "early error" before any execution begins.

The only reasonable explanantion one can give is that the code must first be fully parsed before any execution occurs.

#### Lexical Scope
JS's scope is determined at compile time, the term for this kind of scope is "Lexical Scope".
The key idea for "Lexical Scope" is that it's controlled entirely by the placement of functions, blocks, and variable declarations, in relation to one another.

If you place a variable declaration inside a function, the compiler handles this declaration as it is parsing the function, and associates that declaration with the function's scope.
```js
var a = 7;
var b = 10;
function multiplier(){
    var a = 5;
    return a * b;
}

multiplier(); // 10

```
Here on line 5, b is not found. To find b, JS will start going one layer above until b is found. b is found on line 2.
On line 5, a is found in that function scope only making a on line 1 not useful for this multiplier operation.


If a variable is block scope (`let`, `const` )declared, then it is associated with the nearest enclosing `{...}` block, rather than its enclosing function as `var`.
```js
{
    let a = 'HELLO';
    console.log(a)
}
console.log(a); // ReferenceError.
```

A reference for a variable must be resolved as coming from one of the scopes that are lexically avaiable to it, otherwise the variable is said to be undeclared resulting into an error.

Compilation creates a map of all the lexical scopes that lay out what the program will need while it executes.
Think it of as an planned code that will be executed at a runtime where all the scopes are defined and all the identifiers are registers for each scope.