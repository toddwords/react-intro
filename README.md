# An Introduction to React and JSX

Today we'll be looking at React, a frontend framework made by Facebook for building user interfaces.

React makes it easier to make complex, dynamic webpages without having to write a ton of repetitive HTML. We'll get to how in a bit.

### Setting Up

You can use the `index.js` file in this repo to get started or make your own file from scratch. The important thing is that you'll need these three script files at the top:

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/16.2.0/umd/react.development.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/16.2.0/umd/react-dom.development.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.26.0/babel.min.js"></script>
```

There are two React libraries, `react` and `react-dom`, as well as something called Babel. Babel is a sort of Javascript interpreter, it lets you write things in other formats and then converts them into Javascript. We need it to use JSX, the native language of React which we'll talk about in a moment.

We'll be typing in a <script> tag in the body of our HTML, and for it to work we need to set the type to "text/babel" like so:
```
<script type="text/babel">

</script>
```
This lets our program know we want to process the contents of that script file with babel.

### First Render

```
<script type="text/babel">
	ReactDOM.render(
	  <h1>Hello, World</h1>,
	  document.body
	);
</script>
```
The ReactDOM.render() function writes HTML to the screen, and takes in two parameters:

`ReactDOM.render(contentToAdd, HTMLElementToAddContentTo`

In this case we're adding to the entire `document.body` but we could also make a container by adding `<div id='container'></div` to our HTML and having our code read:

```
<script type="text/babel">
	var destination = document.querySelector('#container')
	ReactDOM.render(
	  <h1>Hello, world</h1>, 
	  destination  
	);
</script>
```

It's hard to tell the difference, so lets add some styles:
```
<style>
#container{
	color: lightblue;
	background-color: #eee;
	padding:50px;
}
</style>
```

Nice.

Also it's important to note you can only add one total element with ReactDOM.render(). So
```
ReactDOM.render(
	  <h1>Hello, world</h1>
    <h1>How are you?</h1>, 
	  destination  
	);
```
wouldn't work, and instead we'd need to make a parent div:
```
ReactDOM.render(
    <div>
	  <h1>Hello, world</h1>
    <h1>How are you?</h1>
    </div>, 
	  destination  
	);
```
But you may have noticed something weird about our previous code. Why doesn't our HTMLhave quotes around it? That's because it's not actually HTML, it's **JSX**, a special Javascript version of HTML used by React.

JSX can write content to the browser as easily as HTML, but we can manipulate it and access data with it as if it were Javascript. This allows us to make **components**.

###Components
