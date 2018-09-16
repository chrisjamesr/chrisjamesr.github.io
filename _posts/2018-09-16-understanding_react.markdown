---
layout: post
title:      "Understanding React"
date:       2018-09-16 18:19:38 +0000
permalink:  understanding_react
---


### React as Declarative

   The React.js library utilizes a declarative programming paradigm to control the user interface and update views for an application or website. Libraries like JQuery or even vanilla JS, when implemented to manipulate an 
application's state, utliize an imperative programming paradigm. In such a case, a program will make changes using step-by-step commands to find, manipulate, add, or remove DOM elements. The sequence of these commands determines 
the control flow of an application and dictate precisely **how** an objective should be completed from beginning to end. In contrast React is used to build a component tree wherein any changes are tracked and the DOM is updated to 
match a virtual representation of the DOM created by React. Instead of going through the motions of selecting and modifying elements, the React library compares the existing DOM's state to the virtual DOM that it maintains and 
replaces any changed elements as needed. For this to work effectively the return values of functions must be consistent and independent of external factors.

### Pure Functions

   All components in React are treated like pure functions. A function is considered pure when it will return the same result every time it is passed the same parameters. Pure functions do not modify or rely on variables outside of their scope. Changes to external variables or context can affect a function's ability to reliably return the expected result. 
#### Impure      
    let x = 0
    function addOne(){
      x++
      return x
    }

    addOne()                             // => 1
    addOne()                             // => 2
    addOne()                             // => 3
    x                                    // => 3
   The return value of addOne() is unreliable. addOne() requires an existing global variable x and modifies it. Everytime the function is called it will return a different result that depends on the value of x, which resides in the global scope accessible across the entire application. The change to the external application state is called a side effect and can make the result of other functions unpredictable, as they may share use of that global variable. Sometimes side effects are unavoidable like in the case of logging, timers, or obtaining information from third party sources.
#### Pure
    function addOnePure(x){
  	x++
	  return x
    }
    addOnePure(x)                       // => 1
    addOnePure(x)                       // => 1
    addOnePure(addOnePure(x))           // => 2
    x                                   // => 0
Function addOnePure() is explicitly passed an argument x. Due to how variables are treated in JS, the variable x points to the number 0. That same value is passed in every time the function is called so it will never increment outside of the function scope. Everytime the function addOnePure() is called it will produce the same result. Due to the consistency of the return values and the absence of side effects, pure functions and specifically React components can be combined modularly in different arrangements and easily passed to other components to assemble larger more complex components. This characteristic is called composability, and when combined with the predictability of utilizing pure functions and components they create a system that can be easily controlled by careful management of the input provided.     			

### Component State & Props
React components have two primary data sources, props and state. "Props", short for "properties", is an object passed manually from a parent to a child component. It contains information that can determine the component's internal state and/or be passed down to components nested within. Any operations performed on incoming props should not modify the props object directly as this violates Reacts top-down data flow philosophy. Unlike props, a component's state is mutable. Generally state is used to store any information about the component's or child components' appearance or to store user input. The passage of props and changes to a component state dictate the control flow of a React application. The unidirectional data flow, composability, and purity of React components creates a mechanism that will only respond new props or changing state. The response happens synchronously in the form of methods that are called automatically when triggered by new props or state changes. 

### Component Lifecycle

React components have hooks or methods that are called at different points in a component's lifecycle to operate on the available state or props. These hooks occur during a components initial mounting, any subsequent updating, and unmounting when it is removed from the DOM.  

#### Mounting 
   *Mounting occurs when a component is first being added to the DOM*

* constructor()  -  
*Initilize local state and bind event handlers. This method is only called during component creation, before it appears in the DOM. Will not be called for any subsequent updates.*

* getDerivedStateFromProps(props, state)  -  
*This method is used to check and/or update a component's state based on the current props. It is called prior to **every** render() call. The return value will be merged into the current component state.*

* render()  - *The render method is called during initial mounting and for every update. It returns whatever elements are   
     contained within, as determined by the current component state and props, to be appended to the DOM.* 

* componentDidMount() -  Called after component has mounted and used to initiate any additional actions to load data. 

#### Updating 
   *Updating occurs anytime a component receives new props, setState(), or forceUpdate() methods are called.* 

* getDerivedStateFromProps(props, state)  -  *Same as above*

* shouldComponentUpdate(nextProps, nextState)  -  *Called prior to render(). Returns a boolean based on comparing 
     current state/props to the nextProps/nextState and can prevent a component from rerendering.*

* render()  -  *Same as above*

* componentDidUpdate(prevProps, prevState)  -  *Method is called immediately after render. Can be used to setState or 
     perform other actions based  on the new props/state.* 
		 
#### Unmounting 
   *When a component is being removed from the DOM it is unmounting.*

* componentWillUnmount()  - *Called immediately before component is removed from the DOM. This method is used to 
     clean up any event listeners, or DOM elements created upon initial mount.*

Ultimately, the implementation of a declarative approach to programmming combined with the top-down data flow of props and state, and the built-in lifecycle methods create an extremely adaptive application that is easy to maintain, modify, and augment.  
