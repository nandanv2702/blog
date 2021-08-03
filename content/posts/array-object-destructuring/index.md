---
title: "Array & Object Destructuring"
date: 2021-07-30T21:01:35-04:00
draft: false
---

This past week, I was tasked with creating an end-of-shift report summary report for all shifts throughout the year. To give you a little background, the company I work at records all automated production cart issues in an Excel file for archival purposes, and a co-worker and I thought it'd be useful to create a tool that helps others analyze which production stations have the most issues. 

## A Brief History of My Pain

Doesn't sound like a really hard task, and to be honest it wasn't quite that hard! After all, I'd made a shell script in Python earlier that did the same thing, except now I just had to redo it in JavaScript (yAy) and port it over to the web. I won't bore you with the specifics about this project, but throughout this task I was using a lot of Objects to store worksheet information. 

> Sidenote: if you work a lot in Excel and know JavaScript, check out [SheetJS](https://sheetjs.org). It's an extremely helpful Open Source tool to creatively analyze Excel files. Works 10 times faster than OpenPyXL (which is still amazing, by the way).

After creating the program, I realized that there was a much simpler way to deconstruct / append an old array to a new one - and that encouraged me to learn more about array and object destructuring in JavaScript.

## Some Common Sightings

The most commonly observed use-case for destructuring is when importing / requiring modules:

```js
import { createApp } from 'vue';

const { expect } = require('chai');
```

But of course everything in programming isn't as straightforward as it looks, right?!

{{<image src="./itcrowd.gif" style="width:100%; border-radius: 6px;" alt="A fitting IT Crowd meme">}}

So let's start with a _simple_ example to demonstrate this concept. Here's a standard response one might get from a random person API:

```js
const Person = {
    id: 15220,
    firstName: 'John',
    lastName: 'Doe',
    email: 'john@johndoe.com',
    parents: {
        mother: {
            id: 120120,
            url: 'http://randomperson.com/api/v1/persons/120120'
        },
        father: {
            id: 111233,
            url: 'http://randomperson.com/api/v1/persons/111233'
        }
    },
    siblings: [
        {
            id: 121121,
            url: 'http://randomperson.com/api/v1/persons/121121'
        }
    ]
};
```

## Let's Break It Down

If you wanted to get each of the values of this object, e.g., you wanted to extract the `firstName`, `lastName`, `email`, etc., the "regular" way of doing it is to assign each of those values to a variable as follows:

```js
// the "regular" way
const firstName = Person['firstName'];
const lastName = Person['lastName'];
const email = Person['email'];
```

Turns out there's a simpler way of doing this - I'll show you an example of how to get the same information using different syntax and then explain to you what's happening in a more visual way.

### Object Destructuring

```js
// using Object Destructuring
const { firstName, lastName, email } = Person;

console.log(firstName); // expect 'John'
console.log(lastName); // expect 'Doe'
console.log(email); // expect 'john@johndoe.com'
```

Think of the `{ }` syntax as an automatic way to grab whatever's in the "main" object and assign it to a `const` with the variable name/(s) you pass. The method shown above ensures accurate results since the variable names `firstName`,`lastName`, and `email` are keys in the Person object. This allows you to directly "grab" that value from the object. 

However, what if you wanted to assign those values to a custom variable name? Here's how you can do that:

```js
const { firstName: name1, lastName: name2, email: emailID } = Person

console.log(name1); // expect 'John'
console.log(name2); // expect 'Doe'
console.log(emailID); // expect 'john@johndoe.com'
```

Using the aforementioned syntax basically maps the value of `Person` with the key `firstName` into a new variable called `name1`. 

It's syntactically the same as saying 

```js
const name1 = Person[firstName]
```

But what if a certain key doesn't exist, e.g., what if a person doesn't have any siblings? 

{{<image src="./itcrowd2.gif" style="width:100%; border-radius: 6px;" alt="Frustration">}}

In this case, assuming that the API doesn't return an object with the key `siblings`, we can still assign a default "value" to the `siblings` object as a placeholder:

```js
const { siblings = "N/A"}; 
```

If the Person doesn't have a sibling, this value will be "N/A," however, if they do, it'll be an array of objects containing their siblings' information


If, for some reason, you knew that a Person with `ID = 123456` only has brothers, you can rename `siblings` to `brothers` to better understand your data (if it helps you), and also pass in a default value in case Person `123423` doesn't have brothers:

```js
const { siblings: brothers = "N/A" }; 
```

### Array Destructuring

Since almost everything in JavaScript is an object, concepts from object destructuring can also be extended to arrays - the only difference is the `{ }` are replaced with `[ ]`. 

For arrays, you can grab specific elements and assign them custom variable names / default values

```js
const food = ['fruits', 'vegetables', 'veganism', 'not veganism', 'mcdonalds'];

let [ helth, , no, , definitely , option2 = "dominos"] = food

console.log(helth) // expect 'fruits'
console.log(no) // expect 'veganism'
console.log(option2) // expect 'dominos'

```

## Conclusion and Other Resources

If you're curious on how far we can take destructuring, just know that you can take these same concepts and apply them to highly nested objects, assigning each one a default value in the case that it doesn't exist; however, in my opinion, extended use of such syntax could possibly complicate things and the code might not be easily readable to others.

Here are a couple of resources I found helpful in understanding array / object destructuring:
- ['A Casual Explanation of Destructuring Objects and Arrays in JavaScript' - Coding Garden](https://www.youtube.com/watch?v=qyDIkCeOHFo)
- [MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- ['Why Is Array/Object Destructuring So Useful And How To Use It' - Web Dev Simplified](https://www.youtube.com/watch?v=NIq3qLaHCIs)




