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

### Components

Let's say we want to make a list of superheroes:

```
<h1>Superman: Super strength and flight</h1>
<h1>Batman: Cool gadgets and money</h1>
<h1>Wonder Woman: Magic lasso and invisible jet</h1>
<h1>Todd: Teaching and bad jokes</h1>
```
And lets say we want to make some changes, like putting the names in bold or using a p tag rather than an h1. We have to go in and change all of the HTML:
```
<p><strong>Superman:</strong> Super strength and flight</p>
<p><strong>Batman:</strong> Cool gadgets and money</p>
<p><strong>Wonder Woman:</strong> Magic lasso and invisible jet</p>
<p><strong>Todd:</strong> Teaching and bad jokes</p>
```
It's kind of a pain because we have to go through and change each line individually. This is a good case for a **React component**.

A **React component** is just a function that returns JSX. It takes in an object called `props` and uses that as a variable to show different information. We could represent the heroes in the list above as:

```
function Hero(props){
  return <p><strong>{props.name}:</strong> {props.power}</p>
}
```
The content in the { } processes as regular Javascript rather than JSX. Also note, always name your React components with a capital letter.

You call a component like an HTML element, filling in the props like HTML attributes.
```
var destination = document.querySelector('#container')
ReactDOM.render(
  <div id='heroList'>
	<Hero name="Superman" power="Super strength and flight" />
	<Hero name="Batman" power="Cool gadgets and money" />
	<Hero name="Wonder Woman" power="Magic lasso and invisible jet" />
	<Hero name="Todd" power="Teaching and bad jokes" />
  </div>,
  destination  
);
```

Now lets try adding links as well. If we go to http://www.dccomics.com/characters/superman or http://www.dccomics.com/characters/batman we can go to the web page for each hero. So lets make dynamic links:

```
function Hero(props){
  return <p><strong><a href={"http://www.dccomics.com/characters/" + props.name}>{props.name}</a>:</strong> {props.power}</p>
}
```
Remember the { } is just javascript so we can add our strings together in there. But we still have a problem, the Wonder Woman page won't open because the appropriate url is http://www.dccomics.com/characters/Wonder-Woman not http://www.dccomics.com/characters/Wonder%20Woman.

Lets fix it with Javascript:
```
function Hero(props){
  return <p><strong><a href={"http://www.dccomics.com/characters/" + props.name.replace(" ", "-")}>{props.name}</a>:</strong> {props.power}</p>
}
```
Now our Hero component will replace any spaces in the name with dashes in the context of the URL. Wonder woman should work now, and lets make a new one for Green Lantern:

```<Hero name="Green Lantern" power="Super powerful magic ring" />```

However, no amount of string manipulation will put our poor 'Todd' hero in the DC database. To add a URL for him, we'll need to use some conditional logic. We can do this by putting an if/else statement in our component:
```
function Hero(props){
  if(props.name != "Todd"){
    return <p><strong><a href={"http://www.dccomics.com/characters/" + props.name.replace(" ", "-")}>{props.name}</a>:</strong> {props.power}</p>
  } else {
    return <p><strong><a href='http://toddwords.com'>{props.name}</a>:</strong> {props.power}</p>
}
```
But there's a much faster way to do this. We can't write an if/else statement in JSX, but we can write a conditionaly using a ternery operator, which is a sort of condensed if/else that looks like this:

`condition ? codeToDoIfConditionIsTrue : codeToDoIfConditionIsFalse`

`mushroom == 'poisonous' ? safeToEat = false : safeToEat = true`

So we can re-write the above statement as:
```
function Hero(props){
  return <p><strong><a href={props.name != "Todd" ? "http://www.dccomics.com/characters/" + props.name.replace(" ", "-D") : "http://toddwords.com"}>{props.name}</a>:</strong> {props.power}</p>
}
```
We can put the ternary statement right in with our JSX, making our code only branch when it needs to.

### Adding Styles

Lets say we want to add some styles to our React, maybe a custom color for each hero. We can't just add style to the JSX in-element like we would in HTML. Instead, JSX expects to see styles added as a Javascript object

```
function Hero(props){
  var styles = {fontFamily: 'Arial', color: props.color}
  return <p style={styles}><strong><a href={props.name != "Todd" ? "http://www.dccomics.com/characters/" + props.name.replace(" ", "-D") : "http://toddwords.com"}>{props.name}</a>:</strong> {props.power}</p>
}

<Hero name="Superman" power="Super strength and flight" color="red" />
<Hero name="Batman" power="Cool gadgets and money" color="black"/>
<Hero name="Wonder Woman" power="Magic lasso and invisible jet" color="blue" />
<Hero name="Green Lantern" power="Super powerful magic ring" color="green" />
<Hero name="Todd" power="Teaching and bad jokes" color="purple" />
```

Note that the css property 'font-family' is replaced with 'fontFamily' in JSX. This is true of all multi-word css properties, and is one of the few weird quirks where JSX is different from HTML. (Another is the use of 'class' in HTML which is replaced with 'className')

### Advanced Components

Components can do more than just show HTML. They can also keep track of information and hold events. To do this we need to write them as a class extending React.Component rather than just a function though.

```
class Hero extends React.Component {
  constructor(props) {
    super(props);
    this.state = {peopleSaved: 0};

    this.handleClick = this.handleClick.bind(this);
} 
  handleClick(){
    this.setState(function(prevState){
     prevState.peopleSaved = prevState.peopleSaved + 1;
     return prevState;
	})
}
  render() {
  var styles = {fontFamily: 'Arial', color: this.props.color}
  return <p onClick={this.handleClick} style={styles}><strong><a href={this.props.name != "Todd" ? "http://www.dccomics.com/characters/" + this.props.name.replace(" ", "-D") : "http://toddwords.com"}>{this.props.name}</a>:</strong> {this.props.power}, People Saved: {this.state.peopleSaved}</p>
 }
}
```


More React docs here: https://reactjs.org/docs/
