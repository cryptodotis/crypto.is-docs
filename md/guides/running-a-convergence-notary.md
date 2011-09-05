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

The configuration shown above has the convergence notary running using port 80 and 443.  However you can specify other ports.  Remember to open those ports as incoming on your firewall.
The Publish steps below will save the configuration, to the notary file.  Make sure you use the same values when following the Publish steps, the file created tells clients (upon importing the exported .notary file) which ports to connect to the notary on.

## Publish

1.      Generate a notary bundle: `$ convergence-bundle`
2.      Publish the resulting file on your website, with a .notary extension.
3.       **You're done!** 

### Anyone can use your notary by clicking on the link to your .notary file.