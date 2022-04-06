---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# Everything I Hate About JavaScript

Actually, there's still more stuff, but I only have 30 minutes

---
layout: center
---

# This is an interactive talk!
Please interrupt, ask questions, contribute your own knowledge, and otherwise engage in this talk!

---
layout: center
---

# Why JavaScript Sucks

- Written in a week
- Unlimited backwards compatibility
- "The show must go on!"

---
layout: center
---

# Automatic Semicolon Insertion

---

# `return` over multiple lines

So egregious it's basically a bug in the language

```js{all|none}
function UCFStudent(name) {
  return {
    name: name,
    school: "UCF",
  }
} 
UCFStudent("Rob"); // { name: "Rob", school: "UCF" }
```
```js{none|all}
function UCFStudent(name) {
  return
  {
    name: name,
    school: "UCF",
  }
} 
UCFStudent("Rob") // undefined
```

<!--
- Even always using semicolons doesn't save you here
-->

---

# Indexes

Why you should probably use semicolons

You write:

```js{all|none}
const a = 5
[1, 2, 3].forEach(x => console.log(x))
```

JavaScript sees:

```js{none|all}
const a = 5[1, 2, 3].forEach(x => console.log(x))
// TypeError: Cannot read properties of undefined (reading 'forEach')
```

---

# Calls

Why you should probably use semicolons (part 2)

You write:

```js{none|all|none}
const numberInString = "1000"
(function() {
  const numberInStringTwice = numberInString + numberInString
  console.log(numberInStringTwice)
}())
```

JavaScript sees:

```js{none|all}
const numberInString = "1000"(function() { const numberInStringTwice = numberInString + numberInString; console.log(numberInStringTwice); }())
// TypeError: "1000" is not a function
```

---
layout: center
---

# Equality, identity, and type coercion


---

# `==`

Does it sorta, kinda look the same?

```js{all|none}
2 == 2;
2 == "2";
[2] == [2];
[2] == "2";
{ a: 2 } == { a: 2 };
undefined == null;
0 == false;
"0" == false;
["0"] == false;
["0"] == [false];
```

<v-click>

```js
2 == 2; // true
2 == "2"; // true
[2] == [2]; // false
[2] == "2"; // true
{ a: 2 } == { a: 2 }; // false
undefined == null; // true
0 == false; // true
"0" == false; // true
["0"] == false; // true
["0"] == [false]; // false
```

</v-click>

<!--
How many are true?
-->

---

# `===`

The patched version of `==`

```js
2 === 2; // true
2 === "2"; // false
[2] === [2]; // false
[2] === "2"; // false
{ a: 2 } === { a: 2 }; // false
undefined === null; // false
0 === false; // false
"0" === false; // false
["0"] === false; // false
["0"] === [false]; // false
```

---
layout: two-cols
---

# The evils of type coercion

<v-click>

```js
0.5.toString(); // '0.5'
0.05.toString(); // '0.05'
0.005.toString(); // '0.005'
0.0005.toString(); // '0.0005'
0.00005.toString(); // '0.00005'
0.000005.toString(); // '0.000005'
0.0000005.toString(); // '5e-7'
```

</v-click>

::right::

<div class="h-4/5 grid place-content-center m-8">
  <img src="/parseint-madness.png">
</div>

<!--
Usually your issues manifest like this.
-->

---
layout: center
---

# Two ways to do everything

---

# `null` vs `undefined`

Because two nothings are better than one

```js{all|none}
let person = null;
// later...
person = Person();
```

```js{none|all|none}
const obj = {};
obj.a; // undefined
```

```js{none|all}
fn(); // undefined
function fn(a) { 
  a; // undefined
  return;
};
```

<!--
`null` is for explicit nothingness:
`undefined` is more like a default value for things that haven't been set:
-->

---

# Default arguments

```js{all|1|none}
function greeting(name = "Rob") {
  return `Hello, ${name}`;
}
greeting(); // "Hello, Rob"
```

```js{none|all|2|none}
function greeting(name) {
  name = name || "Rob"
  return `Hello, ${name}`;
}
greeting(); // "Hello, Rob"
```

```js{none|all|2}
function greeting(name) {
  name = name ?? "Rob"
  return `Hello, ${name}`;
}
greeting(); // "Hello, Rob"
```

