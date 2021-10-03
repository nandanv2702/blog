---
title: "Why Everyone Should Learn a Programming Language"
date: 2021-08-08T19:28:59-04:00
draft: false
---

I've always tried to find the laziest way to do things. Sometimes this makes me come up with creative solutions out of the blue (also known as [jugaad](https://en.wikipedia.org/wiki/Jugaad#:~:text=Jugaad%20(%22Jugaar%22)%20is%20a%20colloquial%20Hindi%20(Devanagari%3A%20%E0%A4%9C%E0%A5%81%E0%A4%97%E0%A4%BE%E0%A4%A1%E0%A4%BC%20(%E0%A4%9C%E0%A5%81%E0%A4%97%E0%A4%BE%E0%A4%A1))%2C%20Bengali%20(%E0%A6%9C%E0%A7%8B%E0%A6%97%E0%A6%BE%E0%A6%A1%E0%A6%BC)%2C%20Marathi%20%E0%A4%9C%E0%A5%81%E0%A4%97%E0%A4%BE%E0%A4%A1%2C%20Punjabi%2C%20Sindhi%20and%20Urdu%20(%D8%AC%DA%AF%D8%A7%DA%91)%20word%2C%20which%20refers%20to%20a%20non-conventional%2C%20frugal%20innovation%2C%20often%20termed%20a%20%22hack%22.%5B)), and other times I just waste 3 whole days doing absolutely nothing :shrug:.

It was a long-standing thought I had (i.e. a thought I had before getting a job) that jugaad can't be used in the industry; I thought things worked a certain way and that there were processes that are low-key set in stone that I (as a co-op) can't make a significant (or even a small) change to.

To my surprise, that totally wasn't the case! This idea of applying one concept to another seemingly unrelated field yielded amazing results.

## The Story :notebook:

I recently got signed on as an engineering co-op, and as part of my responsibilties I had to compile a bunch of Standard Work sheets so that we can analyze the steps in the them to determine how we can reorganize the production line. Most of the work (as is done in every other company) was on Excel and I particularly remember telling myself ages ago that opening 1000 Excel sheets is not something I see myself doing. BUT how else could I go about this? :confused:

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

I decided to use Python and not JavaScript in this case is because it's much easier to sort, filter, and work with dictionaries in Python than it is in JavaScript, especially since it involves a lot of reorganization of data. I worked on another project in a similar fashion but used JavaScript for that since it's a front-end solution that needed to be easily accessible to all members of the team (and not just a CLI tool on my computer).

### Step 3: Writing the Pseudocode

Here's an overview of what I had to do to understand the data:

- Use the `OS` module to list all files in the directory
- Create an empty dictionary that will be used to log the occurrences of the phrase 'Standard Work' for each cell 
- For each Excel workbook:
    - Get a list of sheet names and open any sheet that has 'Standard Work' in its title, ensuring that a second sheet that also contains this string isn't ignored
            sometimes there are two Standard Work sheets for one process / station
    - Iterate through all the cells in the first, second, and third column to find a cell that says 'Standard Work.'
    - Increment the value of the cell in the dictionary by one (if it exists), or create the key in the dictionary and set its value to 1
- Sort the dictionary by value in descending order

Of course, I inserted a bunch of `print` statements the first time around and conducted a trial run with 5 sheets just as a proof of concept. Once I knew this worked, I tried it on all sheets (like 200+) and noticed that there were 16 unique cells that had the phrase 'Standard Work' in them, so my original idea of manually setting a cell to read data after would not work. To be honest, it wasn't a fool-proof way of going through the files to begin with.

### Step 4: Brainstorming the "Final" Version

I said "final" because everyone knows the "real" final version is "final_final_result_6"

#### Compiling the steps
As I needed to obtain _all_ Standard Work steps and not just the ones that had the term 'Standard Work' at the top 3 unqiue cells from my earlier experiment, I settled on following this sequence of steps for one sheet:

- Search the second column for a string that contains 'Standard Work' in it
- If found:
    - Start reading data from the left-most column that's two rows below the current row
    - Stop reading data when a cell in the left-most column doesn't contain any text

The cell containing standard work steps is a merged cell and combines columns 1, 2, and 3 that we searched earlier. Thus, we copy steps from the left-most column (first one).

#### Compiling the time taken for each step by model

Each model may take a different amount of time for the same step

|   Step Description	|       Model 1  | Model 2 |   
|---	|---------------	|---	|
|  Step 1: Something	| ***2*** |   ***3***	|
|   Step 2: Something else	|   ***5***	|    ***0***	|

To determine which model takes the longest time, we define a dictionary when reading every sheet; the keys are the model names, and the values are the _sum_ of the time taken - this is incremented for all models as we read each step.

After extracting all the steps into an array, I sorted the dictionary by value in descending order and returned the steps with time values for the model that took the longest time at that station.

After doing this for every station and all model varieties, I ended up with an array of arrays in this format:

```python
result =
    [
        [
            station_name, 
            step_description, 
            time_in_seconds, 
            model_name
        ],
        ...
    ]
```

I output this result in a csv file using the `csv` module in Python, then did some basic formatting to share it with my team members.

### Step 5: Why I Didn't Painstakingly Do This on My Own

As you can see from my (weirdly) long explanation above, there isn't much code in it. That's because (i) if I share it I might reveal too much, and (ii) it's just logic. Regardless of whether we do this manually or whether automate it, the solution is essentially the same - I could either use a logical set of steps to manually do this __once__, or I could write down / think of the logic and code it up so I can easily make changes to it. 

Imagine how this'd have turned out if I did it manually and then one of my colleagues said, "ya know what, maybe include the model that takes the second longest time to get worked on at the station too" (true story)... yeah.... that'd haunt me until Thanksgiving.

## Takeaways

From what I've noticed, most problems that the 'general' employee may face in their day-to-day involve data aggregation. Many companies spend tremendous amounts of money, like millions of $$$, to install valuable data collection tools, however, there might only be a select set of individuals that know how to leverage that technology - and rightly so. Tech suites like SAP aren't easy to grasp out-of-the-box (especially if doesn't use a web API and forces people like me to hack something together after using _Inspect Element_ and the _Network_ tab). But the ease-of-access to knowledge, and the open-source nature of programming in general provides great incentive to learn and use these skills daily regardless of what industry one chooses.

Relatively easy-to-grasp programming languages, like Python, set a lower barrier to entry for anyone to get "in" on such technologies. This is not to say that programming is "easy" or that Python is a language only for newbies, no. 

What I'd like to emphasize is that these languages, and programming in general, is a tool that we can leverage to make problem-solving easier for us in the long run. It may require an investment of time (and maybe money) by an employee / employers, however, the value that this provides to an individual / organization far exceeds the cost.