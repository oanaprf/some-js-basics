# A short presentation of some JS basics

## Useful array functions

### `map`

  * is generally used for **applying processing** on each item of an array(if needed).
  * **does not mutate** the array that it's applied to.
  * **parameters**: 
     * a callback function that dictates what processing will be done on each array item - this function receives the following parameters:
       * array item
       * [optional] item index
       * [optional] initial array
     * [optional] thisArg - to be used as `this` inside the callback function
  * **return value**: new array with the processed items.
  
  *Example*
  
  ```javascript
  const numbers = [ 1, 4, 9 ];
  const squareRoots = numbers.map((number, index) => 
    console.log(`Calculating square root of item at index ${index}`) || Math.sqrt(number));
  console.log(`Square roots: ${squareRoots}`);
  ```
  
  *Output* 
  ```
  Calculating square root of item at index 0
  Calculating square root of item at index 1
  Calculating square root of item at index 2
  Square roots: 1,2,3
  ```
  
> Notice that cool little trick I did there with the `console.log`?
> I really prefer one-liners instead of using braces for blocks of code(when possible).
> 
> So the thing is that `console.log()` is a *falsy* value(it always returns `false`) and using it with the logical operator OR(|| - which *returns the first truthy
value* or the last value if no truthy value is found) will result in returning `Math.sqrt(number)`.
> 
> Cool, right? I mostly use this trick when debugging and I've gone so far as adding a keyboard shortcut in VSCode for `console.log() ||`.
  
  
