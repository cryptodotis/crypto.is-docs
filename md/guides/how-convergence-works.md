# How Convergence Works


This is a follow-up to the very basic overview given in the
[Setting Up and Using Convergence](1) guide.

At the time of writing there appears to be no official documentation on the
design and inner workings of Convergence.
So this is an account of reading through the
[source code](2).
It's not a lot, go check it out. ;)
Anyway, a disclaimer is in order.
This is all reverse-engineering;
any of the details may be wrong, inaccurate, or missing.
Take with a grain of salt!


## Overview

Before going into details, let's revisit the classic way to do things:
In order to be trusted,
a site's SSL key has to be signed by one of a long list of *certificate
authorities* (CAs).
Every browser ships with this list and they mostly agree on who's on it.
If an attacker can get *any one* of the CAs to sign a fake certificate,
he can impersonate a site and nobody might notice for quite a while.

Convergence introduces the concept of *notaries* into the picture.
A notary is a simple service.
It retrieves, verifies, and remembers
the certificates of sites visited by its users.
Every user of the notary, upon visiting a website,
simply asks whether the certificate they see
matches the one seen by the notary.
To avoid being easily fooled,
every user chooses several notaries to consult in parallel.


## Anatomy

### Client Certificate Cache

Assuming, through the notary system,
a Convergence-enabled web browser has gained confidence
in the authenticity of a given certificate.
It saves a tuple containing the following:

 * *host name* of the site from which the cert was received
 * *port number* of the site
 * *fingerprint*, i.e. cryptographic hash of the certificate

The current implementation saves these cache entries in an SQLite database.
Cf. [NativeCertificateCache.js](3) and [ConnectionWorker.js](4).

### Notary Certificate Cache

Much like the client, each notary remembers
every certificate it has successfully verified:

 * site's host name
 * site's port number
 * certificate fingerprint

This is a "positive-answer" cache.
If a request comes in and a matching entry is found,
the answer is positive,
i.e. the certificate is trusted.
If no match is found,
the certificate is looked up and verified from its source.

Note: All three elements are compared when looking for a match.
So a site can have multiple valid fingerprints.

The current code records the first and last times
it has seen a certificate,
but it does not seem to purge them at any point.
[Confirm?]

Cf. [CacheUpdater.py](5), [FingerprintDatabase.py](6)

### Client Certificate Check

To get control of the SSL connections made by the browser
and to show its custom certificate status,
Convergence hijacks the connections as a man-in-the-middle.
If the site certificate checks out behind the "MITM gap"
(!), a dummy certificate is presented to the browser
in front.
The dummy cert is set to be trusted and thus
makes the SSL connection show up as verified.

Cf. [ConnectionManager.js](7)

When the browser wants to set up a new connection:

1. Convergence grabs it, sets up its two endpoints.
2. Establishes the actual connection to the remote site.
3. Retrieves the SSL cert.
4. Looks up (host, port, cert) in the local cache.
5. If that tuple is in the cache, cert is OK.
6. Otherwise, consults the notaries
7. If cache or notaries say OK,
   cache the result and
   present the good dummy cert to the frontend.

Consulting the notaries means, for each:

1. Establish an SSL connection to the notary.
2. Make an HTTP request containing (host, port, cert).
3. Notary does its thing, returns OK or NOK.
4. Depending on the configuration,
   combine all the answers into one final OK/NOK.
   The default is to require a *majority* of notaries
   to OK the cert.

In order for the notaries' answers to be trustworthy,
the SSL connections to them must be trusted.
This is the bootstrap problem.
Convergence deals with it by saving the certificate of
each notary at the time it is added to the configuration.
This typically happens by the user downloading a `.notary` file
which contains the cert and will be imported by Convergence.

### Notary Certificate Check

When asked to verify a (host, port, cert) tuple,
a notary performs the following steps:

1. Look for (host, port, cert) in the cache.
2. If that tuple is found, cert is OK.
3. Otherwise, establish SSL connection to host and note the cert returned.
4. Verify the site's cert according to the classic (CA) model.
   [Confirm this?! What's the CA list used? CRLs?]
5. If the returned cert does not validate, abort and return NOK.
6. If the returned cert validates, add to/update the cache.
7. If the returned cert matches the one given, return OK, else NOK. 


### Notary Connection Anonymization

In order to avoid your notaries learning about your browsing habits,
Convergence supports a simple anonymizing proxy function.
In addition to the business HTTPS queries,
the notary also responds to HTTP CONNECT requests.
These allow the entire SSL connection to another notary to be proxied.

In order to avoid being usable as a completety open proxy,
however, the software is hard-coded to only accept proxy
requests to the arbitrarily-chosen destination port 4242.

So in addition to the normal SSL port,
the notary listens for SSL on 4242 to support this function.


## Security Considerations

TODO


# Protocol
Convergence uses a fairly simple HTTP 1.0 based protocol. The communication between client and notary consists of one request followed by one reply. Requests are handled by the SSL port or port 4242 of the notary.

## Client Request
The client makes a HTTP POST for page `/target/hostname+port` and as post variables it sends the fingerprint it saw.

    POST /target/duckduckgo.com+443 HTTP/1.0
    Content-Type: application/x-www-form-urlencoded
    Connection: close
    Content-Length: 71
    
    fingerprint=51:1F:8E:C6:22:82:5B:ED:A2:75:CB:3E:95:AB:63:7F:69:D3:18:1C

## Notary Replies
The notary can reply with several status codes that indicate what happened. When you make a valid request the notary will reply with a JSON encoded list of fingerprints and timestamps.
### 200 OK and 409 CONFLICT
These replies happen when the notary can verify the fingerprint without errors. 200 OK is used when the fingerprint is correct and 409 CONFLICT is used when the fingerprint is incorrect. The content of the reply is a signed JSON object. The Signature is for the JSON encoded data minus the signature itself, so the signed data is `{"fingerprintList":[...]}`. The reply shown below has the JSON data with newlines and indentation but an actual server reply is tightly packed without spaces or newlines.

    HTTP/1.0 200 OK
    Date: Tue, 06 Sep 2011 10:48:14 GMT
    Content-Type: application/json
    Server: TwistedWeb/8.2.0
    
    {
        "fingerprintList":
        [
            {"timestamp": {"start": "1312585781", "finish": "1312585781"}, "fingerprint": "51:1F:8E:C6:22:82:5B:ED:A2:75:CB:3E:95:AB:63:7F:69:D3:18:1C"}
        ],
        "signature": kU0P3... removed for brevity ...zAzB6A=="
    }

[1]: https://wiki.crypto.is/page/md/guides/setting-up-and-using-convergence.md
[2]: https://github.com/moxie0/Convergence
[3]: https://github.com/moxie0/Convergence/blob/a7a702ae8c8eca77a5e3dd6c194cccaa49c30f35/client/chrome/content/ssl/NativeCertificateCache.js
[4]: https://github.com/moxie0/Convergence/blob/a7a702ae8c8eca77a5e3dd6c194cccaa49c30f35/client/chrome/content/workers/ConnectionWorker.js
[5]: https://github.com/moxie0/Convergence/blob/a7a702ae8c8eca77a5e3dd6c194cccaa49c30f35/server/convergence/CacheUpdater.py
[6]: https://github.com/moxie0/Convergence/blob/a7a702ae8c8eca77a5e3dd6c194cccaa49c30f35/server/convergence/FingerprintDatabase.py
[7]: https://github.com/moxie0/Convergence/blob/a7a702ae8c8eca77a5e3dd6c194cccaa49c30f35/client/components/ConnectionManager.js
