## What is debounce?

Debounce delays a function's execution until a specified amount of time has passed without another call happening.

For example, suppose the delay is 500 ms:

- You type H → start a 500 ms timer.
- You type He before 500 ms → reset the timer.
- You type Hel → reset it again.
- You stop typing → after 500 ms, the function executes.

The function runs only once, after you stop typing.

```js
function debounce(func, delay) {
  let timeoutId;

  return function (...args) {
    // 1. Clear any active timer currently running
    clearTimeout(timeoutId);

    // 2. Set a new timer to run the function after the delay
    timeoutId = setTimeout(() => {
      // 3. Ensure the original function preserves context ('this') and arguments
      func.apply(this, args);
    }, delay);
  };
}
```

**Where is debounce used?**
- **Search boxes:** Avoid sending an API request for every keystroke.
- **Autocomplete:** Wait until the user pauses typing.
- **Window resizing:** Recalculate layout after resizing stops.
- **Form validation:** Validate after the user finishes entering text.

**One important interview concept**
The timer must be declared outside the returned function. If you declare it inside, each call gets its own timer, so clearTimeout() cannot cancel the previous call's timer. That defeats the purpose of debouncing.
