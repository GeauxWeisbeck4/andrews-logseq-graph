tags:: JavaScript, Data Structures, Algorithms, No Starch Press, Programming Books, Chapter

- title:: Using JavaScript
- chapter:: 1
- book:: [[Data Structures and Algorithms in JavaScript]]
- # 1 Using JavaScript
	- We’ll start with an exploration of some modern JavaScript features that 
	  will simplify coding: arrow functions, classes, spreading values, 
	  destructuring, and modules. This list isn’t exhaustive, and we’ll look 
	  at other features in later chapters, including functional programming, 
	  map/reduce and similar array methods, functions as first-class objects, 
	  recursion, and more. We certainly can’t cover all of the language’s 
	  features, but here the focus is on the most important and newer features
	   that are used throughout the book.
	- ## Arrow Functions
		- JavaScript provides many ways to specify a function, such as:
		  
		      Named functions, which are the most common: function alpha() {...}
		      Nameless function expressions: const bravo = function () {...}
		      Named function expressions: const charlie = function something() {...}
		      Function constructors: const delta = new Function()
		      Arrow functions: const echo = () => {...}
		  
		  All of those definitions work basically the same way, but arrow functions—JavaScript’s new kids on the block—have these important differences:
		  
		      They may return a value even without including a return statement.
		      They cannot be used as constructors or generators.
		      They don’t bind the this value.
		      They don’t have an arguments object or a prototype property.
		  
		  In particular, the first characteristic in the previous list is used a lot in this book; being able to omit the return keyword will make for shorter, more succinct code. For example, in Chapter 12, you will see the following function:
		  
		  const _getHeight = (tree) => (isEmpty(tree) ? 0 : tree.height);
		  
		  Given a tree argument, this function returns 0 if the tree is empty; otherwise, it returns the tree object’s height attribute.
		  
		  The following example uses return and is an equivalent (but longer) way to write the same function:
		  
		  const _getHeight = (tree) => {
		  
		    return isEmpty(tree) ? 0 : tree.height;
		  
		  };
		  
		  The longer version isn’t necessary: shorter code is good.
		  
		  If you use the shortened version and want to return an object, you need to enclose it in parentheses. Here’s another arrow function example from Chapter 12:
		  
		  const newNode = (key) => ({
		  
		    key,
		  
		    left: null,
		  
		    right: null,
		  
		    height: 1
		  
		  });
		  
		  Given a key, this function returns a node (an object, in fact) with that key as an attribute, plus null left and right links and a height attribute set to 1.
		  
		  Another common feature of the arrow function is providing default values for missing parameters:
		  
		  const print = (tree, s = "") => {
		  
		    if (tree !== null) {
		  
		      console.log(s, tree.key);
		  
		      print(tree.left, `${s}  L:`);
		  
		      print(tree.right, `${s}  R:`);
		  
		    }
		  
		  };
		  
		  You will see what this code does in Chapter 12, but the interesting part is that the recursive function, if not provided with a value for s, will initialize it with the empty string.
		  Classes
		  
		  Although we won’t use classes much in this book, modern JavaScript has come far from its beginnings, and instead of having to deal with the prototype and adding tangled code to implement inheritance, now you can achieve inheritance easily. In the past you could use classes and subclasses, different constructors, and all of that, but implementing inheritance wasn’t easy. JavaScript classes now make it much more straightforward. (See https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Inheritance if you want to learn how to do inheritance in old-style JavaScript.)
		  
		  Take a look at a partial, slightly modified example from Chapter 13 that shows an actual class and how to define it:
		  
		  ❶ class Tree {
		  
		  ❷ _children = [];
		  
		  
		  
		  ❸ constructor(rootKey) {
		  
		      this._key = rootKey;
		  
		    }
		  
		  
		  
		    isEmpty() {
		  
		      return this._key === undefined;
		  
		    }
		  
		  
		  
		  ❹ get key() {
		  
		      this._throwIfEmpty();
		  
		      return this._key;
		  
		    }
		  
		  
		  
		  ❺ set key(v) {
		  
		      this._key = v;
		  
		    }
		  
		  }
		  
		  You can either define a simple class, as is the case here ❶, or extend an existing one. For instance, you could have another class BinaryTree extends Tree to define a class based on Tree. You can define attributes outside a constructor ❷; you don’t need to do it inside a constructor ❸. Constructors are available if you need more complex object instance initialization.
		  
		  Getters ❹ and setters ❺ are other powerful features. They bind an object’s property to functions that are invoked whenever we try to modify or access that property.
		  
		  Other features not used in this example are static properties and methods; such attributes aren’t part of the class instances, but rather belong to the class itself.
		  
		      NOTE
		  
		  Starting with ECMAScript 2022, JavaScript also includes private properties: fields, methods, getters, setters, and so on.
		  The Spread Operator
		  
		  The spread operator (...) allows you to, well, spread an array, string, or object into separate values in a single operation, providing some interesting array and object usages.
		  
		  Arrays are applied like this:
		  
		  const myArray = [3, 1, 4, 1, 5, 9, 2, 6];
		  
		  ❶ const arrayMax = Math.max(...myArray);
		  
		  ❷ const newArray = [...myArray];
		  
		  Entering ...myArray is the same as entering 3, 1, 4, 1, 5, 9, 2, 6, so the first usage of ...myArray in this example produces 9 ❶, and the second provides a new array with exactly the same elements of myArray ❷.
		  
		  You can also use the spread operator to build a copy of an object, which you can then modify independently:
		  
		  const myObject = {last: "Darwin", year: 1809};
		  
		  ❶ const newObject = {...myObject, first: "Charles", year: 1882};
		  
		  // same as: {last: "Darwin", first: "Charles", year: 1882};
		  
		  In this case, newObject ❶ first gets a copy of the attributes of myObject, and then the year attribute is overwritten. You could do this “the old way” with many individual assignments, but using the spread operator allows for shorter and clearer code.
		  
		  A third usage of the spread operator is for functions that need to deal with an undefined number of parameters. Earlier versions of JavaScript had the arguments array-like object to handle this situation. The arguments object is “array-like,” because .length is the only array property it provides. The arguments object doesn’t include any other properties that arrays have.
		  
		  For example, you could write your own Math.max() version like this:
		  
		  const myMax = (...nums) => {
		  
		    let max = nums[0];
		  
		    for (let i = 1; i < nums.length; i++) {
		  
		      if (max < nums[i]) max = nums[i];
		  
		    }
		  
		    return max;
		  
		  };
		  
		  You could now use myMax() like you’d use Math.max(), but there’s no reason to reinvent that function. This example shows how you can imitate the features of existing functions—in this case, the ability to pass many arguments to a function.
		  The Destructuring Statement
		  
		  The destructuring statement is related to the spread operator. It allows you to assign several variables at the same time, which means you can combine several independent assignments into one and write shorter code. For example:
		  
		  [first, last] = ["Abraham", "Lincoln"];
		  
		  In this case, you assign "Abraham" to the first variable and "Lincoln" to the last variable.
		  
		  You also can mix destructuring and spreading:
		  
		  [first, last, . . .years] = ["Abraham", "Lincoln", 1809, 1865];
		  
		  Assign the initial elements in the array to first and last, as in the previous example, and assign all of the rest of the elements (the two numbers) to the years array. This combination lets you write code more succinctly, using a single statement, where previously several would have been required.
		  
		  In addition, you can use default values when variables on the left side have no corresponding values on the right:
		  
		  let [first, last, role = "President", party] = ["Abraham", "Lincoln"];
		  
		  In this example, the destructuring statement assigns a default value to role and leaves party undefined.
		  
		  You can also swap or rotate variables, which is a technique used frequently later in this book. Consider this line from code in Chapter 14:
		  
		  [heap[p], heap[i]] = [heap[i], heap[p]];
		  
		  This directly swaps the values of heap[p] and heap[i] without using an auxiliary variable. You also could write something like [d, e, f] = [e, f, d] to rotate the values of three variables, again without requiring more variables.
		  
		  Finally, another pattern that we’ll often use is to return two or more values at once from a function. For example, you could write a function to return two values in order:
		  
		  const order2 = (a, b) => {
		  
		    if (a < b) {
		  
		      return [a, b];
		  
		    } else {
		  
		      return [b, a];
		  
		    }
		  
		  };
		  
		  
		  
		  let [smaller, bigger] = order2(22, 9); // smaller==9, bigger==22
		  
		  The other way of returning many values at once is with an object. You still can do that, but returning an array and using destructuring is more compact.
		  Modules
		  
		  Modules allow you to split code into pieces you can import when needed, providing a way to package functionality that is easier to understand and maintain. Each module should be an aggregation of related functions and classes, providing a set of features. A standard practice related to using modules is high cohesion, which means elements you put together should truly belong together, as unrelated functionalities should not be mixed in the same module. A related concept called low coupling means that distinct modules should be interdependent as little as possible. JavaScript lets you package functions in modules to provide a well-structured design, with greater readability and maintainability.
		  
		  Modules come in two formats: CommonJS modules (an earlier format, used mostly in Node.js) and ECMAScript modules (the latest format, generally used by browsers).
		  CommonJS Modules
		  
		  With CommonJS modules, write the code in the style of this (abridged) example from Chapter 16:
		  
		  // file: radix_tree.js – in CommonJS style
		  
		  
		  
		  ❶ const EOW = "■";
		  
		  const newRadixTree = () => null;
		  
		  ❷ const newNode = () => ({links: {}});
		  
		  const isEmpty = (rt) => !rt; // null or undefined
		  
		  const print = (trie, s = "") => {...}
		  
		  const printWords = (trie, s = "") => {...}
		  
		  const find = (trie, wordToFind) => {...}
		  
		  const add = (trie, wordToAdd, dataToAdd) => {...}
		  
		  const remove = (trie, wordToRemove) => {...}
		  
		  
		  
		  ❸ module.exports = {
		  
		    add,
		  
		    find,
		  
		    isEmpty,
		  
		    newRadixTree,
		  
		    print,
		  
		    printWords,
		  
		    remove
		  
		  };
		  
		  The module.exports assignment at the end ❸ defines what parts of the module will be visible from the outside; whatever is not included ❶ ❷ won’t be accessible for the rest of the system. This way of writing code is well aligned with the black box software concept. Users of a module shouldn’t need to learn or even know about its internal details to allow for higher maintainability. As long as the module keeps providing the same functionality, its developers are free to refactor or improve it without impacting any users.
		  
		  If you wanted to import a pair of the functions that the module exports, for example, you’d use the following code style, which employs destructuring, to specify what you want:
		  
		  const {newRadixTree, add} = require("radix_tree.js");
		  
		  This allows access (via destructuring) to the newRadixTree() and add() functions, out of all the functions exported by the radix_tree module. If you want to add something to the Radix tree, you can call add() directly; similarly, you can call newRadixTree() to create a new tree.
		  
		  Of course, you can also do this:
		  
		  const RadixTree = require("radix_tree.js");
		  
		  In order to add something to a tree or create a new one, you have to call RadixTree.add() and RadixTree.newRadixTree() instead. This usage makes for longer code, but it also lets you access all the functions in the radix_tree module. I prefer the first style that employs destructuring, because it makes clear what I am using, but it’s really up to you.
		  ECMAScript Modules
		  
		  The more modern ECMAScript style of defining modules also works with separate files, but instead of creating a module.exports object, you rewrite the module you just saw in the previous section as follows:
		  
		  // file: radix_tree.js – in modern style
		  
		  
		  
		  const EOW = "■";
		  
		  ❶ export const newRadixTree = () => null;
		  
		  const newNode = () => ({links: {}});
		  
		  ❷ export const isEmpty = (rt) => !rt; // null or undefined
		  
		  const print = (trie, s = "") => {...}
		  
		  const printWords = (trie, s = "") => {...}
		  
		  const find = (trie, wordToFind) => {...}
		  
		  const add = (trie, wordToAdd, dataToAdd) => {...}
		  
		  const remove = (trie, wordToRemove) => {...}
		  
		  
		  
		  ❸ export {
		  
		    add,
		  
		    find,
		  
		    print,
		  
		    printWords,
		  
		    remove
		  
		  };
		  
		  You can export something directly wherever you define it ❶ ❷ or postpone doing so until the end ❸. Both methods work (and I don’t think anybody would really use both styles, as I did for this example), but most people prefer having all export statements together at the end. It’s really your choice.
		  
		      NOTE
		  
		  You can also use ECMAScript import and export statements in Node.js, but only if you use the .mjs extension instead of the .js extension, which is reserved for CommonJS modules.
		  
		  You can import functions from an ECMAScript module in the following way, which is a different usage in comparison with the CommonJS modules, although the end result is exactly the same:
		  
		  import {newRadixTree, add} from "radix_tree.js";
		  
		  If you want to import everything, use the following code instead; this will give you access to an object, including all the functions exported by the module, as earlier:
		  
		  import * as RadixTree from "radix_tree.js";
		  
		  All the exports you’ve seen so far are named exports; you can have as many of them as you want, and you can also have a single unnamed default export. In a given file, instead of defining what you want to export, as described earlier, you include something like this instead:
		  
		  // file: my_module.js
		  
		  export default something = ... // whatever you want to export
		  
		  Then, in other parts of the code, you can do the following to import something:
		  
		  import whatever from "my_module.js";
		  
		  You can name what you imported whatever you like (whatever isn’t a good name) instead of using the name the module creator intended. That isn’t usual practice, but sometimes name conflicts arise when using modules by different authors.
		  Closures and Immediately Invoked Function Expressions
		  
		  Closures and immediately invoked function expressions aren’t actually new, but understanding them will be useful when following the examples in this book. A closure is the combination of a function plus its encompassing scope to which the function has access. It allows you to have private variables, which in turn allows you to create the equivalent of classes and modules. For instance, consider the following function:
		  
		  function createPerson(firstN, lastN) {
		  
		    let first = firstN;
		  
		    let last = lastN;
		  
		    return {
		  
		      getFirst: function () {
		  
		        return first;
		  
		      },
		  
		  
		  
		      getLast: function () {
		  
		        return last;
		  
		      },
		  
		  
		  
		      fullName: function () {
		  
		        return first + " " + last;
		  
		      },
		  
		  
		  
		      setName: function (firstN, lastN) {
		  
		        first = firstN;
		  
		        last = lastN;
		  
		      }
		  
		    };
		  
		  }
		  
		  The returned value (an object) will have access to the first and last variables in the scope of the function. For example, consider the following:
		  
		  const me = createPerson("Federico", "Kereki");
		  
		  console.log(me.getFirst()); // Federico
		  
		  console.log(me.getLast());  // Kereki
		  
		  console.log(me.fullName()); // Federico Kereki
		  
		  
		  
		  me.setName("John", "Doe");
		  
		  console.log(me.fullName()); // John Doe
		  
		  Those variables aren’t accessible anywhere else. If you try to access me.first or me.last, you get undefined. Those variables are in the closure, but there’s no way to access them, because they work as private values.
		  
		  Using closures also allows you to simulate modules. For that, you’ll need an immediately invoked function expression (IIFE), pronounced “iffy,” which is a function defined and executed as soon as it’s defined.
		  
		  Say you want a module to work with taxes. Without using the new modules, you could work in a similar way as with the createPerson(...) function:
		  
		  const tax = (function (basicTax) {
		  
		    let vat = basicTax;
		  
		    /*
		  
		      ...many more tax-related variables
		  
		    */
		  
		    return {
		  
		      setVat: function (newVat) {
		  
		        vat = newVat;
		  
		      },
		  
		      getVat: function () {
		  
		        return vat;
		  
		      },
		  
		      addVat: function (value) {
		  
		        return value * (1 + vat / 100);
		  
		      }
		  
		      /*
		  
		        ...many more tax-related functions
		  
		      */
		  
		    };
		  
		  })(6);
		  
		  You create a (nameless) function and call it immediately, and the result works like a module. You can pass initial values to the IIFE, such as 6 percent for the default value-added tax (VAT). The vat variable, and others you may declare, are internal and cannot be accessed directly. However, the provided functions, addVat(...) and any others you may want, can work with all the internal variables.
		  
		  Use the IIFE-based module as follows:
		  
		  console.log(tax.getVat());    // 6: the initial default
		  
		  tax.setVat(8);
		  
		  console.log(tax.getVat());    // 8
		  
		  console.log(tax.addVat(200)); // 216
		  
		  Modules can provide the same basic functionality, but you will see cases when you’ll want to use closures and IIFEs—for example, in Chapter 5 where memoizing and precomputing an array of values are discussed.
		  
		  JAVASCRIPT STANDARDS AND VERSIONS
		  
		  JavaScript’s frequent updates have caused incompatibilities among versions. This book works with the latest version of the language, which is formally known as ECMAScript. ECMA originally stood for European Computer Manufacturers Association, but it’s now considered to be a noun rather than an acronym. See the ECMA website (https://www.ecma-international.org) for more detailed information about JavaScript, including a very full specification of the whole language.
		  
		  The Mozilla Developer Network (MDN; https://developer.mozilla.org/en-US/docs/Web/JavaScript) is also an invaluable resource for information on features like functions, classes, and modules.
		  
		  All web browsers, as well as Node.js, may not implement the latest version of the language, but if you encounter an issue, you can use Babel to transpile (transpile is a portmanteau of translate and compile) your code into equivalent, but older (yet compatible) code, so you can still run it. See https://babeljs.io for more details on Babel as well as installation and configuration instructions. I ran all the code in the book in the current version of Node.js and didn’t encounter any problems. To install Node.js, see https://nodejs.org/en/download.
		  JavaScript Development Tools
		  
		  Let’s turn our attention to some tools to add to your arsenal to help write better-looking code, check for common defects, and more. You won’t use all of them in this book, but they are helpful and usually the first things I install whenever I start a new project.
		  Visual Studio Code
		  
		  An integrated development environment (IDE) will help you write code quickly and easily. This book uses the Visual Studio Code (VSC) IDE. Other popular IDEs include Atom, Eclipse, Microsoft Visual Studio, NetBeans, Sublime, and Webstorm, and you could work with any of those as well.
		  
		  Why use an IDE? Although a simple text editor, such as Notepad or vi, might be all you need, an IDE like VSC provides more functionality. With a text editor, you have to do more work yourself, constantly switching between tools and entering commands repeatedly. Using VSC (or any IDE) is a time-saver that allows you to work in an integrated fashion and with many tools in a single click.
		  
		  VSC is open source, free, and updated monthly, with new features added frequently. You can use it for JavaScript and many other languages. Frontend developers use VSC for basic configuration and recognition (“IntelliSense”) for HTML, CSS, JSON, and more. You can expand it via a vast catalog of extensions as well.
		  
		      NOTE
		  
		  Visual Studio Code, despite the similar name, is not related to Microsoft’s other IDE, Visual Studio. You can use Visual Studio Code in Windows, Linux, and macOS, because it was developed in JavaScript and packaged for the desktop using the Electron framework.
		  
		  VSC also provides good performance, integrated debugging, an integrated terminal (to launch processes or run commands without having to leave VSC), and integration with source code management (typically Git). Figure 1-1 shows some of my own work in VSC with the code for this book.
		  
		  Figure 1-1: Using Visual Studio Code
		  
		  Go to https://code.visualstudio.com to download the proper version for your environment and follow the installation instructions. If you like to live on the edge, install the Insiders’ Version to gain access to new features, but be aware that you risk suffering from some bugs. For some Linux distributions, instead of downloading and installing the package yourself, you can use your package manager to handle installation and updating.
		  Fira Code Font
		  
		  A quick way to start a (possibly heated) argument among developers is to mention that a given font is the best one for programming. Dozens of monospaced fonts for programming exist, but few include ligatures, which is when two or more characters are joined together. JavaScript code is a good candidate for ligatures, because otherwise you need to enter common symbols (such as ≥ or ≠) as two or three separate characters, which just doesn’t look as good.
		  
		      NOTE
		  
		  The & character originally was a ligature of E and t to spell et, which means “and” in Latin. Another ligature in English is æ (as in encyclopædia or Cæsar) combining the letters a and e. Many other languages include ligatures; German joins two s characters together in ß, like in Fußball (football).
		  
		  The Fira Code font (https://github.com/tonsky/FiraCode) provides many ligatures and enhances the look of your code. Figure 1-2 shows all the possible ligatures for JavaScript. Fira Code includes ligatures for other languages as well.
		  
		  Figure 1-2: A sample of the many ligatures Fira Code font provides (cropped from the Fira Code website)
		  
		  After downloading and installing the font, if you are using Visual Studio Code, follow the instructions at https://github.com/tonsky/FiraCode/wiki/VS-Code-Instructions to integrate the font with the IDE.
		  Prettier Formatting
		  
		  How to format source code can be another source of disagreements. Every developer you work with will likely have their own take on this issue, asserting that their standard is best. If you work with a team of developers, you may be familiar with the situation shown in the “How Standards Proliferate” xkcd comic (Figure 1-3).
		  
		  Figure 1-3: “How Standards Proliferate” (courtesy of xkcd, https://xkcd.com/927)
		  
		  Prettier is an “opinionated” source code formatter that reformats code according to its own set of rules and a few parameters you can set. Prettier’s website states, “By far the biggest reason for adopting Prettier is to stop all the on-going debates over styles.” All the source code examples in this book are formatted with Prettier.
		  
		  Installing Prettier is simple; follow the instructions at https://prettier.io, and if you use Visual Studio Code, also install the Prettier extension from https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode. Be sure to tweak VSC’s settings to enable the editor.formatOnSave option so all code will be reformatted upon saving. Consult the documentation on the Prettier website to learn more about configuring Prettier to your liking.
		  JSDoc Documentation
		  
		  Documenting your source code is development best practice. JSDoc (https://jsdoc.app) is a tool that helps you produce documentation for your code by aggregating specifically formatted comments. If you add comments preceding your functions, methods, classes, and so on, JSDoc will use them to produce documentation for your code. We don’t use JSDoc in this book, because the text explains the code. However, for normal work, using JSDoc helps developers understand all of a system’s pieces.
		  
		  Here’s a code snippet that adds a key to a heap from Chapter 14 to show how JSDoc produces documentation:
		  
		   /**
		  
		   * Add a new key to a heap.
		  
		   *
		  
		   * @author F.Kereki
		  
		   * @version 1.0
		  
		   * @param {pointer} heap – Heap to which the key is added
		  
		   * @param {string} keyToAdd – Key to be added
		  
		   * @return Updated heap
		  
		   */
		  
		  const add = (heap, keyToAdd) => {
		  
		    heap.push(keyToAdd);
		  
		    _bubbleUp(heap, heap.length – 1);
		  
		    return heap;
		  
		  };
		  
		  JSDoc comments start with the /** combination, which is like the usual comment format but with one extra asterisk. The @author, @version, @param, and @return tags describe specific information about the code; the names are self-explanatory. Other tags you can use include @class, @constructor, @deprecated, @exports, @property, and @throws (or @exception). See https://jsdoc.app/index.html for a complete list.
		  
		  After installing JSDoc according to the instructions at https://github.com/jsdoc/jsdoc, I processed this example file, which produced the results shown in Figure 1-4.
		  
		  Figure 1-4: A sample documentation web page automatically generated by JSDoc
		  
		  Of course, this is a simple example with only a single file. For a complete system, you would get a home page with links to every page of documentation.
		  ESLint
		  
		  JavaScript presents many possibilities for misuse and misunderstanding. Consider this simple example: if you use the == operator instead of ===, you may find cases in which x==y and y==z, but x!=z, no matter what the transitive law may say. (Try x=[], y=0, and z="0".) Another tricky case is if you accidentally enter (x=y) instead of (x==y), which would be an assignment rather than a comparison; it’s not very likely you want the former.
		  
		  A linter is a tool that analyzes code and produces warning or error messages about any doubtful or error-prone features you might be using. In some cases, a linter may even fix your code properly. You also can use linters in conjunction with source code versioning tools. Linters can keep you from posting code that doesn’t pass all checks. If you use Git, go to https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks to read about precommit hooks.
		  
		  ESLint works well for linting in JavaScript. It was created in 2013 and is still going strong. Go to https://www.npmjs.com/package/eslint to download and install, and then configure it. Be sure to carefully read the rules at https://eslint.org/docs/rules/, because you can set many different rules, but you shouldn’t turn them all on unless you want to start some linting wars.
		  
		  Finally, don’t forget the VSC extension at https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint, so you can see whatever errors ESLint detects.
		  
		      NOTE
		  
		  With ESLint, the eqeqeq rule (see https://eslint.org/docs/rules/eqeqeq) would have detected the problem with the type-unsafe == operator and even would have fixed it by substituting === instead. In addition, the no-cond-assign rule would have warned about the unexpected assignment.
		  Flow and TypeScript
		  
		  For large-scale coding, consider using Flow and TypeScript, which let you add information about data types to JavaScript. Flow adds comments that describe what data types are expected for function inputs and outputs, variables, and so forth. TypeScript is actually is a superset of JavaScript that is transpiled into it.
		  
		  Figure 1-5 (shamelessly based on an example from the TypeScript home page) shows the kinds of errors you can detect with type information.
		  
		  Figure 1-5: A type error in TypeScript code caught on the fly by ESLint
		  
		  In this example, I’m trying to access an attribute that doesn’t exist (user.name) according to the type data deduced from earlier lines of code. (Note that I’m using ESLint, which is why I can see the error in real time.)
		  
		  We won’t use either of these two tools in this book, but for big projects that involve many classes, methods, functions, types, and so on, consider adding them to your repertoire.
		  Online Feature Availability Resources
		  
		  If you’re working server-side with the latest version of Node.js, you probably won’t need to worry about any specific feature being available. However, if you’re doing frontend work, a given function may not be available, such as Internet Explorer support. If that happens, you’ll need to transpile with something like Babel, as mentioned earlier in the chapter.
		  
		  The Kangax website (https://compat-table.github.io/compat-table/es2016plus/) provides information on multiple platforms, detailing whether a function is fully, partially, or not available. Kangax provides a listing of all the JavaScript language features, with examples for each, and you’ll find a table on the website that shows what features are available for each different JavaScript engine, such as features found on browsers and Node.js. Generally speaking, when you open it with a browser, green “Yes” boxes mean you can use the feature safely; boxes in different colors or text imply the feature is partially available or not available.
		  
		  The Can I Use? website at https://www.caniuse.com/ lets you search by function and shows the available support in different browsers. For instance, if you search for arrow functions, the website will tell you which browsers support it, since what date, and the percentage of global users with direct access to that feature without polyfills or transpiling.
		  
		      NOTE
		  
		  If you are hazy on the term polyfill, see “What Is a Polyfill” at https://remysharp.com/2010/10/08/what-is-a-polyfill by Remy Sharp (creator of the concept). A polyfill is a way to “replicate an API ... if the browser doesn’t have it natively.” The MDN website often provides polyfills for new features, which is helpful if you need to deal with older browsers that don’t provide them or need details on how something works.
		  
		  Figure 1-6 shows information on the availability of arrow functions across browsers; hovering with the mouse provides more data, such as when the feature was first available.
		  
		  Figure 1-6: The Can I Use? website shows whether a given feature is available in browsers.
		  
		  The Can I Use? site provides information only about browsers; it doesn’t include server-side tools like Node.js, but you’ll likely find a need for it at some time.
		  Summary
		  
		  In this chapter, we looked at some of JavaScript’s new and important modern features, including spreading, destructuring, arrow functions, classes, and modules. We also considered some additional tools you might want to include for your development work, such as the VSC IDE, Fira Code font for neater screen displays, Prettier for source code formatting, JSDoc to generate documentation, ESLint to check for defects or bad practices, and Flow or TypeScript to add data type checking. Finally, in order to ensure that you’re not using unavailable functions, two online resources were presented: Kangax, and Can I Use?, both of which will help you avoid unimplemented or only partially implemented JavaScript features.
		  
		  In the next chapter, we’ll go deeper into JavaScript and explore its functional programming aspects, providing a starting point for the examples in the rest of this book.