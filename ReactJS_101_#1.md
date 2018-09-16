JSX Elements

-a basic unit of JSX 

-treated as JS expressions

-can be saved in a variable

-passed to a function

-stored in an object or array

Ex]

```
1.
const navBar = <nav>I am a nav bar</nav>

2.
const myTeam = {
    center:<li>Benzo Walli</li>,
    powerForward: <li>Rasha Loa</li>,
    smallForward: <li>Tayshaun Dasmoto</li>,
    shootingGuard: <li>Colmar Cumberbatch </li>
}
```

JSX elements can have attributes using HTML-like syntax; multiple attributes(name - value pairs) are possible

ex] my-attribute-name= "my-attribute-value"

```
const p = <p id = "large">foo</p>
```

You can *nest* JSX elements inside of other JSX elements, just like in HTML.

Here's an example of a JSX `<h1>` element, *nested* inside of a JSX `<a>` element:

```
<a href="https://www.example.com"><h1>Click me!</h1></a>
```

To make this more readable, you can use HTML-style line breaks and indentation:

```
<a href="https://www.example.com">
  <h1>
    Click me!
  </h1>
</a>
```

If a JSX expression takes up more than one line, then you must wrap the multi-line JSX expression in parentheses. This looks strange at first, but you get used to it:

```
const myDiv = (
  <div>
    <h1>
      Hello world
    </h1>
  </div>)
```

There's a rule that we haven't mentioned: a JSX expression must have exactly *one*outermost element.

In other words, this code will work:

```
const paragraphs = (
  <div id="i-am-the-outermost-element">
    <p>I am a paragraph.</p>
    <p>I, too, am a paragraph.</p>
  </div>
);
```

But this code will not work:

```
const paragraphs = (
  <p>I am a paragraph.</p> 
  <p>I, too, am a paragraph.</p>
);
```

The *first opening tag* and the *final closing tag*of a JSX expression must belong to the same JSX element!

 

Rendering JSX

To *render* a JSX expression means to make it appear onscreen.

```
import React from 'react';
import ReactDOM from 'react-dom';

// Copy code here:
ReactDOM.render(<h1>Hello world</h1>,  //first argument being passed to ReactDOM.render(); the argument should be a JSX expression
document.getElementById('app')); //2nd argument
```

ReactDOM : a JS library

`ReactDOM.render()` is the most common way to *render* JSX. 

It takes a JSX expression, creates a corresponding tree of DOM nodes, and adds that tree to the DOM. 

That is the way to make a JSX expression appear onscreen.

When ReactDOM.render() makes the first arugment appear on the screen, the 2nd argument, document.getElementById('app') selects the space where the 1st argument are to be appended. 

# Passing a Variable to ReactDOM.render()

```
1.
const myList = (<ul>
    <li>apple</li>
    <li>orange</li>
    <li>banana</li>
    <li>mango</li>
  </ul>);
ReactDOM.render(myList, document.getElementById('app'));

2.
const toDoList = (
  <ol>
    <li>Learn React</li>
    <li>Become a Developer</li>
  </ol>
);

ReactDOM.render(
  toDoList, 
  document.getElementById('app')
);
```



# The Virtual DOM

One special thing about `ReactDOM.render()`is that it *only updates DOM elements that have changed*.

That means that if you render the exact same thing twice in a row, the second render will do nothing:

```
const hello = <h1>Hello world</h1>;

// This will add "Hello world" to the screen:

ReactDOM.render(hello, document.getElementById('app'));

// This won't do anything at all:

ReactDOM.render(hello, document.getElementById('app'));
```

This is significant! Only updating the necessary DOM elements is a large part of what makes React so successful.

React accomplishes this thanks to something called *the virtual DOM.*

[the supplement here!!] 

