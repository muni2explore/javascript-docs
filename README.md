# JavaScript Docs

# Object Literal

<pre>
var obj = {}; 
//which is equivalent to this
var obj = new Object();
</pre>

# Types of Object creation by a function in JavaScript

1. Factory function

<pre>
var createObj = function(name) {
  return {
    name: name,
    sayHello: function() {
      console.log("Hello "+this.name);
    }
  }
}

var muni = createObj("muni");
console.log(muni.sayHello()); // Hello muni
var sasi = createObj("sasi");
console.log(sasi.sayHello()); // Hello Sasi

</pre>

2. Constructor function

<pre>
var CreateObj = function(name) {
  this.name = name;
  this.sayHello = function() {
    console.log("Hello "+this.name);
  }
}
var muni = new CreateObj("muni");
console.log(muni.sayHello()); // Hello muni
var sasi = new CreateObj("sasi");
console.log(sasi.sayHello()); // Hello Sasi
</pre>


# bind example

<pre>
var makeReuest = function(url,cb){
  var data = 30;
  
  cb(data);
}

var obj = {
  amt: 50,
  loadData: function(data){
    var sum = this.amt + data;
    console.log(sum);
  },
  prepareRequest: function(){
    var url = "http://example.com";
    
    makeReuest(url, this.loadData.bind(this));
  }
};

obj.prepareRequest();
</pre>

# Data and Accessor Properties

1. Data Descriptor
2. Accessor Descriptor (get & set)

<pre>
var obj = new Object();
Object.defineProperty(objectName, "propertyName", {
  value: "somevalue" //Object descriptor object either Data/Accessor Object
  writable: true //default false
})
</pre>

Example

<pre>
var fruitObj = function(name) {
  var fruit = {};
  Object.defineProperty(fruit, "name", {
   value: name, 
   writable: false 
  });
  return fruit;
};
var apple = fruitObj("Apple");
console.log(apple.name); //Apple
apple.name = "Orange";
console.log(apple.name); //Apple
</pre>

Example : Writable True

<pre>
var fruitObj = function(name) {
  var fruit = {};
  Object.defineProperty(fruit, "name", {
   value: name, 
   writable: true 
  });
  return fruit;
};
var apple = fruitObj("Apple");
console.log(apple.name); //Apple
apple.name = "Orange";
console.log(apple.name); //Orange
</pre>

