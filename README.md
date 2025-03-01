
# Front-end Coding Standards and Best Practices

## Overview
These guidelines, principles and standards allow us to:
- produce code of a consistent quality across all frontend development
- work concurrently with multiple devs on the same codebase at the same time in the same way
- produce code that is less prone to bugs and regressions, is easier to understand and debug
- write code that supports re-use.


# Global Development Guidelines
## Performance
Page load times (both real and perceived) are a key consideration for users of all browsers and device types.

### There are some general things we can do in front-end development:
- Minimise HTTP requests
- Minimise blocking – content should be readable before client side processing
- Lazy load 'supplementary' content (especially images)
- Avoid unnecessary use of display: none;

### Testing and QA should find no problems
- Code is a craft - make it your responsibility to ensure it is the best it can be; that it's tested, bug free, and adheres to these guidelines. Testing and QA folks aren't responsible for quality - developers are.
- Don't use testers as bug catchers - testers should find no problems after you have committed your code. When testers find bugs, tickets need to be opened, developers assigned and scheduled in to fix the problem. This lengthens the time it takes from identifying to resolving a bug.
- Make sure, as much as possible, you have tested your code on a reasonable number of devices so you can catch problems before you commit to the repo.

## Don't Repeat Yourself (DRY)
If you repeat anything that has already been defined in code, refactor it so that it only ever has one representation in the codebase.

If you stick to this principle, you will ensure that you will only ever need to change one implementation of a feature without worrying about needing to change any other part of the code.

### Separation of concerns
Separate structure from presentation from behaviour to aid maintainability and understanding.
- Avoid writing inline CSS or Javascript in HTML
- Where appropriate, use CSS rather than Javascript for animations and transitions

