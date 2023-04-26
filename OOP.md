# Object Oriented Programming (OOP)

Objects in JavaScript are used to model real-world objects, giving them properties and behavior just like their real-world counterparts.

## Creating Objects

```
let duck = {
  name: "Aflac",
  numLegs: 2
};
```

`duck` is the object, while `name`and `numLegs` are properties of the object.

## Dot Notation

Dot notation is used on the object name, followed by the name of the property, to access the value. For example: `duck.name` will return `Aflac`.

## Creating Methods

Methods are properties that are functions. This adds different behavior to an object. Here is the duck example with a method:

```
let duck = {
  name: "Aflac",
  numLegs: 2,
  sayName() {
    return `The name of this duck is ${this.name}.`; // this.name is the equivalent of saying duck.name. this refers to the object.
  }
};
duck.sayName();
```

The `this` keyword is an important way of reducing issues. In the case where the object name changes (say from _"duck"_ to _"dog"_), `this` will simply point to _"dog"_. Otherwise, it would still look for `duck`.

## Constructors

Constructors are functions that create new objects. They define properties and behaviors that will belong to the new object. Think of them as a blueprint for the creation of new objects. For example:

```
function Bird() {
  this.name = "Albert";
  this.color = "blue";
  this.numLegs = 2;
}
```

This constructor defines a `Bird` object with properties `name`, `color`, and `numLegs` set to Albert, blue, and 2, respectively. Constructors follow a few conventions:

- Constructors are defined with a capitalized name to distinguish them from other functions that are not constructors.
- Constructors use the keyword `this` to set properties of the object they will create. Inside the constructor, `this` refers to the new object it will create.
- Constructors define properties and behaviors instead of returning a value as other functions might.

