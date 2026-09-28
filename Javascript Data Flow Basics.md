# JavaScript Data Flow Basics

Practiced tracing data through JavaScript functions and DOM sinks.

### Key concepts

* Function parameters can carry user-controlled data.
* Variables can be reassigned inside conditions.
* `&&` requires both conditions to be true.
* `replace()` changes the first matching occurrence.
* `innerHTML` causes the browser to parse a string as HTML.
* `textContent` treats the value as plain text.

### Example

```js
function showName(name) {
    let message = "Welcome " + name;
    document.getElementById("output").innerHTML = message;
}

let params = new URLSearchParams(location.search);
let name = params.get("name");

showName(name);
```

The important security concept:

```text
Source → Processing → Sink
```

Example:

```text
location.search → function → innerHTML
```

When analyzing JavaScript, trace the value from its source through every transformation until it reaches the final sink.
