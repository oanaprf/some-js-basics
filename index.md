## A short presentation of some JS basics

### Table of contents
 * [Truthy/Falsy values](#truthy-falsy-values)
 * [Short cicuit & ternary operators](#short-circuit---ternary-operators)
 * [Useful array functions](#useful-array-functions) 
   * [map](#map)
   * [reduce](#reduce)
   * [filter](#filter)
   * [find](#find)
   * [some](#some)
   * [every](#every)   


### Truthy/Falsy values

In the next topics I am going to refer quite a lot to these two terms, as they come in handy when you are a lazy coder and a one-liners enthusiast(like me!).
You can totally live without them, but why write more code than needed? But let's get to business!

**Truthy values** are Javascript's way of aknowledging a value's existence, i.e. *a value is **truthy** if JS translates it to `true`*.  
You get the point, so if JS doesn't translate a certain value to `true`, then it certainly is a **falsy value**.  
Actually, we'll go the other way around and play the elimination game since JS only translates **7 values** to `false`:
 * `0`
 * `''`
 * `false`
 * `undefined`
 * `null`
 * `NaN`
 * `0n` which is 0 as a BigInt(bla bla)

**So, all the rest of the values are truthy? Yep!**  
*Even an empty Object?* Yep! Every object is truthy in JS.  
*Even an empty Array?* Most certainly! Wanna know why? Because *JS Arrays are just a special type of Objects*.

> Just to recap, numbers(that are not 0), Strings(that are not empty), Objects and Arrays(including the empty ones) **are all truthy values**! And a few examples:
`1`, `'0'`, `' '`, `'undefined'`, `{}`, `{ uselessAttr: '' }`, `[]`, `[0]` and, of course, the boolean `true` => all truthy


### Short circuit & ternary operators 

Short circuit operators are logical AND `&&` and logical OR `||`. Aside from their obvious and well-known behavior(they check for all(&&), respective at least one(||) operands to be `true` - or truthy), they also have some "hidden" features:  

`&&` will return the **first falsy** operand, and if it doesn't find it, then it returns the last operand => therefore, you can use it as a shorter if  
*Example*
```javascript
const truthyValue = 'truthy value';
truthyValue && console.log('truthyValue is a truthy value indeed!');
```
*Output*
```
truthyValue is a truthy value indeed!
```  

`||` will return the **first truthy** operand, and, similar to `&&` if it doesn't find it, it will return the last operand => you can use this one as a cool trick while debugging, you'll spot it later in this guide  
*Example*
```javascript
const falsyValue = '';
falsyValue || console.log('falsyValue is falsy:(');
```
*Output*
```
falsyValue is falsy:(
```

**Ternary operator** is the short form of an if. It's really simple, look:  

*Example*
```javascript
const truthyValue = 'truthy value';
const falsyValue = '';
truthyValue ? console.log('truthyValue is a truthy value!') : console.log('truthyValue is a falsy value!');
falsyValue ? console.log('falsyValue is a truthy value!') : console.log('falsyValue is a falsy value!');
```
*Output*
```
truthyValue is a truthy value!
falsyValue is a falsy value!
```

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
const someArePerfectSquares = numbers.some((current) => 
    console.log(`Checking if ${current} is a perfect square`) || Number.isInteger(Math.sqrt(current))
);
console.log(`Does the numbers array contain perfect squares? Answer: ${someArePerfectSquares ? 'Yes' : 'No'}`);
```
  
*Output* 
```
Checking if 1 is a perfect square
Does the numbers array contain perfect squares? Answer: Yes
```
> Notice how only one console.log gets printed? That's because `some` needs to find at least one array item for which the callback returns `true`, so after it finds it, the next iterations are skipped.


### `every`

  * similar to `some`, but this one checks if **every** array item satisfies the given condition
  * **does not mutate** the array that it's applied to.
  * **parameters**:
    * a callback function that will get called for each array item - has the following parameters:
      * array item - current item in the array
      * [optional] array item index
      * [optional] initial array
    * [optional] thisArg - to be used as `this` inside the callback function
  * **return value**: boolean: true, if for every array item the callback returns a truthy value and false otherwise
  
*Example*
```javascript
const numbers = [ 1, 2, 3, 4 ];
const allArePerfectSquares = numbers.every(
    (current) => console.log(`Checking if ${current} is a perfect square`) || Number.isInteger(Math.sqrt(current))
);
console.log(`Does the numbers array only contain perfect squares? Answer: ${allArePerfectSquares ? 'Yes' : 'No'}`);
```
  
*Output* 
```
Checking if 1 is a perfect square
Checking if 2 is a perfect square
Does the numbers array only contain perfect squares? Answer: No
```
> Uh oh! Only two console.log got printed! I wonder why is that? Simple! `every` needs the callback function to return a truthy value for all the array items! So if it finds one that doesn't, it stops and returns false, skipping the next iterations.