To check whether an `instance` (see [Instances](#instances)) is part of a `constructor`, you can use the property `.constructor`. Keep in mind that the `.constructor` property can be overwritten. For that reason, it's best to use `instanceof` (covered in [Instances](#instances)).

### Using constructors to create objects

We can create a new object using a constructor by using the `new` operator: `let blueBird = new Bird();`. Without `new`, `this` would not point to the new object. Now blueBird has all the properties defined inside the `Bird` constructor and can be accessed and changed at any time:

```
blueBird.name = 'Elvira';
blueBird.color;
blueBird.numLegs;
```

### Passing arguments

To simplify the process of creating new objects, we can design our constructors to accept parameters:

```
function Bird(name, color) {
  this.name = name;
  this.color = color;
  this.numLegs = 2;
}
```

### Instances

Anytime a constructor function creates a new object, that object is said to be an instance of its constructor. JavaScript gives a convenient way to verify this with the `instanceof` operator. `instanceof` allows you to compare an object to a constructor, returning true or false based on whether or not that object was created with the constructor. Here's an example:

```
// Returns True
let Bird = function(name, color) {
  this.name = name;
  this.color = color;
  this.numLegs = 2;
}

let crow = new Bird("Alexis", "black");

crow instanceof Bird; // Returns true

// Returns False
let canary = {
  name: "Mildred",
  color: "Yellow",
  numLegs: 2
};

canary instanceof Bird; // Returns false
```

Each instance has it's **own property** values, independent of one another despite sharing the same property names. To access the properties for example in an object, consider the following code:

```
let ownProps = [];

for (let property in duck) {
  if(duck.hasOwnProperty(property)) {
    ownProps.push(property);
  }
}
```

#### Prototypes

Some properties are the same for all instances. For example, all birds start with two legs. `numLegs` would be duplicated inside each `Bird` instance. This may not be an issue when there are only two instances, but imagine if there are millions of instances. That would be a lot of duplicated variables.

A better way is to use the `prototype` of the constructor (`Bird` for example). Properties in the `prototype` are shared among ALL instances of the constructor. For example: `Bird.prototype.numLegs = 2;`. Now ALL instances of `Bird` have the `numLegs` property. The `prototype` goes outside of the constructor, as such:

```
let Bird = function(name, color) {
  this.name = name;
  this.color = color;
}

Bird.prototype.numLegs = 2;
```

**Prototype properties** are separate from **own properties**. As mentioned above, we can adjust our function to output **Prototype properties** in a separate array:

```
let ownProps = [];
let prototypeProps = [];

for (let property in duck) {
  if(duck.hasOwnProperty(property)) {
    ownProps.push(property);
  } else {
    prototypeProps.push(property);
  }
}
```

Obviously, setting multiple `prototype` can be a tedious process. As such, it's easier to create a new `Object` which contains the properties, and can be added all at once. **Setting the prototype manually erases the `.constructor` property!** Therefore, we need to also define it in our `prototype`:

```
Bird.prototype = {
  constructor: Bird,
  numLegs: 2,
  eat() {
    console.log("nom nom nom");
  },
  describe() {
    console.log("My name is " + this.name);
  }
};
```

We can check whether an instance inherits prototype properties from a constructor by using the `isPrototypeOf` method: `Bird.prototype.isPrototypeOf(duck); // Returns true`

At this point, you've probably realised that `Prototypes` **are also objects**. Therefore, `prototypes` can also have `prototypes`, called with `Object`:
`Object.prototype.isPrototypeOf(Bird.prototype);`. This creates a prototype chain.

- `Bird` is a supertype for `duck` (a subtype).
- `Object` is a supertype for both `Bird` and `duck`. In fact, it's a supertype for ALL objects in Javascript.

## DRY Method

AKA the "Don't Repeat Yourself" Method

In the example below, notice how the `describe` function appears in two places.

```
Bird.prototype = {
  constructor: Bird,
  describe() {
    console.log("My name is " + this.name);
  }
};

Dog.prototype = {
  constructor: Dog,
  describe() {
    console.log("My name is " + this.name);
  }
};
```

We can follow the DRY principle by creating a `supertype` (or parent) called `Animal`:

```
function Animal() { };

Animal.prototype = {
  constructor: Animal,
  describe() {
    console.log("My name is " + this.name);
  }
};
```

We can now remove `describe` from `Bird` and `Dog`:

```
Bird.prototype = {
  constructor: Bird,
};

Dog.prototype = {
  constructor: Dog,
};
```

### Inheritance

We can attribute an object supertype to its subtypes in JavaScript to reduce repetition through the use of `prototype`. Remember that `prototype` is like the recipe for creating an object. To do so, create a new Object based on the supertype in question. For example:

```
function Animal() { }

Animal.prototype = {
  constructor: Animal,
  eat: function() {
    console.log("nom nom nom");
  }
};

function Dog() { }

Dog.prototype = Object.create(Animal.prototype);
let beagle = new Dog();
```

In the example above, we've got the parent `Animal`. Then, we have one of its subtypes `Dog`, which inherits the functions and properties from `Animal`. Then, we create yet another object from `Dog` called `Beagle`. Therefore, `Beagle` is a subtype of `Dog`, which is a subtype of `Animal`, and inherits everything from its parents.

### Overwriting properties

Even if an instance inherits its properties from its parents, you can still overwrite properties by simply calling them by the same name. For example:

```
Animal.prototype = {
  constructor: Animal,
  eat: function() {
    console.log("nom nom nom");
  }
};

function Bird() { }

Bird.prototype = Object.create(Animal.prototype);
Bird.prototype.constructor = Bird;
Bird.prototype.eat = function() {
  console.log("peck peck peck");
}
```

`eat` for `Bird` is now overwritten with `peck peck peck`.

### Mixins

A mixin is an object which contains a collection of functions. Typically used for objects which share commonalities but are entirely different. For example, both birds and airplanes can fly. However, they're not the same whatsoever.

```
let flyMixin = function(obj) {
  obj.fly = function() {
    console.log("Flying, wooosh!");
  }
};

let bird = {
  name: "Donald",
  numLegs: 2
};

let plane = {
  model: "777",
  numPassengers: 524
};

flyMixin(bird);
flyMixin(plane);

// And now, both bird and plane can use the fly function

bird.fly();
plane.fly(); // Both would display "Flying, wooosh!"
```

## Closures

Variables inside constructors are privileged and can only be changed within the scope of that constructor.

```
function Bird() {
  let hatchedEgg = 10;

  this.getHatchedEggCount = function() {
    return hatchedEgg;
  };
}
let ducky = new Bird();
ducky.getHatchedEggCount();
```

Here `getHatchedEggCount` is a privileged method, because it has access to the private variable `hatchedEgg`. This is possible because `hatchedEgg` is declared in the same context as `getHatchedEggCount`. In JavaScript, a function always has access to the context in which it was created. This is called `closure`.

## Immediately Invoked Function Expression (IIFE)

A common pattern in JavaScript is to execute a function as soon as it is declared.

```
(function () {
  console.log("Chirp, chirp!");
})(); // An anonymous function expression that executes right away and outputs "Chirp, chirp!"
```

Note that the function has no name and is not stored in a variable. The two parentheses `()` at the end of the function expression cause it to be immediately executed or invoked. This pattern is known as an _immediately invoked function expression or IIFE_.

### Use IIFE to create a module

An immediately invoked function expression (IIFE) is often used to group related functionality into a single object or module. For example, we can group two mixins together:

```
// Two mixins created earlier

function glideMixin(obj) {
  obj.glide = function() {
    console.log("Gliding on the water");
  };
}
function flyMixin(obj) {
  obj.fly = function() {
    console.log("Flying, wooosh!");
  };
}

// Grouped together using IIFE

let motionModule = (function () {
  return {
    glideMixin: function(obj) {
      obj.glide = function() {
        console.log("Gliding on the water");
      };
    },
    flyMixin: function(obj) {
      obj.fly = function() {
        console.log("Flying, wooosh!");
      };
    }
  }
})();
```

In the example above, we packaged all the mixins into a single `motionModule` pattern which can be called on later. For example:

```
motionModule.glideMixin(duck);
duck.glide();
```
