# Setup a Mixminion Server

## Installation

0. This guide targets Debian Squeeze servers. For other versions/distributions package names may differ.
1. Install the dependencies: `$ sudo apt-get install build-essential python python-dev python-setuptools python-sqlite`
1. If you can, add a user to your system to run mixminion with: `$ sudo adduser mixminion; su - mixminion`
2. Download the mixminion source: `$ git clone https://github.com/mixminion/mixminion.git`
3. Build mixminion: `$ cd mixminion/; make`
4. Test your build: `$ make test` (Your /tmp-filesystem needs to be > 16m for this to work.)
5. Install your build to your system: `$ sudo make install`
6. Copy the default configuration to your home: `$ mkdir ~/etc; cp ~/mixminion/etc/mixminiond.conf ~/etc/`

## Configuration

* Use your favourite Editor to configure ~/etc/mixminiond.conf
* If you have a mixminion user, and want to keep all of mixminions data in it's home, make a spool directory: `$ mkdir ~/spool; chmod 0700 ~/spool`
* When done, start your mixminion: `$ mixminiond start`. On first start, it will generate its keyset. Your server description file will be on $spooldir/keys/key_0001/ServerDesc.

## Links

* [mixminion homepage](http://mixminion.net/)
* [server manpage](http://mixminion.net/manpages/mixminiond.8.txt)
* [list of known nodes](http://www.noreply.org/mixminion-nodes/) with uptime graphs