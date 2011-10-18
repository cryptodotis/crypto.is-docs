# Setup a Mixminion Server

## Installation

1. Install the dependencies. 
* For Debian Squeeze: `# apt-get install build-essential python python-dev python-setuptools python-sqlite git`
* For CentOS: `# yum install git gcc python-sqlite python-setuptools python-devel openssl-devel`
1. Add a user to your system to run mixminion with: `# useradd -m -r --home-dir /var/spool/mixminion mixminion`
2. Download the mixminion source: `# git clone git://github.com/mixminion/mixminion.git`
3. Build mixminion: `# cd mixminion/; make`
4. Test your build: `# make test` (Your /tmp-filesystem needs to be > 16m for this to work.)
5. Install your build to your system: `# sudo make install`
6. Copy the default configuration to etc: `# cp ~/mixminion/etc/mixminiond.conf /etc/`

## Configuration

* Use your favourite Editor to configure /etc/mixminiond.conf
* Set the BaseDir to /var/spool/mixminion
* When done, start your mixminion: `# su - mixminion -c "mixminion server-start"`. On first start, it will generate its keyset. Your server description file will be on $spooldir/keys/key_0001/ServerDesc.
* Mixminion ships with an init script which you should install: ~/mixminion/etc/mixminion.init

## Links

* [mixminion homepage](http://mixminion.net/)
* [server manpage](http://mixminion.net/manpages/mixminiond.8.txt)
* [list of known nodes](http://www.noreply.org/mixminion-nodes/) with uptime graphs