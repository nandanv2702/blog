---
title: "SSL/TLS Part 2 - Let's Encrypt"
date: 2021-10-16T19:41:59-04:00
draft: true
---

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

## Redirect HTTP to HTTPS

Conventionally, we run HTTP on port 80 and HTTPS on port 443. The app I set up is quite simple and listens on port 80 and port 443, forwarding all port 80 requests (http) to port 443 (https). Here's the code that makes that happen 

```js
// some code here
```