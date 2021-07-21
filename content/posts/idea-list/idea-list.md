---
title: "Idea List"
date: 2021-07-20T21:20:33-04:00
draft: true
---


The blog needs to have a specific theme: I think a good way to go about this would be to make it about:

1. What I've learned so far
2. What I'm excited to learn about
3. Where I see this going in the future / the applications of full-stack development

```js
console.log("this is just here so there's a gap lol");
```

Here's a couple of ideas that I can write about 

1. [ ] Why you should learn a programming language
    - [ ] How I started working at a new place and got my first internship / co-op

    - [ ] The importance of patterns in our every day lives

    - [ ] How every job has certain things that are repetitive

    - [ ] How programming languages like Python can make things easier for you: include Python code for lecture video downloader from class + for video file creator

    - [ ] Simple use cases like this can help automate processes in the industry


2. [ ] Understanding the JavaScript Event Loop:
    - [ ] Imagine what it'd be like if you disabled JS and tried to roam the web - it'd be a nightmare. I got more curious to learn about the JS event loop after messing around with the Chrome DevTools and seeing a timeline of events occurring one after another. Is there any case where things can happen concurrently? What happens if you want data from a variety of sources? Do you need to wait for one Fetch call to finish then move on to the next?

    - [ ] There's a great talk I saw about the JS event loop from the JSConf:
       - // [Watch the video here]("https://youtube.com/watch?v=8aGhZQkoFbQ")
    
    - [ ] By understanding the ins and outs of the event loop, we can try to conscientiously avoid writing code that "blocks" the event loop, thereby slowing down the process - this is probably what makes some websites feel extremely laggy (aside from the fact that they might use a bunch of packages)


3. [ ] Using Python to "automate the boring stuff":
    - [ ] This blog deals with both languages as each language is a tool to achieve something - syntactically JS would be much longer to accomplish the same task; the way I look at it, I'm being productive doing it using either language so as we'd all say:
        > eh, why not...

    - [ ] Why this is useful for you - you can automate a bunch of stuff; for most of the basic stuff you need to do, Python makes it really easy to do that - and yeah, it's used for hardcore data science / AI / ML stuff too so, uh... it's not just pseudocode

    - [ ] Python can be used to automate even more stuff, for e.g., my job requires me to pour through a bunch of Excel files and gain information on all products / parts used in the production process. In order to make this easier for myself, I recognized a pattern in all of the information [after lulling over it for a solid two hours, that is](/posts/my-first-post/my-first-post)
    >
    > REPLACE WITH ACTUAL LINK ABOVE
    > 

    - [ ] OpenPyXl is really helpful in this case

    - [ ] Insert some code (with comments ofc) and explain what each part of it does

    - [ ] Making this functional:
        - This code is easy for me to understand, however, if someone else were to use this program to find parts, they would most likely get confused

        - Creating a GUI for this would be amazing but WAIT - I literally have 0 experience with that

        - Thing about this app is that it does one thing - and one thing only. I kind of cringe at it myself, but hey, at least I made it work somehow; StackOverFlow can really be your saviour in these cases

        - A couple of lines of TKinter code later, voila! We have a running GUI app

    - [ ] Broader applications: most of work is moving towards the automated side. Things can be made easier that way and it can help the human mind do it's thing - aka, be creative.


4. [ ] REST API fundamentals:
    - [ ] you already know what REST stands for

    - [ ] what "standards" make an API a REST API?

    - [ ] why use this rather than directly query a database (SQL/NoSQL)

    - [ ] how do you create a REST API:
        - [ ] show an example using basic express code and show the file structure of a complex project that may include a front-end too (the api / v1 folder) - explain a little bit about the project you're creating at work
        
        - [ ] guide on how to configure the routes for that instead of programming
            ```js
                app.route("/api/v1/:uuid")
                    .get((req,res, next) => {
                        // do something
                    })
                    .post((req,res, next) => {
                        // do something
                    })
            ```

        - [ ] guide on setting up a local MongoDB database

    - [ ] next steps:
        - [ ] you can rate limit your REST API and require authorization for it
        - [ ] connect it to your own front-end to create a full-stack application using seed data from another API (or your own data if you want to use the API to enter data into the database)

5. [ ] What is `next` used for in JavaScript?
    - [ ] Where all `next` is available
    
    - [ ] `req,res,next` example

    - [ ] The difference between next, continue, break, and ___

6. [ ] CORS: what is it and how does it relate to the security of an application?

7. [ ] How to create a pipeline to a NodeJS web app on AWS using GitHub
    - [ ] walk through the flow of how you set up UniTrack

8. [ ] Creating a full-stack application (series):
    - Idea: this can be an on-going series of your progress on how the inventory dashboard is coming along and what things you're getting to learn while actively working on the project, and also when you're back home
    - Blog posts:
        - [ ] An introduction - the idea, inspiration, how I was asked to help, and how I tried to understand the current working system as a whole

        - [ ] How should we set up the database - thought we'd require a bunch of tables but decided to settle on one massive table initially as we prioritized an MVP over an extremely well thought out, delayed project

            this also brings to importance the practise of CI - aka, nothing's "set in stone"

        - [ ] Creating an API for this:
            - [ ] thinking about the user - this helps us set up the routes that we need

            - [ ] commenting on the routes and putting it on one massive document before modularizing it is my way of working through this - I'm new to the whole project structure of a "large", enterprise application and I want to make this code re-usable for any other people that contribute to the codebase later on

            - [ ] this is a real-time application, thus it'll most likely use Socket.io to communicate from the server to the operators' displays; we're not worried about this part as of now (i really hope it doesn't bite me in the yk what)

        - [ ] API development: part 2
            - [ ] how do I _really_ know it works?

            - [ ] unit testing the API by setting up a cloned DB / working off one table alone

            - [ ] handling edge cases: listing down all edge cases, thinking of how likely they are to happen, and creating pseudocode in the form of comments to help put them into paper (the screen, really)

            - [ ] getting the C# app to use the API:
                - [ ] deployment
                - [ ] when do we integrate:
                    - [ ] rate limiting?
                    - [ ] authorization (as an added layer of protection)
                - [ ] creating a workflow to improve the API: how do we _know know_ it works
        
        - [ ] Mapping out the front-end: 
            > finding frameworks that make creating a dashboard easier: d3.js / d5.js / using React components with Hooks?

            > How does Socket.io know when to emit an event? Adding this to the API POST call? How do we connect both?

        - [ ] Creating React components - the big picture & integrating it with the operator view and the supervisor view

        - [ ] Adding login / other auth services to the whole app

        - [ ] trying to break what we just built - testing.


