# Question 01 - Advanced Async Programming

## How would you implement a function retryAsync that retries a given asynchronous function n times with a specified delay between attempts if the function fails?

Here’s a concrete implementation of a `retryAsync` function in JavaScript. This function will retry a given asynchronous function `n` times with a specified `delay` (in milliseconds) between each attempt if the function fails. It uses `async/await` for readability and promises for managing asynchronous operations:

```javascript
/**
 * Retries an asynchronous function a specified number of times with a delay between attempts.
 *
 * @param {Function} asyncFunction - The asynchronous function to retry.
 * @param {number} retries - The number of retry attempts.
 * @param {number} delay - The delay in milliseconds between retries.
 * @returns {Promise<*>} - Resolves with the result of asyncFunction or rejects after all attempts fail.
 */
async function retryAsync(asyncFunction, retries, delay) {
    for (let attempt = 1; attempt <= retries; attempt++) {
        try {
            // Attempt to execute the asynchronous function
            return await asyncFunction();
        } catch (error) {
            if (attempt === retries) {
                // Throw the error if the last attempt fails
                throw new Error(`Function failed after ${retries} attempts: ${error.message}`);
            }
            // Wait for the specified delay before the next attempt
            await new Promise(resolve => setTimeout(resolve, delay));
        }
    }
}

// Example usage:

// Mock asynchronous function that fails the first 2 times
let attempts = 0;
const unreliableAsyncFunction = async () => {
    attempts++;
    if (attempts < 3) {
        throw new Error('Temporary failure');
    }
    return 'Success!';
};

// Retry the unreliable function up to 5 times with a 1000ms delay
retryAsync(unreliableAsyncFunction, 5, 1000)
    .then(result => console.log(result)) // Should print "Success!" after 3rd attempt
    .catch(err => console.error(err.message));
```

### Explanation of the Code
1. **Parameters**:
   - `asyncFunction`: The function to execute, which is expected to return a promise.
   - `retries`: The maximum number of attempts to execute the function.
   - `delay`: The time (in milliseconds) to wait between retry attempts.

2. **Loop**:
   - The function is wrapped in a `for` loop that runs `retries` times.
   - `try/catch` blocks handle the success or failure of the asynchronous function.

3. **Delay**:
   - If the function fails, a `setTimeout` is wrapped in a promise to introduce a delay before the next retry.

4. **Error Handling**:
   - If all attempts fail, the error is thrown after the last retry, providing the caller with feedback.

This implementation is robust, handles asynchronous errors properly, and respects the specified retry count and delay.

