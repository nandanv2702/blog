---
title: "SSL/TLS Part 2 - Let's Encrypt"
date: 2021-10-24T19:41:59-04:00
draft: false
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

This ensures that when a user visits `YOUR_SUBDOMAIN.YOUR_DOMAIN.COM`, the IP address resolves to `YOUR_SERVER_IP_ADDRESS`. This helps you prove to Let's Encrypt that you own the domain and the server that you're requesting the certificate on.

### Install Certbot

According to their [website](https://certbot.eff.org), 

> Certbot is a free, open source software tool for automatically using Let’s Encrypt certificates on manually-administrated websites to enable HTTPS.

The following command installs Certbot:

```bash
apt install certbot
```

We can now use this utility to install a Let's Encrypt certificate on our machine.

### Issue the Certificate

```bash
# if no webserver is currently running on your server, run the following command
certbot certonly --standalone -d YOUR_DOMAIN

# else, run this command
certbot certonly --webroot -d YOUR_DOMAIN
```

The former command starts up a temporary webserver on port 80 in order to verify ownership of the domain / subdomain, so if you already have a webserver running, use the latter command.

### Use the Certificates

Once you run the commands, Certbot will generate an SSL certificate and an SSL certificate private key, providing links for the same which you can use to configure HTTPS on your website.

```js
const privKey = fs.readFileSync('/etc/letsencrypt/live/cryptic-one.nandanv.com/privkey.pem', 'utf-8')
const certificate = fs.readFileSync('/etc/letsencrypt/live/cryptic-one.nandanv.com/fullchain.pem', 'utf-8')

const credentials = { key: privKey, cert: certificate }

const httpsServer = https.createServer(credentials, app)
httpsServer.listen(port, () => console.log(`server active on port ${port}`))
```

It may be necessary to restart your server once the certificates have been generated. If you're running Ubuntu, you can use the following command

```bash
reboot now
```

If everything worked out just fine, you should see a :lock: when you visit your domain. 

Congrats :beer: :fireworks:

## Redirect HTTP to HTTPS

Conventionally, we run HTTP on port 80 and HTTPS on port 443. The app I set up is quite simple and listens on port 80 and port 443, forwarding all port 80 requests (http) to port 443 (https). Here's the code that makes that happen 

```js
const httpApp = express()
.all('*', (req, res) => res.redirect(300, `https://cryptic-one.nandanv.com}`))

const httpServer = http.createServer(httpApp)

httpServer.listen(80, () => console.log('listening for http'))
```

If you want to learn more about SSL/TLS and why it's used, check out my [previous post](../why-ssl-certificates/)!