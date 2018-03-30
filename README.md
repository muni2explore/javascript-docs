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

1. Data Descriptor (value)
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

#  Example : Object.defineProperties

<pre>
var createPerson = function(firstName, lastName) {
  var person = {};
  Object.defineProperties(person, {
    firstName: {
      value: firstName,
      writable: true
    },
    lastName: {
      value: lastName,
      writable: true
    },
    fullName: {
      get: function() {
        return this.firstName+" "+this.lastName;
      },
      set: function(val) {
        this.firstName = val;
        this.lastName = val;
      }
    }
    
  });
  return person;
}
var muniAyothi = createPerson("muni", "ayothi");
console.log( muniAyothi.fullName ) // 'muni ayothi'
muniAyothi.fullName = 'sasi'
console.log( muniAyothi.fullName )// 'sasi sasi'
</pre>

configurable:true property which enable to redefine the particular property

<pre>
var createPerson = function(firstName, lastName) {
  var person = {};
  Object.defineProperties(person, {
    firstName: {
      value: firstName
    },
    lastName: {
      value: lastName
    },
    fullName: {
      get: function() {
        return this.firstName+" "+this.lastName;
      },
      configurable: false // default false
    }
    
  });
  return person;
}
var muniAyothi = createPerson("muni", "ayothi");
Object.defineProperty(muniAyothi, "fullName", {
  get: function() {
  return this.lastName +" "+this.firstName;
  }
} );

console.log(muniAyothi.fullName);
</pre>
Note: Script snippet #1:21 Uncaught TypeError: Cannot redefine property: fullName
    at Function.defineProperty (<anonymous>)
    at <anonymous>:21:8
