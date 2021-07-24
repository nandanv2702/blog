---
title: "How to Use Promise.All() in Javascript"
date: 2021-07-21T19:55:41-04:00
draft: false
---

The File System (`fs`) module in NodeJS can come quite handy, but did you know that the `fs` module can return Promises? This can make things much more logical when it comes to understanding asynchronous programming with reference to the local file system.

## Here's The Problem

As part of my project to create a real-time inventory dashboard at work, I needed to seed a SQL database hosted on Azure with data from a bunch of text files. If there were just a couple of files, an easy way to do this would just be to copy paste all file content into one file and then read that file into the database. However, there are well over 50 files - and let's be honest, who has the brainpower to open and close all of these _manually_.

So in order to successfully complete this task, my first thought was to create this "simple" workflow:
```js
// reads all files present in the test files directory
const files = fs.readdir("./Documents/Test AOS Files", (err, files) => {
        if(!err){
            // console.log(files)
            return files
        } else {
            console.log("fs read err")
        }
    });

// read the contents of each file, append it to an array, then use the array to bulk insert the data into the SQL database
files.map(file => {

    // array that holds all data
    let rawData = [];

    // open file, read contents into object, insert into array
    fs.readFile(file, 'utf8' , (err, data) => {
        if (err) {
            console.error(err)
            return null
        }

        dataArray = data.toString().split(/\r?\n/);

        dataArray.forEach(data => {
            rawData.push(data.split(","))
        });
    return rawData
    });
})
.then((rawData) => {
    // bulk insert it using Sequelize
    db.AOS.bulkCreate(rawData).then((res) => console.log(`res is: \n ${res}`))
});

```

It's a mouthful, yeah, but to anyone who's ever coded anything _ever_ - if something looks like it's gonna work the first time, it's not.

## Storytime

In order to better understand this problem, I like to frame it as a story:

Think of yourself as a proctor at an exam. As soon as time's up, you ___expect___ papers on your desk. Realistically, you know that some students will turn in their papers after others do, so you decide to wait for all the papers to arrive; you really hate walking so this makes it easier for you to take all the completed papers to your office rather than walk to your office as each one comes in (which is inefficient anyway).

If you understand how this works, you pretty much understand how `Promise.all()` works.

In this case, `Promise.all()` is your professor, and your class' completed exam papers are the files - mind you, each "paper" might be submitted a couple of seconds later to the "professor."

## The Asynchronous Nature of Javascript

What I didn't quite understand about the `fs` module earlier was how I could use [Promise-based operations](https://nodejs.org/api/fs.html#fs_promise_example) rather than the [callback form](https://nodejs.org/api/fs.html#fs_callback_example). For those of you that are confused (like me) about what exactly a `Promise` is and what it does, here's a one-line summary of it:

A `Promise` basically promises you an output - whether the task you're doing succeeds or fails, you will get an output _for sure_.

## The Solution

Because I had to push data to the database __only once I had read all the files__, I decided to import the fs module with the Promises API
```js
const fs = require('fs').promises;
```

This ensures that I am able to perform operations on the result only once an asynchronous operation is complete. Referring to the problem of reading multiple files once again, let's break down the problem into actionable steps:

1. Get a list of files in the `Test AOS Files` directory
2. Read the raw data from each file (_note: raw data refers to the .csv-like format of the file_) and add it to an array
3. Format the data to adhere to the model required in the database; then, push it to a "final data" array
4. Once all the information is available, use `bulkCreate` from `Sequelize` to bulk insert the data in the "final data" array

### Step 1: Get a List of Files

This is as easy as 

```js
const files = fs.readdir("./Documents/Test AOS Files")
```

Note that because `const fs` refers to the `promises` API from the `fs` module, we can chain `.then()` to `files`.

As for the professor, think of this as a manifest of all the papers they're supposed to collect.

### Step 2: Read the Raw Data
```js
files
    .then(files => {

        const raw_data_list = []

        files.map(file => {

            // gets the raw data for each file and adds it to the raw data array
            raw_data_list.push(getData(file));

        })
        
        return raw_data_list;
    })
    .then(res => {
        // do other stuff
    })
    .catch(err => console.log(err))
```

By defining the `getData` function as shown below, we know that every element in `raw_data_list` is a Promise

```js
// reads a file and returns its content
function getData(fileName) {
    return fs.readFile("./Documents/Test AOS Files/" + fileName, 'utf8');
};
```

### Step 3: Format the Data
Once all the Promises are resolved, i.e., once all the papers are submitted, we can format the papers to be the way we like:

```js
files
    .then(files => // step 2 code)
    .then(rawData => {

        const final_data_list = [];

        // this will run only when all the papers are submitted
        Promise.all(rawData).then(data => {

            // format the data by removing newline and return characters
            // each array contains contents of one file
            formattedData = data.map(res => res.split(/\r?\n/));

            formattedData.forEach(dataArray => {
                dataArray.forEach(data => {

                        // format the data as required for the database (returns an object)

                        // add the formatted row as a Promise to use for the next step

                        final_data_list.push(new Promise((resolve, reject) => resolve(row)));
                    };
                });
            });

        return final_data_list;
    })
    .then(res => {
        // step 4
    })
```

### Step 4: bulkCreate Data
Since we know that `final_data_list` is a list of Promises that will all resolve to formatted rows to work with, we can chain another `.then()` block to the code above in order to do that.

```js
// above code
    .then(res => {
        Promise.all(res)
            .then(data => {
                        // add to DB
                        db.AOS.bulkCreate(data)
                            .then((res) => console.log(`res is: \n ${res}`));
                });
    });
```

After implementing all this code, it FINALLY worked. I shared it with a co-worker and YAY he was ecstatic too. I then jumped around for 2 mins and got back to work cuz my manager walked in.

## What I Learned

Phew, now that we're done with that huge chunk of code, let's take a little coffee break.

{{< image src="coffee.jpg" alt="Coffee" position="center" style="border-radius: 8px;" >}}

Although all of this can seem quite cumbersome, it was important for me to understand how exactly the fs module worked. As any developer would, I console.logged the hell of out my inital buggy code until I got a basic idea of why it wasn't working. After basically getting a headache trying to figure out this simple task, I learned 3 things:
1. Promises are bæ

    seriously, it's amazing how useful these are especially considering how they don't block any other code (that's a whole other post in itself)

2. Sometimes it's good to take a step back:

    This took me two days to complete (like 4 hours as it's part-time responsibility at work). When I didn't understand it the first day, I really wanted to stay back and solve it; however, the more I tried, the farther I got from the solution. So, I went back home, watched Suits (which is amazing by the way), came back the next day and _voila_ - I typed and stuff just magically happened to work.

3. Add validation to your database model

    I ended up inserting 2000 null rows and 2000 more rows with "N/A" into the DB - yeah, it's not as bad as deleting a table from the production DB but still. Should've prolly done this earlier.

