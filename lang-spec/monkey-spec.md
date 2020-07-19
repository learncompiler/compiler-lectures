# Monkey language in Java

[![Build Status](https://travis-ci.org/lionell/monkey-in-java.svg?branch=master)](https://travis-ci.org/lionell/monkey-in-java)

<div align="center">
  <img width="300px" src="http://tinyclipart.com/resource/monkey-cartoon/monkey-cartoon-142.jpg" />
</div>

Monkey is very simple dynamic language. It supports only three different statement types,
but you can do a lot with it!

Just take a look at example below.

## Example program

We are going to evaluate n-th **Fibonacci number**. To do this, we will use recursive approach. It's not efficient
at all, but it show that we can call function's from themself.

```
let fib = fn(n) {
  if (n == 0) { 1 }
  else {
    if (n == 1) { 2 }
    else { fib(n - 1) + fib(n - 2) }
  }
};

fib(6);
```

You can find this and more examples in `examples` directory. To run this code type `monkey examples/fib.mon`.

## Functions = first class citizens

As you understand from the title functions are first class citizens in Monkey language. It's super powerful feature,
that can be used to implement many different design patterns, closures, and more.

```
let add = fn(a, b) { a + b; };
let sub = fn() { return fn(a, b) { a - b }; }();
let applyFunc = fn(a, b, func) { func(a, b); };

applyFunc(2, 3, add)
  + applyFunc(3, 10, sub)
  + applyFunc(5, 10, fn(a, b) { a * b });
```

To run this code type `monkey examples/funcs.mon`.
We are creating function `applyFunc` that takes function as a third parameter, and function that returns function.
You can also see function-literal that is called at the time of definition.

Having function as first class citizens makes creating **closures** super easy. E.g.

```
let superSecretCode = 1234;
let encrypt = fn(x) { x + superSecretCode; };

let superSecretCode = 4321;

encrypt(17); // having superSecretCode equals 1234
```

## Language specification

Here will be formal language specification in [EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form).

```
<program>               ::= <statement-list>
<statement-list>        :: { <statement> }
<statement>             ::= <let-statement>
                          | <return-statement>
                          | <expression-statement>
<let-statement>         ::= "let" <identifier> "=" <expression> ";"
<return-statement>      ::= "return" <expression> ";"
<expression-statement>  ::= <expression> [ ";" ]


<expression>            ::= <relational-expression>
<relational-expression> ::= <relational-expression> "==" <relation>
                          | <relational-expression> "!=" <relation>
                          | <relation>
<relation>              ::= <relation> ">" <arithmetic-expression>
                          | <relation> "<" <arithmetic-expression>
                          | <arithmetic-expression>
<arithmetic-expression> ::= <arithmetic-expression> "+" <term>
                          | <arithmetic-expression> "-" <term>
                          | <term>
<term>                  ::= <term> "*" <factor>
                          | <term> "/" <factor>
                          | <factor>
<factor>                ::= "-" <primary>
                          | "!" <primary>
                          | <primary>
<primary>               ::= "(" <expression> ")"
                          | <function-call>
                          | <identifier>
                          | <int>
                          | <bool>


<function-call>         ::= <function> "(" <argument-list> ")"
<function>              ::= <function-literal>
                          | <identifier>
<function-literal>      ::= "fn (" <parameter-list> ")" <block-statement>
<block-statement>       ::= "{" <statement-list> "}"
<argument-list>         ::= { <expression> "," }
<parameter-list>        ::= { <identifier> "," }

<int>                   ::= <digit> { <digit> }
<digit>                 ::= "0..9"
<alpha>                 ::= "a..zA..Z"
<bool>                  ::= "true"
                          | "false"
```

## How to use

Project developed using [Bazel](https://bazel.build/) build system. All examples below are going to use it.

To start REPL just type `bazel run //java/monkey`

To run tests type `bazel test //javatests/monkey/...`

## Dependencies

These are libraries used to build Monkey language. If you are using prebuilt JAR archieves, you don't need
them to run Monkey program via interpreter or REPL.

* [Guava](https://github.com/google/guava) (Google common library)
* [JUnit4](http://junit.org/junit4/) (Popular testing library)
* [JUnitParams](https://github.com/Pragmatists/JUnitParams) (Parametrized tests)
* [Truth](https://github.com/google/truth) (Better assertion)

## License

MIT