[https://www.codecademy.com/articles/react-virtual-dom](https://www.codecademy.com/articles/react-virtual-dom)



# class vs className

In JSX, you can't use the word `class`! You have to use `className` instead:

This is because JSX gets translated into JavaScript, and `class` is a reserved word in JavaScript.

When JSX is *rendered*, JSX `className`attributes are automatically rendered as `class` attributes.



# Self-Closing Tags

In JSX, you *have to* include the slash. If you write a self-closing tag in JSX and forget the slash, you will raise an error:

```
Fine in JSX:
  <br /> 
  <input />
  <img />

NOT FINE AT ALL in JSX:
  <br>
  <input>
  <img>
```



# JavaScript In Your JSX In Your JavaScript

regular JavaScript, written inside of a JSX expression, written inside of a JavaScript file

Whoaaaa...

# Curly Braces in JSX

```
ReactDOM.render(
  <h1>2+3</h1>,
  document.getElementById('app')
);

//2+3  (NOT 5)
```

Instead of adding 2 and 3, it printed out "2 + 3" as a string of text. Why?

This happened because `2 + 3` is located in between `<h1>` and `</h1>` tags. Any code in between the tags of a JSX element will be read as JSX, not as regular JavaScript! JSX doesn't add numbers - it reads them as text, just like HTML.

You need a way to write code that says, "Even though I am located in between JSX tags, treat me like ordinary JavaScript and not like JSX."

You can do this by wrapping your code in *curly braces*.

```
ReactDOM.render(
  <h1>{2+3}</h1>,
  document.getElementById('app')
);
//5

const math = (<h1>2+3 = {2+3}</h1>);
ReactDOM.render(math, document.getElementById('app'));

//2+3=5
```

Everything inside of the curly braces will be treated as regular JavaScript.

The curly braces themselves won't be treated as JSX nor as JavaScript. They are *markers* that signal the beginning and end of a JavaScript injection into JSX



# Variables in JSX

When you inject JavaScript into JSX, that JavaScript is part of the same environment as the rest of the JavaScript in your file.

That means that you can access variables while inside of a JSX expression, even if those variables were declared on the outside.

```
// Declare a variable:
const name = 'Gerdo';

// Access your variable 
// from inside of a JSX expression:
const greeting = <p>Hello, {name}!</p>;
```

```
const theBestString = 'tralalalala i am da best';

ReactDOM.render(<h1>{theBestString}</h1>, document.getElementById('app'));

//tralalalala i am da best
```

# Variable Attributes in JSX

it's common to use variables to set *attributes*

```
// Use a variable to set the `height` and `width` attributes:

const sideLength = "200px";

const panda = (
  <img 
    src="images/panda.jpg" 
    alt="panda" 
    height={sideLength} 
    width={sideLength} />
);
```



*Object properties* are also often used to set attributes:

```
const pics = {
  panda: "http://bit.ly/1Tqltv5",
  owl: "http://bit.ly/1XGtkM3",
  owlCat: "http://bit.ly/1Upbczi"
}; 

const panda = (
  <img 
    src={pics.panda} 
    alt="Lazy Panda" />
);
const owl = (
  <img 
    src={pics.owl} 
    alt="Unimpressed Owl" />
);

const owlCat = (
  <img 
    src={pics.owlCat} 
    alt="Ghastly Abomination" />
);
```

```
const goose = 'https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-goose.jpg';

// Declare new variable here:
const gooseImg = (<img src={goose} />);
ReactDOM.render(gooseImg, document.getElementById('app'));
```



# Event Listeners in JSX

You create an event listener by giving a JSX element a special *attribute*. Here's an example:

```
<img onClick={myFunc} />
```

An event listener attribute's *value* should be a function. The above example would only work if `myFunc` were a valid function that had been defined elsewhere:

```
function myFunc() {
  alert('Make myFunc the pFunc... omg that was horrible i am so sorry');
}

<img onClick={myFunc} />
```

event listener names are written in camelCase, such as `onClick`or `onMouseOver`.

```
function makeDoggy(e) {
  // Call this extremely useful function on an <img>.
  // The <img> will become a picture of a doggy.
  e.target.setAttribute('src', 'https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-puppy.jpeg');
  e.target.setAttribute('alt', 'doggy');
}

const kitty = (
	<img onClick={makeDoggy},
		src="https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-kitty.jpg" 
		alt="kitty" />
);

ReactDOM.render(kitty, document.getElementById('app'));
```



# JSX Conditionals: If Statements That Don't Work

some simple ways to write *conditionals* (expressions that are only executed under certain conditions) in JSX

```
//this code will break
(
  <h1>
    {
      if (purchase.complete) {
        'Thank you for placing an order!'
      }
    }
  </h1>
)
```

# JSX Conditionals: If Statements That Do Work

one option is to write an `if` statement, and *not* inject it into JSX

This works because the words `if` and `else`are *not* injected in between JSX tags

```
let message;

if (user.age >= drinkingAge) {
  message = (
    <h1>
      Hey, check out this alcoholic beverage!
    </h1>
  );
} else {
  message = (
    <h1>
      Hey, check out these earrings I got at Claire's!
    </h1>
  );
}

ReactDOM.render(
  message, 
  document.getElementById('app')
);
```

```
let img;

if (coinToss() === 'heads') {
  img = (
    <img src={pics.kitty} />);
      
} else {
  img = (
    <img src={pics.doggy} />);
}

ReactDOM.render(
  img, 
  document.getElementById('app')
);
```



# JSX Conditionals: The Ternary Operator

x ? y : z

```
const headline = (
  <h1>
    { age >= drinkingAge ? 'Buy Drink' : 'Do Teen Stuff' }
  </h1>
);
```

```
function coinToss () {
  // Randomly return either 'heads' or 'tails'.
  return Math.random() < 0.5 ? 'heads' : 'tails';
}
//object properties to be used to set attributes
const pics = {
  kitty: 'https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-kitty.jpg',
  doggy: 'https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-puppy.jpeg'
};

const img = <img src={pics[coinToss()==='heads' ? 'kitty' : 'doggy']} />;

ReactDOM.render(
	img, 
	document.getElementById('app')
);
```

# JSX Conditionals: &&

works best in conditionals that will sometimes do an action, but other times do *nothing at all*

```
const tasty = (
  <ul>
    <li>Applesauce</li>
    { !baby && <li>Pizza</li> }
    { age > 15 && <li>Brussels Sprouts</li> }
    { age > 20 && <li>Oysters</li> }
    { age > 25 && <li>Grappa</li> }
  </ul>
);
```

```
// judgmental will be true half the time.
const judgmental = Math.random() < 0.5;

const favoriteFoods = (
  <div>
    <h1>My Favorite Foods</h1>
    <ul>
      <li>Sushi Burrito</li>
      <li>Rhubarb Pie</li>
      {!judgmental && <li>Nacho Cheez Straight Out The Jar</li>}
      <li>Broiled Grapefruit</li>
    </ul>
  </div>
);

ReactDOM.render(
	favoriteFoods, 
	document.getElementById('app')
);
//Once you click Run, then every time that you refresh the browser, there will be a 50% chance that judgmental will be true. Refresh until you see both versions of your list.
My Favorite Foods
Sushi Burrito
Rhubarb Pie
Nacho Cheez Straight Out The Jar
Broiled Grapefruit

My Favorite Foods
Sushi Burrito
Rhubarb Pie
Broiled Grapefruit
```

# .map in JSX

If you want to create a list of JSX elements, then `.map()` is often your best bet. It can look odd at first:

```
const strings = ['Home', 'Shop', 'About Me'];

const listItems = strings.map(string => <li>{string}</li>);

<ul>{listItems}</ul>
```

we start out with an array of strings. We call `.map()` on this array of strings, and the `.map()` call returns a new array of `<li>`s.

On the last line of the example, note that `{listItems}` will evaluate to an array, because it's the returned value of `.map()`! 

```
const people = ['Rowe', 'Prevost', 'Gare'];

const peopleLis = people.map(person =>
  // expression goes here:
  <li>{person}</li>
);

// ReactDOM.render goes here:
ReactDOM.render(<ul>{peopleLis}</ul>, document.getElementById('app'));
```

Output:

Rowe

Prevost

Gare

# Keys

When you make a list in JSX, sometimes your list will need to include something called `keys`:

```
<ul>
  <li key="li-01">Example1</li>
  <li key="li-02">Example2</li>
  <li key="li-03">Example3</li>
</ul>
```

A `key` is a JSX attribute. The attribute's *name* is `key`. The attribute's *value* should be something unique, similar to an `id` attribute.

`keys` don't do anything that you can see! React uses them internally to keep track of lists. If you don't use keys when you're supposed to, React might accidentally scramble your list-items into the wrong order.

Not all lists need to have `keys`. A list needs `keys` if either of the following are true:

```
const people = ['Rowe', 'Prevost', 'Gare'];

const peopleLis = people.map((person, i) =>
  // expression goes here:
  <li key={'person_' + i}>{person}</li>
);

// ReactDOM.render goes here:
ReactDOM.render(<ul>{peopleLis}</ul>, document.getElementById('app'));
```

1. 
2. The list-items have *memory* from one render to the next. For instance, when a to-do list renders, each item must "remember" whether it was checked off. The items shouldn't get amnesia when they render.
3. The list-items have *memory* from one render to the next. For instance, when a to-do list renders, each item must "remember" whether it was checked off. The items shouldn't get amnesia when they render.
4. The list-items have *memory* from one render to the next. For instance, when a to-do list renders, each item must "remember" whether it was checked off. The items shouldn't get amnesia when they render.
5. A list's order might be shuffled. For instance, a list of search results might be shuffled from one render to the next.
6. The list-items have *memory* from one render to the next. For instance, when a to-do list renders, each item must "remember" whether it was checked off. The items shouldn't get amnesia when they render.
7. A list's order might be shuffled. For instance, a list of search results might be shuffled from one render to the next.
8. The list-items have *memory* from one render to the next. For instance, when a to-do list renders, each item must "remember" whether it was checked off. The items shouldn't get amnesia when they render.
9. A list's order might be shuffled. For instance, a list of search results might be shuffled from one render to the next.
10. The list-items have *memory* from one render to the next. For instance, when a to-do list renders, each item must "remember" whether it was checked off. The items shouldn't get amnesia when they render.
11. A list's order might be shuffled. For instance, a list of search results might be shuffled from one render to the next.
12. The list-items have *memory* from one render to the next. For instance, when a to-do list renders, each item must "remember" whether it was checked off. The items shouldn't get amnesia when they render.
13. A list's order might be shuffled. For instance, a list of search results might be shuffled from one render to the next.
14. The list-items have *memory* from one render to the next. For instance, when a to-do list renders, each item must "remember" whether it was checked off. The items shouldn't get amnesia when they render.
15. A list's order might be shuffled. For instance, a list of search results might be shuffled from one render to the next.

The list-items have *memory* from one render to the next. For instance, when a to-do list renders, each item must "remember" whether it was checked off. The items shouldn't get amnesia when they render.

2.

A list's order might be shuffled. For instance, a list of search results might be shuffled from one render to the next.

# React.createElement

The majority of React programmers do use JSX, and we will use it for the remainder of this tutorial, but you should understand that it is possible to write React code without it.

The following JSX expression: 

```
const h1 = <h1>Hello world</h1>;

//can be rewritten without JSX, like this:
const h1 = React.createElement(
  "h1",
  null,
  "Hello world"
);
```

```
const greatestDivEver = <div>i am div</div>;

const greatestDivEver = React.createElement(
  "div",
  null,
  "i am div"
);
```



# THE COMPONENT

a *React component* is a small, reusable chunk of code that is responsible for one job, which often involves rendering HTML

```
//import the React library and save the library in a variable named React
import React from 'react';
import ReactDOM from 'react-dom';

//declare a new component class, 'MyComponentClass', for building React components
//React.component is a class, NOT a component --> from here, you must subclass in //order to create a component class of your own
//React.component is a property on the object which was returned by 
//import React from 'react'
class MyComponentClass extends React.Component {
  render() {
    return <h1>Hello world</h1>;
  }
}

ReactDOM.render(
	<MyComponentClass />, 
	document.getElementById('app')
);
```

1. line 1 : creates a new variable, 'React', and its value is a particular, imported JS object  -> the object contains properties needed to make React work, such as React.createElement() and React.Component

2. The imported object contains methods that is needed to use React

3. the object is called the React library

4. so line 1 means your get the React library via import React from 'react';

   

React.createElement() call - one of the methods in the React library

- when a JSX element is compiled, it tranforms into a React.createElement() call

For this reason, you *have to* import the React library, and save it in a variable named `React`, before you can use any JSX at all.`React.createElement()` must be available in order for JSX to work.



5. line 1 and 2 both import JS objects: in both lines, the imported object contains React-related methods. 
6. import ReactDOM from 'react-dom' creates another JS object - the object contains methods that help React interact with the DOM, such as ReactDOM.render()
7. The methods imported from `'react-dom'`are meant for interacting with the DOM. You are already familiar with one of them:`ReactDOM.render()`. The methods imported from `'react'` don't deal with the DOM at all.
8. To clarify: the DOM is *used* in React applications, but it isn't *part* of React. After all, the DOM is also used in countless non-React applications. 
9. Methods imported from `'react'` are only for pure React purposes, such as creating components or writing JSX elements.



# Create a Component Class

every component must come from a *component class*

To make a component class, you use a base class from the React library:`React.Component`.

What *is* `React.Component`, and how do you use it to make a component class?

`React.Component` is a JavaScript *class*. **To create your own component class, you must *subclass* `React.Component`. You can do this by using the syntax `class YourComponentNameGoesHere extends React.Component {}`.**

# Name a Component Class

Subclassing `React.Component` is the way to declare a ***new*** *component class*.

Component class variable names must begin with capital letters! (This adheres to JavaScript's class syntax. It also adheres to a broader programming convention in which [class names are written in UpperCamelCase](https://en.wikipedia.org/wiki/Naming_convention_(programming)#Java).)

body of the component class declaration:

-act as a set of instructions

-explains the component class about how it should build a React component

# The Render Function

Instructions should be written in typical JS ES2015 class syntax

there is only one property that is included in your instructions: a render method

A render method is a property whose *name*is `render`, and whose *value* is a function. The term "render method" can refer to the entire property, or to just the function part.

```
classg ComponentFactory extends React.Component {
    render(){}
}
```

A render method must contain a `return`statement. Usually, this `return` statement returns a JSX expression:

```
class MyComponentClass extends React.Component {
  render() {
    return <h1>Hello world</h1>;
  }
}
```

# Create a Component Instance

`MyComponentClass` is now a working *component class!* It's ready to follow its instructions and make some React components.

So, let's make a React component! It only takes one more line:

```
<MyComponentClass />
```

To make a React component, you write a *JSX element.* Instead of naming your JSX element something like `h1` or `div` like you've done before, give it the same name as a *component class*. Voilà, there's your *component instance!*

JSX elements can be either HTML-like, or *component instances*. JSX uses capitalization to distinguish between the two! *That* is the React-specific reason why component class names must begin with capital letters. In a JSX element, that capitalized first letter says, "I will be a component instance and not an HTML tag."

# Render A Component

When you make a component by using the expression `<MyComponentClass />`, what do these instructions do?

Whenever you make a component, that component *inherits* all of the methods of its component class. `MyComponentClass` has one method: `MyComponentClass.render()`. Therefore, `<MyComponentClass />` also has a method named `render`.

You could make a million different `<MyComponentClass />` instances, and each one would inherit this same exact `render`method.

Since your component has a render method, all that's left to do is call it.

To call a component's `render` method, you pass that component to `ReactDOM.render()`. Notice your component, being passed as `ReactDOM.render()`'s first argument:

```
ReactDOM.render(
  <MyComponentClass />,
  document.getElementById('app')
);
```

`ReactDOM.render()` will tell `<MyComponentClass />` to call *its* render method.

`<MyComponentClass />` will call its render method, which will return the JSX element `<h1>Hello world</h1>`. `ReactDOM.render()`will then take that resulting JSX element, and add it to the virtual DOM. This will make "Hello world" appear on the screen.

# Use Multiline JSX in a Component

A multi-line JSX expression should always be wrapped in parentheses! 

```
import React from 'react';
import ReactDOM from 'react-dom';

class QuoteMaker extends React.Component{
  render(){
    return (
      <blockquote>
        <p>What is important now is to recover our senses.</p>
        <cite>
          <a target="_blank" href="https://en.wikipedia.org/wiki/Susan_Sontag">
          </a>
        </cite>
      </blockquote>  
    );
  }
};

//to render the HTML nested in the boby class declaration, ReactDOM.render() and a // React component to be used
ReactDOM.render(
  <QuoteMaker />,
  document.getElementById('app')
);
```

# Use a Variable Attribute in a Component

You can, and often will, inject JavaScript into JSX inside of a render function. 

-> the curly-brace JavaScript injections inside of the render function ; src, alt, width

```
class RedPanda extends React.Component {
  render() {
    return (
      <div>
        <h1>Cute Red Panda</h1>
        <img 
          src={redPanda.src}
          alt={redPanda.alt}
          width={redPanda.width} />
      </div>
    );
  }
}
```

```
import React from 'react';
import ReactDOM from 'react-dom';

const owl = {
  title: 'Excellent Owl',
  src: 'https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-owl.jpg'
};

// Component class starts here:
class Owl extends React.Component {
  render(){
    return (
      <div>
        <h1>{owl.title}</h1>
        <img src={owl.src} alt={owl.title} />
      </div>
    );
  }
}

ReactDOM.render(
  <Owl />,
  document.getElementById('app')
);
```

Inside of the `<h1></h1>`, put `owl.title`. Remember, you will have to use curly braces to access `owl.title`, since it is a JavaScript property.

Don't forget to wrap the whole multiline JSX expression in parentheses!

the values of attributes should be wrapped by {}

# Put Logic in a Render Function

A `render()` function can also be a fine place to put simple calculations that need to happen right before a component renders. Here's an example of some calculations inside of a `render` function:

```
class Random extends React.Component {
  render() {
    // First, some logic that must happen
    // before rendering:
    const n = Math.floor(Math.random() * 10 + 1);
    // Next, a return statement
    // using that logic:
    return <h1>The number is {n}!</h1>;
  }
}
```

```
import React from 'react';
import ReactDOM from 'react-dom';

const friends = [
  {
    title: "Yummmmmmm",
    src: "https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-monkeyweirdo.jpg"
  },
  {
    title: "Hey Guys!  Wait Up!",
    src: "https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-earnestfrog.jpg"
  },
  {
    title: "Yikes",
    src: "https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-alpaca.jpg"
  }
];

// New component class starts here:
class Friend extends React.Component {
  render(){
    const friend = friends[0]
    return (
      <div>
        <h1>{friend.title}</h1>
        <img src={friend.src} />
      </div>
    );
  }
};

ReactDOM.render(
  <Friend />,
  document.getElementById('app')
);
```

# Use a Conditional in a Render Function

Notice that the `if` statement is located *inside* of the render function, but *before* the `return` statement. 

```
import React from 'react';
import ReactDOM from 'react-dom';

class TodaysPlan extends React.Component {
  render() {
    let task;
    if (!apocalypse) {
      task = 'learn React.js'
    } else {
      task = 'run around'
    }

    return <h1>Today I am going to {task}!</h1>;
  }
}
```

```
import React from 'react';
import ReactDOM from 'react-dom';

// New component class starts here:
class TonightsPlan extends React.Component {
  render() {
    const fiftyFifty = Math.random() < 0.5;
    if(fiftyFifty){
      return <h1>Tonight I'm going out WOOO</h1>
    } else {
      return <h1>Tonight I'm going to bed WOOO</h1>
    }
  }
}

ReactDOM.render(
  <TonightsPlan />,
  document.getElementById('app')
);
```

# Use this in a Component

What does *this* mean?

```
class IceCreamGuy extends React.Component {
  get food() {
    return 'ice cream';
  }

  render() {
    return <h1>I like {this.food}.</h1>;
  }
}
```

`this` refers to <u>*an instance of*</u> `IceCreamGuy`

- a new component class, IceCreamGuy, is declared --> IceCreamGuy is an instance 

`this` refers to the object on which `this`'s enclosing method, in this case `.render()`, is called (= the object calls the method, .render(), and this inside .render() looks to the object ; this를 포함하고 있는 method를 부르는 객체 ; 객체가 method를 콜할 때 method 안에 있는 this는 그 객체(the new instance in this case)를 참조함

The **get** syntax binds an object property to a function that will be called when that property is looked up.

```
{get prop() { ... } } //prop:the name of the property to bind to the given function
```

```
import React from 'react';
import ReactDOM from 'react-dom';

class MyName extends React.Component {
	// name property goes here:
  get name () {
    return 'whatever-your-name-is-goes-here';
  }

  render() {
    return <h1>My name is {this.name}.</h1>;     
  }
}

ReactDOM.render(<MyName />, document.getElementById('app'));
```

this refers to MyName

# Use an Event Listener in a Component

In React, you define event handlers as *methods* on a component class.

```
class MyClass extends React.Component {
  myFunc() {
    alert('Stop it.  Stop hovering.');
  }

  render() {
    return (
      <div onHover={this.myFunc}>
      </div>
    );
  }
}
```

Notice that the component class has two methods: `.myFunc()` and `.render()`.`.myFunc()` is being used as an *event handler and the attribute, onHover's value.  `.myFunc()` will be called any time that a user hovers over the rendered `<div></div>`. 

```
import React from 'react';
import ReactDOM from 'react-dom';

class Button extends React.Component {
  scream() {
    alert('AAAAAAAAHHH!!!!!');
  }

  render() {
    return <button onClick={this.scream}>AAAAAH!</button>;
  }
}

ReactDOM.render(
  <Button />,    document.getElementById('app')
);
```

the button tag has onClick attribute and the attribute's value is .scream() method

