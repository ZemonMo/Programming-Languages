# JavaScript Execution, Events, and Data Flow

## Overview

These notes cover JavaScript execution order, event handling, asynchronous API data, string transformations, and source-to-sink analysis.

## 1. Event Loop and Call Stack

JavaScript executes synchronous code before processing queued callbacks.

```js
console.log("Start");

Promise.resolve().then(() => console.log("Promise"));

setTimeout(() => console.log("Timer"), 0);

console.log("End");
```

Expected output:

```text
Start
End
Promise
Timer
```

Promise callbacks use the microtask queue. Timer callbacks use a task queue. A zero-millisecond timer is not immediate.

The call stack tracks active function calls. A called function executes before control returns to its caller.

## 2. Event Bubbling and Delegation

Events can propagate from a target element to its ancestors.

```html
<div id="parent">
  <button id="child">Click me</button>
</div>
```

```js
document.querySelector("#parent").addEventListener("click", () => {
  console.log("Parent");
});

document.querySelector("#child").addEventListener("click", () => {
  console.log("Child");
});
```

Clicking the button prints:

```text
Child
Parent
```

Important properties:

- `event.target`: the original event target.
- `event.currentTarget`: the element whose listener is currently executing.

Event delegation uses a parent listener to handle events from its descendants.

```js
document.querySelector("#parent").addEventListener("click", (event) => {
  const button = event.target.closest("#child");

  if (button) {
    console.log("Child button clicked");
  }
});
```

`closest()` checks the element itself and its ancestors for a CSS selector. It returns the closest match or `null`.

## 3. Event Propagation Controls

```js
button.addEventListener("click", (event) => {
  console.log("First");
  event.stopPropagation();
});

button.addEventListener("click", () => {
  console.log("Second");
});
```

Output:

```text
First
Second
```

Both listeners run because they belong to the same element. The event does not continue bubbling to the parent.

| Method | Purpose |
|---|---|
| `stopPropagation()` | Stops propagation to other elements along the event path. |
| `stopImmediatePropagation()` | Also prevents subsequent listeners on the same element from running. |
| `preventDefault()` | Prevents the browser's default action. |

These methods are not interchangeable.

## 4. Fetch and Data Transformation

```js
fetch("/api/user")
  .then(response => {
    if (!response.ok) {
      throw new Error("Request failed");
    }

    return response.json();
  })
  .then(data => {
    data.name = data.name.substring(0, 3);
    showUser(data);
  });

function showUser(user) {
  console.log(user.name);
}
```

If the API returns:

```json
{
  "name": "Amina"
}
```

The output is:

```text
Ami
```

`response.json()` parses the response body; it does not automatically throw for HTTP statuses such as 404. Checking `response.ok` handles unsuccessful HTTP responses explicitly.

Object property mutation changes the object being referenced. Passing that object to another function allows the function to read the updated property.

## 5. String Transformations

`substring(start, end)` returns a portion of a string. The end index is excluded.

```js
const name = "Amina";
console.log(name.substring(0, 3));
```

Output:

```text
Ami
```

A plain string passed to `replace()` replaces only the first matching occurrence:

```js
"<b>Hello</b>".replace("<", "&lt;");
```

Result:

```text
&lt;b>Hello</b>
```

`replaceAll()` replaces all occurrences of the supplied match:

```js
"<b>Hello</b>".replaceAll("<", "&lt;");
```

Result:

```text
&lt;b>Hello&lt;/b>
```

These are string operations, not general-purpose sanitization methods.

## 6. Source-to-Sink Analysis

A source-to-sink trace follows input from its origin to its eventual use.

```js
const params = new URLSearchParams(location.search);
const name = params.get("name");

document.querySelector("#result").innerHTML = name;
```

The data flow is:

1. **Source:** `location.search` contains the URL query string.
2. **Extraction:** `params.get("name")` retrieves a parameter.
3. **Variable:** `name` holds the extracted value.
4. **Sink:** `innerHTML` parses the value as HTML.

A source reaching a sink is a reason to investigate, not proof of a vulnerability. Assess input control, transformations, execution context, and browser behavior.

## 7. `innerHTML` and `textContent`

`innerHTML` parses a string as HTML:

```js
result.innerHTML = "<b>Hello</b>";
```

The browser renders **Hello** in bold.

`textContent` inserts the string as text:

```js
result.textContent = "<b>Hello</b>";
```

The browser displays the literal characters `<b>Hello</b>`.

When the intention is to display untrusted input as plain text, prefer `textContent`. If an application must render untrusted HTML, use an appropriate HTML sanitizer rather than relying on manual string replacement.

