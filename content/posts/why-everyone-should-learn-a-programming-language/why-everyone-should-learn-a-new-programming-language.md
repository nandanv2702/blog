---
title: "Why Everyone Should Learn a New Programming Language"
date: 2021-08-08T19:28:59-04:00
draft: true
---

## TODO

1. [ ] Why you should learn a programming language
    - [ ] How I started working at a new place and got my first internship / co-op

    - [ ] The importance of patterns in our every day lives

    - [ ] How every job has certain things that are repetitive

    - [ ] How programming languages like Python can make things easier for you: include Python code for lecture video downloader from class + for video file creator

    - [ ] Simple use cases like this can help automate processes in the industry

I've always tried to find the laziest way to do things. Sometimes this makes me come up with creative solutions out of the blue (also known as [jugaad](https://en.wikipedia.org/wiki/Jugaad#:~:text=Jugaad%20(%22Jugaar%22)%20is%20a%20colloquial%20Hindi%20(Devanagari%3A%20%E0%A4%9C%E0%A5%81%E0%A4%97%E0%A4%BE%E0%A4%A1%E0%A4%BC%20(%E0%A4%9C%E0%A5%81%E0%A4%97%E0%A4%BE%E0%A4%A1))%2C%20Bengali%20(%E0%A6%9C%E0%A7%8B%E0%A6%97%E0%A6%BE%E0%A6%A1%E0%A6%BC)%2C%20Marathi%20%E0%A4%9C%E0%A5%81%E0%A4%97%E0%A4%BE%E0%A4%A1%2C%20Punjabi%2C%20Sindhi%20and%20Urdu%20(%D8%AC%DA%AF%D8%A7%DA%91)%20word%2C%20which%20refers%20to%20a%20non-conventional%2C%20frugal%20innovation%2C%20often%20termed%20a%20%22hack%22.%5B)), and other times I just waste 3 whole days doing absolutely nothing :shrug:.

It was a long-standing thought I had (i.e. a thought I had before getting a job) that jugaad can't be used in the industry; I thought things worked a certain way and that there were processes that are low-key set in stone that I (as a co-op) can't make a significant (or even a small) change to.

To my surprise, that totally wasn't the case! This idea of applying one concept to another seemingly unrelated field yielded amazing results.

## The Story :notebook:

I recently got signed on as a manufacturing engineering co-op, and as part of my responsibilties I had to compile a bunch of Standard Work sheets so that we can analyze the steps in the them to determine how we can reorganize the production line. Most of the work (as is done in every other company) was on Excel and I particularly remember telling myself ages ago that opening 1000 Excel sheets is not something I see myself doing. BUT how else could I go about this? :confused:

I needed to come up with the easiest way to get this task done, so naturally I took a break and had some coffee :coffee:

### Step 1: Analyzing the Problem

For those of you that don't know, Standard Work can be defined as

> A detailed definition of the current best practices for performing a process

Since all the files were spread across multiple folders and sub-folders, I extracted all the Excel sheets into one folder with the intention of randomly opening some of them to determine the similarities. As I further investigated this problem, I realised that most of the sheets followed the same pattern of listing the Standard Work steps.

### Step 2: Brainstorming a Solution

Once I had identified this repeating patterns of certain steps being in certain locations, I wanted to know if all of the sheets followed the same pattern. My goal was to come up with a starting point / data attribute to look for that is (i) easy enough to prototype in a couple of minutes, and (ii) comprehensive enough to give me a good understanding of the data.

To give you a better understanding of the situation without giving away too much about the document, here's the header that I was looking for

|   Header 1	|       Header 2   | Header 3 |   
|---	|---------------	|---	|
|   N/A	| ***Standard Work DOC_CODE*** |   N/A	|
|   N/A	|   N/A            	|   N/A	|

After a little thought, I decided to create a Jupyter notebook and install this tool called [OpenPyXL](https://openpyxl.readthedocs.io/en/stable/) to help me analyze the Excel files. I've worked with the [Pandas](https://pandas.pydata.org/) library before, however, I chose to use OpenPyXL as I knew I didn't have to create any visualizations / heavily analyze the data we have.

### Step 3: Writing the Pseudocode

Here's an overview of what I had to do to understand the data:

- Use the OS module to list all files in the directory
- Create an empty dictionary that will be used to log the occurrences of the phrase 'Standard Work' for each cell 
- For each Excel workbook:
    - Get a list of sheet names and open any sheet that has 'Standard Work' in its title, ensuring that a second sheet that also contains this string isn't ignored
            sometimes there are two Standard Work sheets for one process / station
    - Iterate through all the cells in the first, second, and third column to find a cell that says 'Standard Work.'
    - Increment the value of the cell in the dictionary by one (if it exists), or create the key in the dictionary and set its value to 1
- Sort the dictionary by value in descending order

Of course, I inserted a bunch of `print` statements the first time around and conducted a trial run with 5 sheets just as a proof of concept. Once I had this going, I noticed that there were 16 unique cells that had the phrase 'Standard Work' in them, so my original idea of manually setting a cell to read data after would not work. To be honest, it wasn't a fool-proof way of going through the files itself.

### Step 4: Brainstorming the Final Version

#### TODO
- read all data in the row to the left of the standard work one as all rows that have steps are merged
- if the step exists, create a dictionary for all models of vehicles that exist in the sheet and iterate through the steps to find the total time taken for each model along with the respective step
- calculate the model that takes the longest time and return the top 3 steps that take the most time, with the model that takes the second highest time

A reason I decided to use Python and not JavaScript in this case is because it's much easier to sort, filter, and work with dictionaries in Python than it is in JavaScript, especially since it involves a lot of reorganization of data.