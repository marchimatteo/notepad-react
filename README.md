# REACT NOTEPAD

## Basics

React is made of 2 packages, *react* which has the logic to crate components, and *react dom* which is in charge of rendering those components to the real DOM.

### Babel

Preprocessor which compile **jsx** code into regular html/javascript.

### A basic component

Components should return the html/javascript (or jsx code if using Babel) of the component.

```jsx
function Person() {
    return (
        <div class ="person">
            <h1>Matteo</h1>
            <p>Anni: 33</p>
        </div>
    );
}
```

## Styling

If you are using inline styles, an easy way to dynamically change the style is to dynamically assign classes to html elements.  
In this example the css styling for the classes *red* and *bold* ar defined in the global css file.
```jsx
const classes = [];
if (true) {
    classes.push('red');
}
if (true) {
    classes.push('bold');
}

return (
    <p classname={classes.join(' ')}>bla bla</p>
)
```

### Radium (library)

A disadvantage of using in line styles is the lack of **pseudo selectors**.

To fix this we can use a tool called **radium** (install with `npm install --save radium`).

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