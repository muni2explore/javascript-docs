## Scope
```js
var foo = 'bar';

function bar() {
    var foo = 'baz';
}

function baz() {
    foo = 'bam';
    bam = 'yay';
}

baz();
bar();
console.log(foo);
console.log(bam);
```

```js
var foo = 'bar';

function bar() {
    var foo = 'baz';

    function baz() {
        foo = 'bam';
        bam = 'yay';
    }
    baz()
}


bar();
console.log(foo);
console.log(bam);
baz();
```
### Function Declaration vs Function Definition

```js
var foo = function bar() {
    var foo = 'baz';

    function baz() {
        foo = bar;
        foo;
    }
    baz()
}

foo();
bar();
```

```js
function foo() {
    var bar = 'bar';

    function baz() {
        console.log(bar);
    }
    baz()
}
foo();
```

### Cheating Lexical Scope (Compile time scope) - Eval
It converts string content into excuteable script.

```js
var bar = "bar";

function foo(str) {
    eval(str);
    console.log(bar); //42
}

foo("var bar = 42;");
```

## with

```js
var obj = {
    a: 2,
    b: 3,
    c: 4
}

obj.a = obj.b + obj.c;
obj.c = obj.b - obj.a;

with (obj) {
    a = b + c;
    d = b - a;
    d = 3;
}

obj.d; // Undefined
d; // 3 -- oops
```
## IIFE
```js
/** IIFE **/

var foo = "foo"

(function(){
    var foo = "foo2"; 
    console.log(foo); 
})()

console.log(foo);
```

```js
var foo = "foo"

(function(bar){
    var foo = bar; 
    console.log(foo);  // foo
})(foo)

console.log(foo); // foo
```
## let

```js
/** let (block scope) **/

function foo() {
    var bar = "bar";
    for (let i=0; i <bar.length; i++) {
        console.log(bar.charAt(i));
    }
    console.log(i); // ReferenceError
}

foo();
```

```js
function foo(bar) {
    if (bar) {
        let baz = bar;
        if (baz) {
            let bam = baz;
        }
        console.log(bam); // Error
    }
    console.log(baz); // Error
}

foo("bar");
```
- will let keyword hoist?

```js
function foo(bar) {
    /*let*/  {
        let baz = bar;
        console.log(baz); // bar
    }  
    console.log(baz); // Error
}

foo("bar");
```

## Hoisting
```js
a; // ???
b;// ???
var a = b;
var b = 2;
b; // ???
a; // ???

/** Compile */
var a;
var b;
a; // ???
b; // ???
a = b;
b = 2;
b;  // 2
a;  // ???
```

```js
var a = b();
var c = d();
a;
c;

function b() {
    return c;
}

var d = function() {
    return b();
}

/** Compile */
function b() {
    return c;
}
var a;
var c;
var d;
a = b();
c = d();
a;
c;
d = function() {
    return b();
}
```

- functions are Hoisting first.
```js
// Hoisting
foo(); // foo

var foo = 2;

function foo() {
    console.log("bar");
}


function foo() {
    console.log("foo");
}
```
