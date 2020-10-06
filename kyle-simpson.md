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
