<!--
author: sibaku

version:  0.0.2

language: en

attribute: [Sibaku github.io](https://sibaku.github.io/)
    by sibaku (he/him) ([twitter](https://twitter.com/sibaku1), [mastodon](https://mas.to/@sibaku))
    is licensed under [MIT](https://opensource.org/licenses/MIT)
-->

# Converting common math notation to code

If you try to look up an algorithm for a problem you are trying to solve, you might stumble upon scientific papers or articles that explain how to calculate that solution.

Often times though, you might not encounter (pseudo-) code, but just math notations, where the writer assumed (or didn't have time or space in the publication...) that you can just go from there, regardless of your chosen programming language.

Now, from social media, I have seen many people coming from a non-math/computer science background being frustrated and intimidated by the mathematical notation used.

So, inspired by this tweet by Freya Holmér I saw a while ago (https://x.com/FreyaHolmer/status/1436696408506212353) I thought about just compiling a list of symbols and things you may encounter together with a very quick explanation and some words/readings that you can maybe better search for.

The code is in JavaScript, as it can run in your browser, but will look mostly the same in most major languages. For many of the examples, you can execute the code with example values and change that code directly in the text fields and run it again.

This is in no way a guarantee that you will be able to implement every paper you see immediately. Frankly, a lot of papers are not written clearly enough for that to work and sometimes use their own notation (hopefully they defined what they use!). Additionally, math notation, while generally pretty standard, does have some quirks that real languages have, like "dialects" and "creative usage". But hopefully, if you encounter a symbol or a bunch of symbols and indices that seem very complex, you can decipher them bit by bit using this basic guide 😊

Currently, this document contains the following symbols: $\sum, \prod, \bigcup, \bigcap, \{ \in, \ni, \not\in, \not\ni, \delta_i^j, \circ, n!, \binom{n}{k},  \operatorname{f}'(x), \frac{d \operatorname{f}}{d x}, \frac{\partial \operatorname{f}}{\partial x}, \nabla \operatorname{f}$

This document is viewable as a normal markdown document, though it will be missing some of the code features made possible by LiaScript.

Happy coding!

## Summation and product symbols: $\sum$, $\prod$ (and friends $\bigcup$, $\bigcap$!)

The symbols $\sum$, $\prod$, $\bigcup$ and $\bigcap$ all behave in an extremely similar way. They define some kind of series of operations using indices that get combined. First, we will go through the basic case for $\sum$ and $\prod$, as they use basic math operations. Afterwards we will only look at different ways people put indices on these symbols, which works the same for all of them and then finish with the specifics regarding $\bigcup$, $\bigcap$.

### Summation $\sum$

The summation symbol $\sum$ does, as the name suggest, sum things. The symbol is a capital greek letter "Sigma", so **S**igma for **s**um.

**Basic form**: $\sum\limits_{i=0}^{n}f_i$ or $\sum_{i=0}^n f_i$

**Parts**:

* $i=0$: Name of the index with initial value
* $n$: Last value of the named index. **ATTENTION!** The last index is inclusive, so $i$ will take on that value and not stop one before that
* $f_i$: The $i$-th summation term. This could be any expression using (or not using!) $i$

**Meaning**: Sum each term $f_i$, starting at $i=0$ and ending at $i=n$. If the upper stop value is lower than the initial value, then the sum is $0$, e.g. $\sum\limits_{i=1}^0 = 0$.

**Code**:

```js +Summation.js
// You can change the function f in the Demo.js tab!

// this will contain the result in the end
let sum = 0;
// go from startI to n
// ATTENTION! The last index is n, so we use <= instead of <
for(let i = startI; i <= n; i++){
    // the i-th entry, here given as a function
    sum += f(i);
    // subscripts are also commonly associated with arrays, so the i-th element is just the i-th entry of some array like the following line:
    // sum += f[i];
}

console.log(`The sum of all f_i from ${startI} to ${n} is: ${sum}`);
console.log(`The entries f_i:`);
[...new Array(n - startI + 1).keys()].map(i => (i + startI) + ": " + (i + startI)).forEach(x => console.log(x));
```
```js -Demo.js
// here we define the function f and the minimum/maximum index startI/n

function f(i){
    // just return the number itself, so we sum the first n numbers
    return i;
}
// define the maximum index
const startI = 1;
const n = 10;
```
<script>
@input(1)
@input(0)
""
</script>

### Product $\prod$

The product symbol $\prod$ specifies a product of multiple terms. The symbol is a capital greek letter "Pi", so **P**i for **p**roduct.

**Basic form** $\prod\limits_{i=0}^n f_i$ or $\prod_{i=0}^n f_i$

**Parts**:

* $i=0$: Name of the index with initial value
* $n$: Last value of the named index. **ATTENTION!** The last index is inclusive, so $i$ will take on that value and not stop one before that
* $f_i$: The $i$-th product term. This could be any expression using (or not using!) $i$

**Meaning**: Multiply each term $f_i$, starting at $i=0$ and ending at $i=n$. If the upper stop value is lower than the initial value, then the sum is $1$, e.g. $\prod\limits_{i=1}^0 = 0$.

**Code**:

```js +Product.js
// You can change the function f in the Demo.js tab!

// this will contain the result in the end
let product = 1;
// go from startI to n
// ATTENTION! The last index is n, so we use <= instead of <
for(let i = startI; i <= n; i++){
    // the i-th entry, here given as a function
    product *= f(i);
    // subscripts are also commonly associated with arrays, so the i-th element is just the i-th entry of some array like the following line:
    // sum *= f[i];
}
console.log(`The product of all f_i from ${startI} to ${n} is: ${product}`);
console.log(`The entries f_i:`);
[...new Array(n - startI + 1).keys()].map(i => (i + startI) + ": " + (i + startI)).forEach(x => console.log(x));
```
```js -Demo.js
// here we define the function f and the maximum index n

function f(i){
    // just return the number itself, so we multiply the first n numbers, which is called the factorial
    return i;
}
// define the maximum index
const startI = 1;
const n = 6;
```
<script>
@input(1)
@input(0)
""
</script>
### Set notations $\sum\limits_{x\in S} \operatorname{f}(x)$, $\sum\limits_{i\in I}\operatorname{f}(x_i)$

Here, we will only use the sum $\sum$ symbol, as the index variants are the same for all others.

The set notation is very common when you are not dealing with a neat sequence of increasing numbers. 

A set is just a "collection of unique things", so no duplicates. Most programming languages have some kind of set type included. More generally, if you are programming, this usually comes down to having some iterable collection, even just arrays.

You will generally find this notation in two versions:

 $\sum\limits_{x\in S} \operatorname{f}(x)$: Go through each element of the set $S$, plug it in some function $\operatorname{f}$ and use the result as a term in the sum

$\sum\limits_{i\in I}\operatorname{f}(x_i)$: Go through each index in the set $I$, plug the $i$-th element in some function $\operatorname{f}$ and use the result as a term in the sum.

**Code**  $\sum\limits_{x\in S} \operatorname{f}(x)$:

```js +Summation.js
// this will contain the result in the end
let sum = 0;
// go over all elements stored in S
for(const x of S){
    // x is an element, add the result of applying f to x to the sum
    sum += f(x);
}
console.log(`The sum of all f_i is: ${sum}`);
console.log("The entries of S");
S.forEach(x => console.log(x));
```
```js -Demo.js
// define the set S and the function f

// here we use a set, but other containers can be used the same
const S = new Set([1,3,5,7,9]);

function f(x){
    // compute x^2, so the sum of squared terms
    return x * x;
}
console.log("Compute the sum of the first 5 odd squared integers");
```
<script>
@input(1)
@input(0)
""
</script>
**Code**  $\sum\limits_{i\in I}\operatorname{f}(x_i)$:

```js +Summation.js
// this will contain the result in the end
let sum = 0;
// go over all indices stored in I
for(const i of I){
    // i is an index corresponding to some element x. Usually, you would store these in some array-like structure, which addresses elements by their index, like in this example
    sum += f(x[i]);
}
console.log(`The sum of all f_i is: ${sum}`);
console.log("The entries of I");
I.forEach(x => console.log(x));
console.log("The entries of x");
x.forEach((x,i) => console.log(`${i}: ${x}`));
```
```js -Demo.js
// define the index set I, the element vector x and the function f

// here we use a set, but other containers can be used the same
const I = new Set([0,1,2,3,4,5]);

// the vector can contain duplicates
const x = [1,5,10,10,5,1];

function f(x){
    // return x
    return x ;
}
console.log("Compute the sum of the 6-th row of pascal's triangle");
```
<script>
@input(1)
@input(0)
""
</script>
### Nested iterations $\sum\limits_{i=0}^n \sum\limits_{j=0}^m f_{i,j}$, $\sum\limits_{i=0}^n \sum\limits_{j=0}^i f_{i,j}$, $\sum\limits_{i=0}^n f_i\sum\limits_{j=0}^m g_{j}$, $\sum\limits_{i=0}^n \sum\limits_{j=0}^m \sum\limits_{k=0}^p f_{i,j,k}$, ...

Here, we will only use the sum $\sum$ symbol, as the index variants are the same for all others.

Nested iterations are just what the name says: You put an iteration into another one. These can all be reduced to handling a single iterations:

$$
\begin{align*}
\sum\limits_{i=0}^n \sum\limits_{j=0}^m f_{i,j}  &= \sum\limits_{i=0}^n g_i \\
g_i &= \sum\limits_{j=0}^m f_{i,j}
\end{align*}
$$

So even if you have multiple iterations, you can rewrite them one by one using this rule and apply the previous sections.

These multiple iterations will generally be converted into nested for loops, where leftmost symbol is the outer loop and the rightmost symbol is the inner loop.

**ATTENTION:** Sometimes, the indices of one iteration symbol are dependant on the previous ones. A symbol on the left is **never** dependant on the right ones. In code, this makes sense, as the outer loop can't depend on variables of the inner loops. Be careful to check, whether an inner index depends on outer ones.

The code will show the base case of $\sum\limits_{i=0}^n \sum\limits_{j=0}^m f_{i,j}$. This kind of loop often comes up in fields such as image processing, where you iterate over a 2D pixel image.

**Code: $\sum\limits_{i=0}^n \sum\limits_{j=0}^m f_{i,j}$** 

```js +Summation.js
// this will contain the result in the end
let sum = 0;
// go from startI to n for the first/outer loop
// ATTENTION! The last index is n, so we use <= instead of <
for(let i = startI; i <= n; i++) {
    // go from 0 to m for the second/inner loop
    // ATTENTION! The last index is m, so we use <= instead of <
    for(let j = startJ; j <= m; j++) {
        // the entry (i,j), here given as a function
        sum += f(i,j);
        // subscripts are also commonly associated with arrays. In this case, this would be a 2D array. Then the element (i,j) in a 2D array f would look like the following:
        // sum += f[i][j];
    }
}
console.log(`The sum of all f_{i,j} from i = ${startI} to ${n}, j = ${startJ} to ${m} is: ${sum}`);
```
```js -Demo.js
// define f and ranges
function f(i,j){
    return i*j;
}

const startI = 1;
const n = 2;

const startJ = 1;
const m = 3;

console.log("f(i,j) = i*j");
```
<script>
@input(1)
@input(0)
""
</script>

A commonly found variant of the nested iteration is the one, where the i-th and j-th component are not directly coupled. This allows for a very simple direct optimization.

**Code: $\sum\limits_{i=0}^n f_i \sum\limits_{j=0}^m g_{j}$** 

```js +Summation.js
// this will contain the result in the end
let sum = 0;
// go from startI to n for the first/outer loop
// ATTENTION! The last index is n, so we use <= instead of <
for(let i = startI; i <= n; i++) {
    // since f_i is independent of j, we can compute the value here. This allows us to not compute the same value j-times
    const fi = f(i);
    // in array notation
    // const fi = f[i];
    
    // this is the sum of the inner loop
    var sumG = 0;

    // go from startJ to m for the second/inner loop
    // ATTENTION! The last index is m, so we use <= instead of <
    for(let j = startJ; j <= m; j++) {
        // the inner entry is independent of f
        sumG += g(j);
        // in array notation
        // sumG += g[j];
    }
    // apply f_i to the sum of the inner loop, that is the entry to add to the outer sum
    sum += fi * sumG;
}
console.log(`The sum of all (i,j) entries with f_i, g_j from i = ${startI} to ${n}, j = ${startJ} to ${m} is: ${sum}`);
```
```js -Demo.js
// define f and ranges
function f(i){
    return i;
}

function g(j){
    return j*j;
}

const startI = 1;
const n = 2;

const startJ = 1;
const m = 2;

console.log("f(i) = i, g(j) = j*j");
```
<script>
@input(1)
@input(0)
""
</script>
### Index variants $\sum\limits_{i}^n$, $\sum\limits_{i}$

Sometimes, authors don't explicitly write down the ranges of the iteration variables. In these cases, you will have to check the descriptions, what exactly is meant. 

In general, this notation will just mean: "If it isn't written down, use the default full range".

If no lower initial value is given, this is generally the "first" index. So if the formula iterates over an array or some kind of numbered sequence of elements, it will start at 0 (though you might need to be careful, since sometimes mathematicians and some programming languages start with 1!).

If no upper end value is given, this is generally the "last" index. Again, for an array or numbered sequence, this will be "length - 1" (or "length" for 1-based indices).

### Conditions on indices:  $\sum\limits_{\substack{i \in I \\ i \neq \frac{n}{2}}}^n f_i$ , $\sum\limits_{i=0}^n \sum\limits_{\substack{j = 0 \\ j \neq i}}^m f_{i,j}$, ...

Sometimes, you will see conditions added to iteration variables. These are generally written below the initial index and may use any kind of statement, that must be true. If it isn't true, then the index is skipped. Usually, these conditions are not very complicated and often even allow for a way to calculate these indices without explicitly going over each value.

The general form will look something like this:

$$
\sum\limits_{\substack{i = 0 \\ \operatorname{p}(i)}}^n f_i
$$

Here, $\operatorname{p}$ is any condition, for example $\operatorname{mod}(i,2) = 0$.

**Code $\sum\limits_{\substack{i = 0 \\ \operatorname{p}(i)}}^n f_i$**: 

```js +Summation.js
// this will contain the result in the end
let sum = 0;
// go from startI to n
// ATTENTION! The last index is n, so we use <= instead of <
for(let i = startI; i <= n; i++) {
    // check, if the condition p holds. If not, skip this index
    if(!p(i)) {
        continue;
    }
    // the i-th entry, here given as a function
    sum += f(i);
    // subscripts are also commonly associated with arrays, so the i-th element is just the i-th entry of some array like the following line:
    // sum += f[i];
}
console.log(`The sum of all f_i from ${startI} to ${n} is: ${sum}`);
```
```js -Demo.js
// define f, the range and the predicate p
function f(i){
    return i;
}

function p(i){
    // only even numbers
    return i % 2 == 0;
}

const startI = 1;
const n = 10;

console.log("f(i) = i, p(i) = isEven(i)");
```
<script>
@input(1)
@input(0)
""
</script>

### Multiple iterations folded into one: $\sum\limits_{i = 0,j=0}^{n,m}f_{i,j}$, $\sum\limits_{i \in I,j\in J}f_{i,j}$, ...

This is just a shorthand notation, where multiple nested iterations, as seen in a previous section, are combined into one symbol.

This is often combined with things like conditions and other index variants (see previous sections), so they can be a bit harder to read. 

It is best to remember, that you can write them out and then apply rules from the other sections, so for example:

$$
    \sum\limits_{i = 0,j=0}^{n,m} f_{i,j}= \sum\limits_{i = 0}^{n}\sum\limits_{j=0}^{m}f_{i,j}
$$

and

$$
\sum\limits_{i \in I,j\in J}f_{i,j} = \sum\limits_{i \in I} \sum\limits_{j\in J}f_{i,j}
$$

### Union $\bigcup$, intersection $\bigcap$ and general iteration symbol rule

As these symbols, follow the same rules as the sum and product, we will write down a general rule (sometimes people might even define other symbols that work the same way!) and then tell you how these new symbols work.

The union $\bigcup$ is the repeated application of the set union operation $\cup$. A set is just a "collection of unique things", so no duplicates. Most programming languages have some kind of set type included. The union of two sets is a new set that contains all entries of both sets. You can think of $\cup$ as a cup that gathers things falling into it.

The intersection $\bigcap$ is the repeated application of the set intersection operation $\cap$. The intersection of two sets is a new set containing only those elements that are found in both sets. Elements only in one of the sets are not included.

**Basic forms**: $\bigcup\limits_{i=0}^n f_i$ or $\bigcup_{i=0}^n f_i$ and $\bigcap\limits_{i=0}^n f_i$ or $\bigcap_{i=0}^n f_i$

**Parts**: See the section about [Sum](#Summation-$\sum$) or [Product](#Product-$\prod$)

**Meaning**: Apply a union or intersection with each term $f_i$, starting at $i=0$ and ending at $i=n$. If the upper stop value is lower than the initial value, then the union is the empty set $\emptyset$, e.g. $\bigcup\limits_{i=1}^0 = \emptyset$. The intersection is a bit more tricky and you probably won't encounter this issue when implementing an algorithm. The result of the empty intersection is the set of all possible elements. So, if we call that set $A$ (for "all", sometimes also $\Omega$ ("Omega")), then we have $\bigcap\limits_{i=1}^0 = A$.

**General coding rule**: For each iteration symbol we define an initial value $v$ and an operation $\operatorname{op}$ that combines that value with a term in the iteration to produce the new value. Here is a list of the so far discussed symbols:

* $\sum$: $v=0$, $\operatorname{op} = +$
* $\prod$: $v=1$, $\operatorname{op} = *$
* $\bigcup$: $v= \emptyset$ (empty set), $\operatorname{op} = \cup$
* $\bigcap$: $v= A$ (set of all elements), $\operatorname{op} = \cap$
  * Note: For this operation you would probably just start with the first element and handle no elements as a special case
  
Then the general code will look like this:

```js
let v = ...

for(let i = 0; i <= n; i++){
    v = op(v,f(i));
}
```

For the union $\bigcup$ this results in:

```js +Union.js
let v = new Set();

for(let i = startI; i <= n; i++){
    // union is defined in SetOperation.js
    v = union(v,f(i));
}

console.log(`The union of all sets for i = ${startI} to ${n}: {${Array.from(v).join(", ")}}`);
```
```js -SetOperation.js
function union(a,b){
    // depending on your browser, the Set class might have a union method. If not, this is a simple replacement

    // new empty set
    // set takes care of ignoring duplicates
    let result = new Set();

    // add all entries of a
    for (const v of a) {
        result.add(v);
    }
    // add all entries of b
    for (const v of b) {
        result.add(v);
    }

    return result;
}
```
```js -Demo.js
// here we define f and the ranges

// f is a function here, but we will just take the index and look in an array
const sets = [
    new Set([1,2,3]),
    new Set([1,4]),
    new Set([1,2,3,5]),
    new Set([2,5,9]),
    new Set([13]),
];

// iteration term, depending on the implementation, we could just use the array directly
function f(i){
    return sets[i];
}

// ranges
const startI = 0;
const n = sets.length - 1;

console.log("All sets:");
sets.forEach(s => console.log(`{${Array.from(s).join(", ")}}`));
console.log();
```
<script>
@input(1)
@input(2)
@input(0)
""
</script>

For the intersection $\bigcap$ this results in:

```js
// we handle this one a bit special
if(n - startI < 0){
    // handle special case -> set of all possible values
}else{
    // use first value as input
    let v = new Set(f(startI))

    // we start one after the first, as we already included that one
    for(let i = startI+1; i <= n; i++){
        v = intersection(v,f(i));
    }

    console.log(`The intersection of all sets for i = ${startI} to ${n}: {${Array.from(v).join(", ")}}`);
}


```
```js -SetOperation.js
function intersection(a,b){
    // depending on your browser, the Set class might have a intersection method. If not, this is a simple replacement
    // new empty set
    let result = new Set();

    // the intersection is at most as big as the smaller set, since elements must be contained in both. So we make a always the smaller one
    if(a.size > b.size){
        // destructuring swap
        [a, b] = [b, a];
    }

    // add all entries of a, if they are also in b
    for (const v of a) {
        if(b.has(v)){
            result.add(v);
        }
    }

    return result;
}
```
```js -Demo.js
// here we define f and the ranges

// f is a function here, but we will just take the index and look in an array
const sets = [
    new Set([1,2,3]),
    new Set([1,7,2]),
    new Set([1,2,3,5]),
    new Set([2,1,9]),
    new Set([1,2,28,17,13]),
];

// iteration term, depending on the implementation, we could just use the array directly
function f(i){
    return sets[i];
}

// ranges
const startI = 0;
const n = sets.length - 1;

console.log("All sets:");
sets.forEach(s => console.log(`{${Array.from(s).join(", ")}}`));
console.log();
```
<script>
@input(1)
@input(2)
@input(0)
""
</script>
## General: $\{$,  $\in$, $\ni$, $\not\in$, $\not\ni$, $\delta_i^j$, $\circ$

This section contains a few symbols that you encounter in various contexts.

### Cases: $\{$

This is a pretty straightforward analog to if/else if/else constructs. The results of these curly brackets is the value that is listed with the fitting condition. The basic form looks something like this:

$$
\operatorname{f}(x) = \begin{cases}
    0 &  \text{if } x \leq 0\\
    x & \text{if } x < 1 \\
    1 & \text{else}
\end{cases}
$$

While this is a specific example, others will look the same way, just with more or less cases.

**Code**

```js +Cases.js
// code for the above example
function f(x){
    if(x <= 0){
        return 0;
    } else if (x < 1){
        return x;
    } else {
        return 1;
    }
}
```
```js -Demo.js
// compute the function for a few values
const xv = [-2, -1, 0, 0.5, 1, 2];

console.log("Apply function:");
xv.forEach(x=> console.log(`f(${x}) = ${f(x)}`));
```
<script>
@input(0)
@input(1)
""
</script>

### Function composition $\circ$

If you have two functions $\operatorname{f}(x)$ and $\operatorname{g}(x)$, then you can define a third function:

$$
\begin{align*}
   \operatorname{h}(x) &= (\operatorname{g} \circ \operatorname{f}) (x)  \\
   &= g(f(x))
\end{align*}
$$

It basically means: First apply the function on the right. Then use that result and apply the left function. Of course, you can use more than two and repeat that process.

In code, you can either explicitly call the first function and then the second or you can create a new function using something like lambda expressions.

**Code**:

This version applies the argument directly.
In languages, that don't support passing functions as parameters, you might need to explicitly write functions for all combinations or use some other mechanism

```js +Compose.js
function compose(f,g,x){
    return g(f(x));
}
```
```js -Demo.js
// define functions and apply
function f(x){
    return 2*x + 1;
}

function g(x){
    return x*x;
}

// compute the function for a few values
const xv = [-2, -1, 0, 1, 2];

console.log("f(x) = 2x + 1");
console.log("g(x) = x*x");

console.log("Apply function:");
xv.forEach(x=> console.log(`f(${x}) = ${f(x)}, h(${x}) = f(g(${x})) = ${compose(f,g,x)}`));
```
<script>
@input(0)
@input(1)
""
</script>
Here we create a new function $\operatorname{h} = (\operatorname{g} \circ \operatorname{f})$ that is the composition of $\operatorname{f}$ and $\operatorname{f}$ and can be called as $\operatorname{h}(x)$.

```js +Compose.js
function compose(f,g){
    return x => g(f(x));
}
```
```js -Demo.js
// define functions and apply
function f(x){
    return 2*x + 1;
}

function g(x){
    return x*x;
}

const h = compose(f,g);

// compute the function for a few values
const xv = [-2, -1, 0, 1, 2];

console.log("f(x) = 2x + 1");
console.log("g(x) = x*x");

console.log("Apply function:");
xv.forEach(x=> console.log(`f(${x}) = ${f(x)}, h(${x}) = f(g(${x})) = ${h(x)}`));
```
<script>
@input(0)
@input(1)
""
</script>


### Set containment:  $\in$, $\ni$, $\not\in$, $\not\ni$

The set element containment symbol $\in$ signals, that something is part of a set. It looks like an **e** for **e**lement.

In a statement like $i \in I$, the left side $i$ is the element that is contained in the set on the right side $I$.

Sometimes it is written flipped ($\ni$). In that case, the order of "element" and "set" is also switched.

If you want to specify that something is **not** in a set, you strike it out, resulting in $\not\in$ and $\not\ni$.

There are multiple uses for this. One example is seen in the section for iteration symbols with set notation. In that case the meaning is "for each element of ..." or "for each element not of ...".

It can also be used as a condition. For example, "If $x$ is an element of $S$, do ... " is "if $x \in S$, do ...".

**Code**:

Depending on your data structure and language, the code might differ. Generally, you will likely find a function corresponding to $\in$ as one of the following:

* contains
* find
* indexOf
* []

Sometimes, there are others or even operators called in. **Attention:** In JavaScript for example, the *in* operator checks, whether an object has a specific key, which is sometimes used to model a set, by just using keys. This might lead to unexpected behaviour when you are working with arrays or actual sets, in which the object values are the set elements, not the keys.

```js +In.js
// Implementation of the in operator for sets made up of object keys:
function inObject(element, objSet){
    return element in objSet;
}
// Implementation of the in operator for sets made up of array elements
function inArray(element, arr){
    return arr.includes(element);
    // or using the indexOf function
    // return arr.indexOf(element) >= 0;
}
// Implementation of the in operator for the Set class
function inSet(element, set){
    return set.has(element);
}
```
```js -Demo.js
// set up the same set in different structures
const arraySet = [1,3,42];
const set = new Set(arraySet);

const objSet = {};
for(const k of arraySet){
    objSet[k] = true;
}

// values to test, one inside and one not

console.log(`Object set: ${JSON.stringify(objSet)}`);
console.log(`Array set: [${arraySet.join(", ")}]`);
console.log(`Set: [${[...set.keys()].join(", ")}]`);


const test = [42,5];

for(const t of test){
    console.log(`Value ${t} in object: ${inObject(t,objSet)}`);
    console.log(`Value ${t} in array: ${inArray(t,arraySet)}`);
    console.log(`Value ${t} in set: ${inSet(t,set)}`);
}
```
<script>
@input(0)
@input(1)
""
</script>

The implementation of $\not\in$ is just a negation of $\in$, so for example the array case above becomes

```js
// Implementation of the not in operator for sets made up of object keys.
// The other variants work the same way
function notIObject(element, objSet){
    return !inObject(element,objSet);
}
``` 

### Kronecker delta $\delta_i^j$,$\delta_{ij}$, $\delta^{ij}$, ...

This Kronecker delta (greek lowercase delta) $\delta_i^j$ is pretty simple: If the two indices i and j are the same, its value is 1, otherwise 0.

Written with cases (see previous section):

$$
    \delta_i^j = \begin{cases}    
        1 & \text{if } i = j \\
        0 & \text{else}
    \end{cases} 
$$

You might find this symbol with various placements of the indices, as indicated in the title. In some context, e.g. tensors, this placement is important, but often times it isn't. 

**Code**:

```js
function kroneckerDelta(i,j){
    if(i == j){
        return 1;
    } else {
        return 0;
    }
}
```

## Operations: $n!$, $\binom{n}{k}$

Here we will look at a common operations with their own notation.

### Factorial: $n!$

The factorial of a number is specified by an exclamation mark. It is the number of different ways you can order n elements.

It is the product of the first n positive natural numbers, so it is an integer.

$$
n! = 1 \cdot 2 \cdot 3 \cdot \dots \cdot n
$$

Using the product symbol as seen in another section we can also write it as:

$$
    n! = \prod\limits_{i=1}^n i
$$

From this we also get $0! = 1$. It is not defined for negative values.

**Attention**: The factorial increases very rapidly. If you use a 32 bit signed integer, then $13!$ will already overflow. Even for a 64 bit signed integer, $21!$ is too much. So if you see a formula involving factorials, try to cancel out terms as much as possible. For example, if $k<n$: $\frac{n!}{k!} = (k+1)(k+2) \cdots (n-1) n$ 


**Code**:

```js +Factorial.js
function factorial(n){
    // you could add a check, if n < 0 and then throw an error here
    // code is the same as if we were to apply the product iteration
    let f = 1;
    // we could also just start with i = 2, since 1 does not change f
    for(let i= 1; i <= n; i++){
        f *= i;
    }

    return f;
}
```
```js -Demo.js
for(let i = 0; i < 10; i++){
    console.log(`${i}! = ${factorial(i)}`);
}
```
<script>
@input(0)
@input(1)
""
</script>

### Binomial coefficient $\binom{n}{k}$

The binomial coefficient comes up in various combinatorics related formulas. It tells you the following: If you have n different objects and you take out k of them (without putting any back), how many different combinations of objects can you pick out (disregarding the order). n and k must both be non-negative integers and $k\leq n$. For $k>n$ $\binom{n}{k} = 0$.

Be careful not to confuse this one with fractions or vectors/matrices. It is just two numbers on top of each other inside brackets.

It is defined the following way:

$$
\binom{n}{k} = \frac{n!}{k!(n-k)!}
$$

You read this as "n over k".

This formula is generally most useful for proofs, less so for computation.
While you could implement it directly using the factorial formula, this will quickly break due to the extreme growth of the factorial, which will overflow integer variables quickly. Using floats might be tempting, but they will become imprecise pretty quickly and give you wrong results.

We won't go too far into the derivation here, but we can observe that $n!$ and $(n-k)!$ share the first $n-k$ product terms and can thus be cancelled out! This leaves

$$
    \frac{n(n-1)(n-2)\cdots(n-k +1)}{k(k-1)(k-2)\cdots 1}
$$

One can write that as a product (see the product symbol section)

$$
\binom{n}{k} = \prod\limits_{i=1}^k \frac{n+1 -i}{i}
$$

See for example [Wikipedia](https://en.wikipedia.org/wiki/Binomial_coefficient#Multiplicative_formula)

Furthermore, there is a symmetry property that $\binom{n}{k} = \binom{n}{n-k}$. So we can choose the lesser of $k$ and $n-k$ to reduce the work we have to do.

This allows for a pretty simple computation. If a binomial coefficient function exists in your language, it is good to use it, as they have probably/hopefully taken care of range issues like this.

**Code**:

```js +Binomial.js
function binom(n,k){
    if(k > n){
        // there are 0 ways to choose more items than there are
        return 0;
    }
    let p = 1.0;
    if(k > n-k){
        // switch n with n-k if the latter is smaller
        k = n-k;
    }
    // apply the product formula
    for(let i = 1; i <= k; i++){
        // multiply first to avoid non-integer division
        // javascript doesn't strictly have integers, but in general this might be useful
        p *= n + 1 - i;
        p /= i;
    }
    return p;
}
```
```js -Demo.js
const n = 10;
console.log(`Display all binomial coefficients up to n = ${n}`);
console.log("Each line i will show the binomial coefficients i over k");

for(let i = 0; i <= n; i++){
    const b = [];
    for(let k = 0; k <= i; k++){
        b.push(binom(i,k));
    }
    console.log(`${i}: ${b.join(" ")}`);
}
```
<script>
@input(0)
@input(1)
""
</script>

## Differentiation: $\operatorname{f}'(x)$, $\frac{d \operatorname{f}}{d x}$, $\frac{\partial \operatorname{f}}{\partial x}$, $\nabla \operatorname{f}$.

Before you look at any of these, please keep the following in mind: There are various  fields that deal with the computation of the following objects. If you need to really compute any values involving these symbols that go beyond the very basics, it is best to consult some more advanced literature. Here are a few terms to get you started:

* Numerical differentiation
* Symbolic differentiation
* Automatic differentiation

The code snippets in here are just very basic versions of numerical differentiation.

### Derivative $\operatorname{f}'(x)$, $\frac{d \operatorname{f}}{d x}$

The derivative measures how much a value changes with respect to another. For a function $\operatorname{f}$, we can ask how much the value $\operatorname{f}(x)$ changes, if we change $x$ a tiny amount.

The notations $\operatorname{f}'(x)$ ("f prime of x") and $\frac{d \operatorname{f}}{d x}$ ("df [over] dx") mean the same thing. The first one is often used in basic 1D calculus, the second is probably seen more often higher dimensional variants and physics. The "d" is a "differential" quantity, more or less meaning "something very very small".

There are a few different notations, but mainly we have:

$$
\frac{d \operatorname{f}}{d x} = \lim\limits_{h \rightarrow 0} \frac{\operatorname{f}(x + h) - \operatorname{f}(x)}{h}
$$

The $\lim$ is a so called limit and $h\rightarrow 0$ means "let h approach 0". When using a computer, we can't do that (we can with symbolic and automatic differentiation, but not with the simple method) actually compute this. Instead, we approximate this by using a small value of $h$. How do we choose the value of h? Well, that can be more complicated, so usually you will see some slightly hacky fixed value, that is "very" small, like $10^{-7}$. As stated before, this is a very simple code that works, but could be improved in many ways, if needed.

So in math notation, that is called the "difference quotient":

$$
\frac{d \operatorname{f}}{d x} \approx \frac{\operatorname{f}(x + h) - \operatorname{f}(x)}{h}
$$

**Code**:

```js +Derivative.js
function derivative(f, x, h=1E-7){
    return (f(x + h) - f(x)) / h;
}
```
```js -Demo.js
let x = 10;
console.log(`Compute the derivative of f(x) = 10x at x = ${x} (the numerical derivative is exact for linear and constant functions, aside from numerical precision)`);

const fLinear = x => 10*x;
// actual derivative: f'(x) = 10
const dfLinearDxCorrect = x => 10;
console.log(`Correct derivative: ${dfLinearDxCorrect(x)}`);
console.log(`Estimated derivative: ${derivative(fLinear,x)}`);

x = 5;
console.log(`Compute the derivative of f(x) = sin(x) at x = ${x}`);

// actual derivative: f'(x) = cos(x)
const dSinDxCorrect = Math.cos;
console.log(`Correct derivative: ${dSinDxCorrect(x)}`);
console.log(`Estimated derivative: ${derivative(Math.sin,x)}`);
```
<script>
@input(0)
@input(1)
""
</script>

### Partial derivatives $\frac{\partial \operatorname{f}}{\partial x}$, $\frac{\partial \operatorname{f}}{\partial y}$, ...

This is very similar to the normal derivative. Often, functions do not only depend on one variable, but on many, think "what is the temperature at a location (x,y) on the map". A partial derivative tells you how much a function value at a position changes if you change one of the variables a tiny bit.

The squiggly $\partial$ symbols can be read a few ways. Sometimes, people just say "d f d x" or "partial of f with respect to x" or "del f [by] del x". There are a few ways to say it...

Here we will show the definitions for the 2D case.

$$
\begin{align*}
    \frac{\partial \operatorname{f}(x,y)}{\partial x} &= \lim\limits_{h \rightarrow 0} \frac{\operatorname{f}(x + h, y) - \operatorname{f}(x,y)}{h} \\
    \frac{\partial \operatorname{f}(x,y)}{\partial y} &= \lim\limits_{h \rightarrow 0} \frac{\operatorname{f}(x, y + h) - \operatorname{f}(x,y)}{h}
\end{align*}
$$

For the general case, it is easier to number the coordinates, like $x_1, \dots, x_n$. Then we can write:

$$
 \frac{\partial \operatorname{f}(x_1, \dots, x_i, \dots, x_n )}{\partial x_i} = \lim\limits_{h \rightarrow 0} \frac{\operatorname{f}(x_1, \dots, x_i + h, \dots, x_n ) - \operatorname{f}(x_1, \dots, x_i, \dots, x_n )}{h} \\
$$

The only difference (in this simple case) to the 1D derivative is, that a function depends on multiple values and you need to change just one of them. Depending on how you represent these, the implementation changes slightly. The easiest would be to represent the variables as a vector. Then you can specify the variable that you want to differentiate with respect of by its index.

**Code**:

```js +PartialDerivative.js
function partialDerivative(f, x, index, h=1E-7){
    // calculate f(x)
    const fx = f(x);
    // cache the current value in the vector
    const currentXi = x[index];
    // calculate f(x_1, ..., x_i + h, ... x_n)
    x[index] += h;
    const fxh = f(x);
    // restore old x, this might not be needed depending on the situation
    x[index] = currentXi;
    return (fxh - fx) / h;
}
```
```js -Demo.js
// the point where to evaluate
let x = [2,4];
// the function to compute
// in math, indices usually start at 1, but in code mostly at 0
// here we number the variables starting at 0
// x -> x[0], y -> x[1]
let f = x => x[0] + 2*x[1] + x[0]*x[1];
let dfdx0 = x => 1 + x[1];
let dfdx1 = x => 2 + x[0];

console.log(`Compute the derivative of f(x,y) = x + 2y + x*y at x = (${x.join(", ")})`);
console.log(`Correct df/dx: ${dfdx0(x)}`);
console.log(`Estimated df/dx: ${partialDerivative(f,x, 0)}`);
console.log(`Correct df/dy: ${dfdx1(x)}`);
console.log(`Estimated df/dy: ${partialDerivative(f,x, 1)}`);
```
<script>
@input(0)
@input(1)
""
</script>

### Gradient $\nabla \operatorname{f}$

The gradient is an extension of the last section. The gradient of a function is a vector containing all its partial derivatives as its components. 
In the following, we will write all components that the function depends on in a vector $\mathbf{x} = \begin{pmatrix}
    x_1 \\ \vdots \\ x_n
\end{pmatrix}$.

$$
\nabla \operatorname{f}(\mathbf{x}) = \begin{pmatrix}
    \frac{\partial \operatorname{f}(\mathbf{x})}{\partial x_1} \\
    \vdots \\
    \frac{\partial \operatorname{f}(\mathbf{x})}{\partial x_n} \\
\end{pmatrix}
$$

The implementation is basically the same as in the previous section, just with a loop.

**Code**:

```js +Gradient.js
function gradient(f, x, h=1E-7){
    // create the gradient vector with the length of x
    const n = x.length;
    const g = Array(n);

    // in our simple version, we only need to compute f(x) once
    const fx = f(x);
    // generate the values the same way as in the previous section
    for(let i = 0; i < n; i++){
    // cache the current value in the vector
        const currentXi = x[i];
        // calculate f(x_1, ..., x_i + h, ... x_n)
        x[i] += h;
        const fxh = f(x);
        // restore old x
        x[i] = currentXi;
        // set gradient value
        g[i] = (fxh - fx) / h;
    }
    return g;
}
```
```js -Demo.js
// the point where to evaluate
let x = [2,4];
// the function to compute
// in math, indices usually start at 1, but in code mostly at 0
// here we number the variables starting at 0
// x -> x[0], y -> x[1]
let f = x => x[0] + 2*x[1] + x[0]*x[1];
let dfdx0 = x => 1 + x[1];
let dfdx1 = x => 2 + x[0];
// the gradient is the vector of partial derivatives
const gradF = x => [dfdx0(x), dfdx1(x)];

console.log(`Compute the gradient of f(x,y) = x + 2y + x*y at x = (${x.join(", ")})`);
console.log(`Correct gradient: (${gradF(x).join(", ")})`);
console.log(`Estimated gradient: (${gradient(f,x).join(", ")})`);
```
<script>
@input(0)
@input(1)
""
</script>