---

# Event handlers

```html{all|none}
<div onclick="f()"></div>
<script>
  function f() {
    console.log("div was clicked!");
  }
</script>
```
```js{none|all|none}
const div = document.querySelector("#some-id");
div.onclick = () => {
  console.log("div was clicked 1!");
};
div.onclick = () => {
  console.log("div was clicked 2!");
};
```
```js{none|all}
const div = document.querySelector("#some-id");
div.addEventListener("click", () => {
  console.log("div was clicked 1!");
});
div.addEventListener("click", () => {
  console.log("div was clicked 2!");
});
```

---

# Variable Declarations

Variables in JS are just like Python, right?

```js{all|none}
a = 5;
// window.a = 5;
```
```js{none|all|none}
function hello() {
  console.log(a); // undefined
  var a = 5;
}
```
```js{none|all|none}
function hello() {
  console.log(a); // error!
  let a = 5;
}
```
```js{none|all}
function hello() {
  console.log(a);
  const a = 5;
  a = 5; // error!
}
```

---

# Function declarations

Why use the actual `function` keyword?

```js{all|none}
function sum(a, b) {
  return a + b;
}
```
```js{none|all|none}
const sum = (a, b) => {
  return a + b;
};
```
```js{none|all}
const sum = function(a, b) {
  return a + b;
};
```

---

# Modules

```js{all|none}
// CommonJS/Node.js
const fetch = require("node-fetch");
```
```js{none|all|none}
// ES modules
import fetch from "node-fetch";
```
```js{none|all}
// some setups permit this horrible construction
import fetch = require("node-fetch");
```

---

# `class`

You don't really need it

```js
class Widget {
  constructor(a, b, c) {
    this.a = a;
    this.b = b;
    this.c = c;
  }
  someMethod() {
    return this.a + this.b * this.c;
  }
}
```
```js
class Widget {
  constructor(a, b, c) {
    this.a = a;
    this.b = b;
    this.c = c;
  }
  someMethod = () => {
    return this.a + this.b * this.c;
  }
}
```

---

# `class` (part 2)

```js
function Widget(a, b, c) {
  return {
    a,
    b,
    c,
    someMethod() {
      return this.a + this.b * this.c;
    }
  };
}
```
```js
function Widget(a, b, c) {
  return {
    a,
    b,
    c,
    someMethod: function() {
      return this.a + this.b * this.c;
    }
  };
}
```

---

# `class` (part 3)

```js
function Widget(a, b, c) {
  this.a = a;
  this.b = b;
  this.c = c;
  this.someMethod = function() {
    return this.a + this.b * this.c;
  }
}
```
```js
function Widget(a, b, c) {
  this.a = a;
  this.b = b;
  this.c = c;
  this.someMethod = () => {
    return this.a + this.b * this.c;
  }
}
```

---

# `class` (part 4)

```js
function Widget(a, b, c) {
  this.a = a;
  this.b = b;
  this.c = c;
}
Widget.prototype.someMethod = function() {
  return this.a + this.b * this.c;
}
```

---
layout: center
---

# Stupid Stuff

---

# `this`

When `globalThis` is a thing, you know you went wrong somewhere

```js{all|none}
const obj = {
  data: 42,
  logTheAnswer() {
    console.log(this.data);
  }
};
obj.logTheAnswer(); // 42, since this === obj
```
```js{none|all|none}
const f = obj.logTheAnswer;
f(); // undefined, since this === globalThis
```
```js{none|all|none}
const f = obj.logTheAnswer.bind(obj);
f(); // 42, since this === obj
```
```js{none|all}
const obj2 = {
  data: 42,
  logTheAnswer: () => {
    console.log(this.data);
  }
};
obj2.logTheAnswer(); // undefined, since this === globalThis
```

---

# Octal Literals
For all those times you need base 8 numbers

```js{all|5}
const a = 100;
const b = 010;
const c = 001;
console.log(a); // 100
console.log(b); // 8
console.log(c); // 1
```

---

# `Date`
One of these is not like the others...

```js{all|4}
// today is April 6, 2022
const today = new Date();
today.getDate(); // 6
today.getMonth(); // 3
today.getFullYear(); // 2022
```

---
layout: center
---

# Questions?
