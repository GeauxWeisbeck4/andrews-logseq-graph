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
			-