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

