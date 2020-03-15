# REACT NOTEPAD

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

### Radium

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