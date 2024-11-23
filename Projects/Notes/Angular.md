ViewEncapsulation.Emulated

	In Angular, if you do not specify an encapsulation mode for a component, it uses the default encapsulation mode. The default mode is `ViewEncapsulation.Emulated`. This means Angular emulates Shadow DOM by adding unique attributes to styles and DOM elements, effectively scoping styles to the component.
	
	
	Because `ViewEncapsulation.Emulated` is the default, you do not need to explicitly mention it in your component metadata unless you want to be explicit or if you are overriding a previously set encapsulation mode. Here is how it would look if you were to explicitly set it:


		`import { Component, ViewEncapsulation } from '@angular/core';`
	
	`@Component({`
	  `selector: 'app-my-component',`
	  `templateUrl: './my-component.component.html',`
	  `styleUrls: ['./my-component.component.css'],`
	  `encapsulation: ViewEncapsulation.Emulated // This is optional since it's the default`
	`})`
	`export class MyComponent {`
	  `// Component logic`
	`}`


	
	In Angular, `ViewEncapsulation` determines how styles are applied to components. There are three types of view encapsulation strategies:
	
	1. **Emulated (default)**:
	    
	    - Angular emulates Shadow DOM by adding unique attributes to styles and DOM elements.
	    - This scopes the styles to the component without using the actual Shadow DOM.
	    - It is the default encapsulation mode if none is specified.
	2. **None**:
	    
	    - No encapsulation is applied.
	    - Styles are applied globally to the entire application, affecting all components.
	    - This can lead to style conflicts but can be useful for applying global styles.
	3. **ShadowDom**:
	    
	    - Uses the browser's native Shadow DOM implementation.
		    - Encapsulates styles and DOM in the Shadow DOM, providing true style encapsulation.
	    - Supported only in browsers that support Shadow DOM.