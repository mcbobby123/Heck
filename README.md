# Heck

Heck is a language I created for fun. There is no purpose for the language it self. I'm sure there's critical bugs. I made this for the challenge of making it. I didn't google anything about making programming languages. I think my parsing it probably really bad. I think parse time might grow exponentially with the length of the code, not really sure.

## Examples

More examples can be found [here](./examples)

### Hello World
```
return "Hello, World!";
```

#### Notes:
return can be used out side of code blocks to exit the program and print the final value.

Semicolons are may not always cause errors when excluded, but in many places they will, so use them.

### Variables & Datatypes
```
x = 1
_ = "a string"
another2 = true
something = [1, "another string"]
```

#### Notes
Variables names cannot start with a number.

WARNING: Be careful about what values you put in arrays. I discourage mutating variables, but it is valid. say you have this array:
```
x = 1
arr = [x + 1]
```
if print out `arr` you will find `[2]` as expected but say later in the code you reassign `x` to be `2` printing the array will now result in `[3]`. Your first thought may to instead assign `arr = [force (x + 1)]` however instead of forcing the value you assign into the array you are actually assign the result of forcing that value into the array. wtf? Ok so what I mean by that is force doesn't magically run whever it is used, if it is used inside a lazy value than the value won't get forced. What parameters to built in functions are lazy is kind of in consistent so proceed with caution.

### Functions
```
printAlias = fn! arg: print arg;
printAlias "I called a function!";
```
```
add = fn [a, b]: a + b;
return add [1, 2]
```

#### Notes:
Functions are composed of a single expression as the body.

Functions are usually lazy (sometimes its broken tho haha) but if you want a function to always be run immediately when called (maybe for io) use the `!` as seen in the first example.

Functions only accept a single argument, so pass an array and destructure values.

Functions create their own enclosed scope.

Functions have a value in scope called `recurse` that refers to the function being run. This can be useful if you want to use recursion inside a function that is not saved to a variable, but that often leads to uglier code so be careful with that.

### Code Blocks
```
result = {
  a = 1;
  a = a + 1;
  a = a + 1;
  return a;
};
return result + 1;
```
```
add = fn [a, b]: {
  return a + b;
};
return add [1, 2];
```

#### Notes:
Code blocks outside are treated as a single expression. This means they can be placed anywhere that any other value would be placed (Well actually I think theres a few places it might require `()` around it to parse correctly).

A code block can be placed as the body of a function to create longer functions.

Code blocks create their own enclosed scope.

### Built in functions

```
return sum [1,2,3];
```
Take the sum of an array. The `+` operator actually calls this internally. All infix operators also have a named version in scope. Some are limited to 2 parameters like (`divide` and `mod`), but others work for any number of elements like `product`, `any` (used for the or operation, `|`) and `every` (used for the and operator, `&`).
```
return filter [range [0, 20], fn n: n % 2 == 0];
```
`range` returns [begin, end).
`filter` takes an array and a predicate that accepts a value from the array and returns boolean (truthy probably works as well, I'm not sure).
```
addOne = fn a: a + 1;
div2 = fn a: a + 1;
addTwoDiv2 = compose [addOne, addOne, div2];
return addTwoDiv2 6;
```
This example returns 4 (not 5).

### Something cool

```
naturalNumbers = (fn n: [n, recurse n + 1]) 1
print take [100, naturalNumbers]
```

This code prints numbers "1, 2, 3 ... 100";

Here I am defining natural numbers to be an infinite linked list (I am informally defining linked lists as an array of a value, recurse). I used an function a function inline because I only needed to call it once and used `recurse` within the function for recursion. The end of a linked list can just omit the second value in the array.

`take` is a built in method that accepts a number and a linked list. It will return an array contain n elements from the linked list or just until the end was reached if there is one.

More built in functions can be found [./builtins.ts](./builtins.ts).

## Running

`deno run --allow-read mod.ts ./file.heck`
