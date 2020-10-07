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
    if (bar) {
        console.log(baz); //ReferenceError
        let baz = bar;
    }
}

foo("bar");
```

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

### recursion
```js
a(1);

function a(foo) {
    if (foo > 20) return foo;
    return b(foo+2);
}

function b(foo) {
    return c(foo) + 1;
}

function c(foo) {
    return a(foo*2);
}
```


```js
/**
# Instructions

1. Fix the code so it prints out the alphabet A-Z in the console.

2. Cannot:
	- Have **any** global variables at all
	- Delete or combine any function declarations
	- Create any new functions (except IIFEs -- hint!)
	- Rearrange the order of declarations

3. Can/must:
	- Declare extra variables (as long as they're not global)
	- Modify (in-place) function declaration/initialization
	- Add/remove statements/expressions (IIFEs, return, params, etc)
	- Make the fewest changes possible
**/

A();

function C() {
	console.log("OOPS!");
}

function E(f) {
	console.log("E");
	f();
	var f = F;
}

var A = function() {
	console.log("A");
	B();
};

var C;

function G() {
	console.log("G");
	H();

	var H = function() {
		console.log("H");
		I();
	};
}

var D = d;

function d() {
	console.log("D");
	E();
}

function I() {
	console.log("I");
	J();
	J();
}

B = function() {
	console.log("B");
	C();
};

var F = function() {
	console.log("F");
	G();
};

var rest = "KLMNOPQRSTUVWXYZ".split("");
for (var i=0; i<rest.length; i++) {
	(function(i){
		// define the current function
		window[rest[i]] = function() {
			console.log(rest[i]);
			if (i < (rest.length-1)) {
				// TODO: call the next function
			}
		};
	})(i);
}

var J = function() {
	J = function() {
		console.log("J");
		K();
	};
};

C = function() {
	console.log("C");
	D();
};
```
## this (call site, Execution context)
Every function, while executing has a reference to its current execution context, called this.
<br/>
Execution context - how the function is called, when it is called.

```js
/**
 * this : implicit & default binding
 */
function  foo() {
    console.log(this.bar);
}

var bar = "bar1";
var o2 = { bar: "bar2", foo: foo};
var o3 = { bar1: "bar3", foo: foo};
var o4 = { bar: "bar4", foo: foo};

foo(); // "bar1"
o2.foo(); // "bar2"
o3.foo(); // "undefined"
o4.foo(); // "bar4"
```

```js
var o1 = {
    bar: "bar1",
    foo: function() {
        console.log(this.bar);
    }
}

var o2 = {bar: "bar2", foo: o1.foo}

var bar = "bar3";
var foo = o1.foo;

o1.foo();
o2.foo();
foo();
```

```js
function foo() {
    var bar = "bar1";
    baz();
}
function baz() {
    console.log(this.bar);
}
var bar = "bar2";
foo();
```

```js
/**
 * Explicit binding
 */
function foo() {
    console.log(this.bar);
}
var bar = "bar1";
var obj = {bar: "bar2"}
foo();
foo.call(obj);
```

```js
/**
 * this: Hard binding
 */
function foo() {
    console.log(this.bar);
}
var obj = {bar: "bar"}
var obj2 = {bar: "bar2"}
var orig = foo;
foo = function() {
    orig.call(obj);
};

foo();
foo.call(obj2);
```


```js
/**
 * this: hard binding
 */
function bind(fn, o) {
    return function() {
        fn.call(o);
    }
}

function foo() {
    console.log(this.bar);
}

var obj = {bar: "bar"}
var obj2 = {bar: "bar2"}

foo = bind(foo, obj);

foo();
foo.call(obj2);
```


```js
/**
 * this: hard binding
 */
if (!Function.prototype.bind2) {
    Function.prototype.bind2 = function(o) {
        var fn = this; //the function
        return function() {
            return fn.apply(o, arguments)
        }
    }
}

function foo(baz) {
    console.log(this.bar+" "+baz);
}

var obj = {bar: "bar"}

foo = foo.bind2(obj);

foo("baz");
```

## this - new
```js
/*
1. Brand new object created
2. linked to new object
3. object bound to this keyword
4. Implicit return this
*/

/** this: new 
 * we can convert any function into constructor function using new keyword
*/
function foo() {
    this.baz = "baz";
    console.log(this.bar +" "+ baz);
}
var bar = "bar";
var baz = new foo();
```
## this determination

1. Was the function called with `new`?
2. Was the function called with `call` or `apply` specifying an explicit this?
3. Was the function called via a containing/owning object (context)?
4. DEFAULT: global object(except strict mode)


## Closure

> Closure is when function "remembers" its lexical scope even when the function is executed outside that lexical scope.

```js
function foo() {
    var bar = "bar";

    function baz () {
        console.log(bar);
    }
    bam(baz);
}

function bam(baz) {
    baz();
}

foo();
```

```js
function foo() {
    var bar = "bar";

    return function() {
        console.log(bar);
    };
}

function bam(baz) {
    foo()();
}

bam();
```

```js
function foo() {
    var bar = "bar";

    setTimeout(function() {
        console.log(bar);
    }, 1000);
}

foo();
```

```js
function foo() {
    var bar = "bar";

    $("#btn").click(function(evt){
        console.log(bar);
    });
}

foo();
```

```js
function foo() {
    var bar = "bar";

    setTimeout(function() {
        console.log(bar++);
    }, 100);

    setTimeout(function() {
        console.log(bar++);
    }, 200);
}

