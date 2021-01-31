## A short presentation of some JS basics


### Useful array functions


### `map`

  * is generally used for **applying processing** on each item of an array(if needed).
  * **does not mutate** the array that it's applied to.
  * **parameters**: 
     * a callback function that gets called for each array item - this function receives the following parameters:
       * array item
       * [optional] array item index
       * [optional] initial array
     * [optional] thisArg - to be used as `this` inside the callback function
  * **return value**: new array with the processed items.
  
*Example*
```javascript
const numbers = [ 1, 4, 9 ];
const squareRoots = numbers.map((current, index) => 
  console.log(`Calculating square root of item at index ${index}`) || Math.sqrt(current));
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
> Cool, right? I mostly use this trick when debugging and I've gone so far as binding it to a keyboard shortcut in VSCode.


### `reduce`

  * this one's usually used for **obtaining a single result**(be that an object or simply a mere primitive) by processing the items of an array.
  * **does not mutate** the array that it's applied to.
  * **parameters**:
    * a reducer function that will be applied to each array item and generates a single output - has the following parameters:
      * result - the result of calling the reducer function on the previous iteration
      * array item - current item in the array
      * [optional] array item index
      * [optional] initial array
    * [optional] an initial value 
      * if provided, at the first iteration, *result = initial value* and *current array item = first item in the array*
      * if not provided, at the first iteration, *result = first item in the array* and *current array item = second item in the array*
  * **return value**: a single value, based on the reducer function
  
  > *It can also be used to obtain another array, by giving the initial value as an empty array, but that's a bit pointless, right? I mean, that's what `map` is for!*
  
*Example*
```javascript
const numbers = [ 1, 2, 3 ];
const sum = numbers.reduce((result, current, index) => result + current, 0);
console.log(`Sum: ${sum}`);
```
  
*Output* 
```
Sum: 6
```


<details>
  <summary>
     <em>Click to see a more complex example</em>
  </summary>
  
```javascript
const cars = [{ brand: "BMW", color:"white" },
              { brand:"Audi", color:"red" },
              { brand:"Tesla", color:"black" }];
const attributes = cars.reduce(
    (result, current) => ({ brands: [...result.brands, current.brand],  //reducer function
                            colors: [...result.colors, current.color] }),
   
    { brands: [], colors: [] } // initial value
);
```
In this case, we want to gather the brands and the colors of all the items in the cars array.
So, the `attributes` object will be:
```
{ 
  brands: (3) ["BMW", "Audi", "Tesla"]
  colors: (3) ["white", "red", "black"]
}
```
</details>



 ### `filter`

   * exactly like the name suggests, this one is used for filtering an array based on a condition(a callback function)
   * **does not mutate** the array that it's applied to.
   * **parameters**:
     * a callback function that will be applied to each array item(needs to return a **truthy value** to include the array item in the final array) - has the following parameters:
       * array item - current item in the array
       * [optional] array item index
       * [optional] initial array
     * [optional] thisArg - to be used as `this` inside the callback function
   * **return value**: a new array with the filtered items(the ones for which the callback function returned a truthy value)

 *Example*
 ```javascript
 const grades = [ 7, 4, 10, 5 ];
 const passedGrades = grades.filter((current) => current >= 5);
 console.log(`Passed grades: ${passedGrades}`);
 ```

 *Output* 
 ```
 Passed grades: 7,10,5
 ```


### `find`

  * used for finding the first occurence of an array item that satisfies a certain condition.
  * **does not mutate** the array that it's applied to.
  * **parameters**:
    * a callback function that will be applied to each array item(needs to return a **truthy value** to return that particular array item and skip the next iterations) - has the following parameters:
      * array item - current item in the array
      * [optional] array item index
      * [optional] initial array
    * [optional] thisArg - to be used as `this` inside the callback function
  * **return value**: a single value, i.e. the first array item for which the callback returns a truthy value
  
*Example*
```javascript
const numbers = [ 1, 2, 3, 4 ];
const perfectSquare = numbers.find((current) => Number.isInteger(Math.sqrt(current)));
console.log(`Perfect square: ${perfectSquare}`);
```
  
*Output* 
```
Perfect square: 1
```
> Notice that the `numbers` array contains two perfect squares, but `find` will only return the first one, i.e. 1.


### `some`

  * used for checking if **at least one** array item satisfies a certain condition(given by the callback).
  * **does not mutate** the array that it's applied to.
  * **parameters**:
    * a callback function that will get called for each array item - has the following parameters:
      * array item - current item in the array
      * [optional] array item index
      * [optional] initial array
    * [optional] thisArg - to be used as `this` inside the callback function
  * **return value**: boolean: true, if at least one array item for which the callback returns a truthy value and false otherwise
  
*Example*
```javascript
const numbers = [ 1, 2, 3, 4 ];
const numbersContainPerfectSquares = numbers.some((current) => console.log(`Checking if ${current} is a perfect square`) || Number.isInteger(Math.sqrt(current)));
console.log(`Does the numbers array contain perfect squares? Answer: ${numbersContainPerfectSquares ? 'Yes' : 'No'}`);
```
  
*Output* 
```
Checking if 1 is a perfect square
Does the numbers array contain perfect squares? Answer: Yes
```
> Notice how only one console.log gets called? That's because `some` needs to find at least one array item for which the callback returns `true`, so after it finds it, the next iterations are skipped.
