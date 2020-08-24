# Styled Components Best Practice.

- Styled-components keeps track of which components are rendered on a page and injects their styles and nothing else, fully automatically. Combined with code splitting, this means your users load the least amount of code necessary.

- Styled-components generates unique class names for your styles. You never have to worry about duplication, overlap or misspellings.

- The basic idea of styled components is to enforce best practices by removing the mapping between styles and components.

- As it turns out, by doing so, we also enforce the split between container and presentational components, and we make sure that developers can only build small and focused components.

- Another interesting feature of styled components is that it allows us to write actual CSS in our JavaScript (not just CSS-as-JavaScript objects). 

1. [FULL CSS SUPPORT](#Full-CSS-Support)


# SPLIT CONTAINER AND PRESENTATIONAL COMPONENTS

- With React, some of our components may have state associated with them. It's important to split components that handle data and/or logic (for example, data formatting) from components that handle styling. By separating these two concerns, reasoning about changes in our code base will be a lot easier.

- By separating these two types of components, you avoid doing multiple unrelated changes at the same time, thus avoiding accidental errors.


# Full CSS Support

- Because styled-components is just CSS, it supports all of CSS perfectly fine. Media queries, pseudo-selectors, even nesting just work. We are generating a class name and injecting the CSS into the DOM; so, whatever works in CSS works with styled-components, too.

```JS
const Input = styled.input`
  font-size: 1.25em;
  border: none;
  background: papayawhip;
  /* ...more styles here... */

  &:hover {
    box-shadow: inset 1px 1px 2px rgba(0,0,0,0.1);
  }

  @media (min-width: 650px) {
    font-size: 1.5em;
  }
`;
```

This Input component will now have nice hover styles and will resize itself to be a bit bigger on large screens.


# Global styles

- Some styles are needed in all pages of our product, these styles stays inside index.global-styles.js files. These styles include the general styles needed in most of the project. The styled-components library provides us a helper function as named export for that too — injectGlobal can be used to add global styles using the now familiar tagged template literal syntax.
 
 ``` JS
 injectGlobal`  
* {    
 border: none;    
 margin: 0;    
 padding: 0;    
 box-sizing: border-box;  
}  
 html,  body,  #root {    
 height: 100vh;    
 width: 100%;
 overflow-x: hidden;
}
  
a,  a:hover,  
a:active,  
a:visited {
 text-decoration: none;    
 color: inherit;
  
}`;
```
# Theming

We created a theme.js default theme object which accepts our theme fundamentals like color and font size. And later we export to use it.

``` JS
export const theme = {
color: {    
  primary: '#064acb',
  secondary: '#f2f3f3',
  light: '#fafafa',    
},
fontSize: {    
  lg: '34px',    
  md: '23px',    
  sm: '16px',    
  xs: '12px',
   }
,};
export const getFontSize = size => props => props.theme.fontSize[size];
export const getColor = color => props => props.theme.color[color];
```
In our index.js file we wrap our App component where our application lives with Theme Provider and pass theme object. This will serve as a default theme.

``` JSX

const rootElement = document.getElementById('root');
ReactDOM.render(  
<ThemeProvider theme={theme}>    
  <BrowserRouter>      
    <App />    
  </BrowserRouter>  
</ThemeProvider>,  
rootElement,
);

```

# Extending Styles

Quite frequently you might want to use a component, but change it slightly for a single case. Now, you could pass in an interpolated function and change them based on some props, but that's quite a lot of effort for overriding the styles once.

To easily make a new component that inherits the styling of another, just wrap it in the styled() constructor. Here we use the button from the last section and create a special one, extending it with some color-related styling:


``` JSX
// The Button from the last section without the interpolations
const Button = styled.button`
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

// A new component based on Button, but with some override styles
const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

render(
  <div>
    <Button>Normal Button</Button>
    <TomatoButton>Tomato Button</TomatoButton>
  </div>
);
```

# Passed props


If the styled target is a simple element (e.g. styled.div), styled-components passes through any known HTML attribute to the DOM. If it is a custom React component (e.g. styled(MyComponent)), styled-components passes through all props.

This example shows how all props of the Input component are passed on to the DOM node that is mounted, as with React elements.

```JSX
// Create an Input component that'll render an <input> tag with some styles
const Input = styled.input`
  padding: 0.5em;
  margin: 0.5em;
  color: ${props => props.inputColor || "palevioletred"};
  background: papayawhip;
  border: none;
  border-radius: 3px;
`;

// Render a styled text input with the standard input color, and one with a custom input color
render(
  <div>
    <Input defaultValue="@probablyup" type="text"
/>
    <Input defaultValue="@geelen" type="text" inputColor="rebeccapurple" />
  </div>
);
```

# Define Styled Components outside of the render method

- It is important to define your styled components outside of the render method, otherwise it will be recreated on every single render pass. Defining a styled component within the render method will thwart caching and drastically slow down rendering speed, and should be avoided.

Write your styled components the recommended way:

```JSX
const StyledWrapper = styled.div`
  /* ... */
`

const Wrapper = ({ message }) => {
  return <StyledWrapper>{message}</StyledWrapper>
}

```


Bad Way

```JSX
const Wrapper = ({ message }) => {
  // WARNING: THIS IS VERY VERY BAD AND SLOW, DO NOT DO THIS!!!
  const StyledWrapper = styled.div`
    /* ... */
  `

  return <StyledWrapper>{message}</StyledWrapper>
}
```

[Styled-Components](https://www.styled-components.com/)