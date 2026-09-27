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
