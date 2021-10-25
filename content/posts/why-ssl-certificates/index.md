---
title: "SSL/TLS Part 1 - Why the 🔒?"
date: 2021-10-16T17:48:33-04:00
draft: false
---

Alright, I'm just gonna say it - SSL stuff scares me. It was easy to get this set up for my website since AWS took care of it and made the certificate-issuing process a simple one, however, when I was setting up [cryptic-one](https://cryptic-one.nandanv.com) hosted on a [Linode](https://linode.com) server for a [YouTube challenge](https://youtu.be/ZwSIWS2dJ0E), I was unsure about how to get the :lock: to show up. 

This guide aims to make it easier for me and others to follow get a :lock: on their website using Let's Encrypt.

This article is written in two parts:
- The first (this one) focuses on why we use SSL/TLS certificates
- The second is a tutorial on how to set up Let's Encrypt with your server

## What is SSL

Although TLS (Transport Layer Security) is the protocol we currently use around the web, the :lock: is still commonly referred to as SSL (TLS' predecessor).

> The Secure Sockets Layer is a transport protocol that establishes authenticated and encrypted links between network computers. Read more [here](https://www.ssl.com/faqs/faq-what-is-ssl/).


The :lock: you see on any `https` website signifies the SSL/TLS certificate - a digital document that uses cryptographic magic to identify a particular website.

## The Confusing Part/(s)

The certifying process has 3 main entities:

1. Your Computer (browser)
2. The Server
3. A trusted third-party, i.e., the Certifying Authority (CA)

The CA exists in order to verify the legitimacy of the server's SSL/TLS certificate. To simplify this explanation, here's an all-too-familiar analogy

## The Story

Assume that you're back in high school and it's midterm season.

You want to take a day off school because you're sick, but your principal doesn't trust you. They want to know if you're _really_ unwell.

In order to show them that it's not worth waking up at 6am with a 104 degree fever, you head to the doctor for a check-up. The doctor then signs a note saying "hey dude, this man's really unwell. Let him skip school and watch New Girl." 

You (the server) show this signed doctor's note (the SSL/TLS certificate) to your principal (the client). Your principal, being the skeptic they are, calls the doctor (the CA) and asks, "yo, is this lad actually sick?" The doc says "yep, he couldn't stop shivering." The principal then replies to you saying, "yeah, cool, you can take a sick day."

You then proceed to watch New Girl for 12 hours straight (it's a great show btw).

## The Problem
If you've used a [self-signed SSL certificate](../generating-ssl-certificates-for-development/) issued using `OpenSSL`, you'd be familiar with the __Potential Security Risk Ahead__ message displayed by the browser.

{{<image src="https://www.https.in/blog/wp-content/uploads/2020/03/browser_warnings_1.png" style="width:100%; border-radius: 6px;" alt="Bad SSL error from https://www.https.in/blog/self-signed-ssl-certificate-risk/">}}

This occurs because the Certificate Authority (aka you!) is not one that the browser inherently trusts. Now, in a development environment, we know that we can trust ourselves, so we can add this certificate to our Trusted Root Certificates in the browser. This will let the browser know that we can be trusted - but this will _only_ work during development.

> Check out [this](../generating-ssl-certificates-for-development) article to learn more about issuing your own CA and SSL/TLS certificate for development.

## The Solution

When deploying our app / website, we need to use a different method; we need to obtain an SSL certificate from a trusted authority. This is where Let's Encrypt comes in.

In the [next blog post](../lets-encrypt-a-simple-guide), I'll run through how to set up Let's Encrypt to show the :lock: on a website hosted on your public server.

## Helpful Resources
- [How Does SSL/TLS Work?](https://security.stackexchange.com/questions/20803/how-does-ssl-tls-work/20833#20833)
- [The TLS Handshake In-Depth](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)

