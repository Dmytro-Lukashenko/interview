# Interview Questions and Answers

### Table of Contents

<!-- TOC_START -->
| No. | Questions |
| --- | --------- |
| 1 | [Memory management In Javascript](#memory-management-in-javascript) |
| 2 | [Garbage Collection](#garbage-collection) |
<!-- TOC_END -->

<!-- QUESTIONS_START -->
### Memory management In Javascript
JavaScript automatically allocates memory when objects are created and frees it when they are not used anymore (garbage collection).

#### Memory life cycle
- Allocate the memory you need
- Use the allocated memory (read, write)
- Release the allocated memory when it is not needed anymore

#### Allocation memory in JS
##### Value initialization
In order to not bother the programmer with allocations, JavaScript will automatically allocate memory when values are initially declared.
```javascript
const n = 123; // allocates memory for a number
const s = "string"; // allocates memory for a string

const o = {
  a: 1,
  b: null,
}; // allocates memory for an object and contained values

// (like object) allocates memory for the array and
// contained values
const a = [1, null, "str2"];

function f(a) {
  return a + 2;
} // allocates a function (which is a callable object)

// function expressions also allocate an object
someElement.addEventListener(
  "click",
  () => {
    someElement.style.backgroundColor = "blue";
  },
  false,
);
```

##### Allocation via function calls

Some function calls result in object allocation.
--javascript
const d = new Date(); // allocates a Date object
const e = document.createElement("div"); // allocates a DOM element
```
Some methods allocate new values or objects:
--javascript
const s = "string";
const s2 = s.substr(0, 3); // s2 is a new string
// Since strings are immutable values,
// JavaScript may decide to not allocate memory,
// but just store the [0, 3] range.

const a = ["yeah yeah", "no no"];
const a2 = ["generation", "no no"];
const a3 = a.concat(a2);
// new array with 4 elements being
// the concatenation of a and a2 elements.
```
##### Using values
Using values basically means reading and writing in allocated memory. This can be done by reading or writing the value of a variable or an object property or even passing an argument to a function.

##### Release when the memory is not needed anymore
The majority of memory management issues occur at this phase. The most difficult aspect of this stage is determining when the allocated memory is no longer needed.

Low-level languages require the developer to manually determine at which point in the program the allocated memory is no longer needed and to release it.

Some high-level languages, such as JavaScript, utilize a form of automatic memory management known as garbage collection (GC). 
The purpose of a garbage collector is to monitor memory allocation and determine when a block of allocated memory is no longer needed and reclaim it. 
This automatic process is an approximation since the general problem of determining whether or not a specific piece of memory is still needed is undecidable.

### Garbage Collection
#### References
The main concept that garbage collection algorithms rely on is the concept of reference. 
Within the context of memory management, an object is said to reference another object if the former has access to the latter (either implicitly or explicitly).
For instance, a JavaScript object has a reference to its prototype (implicit reference) and to its properties values (explicit reference).

In this context, the notion of an "object" is extended to something broader than regular JavaScript objects and also contain function scopes (or the global lexical scope).

#### Reference-counting garbage collection
This is the most naïve garbage collection algorithm. This algorithm reduces the problem from determining whether or not an object is still needed to determining if an object still has any other objects referencing it. 
An object is said to be "garbage", or collectible if there are zero references pointing to it.

There is a limitation when it comes to circular references. In the following example, two objects are created with properties that reference one another, thus creating a cycle. They will go out of scope after the function call has completed. 
At that point they become unneeded and their allocated memory should be reclaimed. 
However, the reference-counting algorithm will not consider them reclaimable since each of the two objects has at least one reference pointing to them, resulting in neither of them being marked for garbage collection. 
Circular references are a common cause of memory leaks.
```javascript
function f() {
  const x = {};
  const y = {};
  x.a = y; // x references y
  y.a = x; // y references x

  return "azerty";
}

f();
```

#### Mark-and-sweep algorithm
This algorithm reduces the definition of "an object is no longer needed" to "an object is unreachable".

This algorithm assumes the knowledge of a set of objects called roots. 
In JavaScript, the root is the global object. Periodically, the garbage collector will start from these roots, find all objects that are referenced from these roots, then all objects referenced from these, etc. 
Starting from the roots, the garbage collector will thus find all reachable objects and collect all non-reachable objects.

This algorithm is an improvement over the previous one since an object having zero references is effectively unreachable. 
The opposite does not hold true as we have seen with circular references.

Currently, all modern engines ship a mark-and-sweep garbage collector. 
All improvements made in the field of JavaScript garbage collection (generational/incremental/concurrent/parallel garbage collection) over the last few years are implementation improvements of this algorithm, 
but not improvements over the garbage collection algorithm itself nor its reduction of the definition of when "an object is no longer needed".

The immediate benefit of this approach is that cycles are no longer a problem. 
In the first example above, after the function call returns, the two objects are no longer referenced by any resource that is reachable from the global object. 
Consequently, they will be found unreachable by the garbage collector and have their allocated memory reclaimed.

However, the inability to manually control garbage collection remains. 
There are times when it would be convenient to manually decide when and what memory is released. 
In order to release the memory of an object, it needs to be made explicitly unreachable. 
It is also not possible to programmatically trigger garbage collection in JavaScript — and will likely never be within the core language, although engines may expose APIs behind opt-in flags.
<!-- QUESTIONS_END -->

