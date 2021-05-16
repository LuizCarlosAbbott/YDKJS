# Chapter 2

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

The refeference to the inner _add()_ function gets returned with each call to the outer function _makeAdder()_ which is able to remember
wherever 'x' value was passed to the _makeAdder()_ function.

curisiosity: 'NaN' is the only value in JavaScript that is not equal to itself. So the 'Nan' value is the only one that would make x !== x be true.

# Scope & Closures

## Compiler Theory

JavaScript is a compiled language that follow three steps before is executed: _Tokenizing_, _Parsing_ and _Code-generation_. JavaScript compilation
doesn't happen in a build step ahead of time, as with other languages, any snippet of JavaScript has to be compiled before (usually right before!) it's executed.
