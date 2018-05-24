---
layout: post
title:      "Hoisting, Scope, and Variable Declaration"
date:       2018-05-23 21:06:52 -0400
permalink:  hoisting_scope_and_variable_declaration
---


Modern Javascript code is converted into machine code in the browser prior to execution. During this process, variable and function declarations in the code are registered to the global or local scope in which they were declared. This is commonly known as ‘hoisting’. Hoisting is often described as if the browser engine were actually relocating the declarations to the very top of the scope. In truth the browser’s initial pass over the code identifies and saves this information so that when the code is executed these declarations are already stored in memory.
	The scope of a variable or function is determined by where and how it is declared. The recommended practice is to initialize all variables at the beginning of the scope ahead of any operations.  When a variable is initialized without a value it will return  ‘undefined’ until it is explicitly assigned a value.

Any variable declared inside of a function is considered a local variable. The variable declaration is hoisted within the confines of the function and it is only available within that function. A variable declared outside of a function is considered a global variable. Global variables are accessible within local scopes but the local scope will take precedence over global scope. In the example below the variable ‘a’ is initialized with the value “Global!” and then a different variable also named ‘a’ is initialized with a different value. Each variable logs its assigned value to the console and the final call to ‘a’ outside the function will log the same value assigned in the global scope outside of the function, at the top of the example. 

	var a = "Global!"
	console.log(a)                  -> “Global!”
	function someThing(){
		var   a = "Local"
		console.log(a)              -> “Local”
	}
	console.log(a)                  -> “Global!”

If we remove the initialization inside the function, the value of the global variable ‘a’ is logged in the console. When variable ‘a’ is called in this example, the variable declared within the local scope takes precedence over the global variable of the same name. When a variable is called and there is not a local variable declaration, the javascript engine will then look to the outer scope for the requested variable 

	var a = "Global!"
	console.log(a)                     -> “Global!”  // Global Scope
	function someThing(){
		// var   a = "Local"
		console.log(a)                 -> “Global!”  // Local Scope someThing
		return function(){
					console.log(a)     -> “Global!”  // Local Scope anonymous function	
		}                                                   nested within someThing
		console.log(a)                 -> “Global!”  // Local Scope someThing
	}

Like variable declarations, function declarations and expressions will be hoisted within their respective scope. A function expression is when a named or anonymous function is assigned as a value. As in variable declarations, the value of a function variable will not be hoisted and retains a value of ‘undefined’ until it is assigned in the code. Function declarations operate slightly differently. A function declaration is a named function that will be hoisted along with the entire function body.   

	var a = "Global!"
	someThing()                      ->  "Global!"
	console.log(otherThing)          ->  "undefined"
		otherThing()                 ->  "TypeError otherThing is not a  function            
	function someThing(){            -> function declaration
		console.log(a)    
	}
	var otherThing = function(){     -> function expression
		console.log(a)
	}
	otherThing()                     -> "Global!"

In the example above, the function declaration someThing is hoisted along with all of the logic contained within the function block while the function expression assigned to the variable otherThing has a value ‘undefined’. It cannot be called as a function until the variable is assigned the function expression. 

In addition to the ‘where’ of variable declaration and how it relates to scope, there are several methods for variable declaration.  The keyword ‘var’ assigns a variable to the scope in which it was declared and hoisted as has been discussed thus far. Other methods ‘let’ and ‘const’ are more widely used for variable declaration and have a few more rules regarding their behavior. 

Foremost, unlike ‘var’, neither ‘let’, nor ‘const’ are initialized with the value ‘undefined’. Calling one of these variables prior to assignment will result in a ‘ReferenceError’. As with ‘var’, 'let' and 'const' are bound to the scope in which they are declared, but unlike ‘var’ they are also ‘block scoped’ which is a way of saying that these variables will be bound within any block statement like 'if' or 'for' as well as inside function.

	let i = 1
	if (i > 0) {
	 var b = "Yes" 
	 let a = "Hello!"  
	 const c = "Goodbye"
	 console.log(b)               ->  “Yes”
	 console.log(a)               ->  “Hello!”
	 console.log c = "Goodbye"
	}
	console.log(b)                ->  “Goodbye”
	console.log(a)                ->  "Reference Error: a is not defined"
	console.log(c)                ->  "Reference Error: c is not defined"

There is another significant difference between variables declared with 'let' and 'const'. Variables declared with ‘let’ can be declared without a value and reassigned. Variables declared with ‘const’ must be initialized with a value as the value cannot be changed. 

	let i = "Yes" 
	console.log(i)          -> “Yes”
	i = "No"
	console.log(i)          -> “No”
	 Versus
	const a;                -> Cannot be reassigned and will return a ReferenceError 
		if called
	const i = "Yes"
	console.log(i)          -> “Yes”
	i = "No"                -> “SyntaxError: Assignment to constant variable”

If a variable initialized using const has a value that is an object or array, the contents of that data structure can be modified but the reference to the structure itself cannot be changed.
	

	const a = []            ->  ”[]”
	a.push(1)	           ->  ”[1]” 	 
	a.push(2)	           ->  “[1,2]”
	a.push(3)               ->  “[1,2,3]”
	a.name = "chris"        ->  "[1,2,3, name: 'chris']
	delete a.name           ->  “[1,2,3]”
	a.pop()	             ->  “[1,2]”	
	a.pop()                 ->  ”[1]” 	 
	a.pop()	             ->  ”[]”	
	a = [1,2,3]             ->  ”SyntaxError: Assignment to constant variable”

	
Hoisting and scope are two integral parts of Javascript mechanics. The 'let' and 'const' methods of variable declaration are relatively new to javascript. Using them correctly allows for more precise code and can prevent variables from leaking out of their scope.  
	



