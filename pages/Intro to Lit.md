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
-