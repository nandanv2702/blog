---
title: "My First Post"
date: 2021-07-20T19:03:30-04:00
draft: false
---

here's a couple of things w'll take abot

In this we're going to talk about a lot of stuff:
- [ ] about things we can work on
- [ ] what more we cna improve
- [ ] here's where we are

Having just written my first (and arguably last) final exam for this semester, it’s hardly hitting me that the rest of my friends and I are soon going to be juniors.

```js
const menuTrigger = document.querySelector('.menu-trigger')
const menu = document.querySelector('.menu')
const mobileQuery = getComputedStyle(document.body).getPropertyValue('--phoneWidth')
const isMobile = () => window.matchMedia(mobileQuery).matches
const isMobileMenu = () => {
  menuTrigger.classList.toggle('hidden', !isMobile())
  menu.classList.toggle('hidden', isMobile())
}

isMobileMenu()

menuTrigger.addEventListener('click', () => menu.classList.toggle('hidden'))

window.addEventListener('resize', isMobileMenu)
```

Junior year almost holds that adult-like feeling to it, yet there’s not much resemblance of an adult in me – guess that’s because I’m always going to be like a child on the inside. Nevertheless, I digress. The real reason I wanted to talk to you today was because of the circumstances we’re in. Two days ago, while I was working out and weight-lifting in the 6th floor lounge using a repurposed milk carton as my “weight,” I glanced down upon 3 people – one couple and (I’m guessing) their mutual best friend – wearing really fancy outfits and graduation caps. Initially it took me a while to recollect what date it was because literally every day feels the same now, and that’s when I remembered it was May 3rd. Now, I don’t know the exact date that seniors graduate but I'm guessing it's then

{{< code language="css" title="Really cool snippet" id="1" expand="Show" collapse="Hide" isCollapsed="true" >}}
pre {
  background: #1a1a1d;
  padding: 20px;
  border-radius: 8px;
  font-size: 1rem;
  overflow: auto;

  @media (--phone) {
    white-space: pre-wrap;
    word-wrap: break-word;
  }

  code {
    background: none !important;
    color: #ccc;
    padding: 0;
    font-size: inherit;
  }
}
{{< /code >}}

The couple then proceeded to take a picture in front of our residence hall and that’s when it hit me – this is what graduation was for them. Soon after I saw this, a plethora of thoughts flooded my brain – is this going to be my friends and I coming back and seeing our very first residence hall? The place where we studied and slept, cried and laughed together for almost two years? The place we called home? None of us saw this situation coming and nothing makes me sadder than seeing the Class of 2020 go through their graduation like… this. No one deserves that. But this was the moment when it hit me – we’re going to be juniors.

{{< image src="/posts/my-first-post/Screen Shot 2021-07-12 at 6.44.40 PM.jpg" alt="Hello Friend" position="center" style="border-radius: 8px;" >}}

