---
title: "Generating SSL Certificates for Development"
date: 2021-10-04T20:46:32-04:00
draft: false
---

## Why I'm Writing This

If you've ever wanted to create an HTTPS server for local development, you know that it's really frustrating to understand OpenSSL, the crossed out :lock: sign, and an endless stream of terminology.

After a lot of research both in and out of work, I compiled a list of steps that will help make it easier to get the :lock: sign on `localhost` - truly satisfying, isn't it :smile:

{{<image src="./siliconvalley.gif" style="width:100%; border-radius: 6px;" alt="A fitting Silicon Valley meme">}}

## Let's Do This

Here's all the steps I followed to create an SSL :lock: certificate for local development purposes:

1. __Generate a Certificate Authority__

    We import the generated `rootCACert.pem` file to the browser and add it to our Trusted Root Certificates

2. __Create an RSA Private Key__ 

    This is what we use to "sign" a new certificate signing request

3. __Create a Certificate Signing Request__

    Done using the Private Key

4. __Create a Config File__

    Sets up DNS stuff for localhost

5. __Generate a Public Certificate__

    Signed with Private Key and by Certificate Authority

6. __Forward Secrecy__

    From NodeJS documentation using [Diffie-Hellman key-agreement protocol](https://nodejs.org/api/tls.html#tls_perfect_forward_secrecy)

__IMPORTANT NOTE__: if you're prompted for a `Common Name` while issuing a _Certificate_, enter __localhost__. For development purposes, I set the `Common Name` of the _Root CA_ to my name (cuz why not).

As we want to use TLS v1.3 - the most secure version - we need to install Node 12.x or higher. These versions automatically enforce TLSv1.3, so it's easier to work with. I chose to use the current version of Node, i.e., version 16.10.0. 

Here's how you can update your node version
- Check version using `node --version`
- Switch version using `nvm use 16.10.0`
- Install it using `nvm i 16.10.0`). This ensures TLS v1.3 is enforced (better security).

All OpenSSL-related tasks will occur in a folder titled `certs` in this repo. Run the following commands to set it up:
```bash
mkdir certs
cd certs
```

The procedure below highlights how we can generate 4096-bit RSA private keys, however, 2048-bit keys are very secure too and take slightly less time to verify during the TLS handshake. Now, let's proceed with the steps to generate a Root Certifying Authority (CA) and Certificate.

## Generate a Certificate Authority

A Certifying Authority is basically someone that says "yep, I issued this certificate - you can trust me." For developmental purposes, we can install this certificate (the `rootCACert.pem` file) generated from the commands below in our browser's Trusted Root Certificates. Here's how we issue the certificate:

Step 1: Generate a Private Key

```bash
openssl genrsa -out rootCAKey.pem 4096
```

Step 2: Generate a Self-Signed Root Certificate Authority
```bash
openssl req -x509 -sha256 -new -nodes -key rootCAKey.pem -days 3650 -out rootCACert.pem
```

This certificate, however, (obviously) won't be available on everyone else's devices and will still produce the `Not Trusted` message when visiting `https://your-website.com`.

## Create a Private Key

This is the private key we'll use to sign our public certificate - it's unique to our server. This step and the next two steps can be replicated for any number of servers you own.

```bash
openssl genrsa -out server-key.pem 4096
```

## Create a Certificate Signing Request

We create the CSR and send it over to the Root CA to be signed. 

Here's a random analogy - the Root CA has the same authority that your doctor has when they sign a note saying you were sick the day of your calc test - everyone trusts the doc :joy:

```bash
openssl req -new -key server-key.pem -out server-csr.pem
```

## Create a Config File

This file can help "resolve" the DNS to localhost. I was facing the issue before where it said it couldn't verify the 'Common Name' of the certificate. I tried using `127.0.0.1`, `localhost`, `localhost:3001`, `https://localhost:3001`, and so on, however, that didn't resolve it. 

The following solution helps get around that.

This will create a new file `v3.ext` and open a text editor

```bash
touch v3.ext
nano v3.ext
```

Add the following contents to the file 
```bash
authorityKeyIdentifier=keyid,issuer
# CA:FALSE means the certificate we generate using this file 
# will not be an intermediate certifying authority
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = localhost
```

