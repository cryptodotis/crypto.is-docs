# Running A Convergence Notary

## Installation

1.     Install the dependencies: `$ sudo apt-get install python python-twisted-web python-m2crypto python-openssl python-json`
2.     Download the notary source: `$ wget http://convergence.io/releases/server/convergence-notary-current.tar.gz`
3.     Unpack the source: `$ mkdir convergence-notary; cd convergence-notary; tar zxvf ../convergence-notary-current.tar.gz`
4.     Run the install script: `$ sudo python ./setup.py install`

## Configuration

1.     Generate a key pair: `$ convergence-gencert`
2.     Create database: `$ sudo convergence-createdb`
3.     Start the server: `$ sudo convergence-notary -p 80 -s 443 -c path/to/certificate.pem -k path/to/key.key`

> ### On Ports:
> The configuration shown above has the convergence notary running using port 80 and 443.  However, you can specify other ports.  Remember to open those ports as incoming on your firewall.
> The Publish section below details steps that will save the Notary configuration to the .notary file.  So make sure you use the same values you use here when following the Publish steps.  
The resulting .notary file that is created tells the notary client (upon importing the exported .notary file) which ports to use to connect to the notary with.

> ### On the Public Cert and matching Private Key:
> For clarity's sake the certificates you are specifying here are to establish communications between the Notary and the Client, and not for any Webserver you may have running on the existing box or elsewhere.  Remember, the Convergence paradigm doesn't work with Internet Root CA servers, so this means that you can generate a Self Signed CA Cert to use here.
However, you are correct in that you can generate a self signed cert for your website, which will be all that is necessary with the convergence system.

**At this time we are not aware if the Notary client will attempt to verify the certificate presented by the Notary (when it connects to the Notary to make requests) before establishing the SSL connection by running it through other existing Notaries first.**

## Publish

1.      Generate a notary bundle: `$ convergence-bundle`
2.      Publish the resulting file on your website, with a .notary extension.
3.       **You're done!** 

### Anyone can use your notary by clicking on the link to your .notary file.