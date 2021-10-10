---
title: "The Debounce Function - a Savior"
date: 2021-10-09T20:33:33-04:00
draft: false
---

In a project I'm working on, part of our decision-making logic uses data from a PLC (Programmable Logic Controller). Without getting very technical, the PLC communication interface we use ([Node OPCUA](https://github.com/node-opcua/node-opcua)) asks the PLC to monitor certain tags, i.e., we get notified anytime the value changes.

## The Problem

Now, one piece of information that we use in the application is a 17-character long string - each character has a separate tag, so anytime the string gets updated, 17 or more `change` events may be triggered.

### The Code

`monitoredItemGroup` is one set of 17 character strings. There are 150 more `monitoredItemGroup` variables since this is a snippet from a `for` loop. The logic is as follows:
- Watch the `monitoredItemGroup`
- When something changes, read another PLC tag 
- Use the result to run some more logic (some may involve more PLC tags)
```js
    // function that reads a PLC tag
    const findValue = (station, session) => {
      session.read(
        { nodeId: `${station}.PLCTag`, attributeId: AttributeIds.Value },
        (err, dataValue) => {
          if (!err) {
            const tagValue = dataValue.value.value.toString();

            anotherFunction(session, station, tagValue, socket);
          }
        }
      );
    };
    
    // watches for change event
    monitoredItemGroup.on('changed', (monitoredItem, dataValue, index) => {
        // some logic
        // ...
        findValue(station, session)
    })
```

This is _clearly_ not ideal **at all**. Why, you ask? 

Well, anytime the 17 character string changes, we have a hook that runs some logic on our backend. If the string changes over 17 times (one `change` event per character), we'd trigger the hook way more than 17+ times - and this ain't good at all.

So, I needed a better way to do this - what if we could "wait" for the `change` events to finish occurring and _then_ initiate the hook? That's exactly where a debounce function can help us.

## The Solution

If you want the code and that's all you're here for - sure, here you go:

```js
// Source - https://underscorejs.org/#debounce
function debounce(func, wait, immediate) {
    var timeout;
    return function () {
        var context = this, args = arguments;
        var later = function () {
            timeout = null;
            if (!immediate) func.apply(context, args);
        };
        var callNow = immediate && !timeout;
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
        if (callNow) func.apply(context, args);
    };
};
```

Here's a link to the function as a [GitHub Gist](https://gist.github.com/nandanv2702/ec923ff4c8b87b19197ea129538e2117) if you'd like to share it.

### What It Does

The debounce function takes in 3 arguments:
- A function `func`
- An integer `wait` that waits for `wait` milliseconds before executing `func`
- A boolean `immediate`:
    - Set to `true` if you want to execute `wait` milliseconds after the __first__ call to `func`
    - Set to `false` if you want to execute `wait` milliseconds after the __last__ call to `func`

### How to Implement It

After defining the `debounce` function globally, I 'wrapped' the `findValue` function in the `debounce` function:

```js
const findValue = debounce((station, session) => {
      session.read(
        { nodeId: `${station}.PLCTag`, attributeId: AttributeIds.Value },
        (err, dataValue) => {
          if (!err) {
            const tagValue = dataValue.value.value.toString();

            anotherFunction(session, station, tagValue, socket);
          }
        }
      );
    }, 250);

    // watch `monitoredItemGroup` code
```

This ensures that, regardless of how many times `findValue` is called, it runs only the last time it's called, i.e., if it's not called again within the 250ms. As I didn't explicitly set `immediate`, it's considered a [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) value, i.e., it's `undefined`, thus `false` (very simplified, but read the MDN docs to learn more).

## Possible Use Cases

Aside from the situation I encountered, the `debounce` function can also prevent:
- Submitting a payment twice (stonks)
- Hitting an API rate limit (when your users wanna mess with you)
- Expensive DOM events - e.g. update something anytime a user scrolls
- Excessively auto-saving / autocompleting something (speed is good, but practicality is prolly better)