tags:: Web Components, Lit, UI Design, JavaScript, Typescript

- # Intro to Lit
	- Learn the basics of Lit
	- ## Define a Component
		- ### Define a component [1]
			- We almost always start with defining a component
			- The `MyElement` class provides the implementation for your new component. The `@customElement` decorator registers it with the browser as a new element type named `my-element`.
		- ### Add a `render()` method to your class [2]
			- The `render()` method defines your component's internal DOM. The `html` tag function processes a template literal and returns a `TemplateResult`—an object that describes the HTML for Lit to render. Every Lit component should include a `render()` method.
			-