# A short presentation of some JS basics

## Table of contents
 * [Truthy and Falsy values](#truthy-and-falsy-values)
 * [Short circuit and Ternary operators](#short-circuit-and-ternary-operators)
 * [Few ES6 useful features](#few-es6-useful-features)
   * [Template literals](#template-literals)
   * [Destructuring assignment](#destructuring-assignment)
     * [Object destructuring](#object-destructuring)
     * [Array destructuring](#array-destructuring)
   * [Rest parameter](#rest-parameter)
   * [Spread operator](#spread-operator)
 * [Useful array functions](#useful-array-functions) 
   * [map](#map)
   * [reduce](#reduce)
   * [filter](#filter)
   * [find](#find)
   * [some](#some)
   * [every](#every)   


## Truthy and Falsy values

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


## Short circuit and Ternary operators

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

## Few ES6 useful features

### Template literals

**Template literals** are a brand new(and prettier) way of creating strings which supports the following:
  * embedding variables and expressions(this is what I mostrly use template literals for)
  * multi-line strings

So, instead of the old fashioned `+` operator you were using to concatenate strings with variables/expressions and the `\n` escape character you were using to add a new line, now you have a more elegant solution.

Template literals are enclosed by the backtick characters ` `` ` and in order to embedd a certain variable/expression you have to place it inside curly braces `{}` preceded by a `$`.
So it will be something like this: `${randomVariable}`.
Everything that is included inside the curly braces will be evaluated at runtime.
Let's see an example!

*Example*
```javascript
const worldString = 'World';
const plusOperatorString = 'Hello ' + worldString + '!\nI am a multi-line string';
const templateLiteralsString = `Hello ${worldString}! 
I am a multi-line string`;
console.log(plusOperatorString); // should log exactly the same thing
console.log(templateLiteralsString);
```

*Output* 
```
Hello World!
I am a multi-line string
Hello World! 
I am a multi-line string
```

### Destructuring assignment

Another cool feature from ES6 is the **destructuring assignment**, which basically lets you extract specific parts of an object/array into variables.
I would say that *object destructuring* is more common than *array destructuring*, so let's start with that.

#### Object destructuring

Object destructuring gives you the posibility to extract only some *properties* of an object and assign them to certain variables. You can also extract all the object properties, don't get me wrong, but if an object has quite a lot of properties, that would be a headache. Let's see a simple example!

*Example*
```javascript
const car = { color: 'red', brand: 'BMW', model: 'X1' };
const { color, brand } =  car; // extracting properties 'red' and 'brand' from the 'car' object
console.log(`This is a ${color} ${brand}`);
```
*Output*
```
This is a red BMW
```
>Notice we didn't need the `model` property, so we didn't mention it.

This is pretty useful when declaring function parameters, as you can precisely name the exact properties of an object you need inside the function's body.
Let's see another example!

*Example*
```javascript
const car = { color: 'red', brand: 'BMW', model: 'X1', year: '2020' };
const printCar = ({ color, brand, ...rest }) => console.log(`This is a ${color} ${brand}. It's an ${rest.model} release in ${rest.year}`);
printCar(car);
```
*Output*
```
This is a red BMW. It's an X1 released in 2020
```
>In this example, the function knows to extract 'color' and 'brand' properties from the object it receives as the parameter. But Javascript's freedom(*loosely typed*) is a two way street, so since it only expects an object as a parameter, it will give you an error if you pass `null` or `undefined` in. A trick for this would be another ES6 feature, the **default values**, about which I will talk in a bit.

>The three-dotted thing you see there is called the **rest parameter**(I will talk about this also) and it gathers all the remaining(not named in the extraction) properties under an object(named any way you want). You can access those properties via this object, in this case, `rest.propertyName`. *Rest parameter* must be the **last element**!

It's also worth mentioning that if a certain extracted property does not exist on that object, the variable would be, you guessed it, `undefined`.

#### Array destructuring

Array destructuring works in the same way as the object destructuring, except that, instead of properties, you extract *items* from the array. Example comming up!

*Example*
```javascript
const fruits = [ 'bananas', 'apples', 'cherries', 'kiwis', 'peaches' ];
const [ bananas, apples, , kiwis ] = fruits;
console.log(`I gotta buy some ${bananas}, ${apples} and ${kiwis}`);
```
*Output*
```
I gotta buy some bananas, apples and kiwis
```
>An obvious difference from the object destructuring is that you can name your extracted variables any way you want, as opposed to the other case, when you need to use the properties' names.

>Another difference would be that in this case, the order of the extracted variables matters, so the first extracted variable corresponds to the first item in the array and so on.

>A similarity we can observe is that we can extract only the desired items in the array, so, for example, we didn't need the third item in the array(cherries) so we skipped it, but still took into consideration its position(hence the nearby double commas). We also didn't need the last item in the array(peaches), but because it was the last one, we didn't have to mark it at all.

Another cool trick we can do with the array destructuring is extract only some of the first items and collect the rest by making use of the **rest parameter**, similar to what we did with object destructuring, but not quite. Let's see a quick example!

*Example*
```javascript
const fruits = [ 'bananas', 'apples', 'cherries', 'kiwis', 'peaches' ];
const [ bananas, apples, ...rest ] = fruits;
console.log(`I gotta buy some ${bananas}, ${apples}, but I don't need any ${rest.join(',')}`);
```
*Output*
```
I gotta buy some bananas, apples, but I don't need any cherries,kiwis,peaches
```
>So we only extracted bananas and apples, and we collected the rest of the array items under the `rest` array - YES! - in this case, `rest` is an array. *Rest parameter* must be the **last element** in the array!

>`rest.join(',')` creates a string with the array elements concatenated by a comma

### Rest parameter

As promised, here is a short overview of the **rest parameter**, another ES6 feature. This one was introduced more for allowing an indefinite number of parameters for functions, but, as you've seen, it can be very useful in some other cases as well.
It's syntax is simple: three dots followed by a variable name(any name you want).

*Example*
```javascript
const printFruits = (...args) => console.log(`I gotta buy some ${args.join(',')}`);
printFruits('bananas', 'apples', 'cherries', 'kiwis', 'peaches');
```
*Output*
```
I gotta buy some bananas,apples,cherries,kiwis,peaches
```
>`args` variable is an array consisting of every argument you pass to the `printFruits` function, in this case its value will be `['bananas', 'apples', 'cherries', 'kiwis', 'peaches']`

You can also name some of the first arguments passed to the function, like in the following example.

*Example*
```javascript
const printFruits = (bananas, apples, ...args) => 
  console.log(`I gotta buy some ${bananas}, ${apples}, but I don't need any ${args.join(',')}`);
printFruits('bananas', 'apples', 'cherries', 'kiwis', 'peaches');
```
*Output*
```
I gotta buy some bananas, apples, but I don't need any cherries,kiwis,peaches
```
>But remember, any named parameters must come before the rest parameter!

### Spread operator




## Useful array functions


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
