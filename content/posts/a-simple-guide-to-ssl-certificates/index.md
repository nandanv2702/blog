---
title: "A Simple Guide to SSL Certificates"
date: 2021-10-03T17:48:33-04:00
draft: true
---

Alright, I'm just gonna say it - SSL stuff scares me. It was easy to get this set up for my website since AWS took care of it and made the certificate-issuing process a simple one, however, when I was setting up [cryptic-one](https://cryptic-one.nandanv.com) hosted on a [Linode](https://linode.com) server for a [YouTube challenge](https://youtu.be/ZwSIWS2dJ0E), I was unsure about how to get the :lock: to show up. 

This guide aims to make it easier for me and others to follow get a :lock: on their website using Let's Encrypt.

## What is SSL + Some Obscure Terminology

Although TLS (Transport Layer Security) is what we currently use around the web, the :lock: is still commonly referred to as SSL (TLS' predecessor).

> The Secure Sockets Layer is a transport protocol that establishes authenticated and encrypted links between network computers. Read more [here](https://www.ssl.com/faqs/faq-what-is-ssl/).


The :lock: you see on any `https` website signifies the SSL certificate - a digital document that uses cryptographic magic to identify a particular website. If you've used a self-signed SSL certificate issued using `OpenSSL`, you'd be familiar with the __Potential Security Risk Ahead__ message displayed by the browser.

{{<image src="https://www.https.in/blog/wp-content/uploads/2020/03/browser_warnings_1.png" style="width:100%; border-radius: 6px;" alt="Bad SSL error from https://www.https.in/blog/self-signed-ssl-certificate-risk/">}}

This occurs because the Certificate Authority (aka you!) is not one that the browser inherently trusts. Now, in a development environment, we know that we can trust ourselves, so we can add this certificate to our Trusted Root Certificates in the browser. This will let the browser know that we can be trusted - but this will _only_ work during development.

When deploying our app / website, we need to use a different method; we need to obtain an SSL certificate from a trusted authority. This is where Let's Encrypt comes in.

## Let's Encrypt

Let's Encrypt is a free, automated, and open certificate authority - if you want to learn more, you can visit [letsencrypt.org](https://letsencrypt.org), but in this blog post I'm going to break down how to actually issue the certificate.

### A Few Assumptions
1. You have a server to host your website - I'm using [Linode](https;//linode.com) for this simply because I found a discount code online (hey man, it still works oK)
2. You have a domain you can use - in my case, this was the subdomain cryptic-one.nandanv.com
3. You can SSH into your server

## How To

Here's the steps to issue an SSL certificate on your server using Let's Encrypt

### Point Your Subdomain to the Server's IP

- Go to DNS Management Portal (_Route53_ DNS Management if you're using AWS)
- Create an `A` record
    - Record Name: `YOUR_SUBDOMAIN`
    - Record Type: `A`
    - Value: `YOUR_SERVER_IP_ADDRESS`

### Install Certbot

TODO: Insert Commands

```bash
apt-get install certbot
```

### Issue the Certificate

TODO: Insert Commands

certbot --certonly

### Renew a Certificate

TODO: Insert Commands
