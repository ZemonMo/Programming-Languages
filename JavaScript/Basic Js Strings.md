# JavaScript String Basics

Strings are primitive, immutable data types used to store text. They can be written using single quotes, double quotes, or backticks.

### Key Concepts
- **Indexing:** Access individual characters using bracket notation, starting from index `0`.
- **Newlines:** Use `\n` to create line breaks.
- **Escaping:** Use `\` to escape special characters.
- **Template literals:** Use backticks and `${}` to insert variables into strings.

### Common String Methods

| Method | Purpose |
|---|---|
| `charAt()` / `charCodeAt()` | Access character information |
| `indexOf()` | Find a substring's position |
| `includes()` | Check whether text exists |
| `slice()` | Extract part of a string |
| `toUpperCase()` | Convert to uppercase |
| `toLowerCase()` | Convert to lowercase |
| `replace()` | Replace the first matching value |
| `replaceAll()` | Replace all matching values |
| `repeat()` | Repeat a string |
| `trim()` | Remove whitespace from both ends |
| `trimStart()` | Remove leading whitespace |
| `trimEnd()` | Remove trailing whitespace |

### Example

```js
const language = "JavaScript";

console.log(language.slice(0, 4)); 
// Java

console.log(language.toUpperCase());
// JAVASCRIPT

console.log(language.includes("Script"));
// true
```

### User Input

The `window.prompt()` method displays a dialog box and allows users to enter information.

```js
const answer = window.prompt("What's your favorite language?");
console.log(answer);
```

**Main takeaway:** JavaScript provides built-in string methods that make it easier to search, extract, modify, and manipulate text.