foo();
```

```js
function foo() {
    var bar = "bar";

    setTimeout(function() {
        var baz = 1;
        console.log(bar++);

        setTimeout(function() {
            console.log(bar+baz);
        }, 200);
    }, 100);   
}

foo();
```

```js
for(var i=1; i <= 5; i++) {
    setTimeout(() => {
        console.log("i: "+i);
    }, i*1000);
}
```

```js
for(var i=1; i <= 5; i++) {
    (function(i){
        setTimeout(() => {
            console.log("i: "+i);
        }, i*1000);
    })(i);
}
```

```js
for(let i=1; i <= 5; i++) {
    setTimeout(() => {
        console.log("i: "+i);
    }, i*1000);
}
```

```js
/**
 * It's object reference
 * Not an closure
 */
var foo = (function(){
    var o = {bar: "bar"};
    return {obj: o}
})();

console.log(foo.obj.bar);
```

```js
/*
Closure: classic module pattern

1. There must be atleast one outer wrapping function call
2. The outer function call must return an object with one inner function.

where returned object function will have reference lexcial scope of outer wrapping function
*/
var foo = (function(){
    var o = {bar: "bar"}
    return {
        bar: function() {
            console.log(o.bar);
        }
    }
})();

foo.bar();
```

```js
var foo = (function(){
    var publicAPI = {
        bar: function() {
            publicAPI.baz()
        },
        baz: function() {
            console.log('baz');
        }
    };
    return publicAPI;
})();

foo.bar();
```

```js
/*
Closure: modern module pattern
*/
define("foo", function(){
    var o = {bar: "bar"};

    return {
        bar: function() {
            console.log(o.bar);
        }
    }
})
```

```js
/*
Closure: future /ES6+ module pattern

foo.js
*/

var o = {bar: "bar"};

export function bar() {
    console.log(o.bar);
}


//bar.js

import {bar} from "foo";
bar(); 

import "foo"
foo.bar();
```

### Disadvantages (tradeoff): (Module Pattern)

1. we can not unit testing for inner private functions

2. When call module pattern 1000 times, it returns 1000 object


```js
# Instructions

1. In this exercise, you will modify existing code for a simple note taking app. You will not add/remove functionality per se, but instead organize the code into a more proper module design and make it more flexible/reusable.

2. Using what you learned about closure and the module pattern, modify your previous code to wrap all the functionality up into a simple object (call it "NotesManager" or something appropriate), with a simple API, consisting of:
  - an `init()` method, which you will call from the outside when jQuery's `document.ready` event is fired, and pass in the data from the "database".
  - a public method to add in notes "in bulk" after retrieval from the "database". hint: this can/should be called **before** you run the "init" method.

3. Make sure you have a "private" storage of the `notes` data list inside your module. Why is it a good idea to keep the data "private" inside the module?

4. What do you notice about the structure of this code as it relates to the DOM access and the usage of jQuery? Would it make sense to "generalize" this code so that the module didn't have hardcoded into it the various DOM elements it would operate on? Explore how you would modify the code in this fashion. What are the benefits and tradeoffs?

// assume this data came from the database
var notes = [
	"This is the first note I've taken!",
	"Now is the time for all good men to come to the aid of their country.",
	"The quick brown fox jumped over the moon."
];

function addNote(note) {
	$("#notes").prepend(
		$("<a href='#'></a>")
		.addClass("note")
		.text(note)
	);
}

function addCurrentNote() {
	var current_note = $("#note").val();

	if (current_note) {
		notes.push(current_note);
		addNote(current_note);
		$("#note").val("");
	}
}

function showHelp() {
	$("#help").show();

	document.addEventListener("click",function __handler__(evt){
		evt.preventDefault();
		evt.stopPropagation();
		evt.stopImmediatePropagation();

		document.removeEventListener("click",__handler__,true);
		hideHelp();
	},true);
}

function hideHelp() {
	$("#help").hide();
}

function handleOpenHelp(evt) {
	if (!$("#help").is(":visible")) {
		evt.preventDefault();
		evt.stopPropagation();

		showHelp();
	}
}

function handleAddNote(evt) {
	addCurrentNote();
}

function handleEnter(evt) {
	if (evt.which == 13) {
		addCurrentNote();
	}
}

function handleDocumentClick(evt) {
	$("#notes").removeClass("active");
	$("#notes").children(".note").removeClass("highlighted");
}

function handleNoteClick(evt) {
	evt.preventDefault();
	evt.stopPropagation();

	$("#notes").addClass("active");
	$("#notes").children(".note").removeClass("highlighted");
	$(evt.target).addClass("highlighted");
}

function init() {
	// build the initial list from the existing `notes` data
	var html = "";
	for (i=0; i<notes.length; i++) {
		html += "<a href='#' class='note'>" + notes[i] + "</a>";
	}
	$("#notes").html(html);

	// listen to "help" button
	$("#open_help").bind("click",handleOpenHelp);

	// listen to "add" button
	$("#add_note").bind("click",handleAddNote);

	// listen for <enter> in text box
	$("#new_note").bind("keypress",handleEnter);

	// listen for clicks outside the notes box
	$(document).bind("click",handleDocumentClick);

	// listen for clicks on note elements
	$("#notes").on("click",".note",handleNoteClick);
}

$(document).ready(init);

```