### Write code to be read
Follow the principles of ['Keep It Simple, Stupid' (KISS)](https://en.wikipedia.org/wiki/KISS_principle) hard to read or obfuscated code is difficult to maintain and debug. Don't be too clever; write code to be read.

### Commenting
Explain design or architectural decisions that cannot be conveyed in code alone by adding comments to your code.

Be sure that in conjunction with writing code that adheres to these guidelines, someone can pick up your code and immediately understand it.
Be verbose with your comments but ensure:

- Your comments add something to the code, they don't just repeat what is there

- They are kept up to date, if you change something that has been commented, ensure you up date the comment as well

- If code needs extensive commenting, can it be refactored to make it less complex / easier to understand?

- You focus on why rather than how - unless your code is too complex, it should be self documenting

Don't leave commented out chunks of code in the codebase. It makes the code look unfinished, and can be confusing for other developers.

## HTML Markup
### General Guidelines
- Use well-structured, semantic markup.
- Use double quotes on all attributes. When using React.js for generating markup, we rely on single quotes.
- Use soft tabs, 2 space indents.
- Ensure you write valid HTML. Check using tools such as the [W3C Markup Validation Service.](https://validator.w3.org/)
- Omit protocols from external resources to prevent unintended security warnings through accidentally mixing protocols:
```
<!-- Don't do -->
<script src="http://www.google.com/js/gweb/analytics/autotrack.js"></script>

<!-- Do -->
<script src="//www.google.com/js/gweb/analytics/autotrack.js"></script>
```
### Write semantic markup
Ensure the markup you write is relevant and has meaning in the context of the content it is being applied to. Don't use markup to infer style.
```
<!-- bad: -->
<div class="mainHeading">My Blog</div>
<br /><br />
... content ...
<br /><br />

<!-- bad: -->
<h1>I want to draw attention to this as I am important</h1>
<h1>and so am I</h1>

<!-- good: -->
<h1>My Blog</h1>
<p>
   ... content ...
</p>
```
### Input labels
Use label tags for input fields so that the input element acquires focus when the label is clicked.

When using labels, try to use the wrapping pattern rather than the for attribute so we can avoid using ids which may interfere with integration with backend systems:
```
<label>Address
  <input type="text" name="address" />
</label>

<label for="address">Address</label>
<input type="text" id="address" name="name" />
```
### Boolean attributes
Use the single word syntax for boolean attribute values to aid readability, reduce clutter and prevent unnecessary bytes going down the wire.

The presence of the attribute itself implies that the value is "true", an absence implies a value of "false":

```
// old hat
<option selected="selected">value 1<option>

// cutting edge
<option selected>value 1<option>
```
## Behaviour & JS
### General guidelines
- Use ES6 - Babel will transpile it.
- Use soft-tabs with a two space indent.
- Use Typescript
- Avoid applying styles with Javascript, preferably add or remove classes. Keep style in CSS to make it easier to maintain and debug.
```
//  no good
<span style="display:none;" name="address_1">Icon label text</span>
```
- Use is-* or has-* state rules to apply state to elements, for example class='is-visually-hidden' class='has-icon'.
```
//  all good
<span class="is-visually-hidden has-icon">Icon label text</span>
```
- Don't recreate functionality that may already be present in a utility library that is already in use.
- Always use parentheses in blocks to aid readability:
```
// good...
while (true) {
  shuffle()
}

// not good...
while (true)
  shuffle()

// also not good...
while (true) shuffle()
```
- Use the === comparison operator to avoid having to deal with type coercion complications.
- Use event.preventDefault() instead of return false to prevent default event actions. Returning a boolean value is semantically incorrect when considering the context is an event.
```
//  returning false doesn't make sense in this context
htmlElement.addEventListener('click', (e) => {
  // do stuff
  return false
})

// use preventDefault
htmlElement.addEventListener('click', (e) => {
  e.preventDefault()
  // do stuff
})
```
- Don't use var any more - we have const and let which Babel will compile as necessary. Favour const over let in ES6. In JavaScript, const means that the identifier can’t be reassigned. let is a signal that the variable may be reassigned, it also signals that the variable will be used only in the block it’s defined in.
**Don't use semi-colons - javascript doesn't require them**
## Avoid excessive function arguments
If a function requires more than three arguments, considering refactoring to use a configuration object:
```
// bad
let myFunction1 = function(arg1, arg2, arg3, arg4) {}

myFunction1('firstArgument', argument2, true, 'My Third Argument')

// good
let myFunction2 = function(config) {}

myFunction2({
  arg1: 'firstArgument',
  arg2: argument2
  arg3: true,
  arg4: 'My Third Argument'
})
```
Configuration objects:
- allow for optional parameters,
- are easier to read,
- are easier to maintain.
## Naming conventions
- Use descriptive yet concise variable names for long-lived variables.
- Avoid using what might be considered reserved variable names out of context. For example, e in javascript is considered an event object. Don't use it for anything else.

## Break code into separate lines
Where applicable, ensure code is written on separate lines to aid Git diffs and error reporting:
```
 // good
page.setViewport({
  width: 1280,
  height: 1024
})

 // not so good
page.setViewport({width: 1280, height: 1024})
```

# Guidelines and best practices for React.js
## Use meaningful component names
Give component names that reflect their functionality. Avoid using generic names like "Box" or "DoesTheThing". Meaningful names make it easier to understand what a component does and what are its purposes in the application. Clear and concise component names reduce the cognitive load of developers, making it easier to navigate and maintain the codebase.

## Break down components
Breaking down complex components into smaller ones makes the code easier to understand, maintain, and reuse. Smaller components facilitate bug fixes, feature additions, and effective unit testing, improving code quality. This approach separates concerns within the application, enhancing codebase management and potentially improving performance by reducing the amount of code processed by the browser.

## Use destructuring
Destructuring props and state makes code more concise and readable. Instead of writing `props.title`, you can write `const {title} = props.` It allows you to extract values from objects or arrays in a more concise and readable way, reducing the amount of code you need to write. Destructuring can make your code more readable by explicitly declaring which properties or elements you are interested in.

## Keep components small
Keeping components small and focused on a single responsibility makes it easier to test and debug code. Smaller components are easier to reuse across the application, making it easier to maintain and scale the codebase.

## Use functional components
Using functional components instead of class components whenever possible is wise because functional components are easier to read and write. Functional components are generally faster and more efficient than class components because they don't require a constructor or lifecycle methods.

## Use arrow functions
Arrow functions make code more concise and easier to read. They are a better way to write functions in JavaScript, which can lead to cleaner and more readable code. Arrow functions automatically bind to the parent scope, which can be helpful in cases where you need to access the parent scope's ‘this’ keyword.

## Use stateless components
One should use stateless components whenever possible. Stateful components are more complex and harder to reason about. Stateless components are easier to reuse and compose since they only have to render based on the data passed to them through props. They are also easier to test since they don't have any internal state to manage

## Use the spread operator
Using the spread operator (...) in your React code is important for several reasons such as it allows you to write shorter and more concise code, especially when working with arrays or objects. The spread operator allows you to create new objects or arrays without modifying the original data, which is an important principle of functional programming.

# Naming Conventions
### Pascal Case
*Pascal Case typically refers to the convention of writing compound words in which the first letter of each word is capitalized and there are no spaces or punctuation marks between the words.*

In React, we can use pascal case for below elements:
-  React Component
```
// React Component
const Todo = () => {
   //...
}
```
- CSS Class Names
- Enumerations

### Camel Case
*Camel case refers to the convention of writing compound words or phrases where each word begins with a capital letter except the first word, which starts with a lowercase letter.*

In React, we can use camel case for below elements:
- Variable Names
```
// Variable Name
const userName = "sathishskdev";
```
- Function Names
```
// Function Name
const getFullName = (firstName, lastName) => {
    return `${firstName} ${lastName}`;
}
```
- Object Properties
```
// Object Properties
const user = {
  userName: "johndoe",
  firstName: "John",
  lastName: "Doe"
}
```
- CSS Module Class Names
```
// CSS Module Class Names
.headerContainer {
    display: "flex";
}
```
- Custom Hooks
```
const useTodo = () => {
  //...
}
```
- Higher Order Component
```
const withTimer = () => {
  //...
}
```
### Kebab Case
*Kebab case refers to the convention of writing compound words in lowercase letters and separating them with hyphens("-").*

In React, we can use kebab case for below elements:
- CSS Class Names
```
// CSS Class Names
header-container {
    display: "flex";
}

<div className="header-container">
  //...
</div>
```
- Folder Names
```
// Folder Names
  src
     todo-list // Folder name
         TodoList.jsx
         TodoList.module.scss
     todo-item // Folder name
         TodoItem.jsx
```
### SCREAMING_SNAKE_CASE
*SCREAMING_SNAKE_CASE refers to the convention of writing compound words or phrases in uppercase letters, with words separated by underscores ("_").*
- Constants
```
// Constants
const BASE_PATH = "https://domain.services/api";
```
- Enumeration Properties
```
// Enumeration Properties
const RequestType = { // Name in Pascal Case
  // Properties in Screaming Snake Case
  GET: 'GET',
  POST: 'POST',
  PUT: 'PUT',
  DELETE: 'DELETE',
};
```
- Global Variables
```
// Global Variables
const ENVIRONMENT = 'PRODUCTION';
const PI = 3.14159;
```
## Higher-Order Component Naming Convention
Here are best practices for naming Higher-Order Component:
- Use "with" as Prefix
Common convention is to use the prefix "with" followed by the functionality or purpose of the HOC.
- Use "Original Component" as Suffix
Include the original component name in the HOC's name to indicate the component it is enhancing or wrapping
```
// HOC: name have "with" as Prefix
// "Filter" is add as Suffix which is original component
const withFilter = () => {
 //...
}

// Usage of the HOC
const Filter = withFilter(/*Component Name*/);
```
## Custom Hooks Naming Convention
Here are best practices for naming custom hooks:
- Use "use" as Prefix
Common convention is to use the prefix "use" followed by the functionality or purpose of the Custom Hooks.
- Use "Behaviour of hook" as Suffix
Name the custom hooks that accurately describes the purpose or behaviour of the custom hook.
```
// Custom Hook: useTimer
// name have "use" as Prefix
// "Timer" is add as Suffix which is behaviour of hook
const useTimer = (initialTime) => {
  // ... hook implementation
};

// Usage of the custom hook
  const { time, start, stop, reset } = useTimer(60);
```
## Use more descriptive and specific names
It's important to avoid using generic or unclear names for your components, variables, or functions.
- Pitfalls to Avoid
```
const MyComponent = () => { 
// What kind of component is this?

const data = getData() 
// What kind of data is this?

const onClick = () => {
  // What does it do?
}
//...
}
```
- Best Practice
```
const ProductDetails = () => {

const productInfo = fetchProductInfo();
// Fetches detailed product information

const addProductToCart = () => {
  // Add the product to the shopping cart
};
//...
}
```
## Avoid Excessive Abbreviations
- Bad example
```
// Excessive abbreviation
const selUsr = {
  usrId: '1',
  usrNm: 'John Doe',
  usrEmail: 'johndoe@domain.com',
};

// Usage
selUsr.usrId
```
- Best Practice
```
// Descriptive object and property names
const selectedUser = {
  userId: 1,
  userName: 'johndoe',
  userEmail: 'johndoe@domain.com',
}

// Usage
selectedUser.userId
```
## Writing Clean and Efficient React Code
### Implement error boundaries to handle component errors gracefully
Wrap your components or specific sections of your application with error boundaries to catch and handle errors in a controlled manner.

This prevents the entire application from crashing and provides a fallback UI or error message, improving the user experience and making it easier to debug issues.
```
// HOC for error boundary
const withErrorBoundary = (WrappedComponent) => {
  return (props) => {
    const [hasError, setHasError] = useState(false);
    const [errorInfo, setErrorInfo] = useState(null);

    useEffect(() => {
      const handleComponentError = (error, errorInfo) => {
        setHasError(true);
        setErrorInfo(errorInfo);
        // You can also log the error to an error reporting service here
      };

      window.addEventListener('error', handleComponentError);

      return () => {
        window.removeEventListener('error', handleComponentError);
      };
    }, []);

    if (hasError) {
      // You can customize the fallback UI or error message here
      return <div>Something went wrong. Please try again later.</div>;
    }

    return <WrappedComponent {...props} />;
  };
};
```
#### Usage
```
// HOC for error boundary
import withErrorBoundary from './withErrorBoundary';

const Todo = () => {
  // Component logic and rendering
}

const WrappedComponent = withErrorBoundary(Todo);
```
### Use React.memo for functional components
React.memo is a higher-order component that memoizes the result of a functional component, preventing unnecessary re-renders.

By wrapping your functional components with React.memo, you can optimize performance by skipping re-renders when the component's props have not changed.

However, it's important to note that React.memo performs a shallow comparison of props. If your component receives complex data structures as props, ensure that you handle prop updates appropriately for accurate memoization.

### Use Linting for Code Quality
Utilizing a linter tool, such as ESLint, can greatly improve code quality and consistency in your React projects.
By using a linter, you can:
- Ensure consistent code style
- Catch errors and problematic patterns
- Improve code readability and maintainability
- Enforce coding standards and conventions

### Avoid default export
The problem with default exports is that it can make it harder to understand which components are being imported and used in other files. It also limits the flexibility of imports, as default exports can only have a single default export per file.
- Named imports work well with tree shaking.
- Refactoring becomes easier.
- Easier to identify and understand the dependencies of a module.

### Use object destructuring
```
// Use object destructuring
const { id, name = "Task", completed } = todo; 
```
- It reduces the need for repetitive object property access.
- Supports the assignment of default values.
- Allows variable renaming and aliasing.

### Use fragments
Fragments allow for cleaner code by avoiding unnecessary wrapper divs when rendering multiple elements.
```
const Todo = () => (
  <>
    <h1>Title</h1>
    <ul>
      // ...
    </ul>
  </>
);
```

### Prefer passing objects instead of multiple props
when we use multiple arguments or props are used to pass user-related information to component or function, it can be challenging to remember the order and purpose of each argument, especially when the number of arguments grows.
```
// Use object arguments
const updateTodo = (todo) => {
 //...
}

const todo = {
   id: 1,
   name: "Morning Task",
   completed: false
}

updateTodo(todo);
```
- Function becomes more self-descriptive and easier to understand.
- Reducing the chances of errors caused by incorrect argument order.
- Easy to add or modify properties without changing the function signature.
- Simplify the process of debugging or testing functions to passing an object as an argument.

### Avoid using indexes as key props
Using indexes as key props can lead to *incorrect rendering* especially when adding, removing, or reordering list items.

It can result in poor performance and incorrect component updates.

**Use unique and stable identifiers**

### Use implicit return in small functions
```
// ❌ Avoid using explicit returns 
const square = value => {
  return value * value;
}
```
```
// ✅ Use implicit return
const square = value => value * value;
```
- Implicit return reduces code verbosity.
- Improves code readability.
- It can enhance code maintainability by focusing on the main logic rather than return statements.

### Prefer using template literals
```
// ❌ Bad Code
const title = (seriesName) => 
      "Welcome to " + seriesName + "!";
```
```
// ✅ Use template literals
const title = (seriesName) => 
      `Welcome to ${seriesName}!`;
```
- It simplify string manipulation by allowing variable interpolation within the string.
- It makes code more expressive and easier to read.
- It support multi-line strings without additional workarounds.
- Improving code formatting.
