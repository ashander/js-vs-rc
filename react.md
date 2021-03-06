---
layout: page
permalink: "/react/"
title: "Building Dynamic Web Pages with React"
---

> **Questions**
>
> - FIXME

- In the beginning, people created HTML pages by typing them in
- Quickly realized that a lot of pages shared a lot of content
  - Headers, footers, etc.
- Create a _template_ with some embedded commands to:
  - Include other bits of HTML (like headers)
  - Loop over data structures to create lists and tables
- _Server-side page generation_ because:
  - That's where the data was
  - That was the only place code could be run

FIXME: diagram

- Balance shifted as browsers and JavaScript became more powerful
- Current standard model is:
  - JavaScript running in the browser fetches data from one or more servers
  - Uses that data to generate HTML in the browser for display
- Allows the client to decide how best to render data
  - Increasingly important as mobile devices take over from PCs

FIXME: diagram

- Lots and lots (and lots) of JavaScript frameworks for building views
- We have chosen React because it is:
  - Freely available
  - Simpler than many alternatives
  - Widely used
  - Well documented
- Central design principles are:
  - Use functions to describe the desired HTML
  - Let React decide which functions to run when data changes
- Show how to do it the pure-JavaScript way
- Then introduce a tool called JSX that simplifies things

## Hello, World

<!-- @src/react/hello.html -->
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello World</title>
    <meta charset="utf-8">
    <script src="https://fb.me/react-15.0.1.js"></script>
    <script src="https://fb.me/react-dom-15.0.1.js"></script>
  </head>
  <body>
    <div id="app">
      <!-- this is filled in -->
    </div>
    <script>
      ReactDOM.render(
        React.DOM.h1(null, "Hello, world"),
        document.getElementById("app")
      )
    </script>
  </body>
</html>
```

- Head of the page loads two React libraries from the web
  - Use locally-installed libraries later
- Body contains a `div` with an ID to make it findable
  - React will replace this entire `div`
- Script
  - Create an `h1` with the text "Hello, world"
  - Find the element whose ID is "app"
  - Insert the former into the latter
- Notes
  - Alters the representation of the page in memory, not the source of the page on disk
  - Can't put the script in a separate JavaScript file and load it in the head
    because the body might not exist in memory when the script is run

- The first parameter to `React.DOM.h1` can be an object of attributes

<!-- @src/react/stylish.html -->
```html
  <body>
    <div id="app">
      <!-- this is filled in -->
    </div>
    <script>
      const attributes = {
        'style': {
          'background': 'pink',
          'font-style': 'italic'
        }
      }
      ReactDOM.render(
        React.DOM.h1(attributes, "Hello, world"),
        document.getElementById("app")
      )
    </script>
  </body>
```

## JSX

- Writing nested functions is a clumsy way to write HTML
- So add a tool called JSX that translates HTML into JavaScript function calls

<!-- @src/react/hello-jsx.html -->
```html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello World</title>
    <meta charset="utf-8">
    <script src="https://fb.me/react-15.0.1.js"></script>
    <script src="https://fb.me/react-dom-15.0.1.js"></script>
    <script src="https://unpkg.com/babel-standalone@6/babel.js"></script>
  </head>
  <body>
    <div id="app">
      <!-- this is filled in -->
    </div>
    <script type="text/babel">
      ReactDOM.render(
        <h1>Hello, world</h1>,
        document.getElementById("app")
      )
    </script>
  </body>
</html>
```

- Include Babel to translate mixed content into pure JavaScript
- Add `type="text/babel"` to the `script` tag to tell Babel where to do its work
- Remember: the script is translated into pure JavaScript and then run as before

- Why bother?
- Because we can put JavaScript inside our HTML (inside our JavaScript)
- E.g., use `map` to turn a list of strings into an HTML list

<!-- @src/react/jsx-list.html -->
```html
  <body>
    <div id="app">
      <!-- this is filled in -->
    </div>
    <script type="text/babel">
      const allNames = ['McNulty', 'Jennings', 'Snyder', 'Meltzer', 'Bilas', 'Lichterman']
      ReactDOM.render(
        <ul>{allNames.map((name) => { return <li>{name}</li> })}</ul>,
        document.getElementById("app")
      )
    </script>
  </body>
```

- FIXME: explain why we can't just use a loop
  - Because it has to return something
- FIXME: explain need to return one root node
  - Because one function call

## Creating Components

- If we're defining functions, we can write new ones

<!-- @src/react/create-components.html -->
```html
  <body>
    <div id="app">
      <!-- this is filled in -->
    </div>
    <script type="text/babel">
      const allNames = ['McNulty', 'Jennings', 'Snyder', 'Meltzer', 'Bilas', 'Lichterman']

      const ListOfNames = () => {
        return (<ul>{allNames.map((name) => { return <li>{name}</li> })}</ul>)
      }

      ReactDOM.render(
        <div>
          <ListOfNames />
        </div>,
        document.getElementById("app")
      )
    </script>
  </body>
```

- What we really want to do is parameterize
  - After all, the JSX is being turned into a function
- All the attributes are passed to our function in a single `props` object

<!-- @src/react/pass-parameters.html -->
```html
  <body>
    <div id="app">
      <!-- this is filled in -->
    </div>
    <script type="text/babel">
      const allNames = ['McNulty', 'Jennings', 'Snyder', 'Meltzer', 'Bilas', 'Lichterman']

      const ListElement = (props) => {
        return (<li id="{props.name}"><em>{props.name}</em></li>)
      }

      ReactDOM.render(
        <div>
          <ul>{allNames.map((name) => { return <ListElement name={name} /> })}</ul>
        </div>,
        document.getElementById("app")
      )
    </script>
  </body>
```

- Gives us exactly one logical place to do calculations, set style, etc.
