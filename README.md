# REACT NOTEPAD

## Basics

React is made of 2 packages, *react* which has the logic to crate components, and *react dom* which is in charge of rendering those components to the real DOM.

### Projects workflow

In order to write and manage efficiently your code generally you'll need more than just React.
- Dependency Management tool - **npm** or **yarn**
- Bundler: group different imports in a single file and can also do other work during the build process - **Webpack**
- Compiler: translate **jsx** into javascript and code written using last generation javascript into older versions of javascript - **Babel**

When creating a new project it's recommended doing so using a tool called **create-react-app**.

### Folder structure

- **package.json** - list of dependencies and command line scripts
- **node_modules/** - actual dependencies files, generated automatically
- **public/** - root folder served by the webserver
- **public/index.html** - in a Single Page Application it will be the only html file actually present
- **src/** - script files folder
- **src/index.js** - in a SPA it will wire the main component to the root element defined in the public/index.html file

### JSX

It allows us to write more readable code, without it we would have to build and compose elements manually using `React.createElement()`.

This javascript
```javascript
React.createElement('div', {className: 'App'}, React.createElement('h1', null, 'Ciao'));
```
can be written like this in jsx
```jsx
<div className="App">
    <h1>Ciao</h1>
</div> 
```

Then it will require a compiler like **Babel** in order to be translated in javascript.

A few additional notes:
- the elements are not *html* even if they look a lot like it, they are provided by React
- *class* is a reserved word so React provide *className* to use instead
- the jsx code must be written inside a single root element you return (like a *div*)

## Components

Components should return the html/javascript (or jsx code if using Babel) of the component, in case of *class components* this will come from the render() method.

### Basic usage

```html
<div id="p1"></div>
```
```jsx
function Person(props) {
    return (
        <div className="person">
            <h1>{props.name}</h1>
            <p>Anni: {props.age}</p>
        </div>
    );
}

ReactDOM.render(<Person name="Max" age="28" />, document.querySelector('#p1'));
```

### Class components

The most common way to define a component.

```jsx
import React, { Component } from 'react';

class App extends Component {
    render() {
        return (
            <div className="App">
                <h1>Ciao</h1>
            </div>
        );   
    }
}

export default App;
```

### Functional components

```jsx
// Person.js
import React from 'react';

const person = () => {
    return <p>Ciao</p>
}

export default person;
```
When we'll use it we'll need to be import it using a capital name `import Person from 'Person';`

### Props

Values you pass to your components.
```jsx
// App.js
//...
import Person from 'Person';

class App extends Component {
    render() {
        return (
            <div className="App">
                <Person name="Pippo" age="33">Eheh</Person>
            </div>
        );
    }
}

export default App;
```
```jsx
// Person.js
import React from 'react';

const person = (props) => {
    return (
        <div>
            <p>Ciao {props.name} hai {props.age} anni</p>
            <p>{props.children}</p>
        </div>
    )
};

export default person;
```
Please note in case of class-based components you access props with `this.props`

## State

### Inside a class component

To update a state use the built in method `setState()`, do not change the *state* property directly.
With class components the object you pass to setState() merge the new object with the old one, this means it doesn't delete elements present in the previous state, but missing in the new one.
```jsx
//...
class App extends Component {
    state = {
        persons: [
            { name: 'Pippo', age: 33 },
            { name: 'Pluto', age: 34 }
        ],
        otherState: 'blabla'
    }
    
    switchNameHandler = () => {
        this.setState({
            persons: [
                { name: 'Bah', age: 30 },
                { name: 'Mah', age: 1 }
            ]
        })
    }
//...
```

### Inside a functional component

In this case we must use the `useState()` hook, this return an array of 2 elements:
1. The current state
2. A function that allows us to update the state

Unlike class components, with functional components the object you pass to useState() **doesn't merge** with the existing one, but it completely overwrites it.

Because of this we have to use multiple useState(), unless we want to manually merge the previous state every time we update it.
```jsx
import React, { useState } from 'react';

const App = props => {
    const [personState, setPersonState] = useState({
        persons: [
            { name: 'Pippo', age: 33 },
            { name: 'Pluto', age: 34 }
        ]
    });
    const [otherState, setOtherState] = useState('blabla');

    const switchNameHandler = () => {
        setPersonState({
            persons: [
                { name: 'Bah', age: 30 },
                { name: 'Mah', age: 1 }
            ]
        })
    };
    //...
}
```

## Styling

The easier way to apply styles is to do it globally, this is done through CSS files as normal.

If you need to dynamically change the style you can dynamically assign classes to html elements.  
In this example the css styling for the classes *red* and *bold* are defined in the global css file.
```jsx
const classes = [];
if (true) {
    classes.push('red');
}
if (true) {
    classes.push('bold');
}

return (
    <p className={classes.join(' ')}>bla bla</p>
)
```

### In line

Easy but you can't use **pseudo selectors**.
```jsx
//...
class App extends Component {
    render () {
        const style = {
            backgroundColor: 'white',
            border: '1px solid blue'
        };

        return (
            <div className="App">
                <p style={style}>Ciao</p>
            </div>    
        )
    }
}
//...
```

### Radium (library)

If you need both in-line styles and **pseudo selectors** you can use a tool called **radium** (install with `npm install --save radium`).

To use it simply import it and then call it as an high order function while exporting a class component or a function component:
```jsx
import React, { Component } from 'react';
import Radium from 'radium';

class App extends Component {
    render() {
        const style = {
            backgroundColor: 'green',
            ':hover': {
                backgroundColor: 'red'
            }
        }
    
        return (
            <div className="App">
                <button style={style}></button>
            </div>
        );
    }
}

export default Radium(App);
```

When using **transforming selectors** like **media queries** we need to wrap the entire application in a special component called **StyleRoot**.

```jsx
import Radium, { StyleRoot } from 'radium';
//...
class App extends Component {
    render() {
        return (
            <StyleRoot>
                <div className="App">
                    //...
                </div>
            </StyleRoot>
        );
    }
}
```

### Styled Components (library)

Install with `npm install --save styled-components`.

Usage:
- import the styled-component component
- call the html element you want to render/style as a function of the styled component (ex. `styled.div`)
- follow the function name with a **tagged template literal** (two backticks ``)
- inside the backticks write your css.

A nice way to use it is to store the result of the method call of the styled component inside a variable and use it *as a component*, but without props.
```javascript
import React from 'react';
import styled from 'styled-components';

const StyledDiv = styled.div`
    width: 60%;
           
    @media (min-width: 500px) {
        width: 450px;
    }
`;

const person = (props) => {
    return (
        <StyledDiv>
            //...
        </StyledDiv>
    );
}
export default person;
```
Remember to use regular CSS in the tagged template, not camel cased.

To dynamically compose the style you can feed it with props, they are then accessible from the inside of the template literal.
```javascript
//...
const StyledButton = styled.button`
    background-color: ${props => props.somePropName ? 'red' : 'green'};
`;
//...
<StyledButton 
    somePropName={true}
>
    bla
</StyledButton>
//...
```

### CSS Modules

Good when you want to apply CSS to specific components and not on your whole project.

To use them you have to tweak the project configuration, this is the procedure if you are on version 1 of react-scripts.
```
npm run eject
```
This will unlock and spawn the configuration files for us to change. In `webpack.config` (both prod and dev versions) edit this section:
```javascript
//...        
test: /\.css$/,
use: [
    require.resolve('style-loader'),
    {
        loader: require.resolve('css-loader'),
        options: {
            importLoaders: 1,
        },
    },
//...
```
adding the followings to the options object:
```javascript
modules: true,
localIdentName: '[name]__[local]__[hash:base64:5]'
```

To use them you have to import a css file not as it is but as an object, doing so will covert each classes from the css file to randomly generated unique class names of the new object.
```javascript
// App.js
import classes from './App.css'; // obj name 'classes' is arbritary
//...
class App extends Component {
    render() {
        return (
            <div className={classes.App}>
                <button
                    className={classes.Button}
                >
                    Click me
                </button>
            </div>
        );
    };
}
```
```css
/* App.css */
.App {
  text-align: center;
}
.Button {
  background-color: green;
}
```