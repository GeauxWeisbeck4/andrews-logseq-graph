tags:: Web Components, Lit, UI Design, JavaScript, Typescript

- # Intro to Lit
	- Learn the basics of Lit
	- ## Define a Component
		- ### Define a component [1]
			- We almost always start with defining a component
			- The `MyElement` class provides the implementation for your new component. The `@customElement` decorator registers it with the browser as a new element type named `my-element`.
		- ### Add a `render()` method to your class [2]
			- The `render()` method defines your component's internal DOM. The `html` tag function processes a template literal and returns a `TemplateResult`—an object that describes the HTML for Lit to render. Every Lit component should include a `render()` method.
		- > **Decorators** 
		  > The TypeScript version of this code uses the `@customElement` decorator to register your class with the browser as a new element. The JavaScript version uses `customElements.define()`
		  to do the same thing. This is a matter of style. You can use 
		  decorators—or not use decorators—in either language. For more 
		  information, see [Decorators](https://lit.dev/docs/components/decorators/).
			- ## `my-element.ts`
				- ```typescript
				  import {LitElement, html} from 'lit';
				  import {customElement} from 'lit/decorators.js';
				  
				  @customElement('my-element')					// [1]
				  export class MyElement extends LitElement {     // [1]
				    render() {									// [2]
				      return html`								// [2]
				        <p>Hello world! From my-element.</p>		// [2]
				      `;											// [2]
				    }
				  }
				  
				  ```
	- ## Properties and Expressions
		- Lit components can have reactive properties -> changing one trigers the component to update -> we can declare a reactive property and use it in an expression in the component's template.
		- ### Declare a reactive property [1]
			- The code snippet above adds a string property called `message` to your element class. The `@property` decorator identifies it as a reactive property.
		- ### Add an expression to your template [2]
			- You can add dynamic values to your Lit templates with JavaScript expressions.
		- ```typescript
		  import {LitElement, html} from 'lit';
		  import {customElement, property} from 'lit/decorators.js';
		  
		  @customElement('my-element')
		  export class MyElement extends LitElement {
		  // TODO: Add a reactive property
		    @property()							// [1]
		    message: string = 'Hello again.';		// [1]
		  
		    render() {
		      return html`
		        <p>${this.message}</p>			// [2]
		      `;
		    }
		  }
		  
		  ```
	- ## Declarative Event Listeners
		- To add interactivity to your components, you'll probably want to add some event handlers. Lit makes it easy to add a *declarative* event listener in the template, using an expression like this:
			- ```typescript
			  <button @click=${this.handleClick}>Click me!</button>
			  ```
			- Here we've provided a name tag component with a message and an input element. In this step you'll use a declarative event listener so the 
			  component can handle input events.
		- ### Add a declarative event listener [1]
			- This `@input` expression adds `this.changeName` as an event listener for the `input` event. (You can use this syntax for any event listener—just replace input with any event name and `this.changeName` with any JavaScript expression that evaluates to an event handler.)
		- ### Add the event handler method [2]
			- Since `name` is a reactive property, setting it in the event handler triggers the component to update.
			- For more information about declarative event handlers, see [Events](https://lit.dev/docs/components/events/).
				- ```typescript
				  import {LitElement, html} from 'lit';
				  import {customElement, property} from 'lit/decorators.js';
				  
				  @customElement('name-tag')
				  export class NameTag extends LitElement {
				    @property()
				    name: string = 'Your name here';
				  
				    render() {
				      // TODO: Add declarative event listener to input.
				      return html`
				        <p>Hello, ${this.name}</p>
				        <input @input=${this.changeName} placeholder="Enter your name">  // [1]
				      `;
				    }
				  
				    // TODO: Add event handler method.
				    changeName(event: Event) {							// [2]
				      const input = event.target as HTMLInputElement;		// [2]
				      this.name = input.value;							// [2]
				    }
				  }
				  ```
	- ## More Expressions
		- On the previous pages you used expressions to add text content (child 
		  nodes) and add an event listener. You can also use expressions to set 
		  attributes or properties.
		- ### Find the text input and add this expression [1]
			- The `?attributeName` syntax tells Lit you want to set or remove a boolean attribute based on the value of the expression
			- There are five common positions for expressions in Lit templates
				- ```typescript
				  <!-- Child nodes -->
				  
				  <h1>${this.pageTitle}</h1>
				  
				  
				  <!-- Attribute -->
				  
				  <div class=${this.myTheme}></div>
				  
				  
				  <!-- Boolean attribute -->
				  
				  <p ?hidden=${this.isHidden}>I may be in hiding.</p>
				  
				  
				  <!-- Property -->
				  
				  <input .value=${this.value}>
				  
				  
				  <!-- Event listener -->
				  
				  <button @click=${() => {console.log("You clicked a button.")}}>...</button>
				  ```
				- For more information, see [Expressions](https://lit.dev/docs/templates/expressions/).
				- ```typescript
				  import {LitElement, html} from 'lit';
				  import {customElement, property} from 'lit/decorators.js';
				  
				  @customElement('more-expressions')
				  export class MoreExpressions extends LitElement {
				    @property()
				    checked: boolean = false;
				  
				    render() {
				      return html`
				        <div>
				           <!-- TODO: Add expression to input. -->
				           <input type="text" value="Hello there." ?disable=${!this.checked}> // [1]
				        </div>
				        <label><input type="checkbox" @change=${this.setChecked}> Enable editing</label>
				      `;
				    }
				  
				    setChecked(event: Event) {
				      this.checked = (event.target as HTMLInputElement).checked;
				    }
				  }
				  
				  ```
	- ## Template Logic
		- Now that you've got some of the basics, we'll introduce a more 
		  complicated element. In the remainder of this tutorial, you'll build a 
		  to-do list component. Here we've provided some of the boilerplate for 
		  your to-do-list.
		- Since the list items are private to the component, the `_listItems` property is defined as *internal reactive state*.
		  It works just like a reactive property, but it isn't exposed as an 
		  attribute, and shouldn't be accessed from outside the component. For 
		  more information, see [Public properties and internal state](https://lit.dev/docs/components/properties/#public-properties-and-internal-state).
		- You can use standard JavaScript in Lit expressions to create conditional or repeating templates. In this step, you'll use `map()` to turn an array of data into a repeating template.
		- ### Render list items [1]
			- Note the nested `html` inside the `map()` callback. For more information, see [Lists and repeating templates](https://lit.dev/docs/templates/lists/).
		- ### Add the click handler [2]
			- The @query decorator (used in the TypeScript version of the code) is a handy way of getting a reference to a node in your component's internal DOM. It's equivalent to this code in the JavaScript version:
				- ```typescript
				  - get input() {
				  - 	return this.renderRoot?.querySelector('#newitem') ?? null;
				  - }
				  ```
			- > **Mutating objects and arrays.** 
			  > Note that instead of mutating the _listItems array, addToDo()
			  creates a new array that includes the new item. Using this immutable 
			  data pattern ensures that the components see the new data. For more 
			  information, see [Mutating objects and arrays](https://lit.dev/docs/components/properties/#mutating-properties).
				- ```typescript
				  import {LitElement, html} from 'lit';
				  import {customElement, state, query} from 'lit/decorators.js';
				  
				  @customElement('todo-list')
				  export class ToDoList extends LitElement {
				    @state()
				    private _listItems = [
				      { text: 'Start Lit tutorial', completed: true },
				      { text: 'Make to-do list', completed: false }
				    ];
				  
				    render() {
				      return html`
				        <h2>To Do</h2>
				        <ul>
				          ${this._listItems.map((item) =>		// [1]
				            html`<li>${item.text}</li>`)}		// [1]
				        </ul>
				        <input id="newitem" aria-label="New item">
				        <button @click=${this.addToDo}>Add</button>
				      `;
				    }
				  
				    @query('#newitem')					// [2] and rest of code
				    input!: HTMLInputElement;
				  
				    addToDo() {
				      this._listItems = [...this._listItems,
				          {text: this.input.value, completed: false}];
				      this.input.value = '';
				    }
				  }
				  
				  
				  ```
	- ## Styles
		- You can add *encapsulated styles* to a Lit component. Here the starting code is carried over from last step, but 
		  we've added the ability to mark an item as completed by clicking on it.
		- ### Add a static `styles` class field [1]
			-