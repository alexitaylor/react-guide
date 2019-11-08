# ⚛️ 🚀 React Component Patterns  
  
## Overview 

This documentation will help identify the trade-offs of the different React patterns and when each pattern would be most appropriate. The following patterns will allow for more useful and reusable code by adhering to design principles like **separation of concern**, **DRY**, and **code reuse**. Some of these patterns will help with problems that arise in large React applications such as **prop drilling.** Each major pattern includes an example hosted on [CodeSandBox](https://codesandbox.io/).

### 📚 Table of Contents
- [Compound Components](#compoundComponents)
- [Compound Components CodeSandBox](#compoundComponentsCodeSandBox)
- [Flexible Compound Components](#flexibleCompoundComponents)
- [Context API](#contextAPI)
- [Flexible Compound Component w/ Context API CodeSandBox](#flexibleCompoundComponentCodeSandBox)
- [Render Props](#renderProps)
- [Render Props CodeSandBox](#renderPropsCodeSandBox)
- [Prop Collections](#propCollections)
- [Prop Collections CodeSandBox](#propCollectionsCodeSandBox)
- [Prop Getters](#propGetters)
- [Prop Getters CodeSandBox](#propGettersCodeSandBox)
- [Provider Pattern](#providerPattern)
- [Provider Pattern CodeSandBox](#providerPatternCodeSandBox)
- [HOC](#HOCCode)
- [HOC CodeSandBox](#HOCCodeSandBox)
- [React Hooks](#reactHooks)
- [React Hooks CodeSandBox](#reactHooksCodeSandBox)
- [Forward Refs](#forwardRefs)

## [⬆️](#compoundComponents) Compound Components

- Compound components is a pattern where components are used together such that they share an implicit state that lets them communicate with each other in the background.
- Think of compound components like the `<select>` and `<option>` elements in HTML. Apart they don't do too much, but together they allow you to create the complete experience.

### [⬆️](#compoundComponentsCodeSandBox) [Compound Components CodeSandBox](https://codesandbox.io/s/844n4m7qxj)

## [⬆️](#flexibleCompoundComponents) Flexible Compound Components

- The problem with compound components is that it can only clone and pass props to  **immediate**  **children.** To allow for compound components to  implicitly accept props regardless of where they're rendered within the parent component we can use React's **Context API**.

###  [⬆️](#contextAPI) Context API (v16.3)

#### React.createContext

- **Creates a Context object**. When React renders a component that subscribes to this Context object it will read the current context value from the closest matching Provider above it in the tree.
 - The  **defaultValue argument**  is only used when a component does not have a matching Provider above it in the tree.

```javascript
const MyContext = React.createContext(defaultValue);
```

#### Context.Provider

- Every Context object comes with a  **Provider Component**  that  **allows consuming components (child components) to subscribe to context changes.**
    - It  **accepts a value prop to be passed to consuming components that are descendants of this Provider**. One Provider can be connected to many consumers. Providers can be nested to override values deeper within the tree.
    - All consumers that are descendants of a Provider will re-render whenever the Provider’s value prop changes.

```jsx
<MyContext.Provider value={this.state}>  
  {this.props.children}  
</MyContext.Provider>
```

#### Context.Consumer

- **A React component that subscribes to context changes**. This lets you subscribe to a context within a function component.
 - Requires a  **function as a child**. The function receives the current context value and returns a React node.
- The  **value argument**  passed to the function will be equal to the  **value prop**  of the  **closest Provider**.
- If there is  **no Provider for this context above, the value argument will be equal to the defaultValue that was passed to createContext().**
        
```jsx
<MyContext.Consumer>  
  {value => /* render something based on the context value */}  
</MyContext.Consumer>
```

### Summary

- **React.createContext** accepts an initial value as an argument and returns an object with a Provider and a Consumer
- **Provider component**  wraps around the **consumer components** (child components)  to manage the shared state.
- **Consumer component**  is used within the  **provider component** to access or update shared state.

### [⬆️](#flexibleCompoundComponentCodeSandBox) [Flexible Compound Component w/ Context API CodeSandBox](https://codesandbox.io/s/5jwm1pop)

## [⬆️](#renderProps) Render Props

- **Usage Case:**  Fetching data and showing data are two separate concerns. Ideally, a single component has a  [single responsibility](https://blog.bitsrc.io/solid-principles-every-developer-should-know-b3bfa96bb688).
    - **1st component**  is a ****functional** component**. Its primary concern is rendering the view (how it's going to look).  
    - **2nd component**  is a ****class** component**. Its primary concern is handling state, data fetching, and logic.
- The Render Prop API gives the user control over what they are rendering. The render responsibility is under the ownership of the user and not the component implementation.
- This pattern differs from the Compound Component pattern by making the state explicit allowing the user to render custom components.

### [⬆️](#renderPropsCodeSandBox) [Render Props CodeSandBox](https://codesandbox.io/s/vyz5jlxkvl)

---

#### [⬆️](#propCollections) Prop Collections

- Often when using the render prop, some elements commonly require the same props applied for accessibility or interactivity purposes to certain elements.
- With prop collection, you can collect these props into an object for users to simply apply to their elements
- When a user's component has common props such as `onClick` and `aria-expanded` on a button we can add a collection of props that can easily be used for the end user.
- The user can spread the object (defined in the parent component) on an element where the object's properties are attributes or event handlers.

#### [⬆️](#propCollectionsCodeSandBox) [Prop Collections CodeSandBox](https://codesandbox.io/s/zxypkwx9px)

---

#### [⬆️](#propGetters) Prop Getters

- Instead of spreading an object, we call a function that returns an object. That function accepts all the props (attributes and event handlers) that we want to be applied to the element. Then the function's job is to compose all the props together so that it spreads the desired props that should be applied to the element.
- **Why use a function to get props? Why not use an object and allow the user to spread it on the element?**
    - In our example below the prop getter function, **_getTogglerProps_**, composes all of the props passed in from the user of the component and the component's default prop collection. This allows for the user's custom  
        **_onButtonClick_**  and the component's default **_toggle_** to both be called. If we were to spread the prop collection like in the previous example like so: _**<button onClick={this.onButtonClick} {...togglerProps} />**._ The  **_onClick_**  function in _togglerProps_ will override the user's custom _**onButtonClick**._

#### [⬆️](#propGettersCodeSandBox) [Prop Getters CodeSandBox](https://codesandbox.io/s/qqz1j9xox4)

## [⬆️](#providerPattern) Provider Pattern

- Provider pattern solves the problem of **prop drilling**.
- Provider pattern uses compound components and it exposes a Context.Consumer for the user component.
- Provider pattern  **allows to share state anywhere in the tree**  as well as utilize the  **render props pattern**.
- Usually only expose the Consumer and keep the provider in the root component that's handling the state management.

### [⬆️](#providerPatternCodeSandBox) [Provider Pattern Example](https://codesandbox.io/s/k51x2m2n5o)

## [⬆️](#HOC) Higher Order Components (HOC)

- A higher-order component (HOC) is an advanced technique in React for reusing component logic. A useful mechanism for sharing code.
- HOC is another way to share code and allows you to create new components to render statically.
- When your app boots up higher order components receive components to create and then render, with Render props all of this happens at render time.
- HOC can provide a nicer API for the code you're trying to share.
- Usually, if you build HOC, under the hood, you want to implement/ utilize render prop.

### [⬆️](#HOCCodeSandBox) [Higher Order Components CodeSandBox](https://codesandbox.io/s/myjr3rxrjj)

## [⬆️](#reactHooks) React Hooks (v16.8)

- In version 16.8 React introduced Hooks, allowing to handle internal state without using a class component! With hooks, we can write functional components that can handle its state. As well it introduces the ability to create side effects (data fetching, subscriptions, or manually changing the DOM) in your functional components.
- **Hooks let you use more of React’s features without classes.**

### State Hook

- The **useState()** allows you to add state to a functional component.
- The useState hook accepts **one argument** which is the initial state. It **returns a pair (an array)** containing **the current state** (equivalent to this.state) and **function to update that state** (equivalent to this.setState).

```jsx
import React, { useState } from 'react';  
   
function Counter() {  
    // Array destructuring allows to give different names to the state variables  
    const [count, setCount] = useState(0);  
   
 return ( <div> { /** Reading our state.  */ } <p>Current count is { count }</p< <button onClick={() => setCount(count + 1)}>+1</button> <button onClick={() => setCount(count - 1)}>-1</button> </div> );}
```

### Effect Hook

- The **useEffect()** hook allows you to **perform side effects** (data fetching, subscriptions, or manually changing the DOM) in your functional components.
- It serves the same purpose as **componentDidMount**, **componentDidUpdate**, and **componentWillUnmount** in React classes, but unified into a single API. Or you can think of Effects happening **after render**.
- The **useEffect** function accepts **two arguments**: (1) the function you want to run (the "effect") and (2) an optional array. The value passed into the array allows React to skip applying an effect by comparing the values from the previous render and the next render. If the value passed in hasn't changed, then it will skip the effect.
- Optionally, if your effect returns a function, React will run it after the component un-mounts performing a cleanup of the component. For example, if we set up a subscription in the effect we would want to clean it up to prevent any memory leaks. So we would unsubscribe from the subscription in the returned function.

```jsx
import React, { useState, useEffect } from 'react';  
   
function FriendStatus(props) {  
  const [isOnline, setIsOnline] = useState(null);  
   
  useEffect(() => {  
    function handleStatusChange(status) {  
      setIsOnline(status.isOnline);  
    }  
   
 ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange); // Specify how to clean up after this effect: return () => { ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange); };  }, [props.friend.id]); // Only re-subscribe if props.friend.id changes  
   
  if (isOnline === null) {  
    return 'Loading...';  
  }  
  return isOnline ? 'Online' : 'Offline';  
}
```

### Rules of Hooks

- Only call Hooks **at the top level**. Don’t call Hooks inside loops, conditions, or nested functions.    
- Only call Hooks **from React function components or your custom Hooks**. Don’t call Hooks from regular JavaScript functions or classes. Hooks **do not** work inside classes.
- React provides an [eslint-plugin](https://www.npmjs.com/package/eslint-plugin-react-hooks) for React Hooks.
- Next time you discuss React class component vs functional component, please do not refer to functional components as stateless components 😄.

### [⬆️](#reactHooksCodeSandBox) [Hooks CodeSandBox](https://codesandbox.io/s/14wy8nrzlq)

## [⬆️](#forwardRefs) Forward Refs

- **Ref forwarding**  is an opt-in feature that lets some components take a ref they receive, and pass it further down (in other words, “forward” it) to a child.
- Ref forwarding is a technique for automatically passing a  **ref**  through a component to one of its children. This is typically not necessary for most components in the application. However, it can be useful for some kinds of components, especially for reusable components.

```jsx
const FancyButton = React.forwardRef((props, ref) => (  
  <button ref={ref} className="FancyButton">  
    {props.children}  
  </button>  
));  
   
// You can now get a ref directly to the DOM button:  
const ref = React.createRef();  
<FancyButton ref={ref}>Click me!</FancyButton>;
```

Here is a step-by-step explanation of what happens in the above example:

- We create a  **React ref**  by calling  **React.createRef**  and assign it to a  **ref**  variable.
- We pass our  **ref**  down to  _**<FancyButton ref={ref}>**_  by specifying it as a JSX attribute.
- React passes the  **ref**  to the  _(**props, ref) => ...**_  function inside  **forwardRef**  as a second argument.
- We forward this  **ref**  argument down to  _**<button ref={ref}>**_  by specifying it as a JSX attribute.
- When the ref is attached,  **ref.current**  will point to the  _**<button>**_  DOM node.

### Forward Refs with HOC

- HOC passes all  **props**  through to the component it wraps, so the rendered output will be the same. But there is one caveat:  **refs will not get passed through.**  That’s because  **ref is not a prop**. This means that  **refs**  intended for the  **inner component**  will be attached to the  **HOC component**.
- **React.forwardRef**  accepts a render function that receives  **props**  and  **ref**  parameters and returns a React node. For example:

```jsx
function logProps(Component) {  
  class LogProps extends React.Component {  
    componentDidUpdate(prevProps) {  
      console.log('old props:', prevProps);  
      console.log('new props:', this.props);  
    }  
   
 render() { const {forwardedRef, ...rest} = this.props;       // Assign the custom prop "forwardedRef" as a ref  
 return <Component ref={forwardedRef} {...rest} />; }  }  
   
  // Note the second param "ref" provided by React.forwardRef.  
  // We can pass it along to LogProps as a regular prop, e.g. "forwardedRef"  
  // And it can then be attached to the Component.  
  return React.forwardRef((props, ref) => {  
    return <LogProps {...props} forwardedRef={ref} />;  
  });  
}
```

# Happy Coding 🚀
