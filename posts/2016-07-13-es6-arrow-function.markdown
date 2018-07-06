---
layout: post
title:  "ES6: Arrow function"
date:   2016-07-13 09:18:37 +1000
categories: es6 tutorial
---

notes: This is part of the series [Dive in ES6](./dive-in-es6.html)

# Arrow function to rescue

So we have arrow function now ```()=>{}```. But why?

## Prerequisite

How to we write class in js?

**We DON'T, it is bad code smell, do Functional Reactive Programming (ideally)**

**OR at lease, try to understand prototypical inheritance first [JS Objects: De"construct"ion](https://davidwalsh.name/javascript-objects-deconstruction)**

In reality, the following is one of the many ways

```javascript
function ClassA(name) {
	this.name = name;
}
ClassA.prototype.hello = function() {
	console.log('hello from ', this.name);
}
new ClassA('instanceA').hello();
```

In the real world, especially within html DOM, things getting messier

## ```this``` is a problem.

Take a look at the following:

<button id="clickme">clickme</button>

```javascript
window.Person1 = function() {
	this.personName = 'personA';
}
window.Person1.prototype.hookToButton = function(id) {
	document.querySelector('#'+id).addEventListener('click', function(){
		alert('hello, I am ' + this.personName);
	});
}
new window.Person1().hookToButton('clickme');
```

One of the way to work around is to assign ```this``` to a local variable

<button id="clickme2">clickme2</button>

```javascript
window.Person2 = function() {
	this.personName = 'personA';
}
window.Person2.prototype.hookToButton = function(id) {
	var self = this;
	document.querySelector('#'+id).addEventListener('click', function(){
		alert('hello, I am ' + self.personName);
	});
}
new window.Person2().hookToButton('clickme2');
```

Another workaround is to use the ```.bind()```

<button id="clickme3">clickme3</button>

```javascript
window.Person3 = function() {
	this.personName = 'personA';
}
window.Person3.prototype.hookToButton = function(id) {
	var self = this;
	document.querySelector('#'+id).addEventListener('click', function(){
		alert('hello, I am ' + self.personName);
	}.bind(this));
}
new window.Person3().hookToButton('clickme3');
```

**NOW we have arrow function**

<button id="clickmeArrow">clickmeArrow</button>

```javascript
window.PersonArrow = function() {
	this.personName = 'personA';
}
window.PersonArrow.prototype.hookToButton = function(id) {
	document.querySelector('#'+id).addEventListener('click', () => {
		alert('hello, I am ' + this.personName);
	});
}
new window.PersonArrow().hookToButton('clickmeArrow');
```

**Key takeaway is: ()=>{} will bind ```this``` to the scope where it is defined (lexical scope)**

## There're more about arrow function

***GOOD*** developers are lazy, arrow function ```()=>{}``` save you a lot of key strokes

```javascript
function arrowAwesome() {
	return {
		a: name => {
			return 'hello ' + name
		},
		b: name => 'hello ' + name,
		c: (name, age) => {
			return {
				name: 'hello ' + name, age: age
			}
		},
		d: (name, age) => ({name: 'hello ' + name, age: age})
	}
}

console.log('', arrowAwesome().a('shine'));
console.log('', arrowAwesome().b('shine', 99));
console.log('', arrowAwesome().c('shine', 99));
console.log('', arrowAwesome().d('shine', 99));

```