You can also add an IP address to `alt_names` in the following way
```bash
# Uncomment below to include local IP
IP.1 = 127.0.0.1
IP.2 = 0.0.0.0
```

Exit the file by clicking `Ctrl+X` followed by `y` to save the changes.

## Generate a Public Certificate

Using the CSR, Root CA, and the `.ext` file generated above, we'll finally create a public certificate.

```bash
openssl x509 -req -in server-csr.pem -CA rootCACert.pem -CAkey rootCAKey.pem -CAcreateserial -out server-cer.pem -days 500 -sha256 -extfile v3.ext
```

This is what the server sends to the client when they request the `https` site. The certificate has been signed by the Root CA and this can be verified by the client after doing some crypto magic since they have the Root CA's certificate (as we did in the first step)

### Verify Certificate Contents

The command below can be used to parse the certificate's contents just to check that it's "normal" and has everything; to simply it, if you see confusing `Hackerman` style stuff, it worked.

```bash
openssl x509 -in server-cer.pem -text -noout
```

## Forward Secrecy

Intense security stuff that's quite good. I highly recommend watching [this](https://www.youtube.com/watch?v=AlE5X1NlHgg) video by [Hussein Nasser](https://www.youtube.com/c/HusseinNasser-software-engineering/videos) for a great explanation of TLS1.2 vs TLS1.3

> Perfect forward secrecy was optional up to TLSv1.2, but it is not optional for TLSv1.3, because all TLSv1.3 cipher suites use ECDHE - from the [NodeJS TLS Documentation](https://github.com/nodejs/node/blob/master/doc/api/tls.md)

```bash
openssl dhparam -outform PEM -out dhparam.pem 2048
```

## Setting Up a Basic HTTPS Server

Assuming you followed the same directory structure from above, you can either clone my [https-demo](https://github.com/nandanv2702/https-demo) repo or copy the following code to run a basic HTTPS server.

```js
const https = require('https')
const fs = require('fs')

const options = {
    ca: fs.readFileSync('./certs/rootCACert.pem'),
    key: fs.readFileSync('./certs/server-key.pem'),
    cert: fs.readFileSync('./certs/server-cer.pem'),
    dhparam: fs.readFileSync('./certs/dhparam.pem'),
    ecdhCurve: 'auto',
    honorCipherOrder: true,
}

https.createServer(options, function (req, res) {
    res.writeHead(200)
    res.end('tls v1.3')
}).listen(4555, "0.0.0.0", () => {
    console.info(`Listening`)
})
```

This can, of course, be configured with Express and other frameworks to generate dynamic web pages.

## Takeaways

{{<image src="./mr-robot.png" style="width:100%; border-radius: 6px;" alt="Hackerman Rami Malek ftw">}}

I've struggled with this for an exTREMELY long time now... it was only when I took some time away from the actual computer and confusing OpenSSL documentation (I swear, it's me - not them) that I understood how all this actually works. 

Learning about what TLS actually is, the TLS handshake, TLSv1.2 vs TLSv1.3 - all this let me scratch the surface and learn more about how the web works. It's truly interesting stuff... watching the video I referred to earlier ([this](https://www.youtube.com/watch?v=AlE5X1NlHgg) one if you're lazy) helped me visually understand things. I find that this type of mixed-learning style really helps, especially in back-end stuff since you can't "see" the changes the same way you do in front-end development.

## Some Helpful Links
- [DigiCert](https://www.digicert.com/kb/ssl-support/openssl-quick-reference-guide.htm)
- [Blog with DNS Workaround](https://www.freecodecamp.org/news/how-to-get-https-working-on-your-local-development-environment-in-5-minutes-7af615770eec/)
- [How to Add Certificate Authority in Chrome](https://ugetfix.com/ask/how-to-fix-this-site-is-not-secure-pop-up-with-an-error-code-dlg_flags_sec_cert_cn_invalid/)
- [Pointing an IP address to Self-Signed Certificate](https://stackoverflow.com/questions/6793174/third-party-signed-ssl-certificate-for-localhost-or-127-0-0-1)