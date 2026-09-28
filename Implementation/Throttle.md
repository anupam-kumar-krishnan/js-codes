## Throttle

```js
function throttle(func, delay) {
  let lastCall = 0;

  return function (...args) {
    const now = Date.now();

    if (now - lastCall >= delay) {
      lastCall = now;
      func.apply(this, args);
    }
  };
}
```

**When the throttled function is called:**
- Get the current time using Date.now().
- Subtract lastCall from the current time.
- If the difference is greater than or equal to delay, execute the function.
- Update lastCall so the function cannot execute again until the delay has passed.

### Why use func.apply(this, args)?

- args preserves the arguments passed to the throttled function.
- this preserves the calling context.
- For example, if your original function accepts arguments, the throttled version will still pass them through correctly.

<img width="1103" height="670" alt="image" src="https://github.com/user-attachments/assets/da3bcd3b-7664-463f-a047-951c07e1d764" />
