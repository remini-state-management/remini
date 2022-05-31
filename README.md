## Global state management with React

<!-- Perfect code-splitting, pretty and minimalistic syntax, well structured and maintainable codebase. -->

Your coding time saver!

Minimal, well structured, and flexible codebase save a lot of developer time for maintain and grouth your React applications.

How it works

Usually when you just start React project or have a very small one, your codebase is short, undestable and simple, you can easily google examples of common issues.

But as you write the business logic of your application, the code gets larger and it becomes more and more difficult to understand the abundance of files, tricks and code pieces.

You should clear understand where is a place to your logic, how you can write as many code as you want without reduce your application maintance.

- How to make a simple React application who can easily upscale to large application by business demand
- How to organize your code clean with minimal React components and convinient separated logic
- How to speed up your application and reduce boilerplate

# Remini

Tiny frontend with service-oriented architecture.

The key to winning is the shared state and logic. Pure React doesn't have a convenient way to organize shared states that can be used whole the application. Suggested React ways are passing state by props thought nesting components from parent to child, and using Context for giving shared access to some state values in children components. Both ways can't share state with any part of your app!

Architecture with a shared state provides more simplest code. You can control your state and logic in separate files that can be accessed whole the app. You can easily change your shared state and read it everywhere.

## The dark mode switcher

A good example of a shared state benefit is the Dark mode switcher. Because you should get access to user choice in a big set of React components, it is very inconvenient to use props passing pattern.

What is necessary to implement:

- Provide convenient functions for changing user choices.
- Provide access to user choice around the app code.
- Keep user choice across browser sessions.

Will clearly demonstrate how to create, use and propagate a shared state.

Each shared state is stored in a special place created by calling the "re" function. This will be a reactive variable, which means we will be able to update all places where it is used when it changes.

We will keep the dark mode enabled state in this way.

To update the value of a reactive variable, we will use the "update" function. That takes the dark mode reactive variable as the first argument and the updater function as the second one. The updater function receives the current state in the first argument and returned the new state of dark mode.

```javascript
  // dark-mode.shared.js
  import { re, update } from "remini"

  // create new reactive variable with "false" by default
  export const $darkMode = re(false)

  // create a function that should change dark mode to opposite each time calling
  export const toggleDarkMode = () => {
    update($darkMode, (enabled) => !enabled)
  }
```

Now we can read and subscribe to dark mode changes everywhere we need.

For easy binding to the React components, the "useRe" hook function is used. It allows you to get the value of the reactive variable, as well as automatically update the React component when the value changes.

```javascript
  import { useRe } from "remini"
  import { $darkMode, toggleDarkMode } from "./dark-mode.shared.js"

  const DarkModeButton = () => {
    const darkMode = useRe($darkMode)

    return (
      <button onClick={toggleDarkMode}>
        Switch to {darkMode ? "light" : "dark"} mode
      </button>
    )
  }
```

Excellent! Now you can easily derive dark mode state to any React component using the same way. This is very simple, you should get state of the dark mode using the "useRe" hook, and it's all that you need. Each time when dark mode state will be changed, and all components using it will be updated automatically.

And finally, we should make some code updates, because we almost forget to save user choice to browser local storage, to keep persistent between browser sessions.

For accessing storage we will use the "localStorage" browser API. We will call "getItem" to retrieve the saved state, and call "setItem" to save it.

```javascript
  import { write, on } from "remini"

  // try to get choice from previous browser session when reactive variable create
  write($darkMode, localStorage.getItem("darkMode") === "on")

  // update user choice in browser local storage each time then it changed
  on($darkMode, (enabled) => {
    localStorage.setItem("darkMode", enabled ? "on" : "off")
  })
```

The last operation in this example call of "on" function. It means that we subscribe to changes in dark mode reactive variable, and react on them each time changes for saving state to browser persistence storage.

Brilliant! Now you can use it everywhere you want, it's worked well and should provide benefits for your users!

It's looking good and provides you with convenient opportunities for controlling your shared state, and deriving in any parts of your application. You can create as many reactive variables as you want, it's quick and useful!

## Perfect frontend with modular architecture.

<!--
**Pure reactivity**

```javascript
import { re, read, write, update, wrap, on } from "remini"

const $value = re(0)
const $next = wrap(() => read($value) + 1)

on($value, n => console.log('The current value:', n))

update($value, n => n + 1)  // The current value: 1
write($value, 2)            // The current value: 2

console.log(read($next))    // 3
```
-->

<!-- **Modularity** -->

```javascript
// counter.shared.js
import { re, wrap, read, write, shared } from "remini"

const Counter = () => {
  const $value = re(0)
  const $next = wrap(() => read($value) + 1)

  const inc = () => update($value, n => n + 1)
  const reset = () => write($value, 0)

  return { $value, $next, inc, reset }
}

export const sharedCounter = () => shared(Counter)
```

```javascript
import { observe, read } from "remini"
import { sharedCounter } from "./counter.shared"

const Counter = observe(() => {
  const { $value, $next, inc } = sharedCounter()

  return <p>
    {read($value)}
    <button onClick={inc}>becomes</button>
    {read($next)}
  </p>
})

const Reset = () => {
  const { reset } = sharedCounter()
  return <button onClick={reset}>↻</button>
}

export const App = () => {
  return <>
    <Counter />
    <Counter />
    <Reset />
  </>
}
```

[![Edit Counter with Remini](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/counter-with-remini-mp2ldi?file=/src/App.js)

<!--

## Modularity
- No need to wrap Application to Context Provider for each module
- import and using, easy code for embbedding
- created just when it used, by demand, that increase in performance

## Work together with Redux

## The authorized user state

## Product cart

-->

Enjoy your code!

