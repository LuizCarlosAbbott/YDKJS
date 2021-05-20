# Basics

## Dot notation

obj.a

## Bracket notation

obj["a"]

### You can use bracket notation if you want to access a property/key but the name is stored in another variable

```js
var obj = {
  joao: "32"
  maria: "joao"
};

obj[joao];    // "32"
obj[obj.maria];    // "32"
obj[maria];   // "joao"
```

## Closure

```js
function makeAdder(x) {
  function add(y) {
    return y + x;
  }

  return add;
}

var plusTwo = makeAdder(2);
var plusFive = makeAdder(5);

plusTwo(6); // 8 <-- 6 + 2;
plusFive(7); // 13 <-- 7 + 5;
```

The refeference to the inner `add()` function gets returned with each call to the outer function `makeAdder()` which is able to remember
wherever 'x' value was passed to the `makeAdder()` function.

curisiosity: 'NaN' is the only value in JavaScript that is not equal to itself. So the 'Nan' value is the only one that would make x !== x be true.

# Scope & Closures

## Compiler Theory

JavaScript is a compiled language that follow three steps before is executed: `Tokenizing`, `Parsing` and `Code-generation`. JavaScript compilation
doesn't happen in a build step ahead of time, as with other languages, any snippet of JavaScript has to be compiled before (usually right before!) it's executed.

## Understanding Scope

_My understanding:_
The JavaScript Compiler will declare the variables in the frist step and then the Engine will assign the values to the variables (In each step the Scope will
verify if the variable isn't declared and if it is accessible to asign the value).

_Book Resume_
Two distinct actions are taken for a variable assignment: First, Compiler declares a variable (if not previously declared in the current scope), and second, when executing, Engine looks up the variable in Scope and assigns to it, if found.

## Nested Scope

As blocks of codes or functions can be inside other blocks of codes or functions, scopes can be insider other scopes. So, if a variable is not found in the
immediate scope, Engine consults the next outer containing scope, and keep consulting until found or until reach the global scope.

## Error

`ReferenceError` is Scope resolution-failure related, whereas `TypeError` implies that Scope resolution was successful, but that there was an illegal/impossible
action attempted against the result.

- Unfulfilled RHS references result in `ReferenceError`s being thrown;
- Unfulfilled LHS references result in an automatic, implicitly-created global of that name, or a `ReferenceError`;

## Lexical Scope

Lexical scope means that scope is defined by author-time decisions of where functions are declared. The lexing phase of compilation is essentially able to know where and how all identifiers are declared, and thus predict how they will be looked-up during execution.

Two mechanisms in JavaScript can "cheat" lexical scope: `eval(..)` and `with`. The former can modify existing lexical scope (at runtime) by evaluating a string of "code" which has one or more declarations in it. The latter essentially creates a whole new lexical scope (again, at runtime) by treating an object reference as a "scope" and that object's properties as scoped identifiers.

### eval(..)

```js
function foo(str, a) {
  eval(str); // cheating!
  console.log(a, b);
}

var b = 2;

foo("var b = 3;", 1); // 1 3
```

The use-cases for dynamically generating code inside your program are incredibly rare, as the performance degradations are almost never worth the capabillity.

### with

```js
function foo(obj) {
  with (obj) {
    a = 2;
  }
}

var o1 = {
  a: 3,
};

var o2 = {
  b: 3,
};

foo(o1);
console.log(o1.a); // 2

foo(o2);
console.log(o2.a); // undefined
console.log(a); // 2 -- Oops, leaked global!
```

### Conclusion

Your code will almost certainly tend to run slower simply by the fact that you include an `eval(..)` or `with` anywhere in the code. No matter how smart the Engine way be about trying to limit the side-effects of these pessimistic assumptions, there's no getting around the fact that without the optimizations, code runs slower.

# Function vs. Block Scope

## let

Declarations made with `let` will not hoist to the entire scope of the block they appear in. Such declarations will not observably "exist" in the block until the declaration statement. Diferently of `var` that can be hoisted.

```js
{
  console.log(bar); // ReferenceError!
  let bar = 2;
}
```

_let_ => block scope.
_var_ => function scope.

# Hoisting

We can be tempted to look at `var a = 2;` as one statement, but the JavaScript Engine does not see it that way. It sees `var a` and `a = 2` as two separate statements, the first one a compiler-phase task, and the second one an execution-phase task.

What this leads to is that all declarations in a scope, regardless of where they appear, are processed first before the code itself is executed. You can visualize this as declarations (variables and functions) being "moved" to the top of their respective scopes, which we call "hoisting". Function declarations have more precendence than variable declarations.

Declarations themselves are hoisted, but assignments, even assignments of function expressions, are not hoisted.

# Closure

The function is being invoked well outside of its author-time lexical scope. _Closure_ lets the function continue to access the lexical scope it was defined in at author-time

```js
function foo() {
  var a = 2;

  function baz() {
    console.log(a); // 2
  }

  bar(baz);
}

function bar(fn) {
  fn(); // look ma, I saw closure!
}
```

We pass the inner function `baz` over to `bar`, and call that inner function (labeled `fn` now), and when we do, its closure over the inner scope of `foo()` is observed, by accessing `a`.
