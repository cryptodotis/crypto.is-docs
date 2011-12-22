# Setting Up and Sending Email With Mixminion

The video for this screencast can be found at [YouTube](http://www.youtube.com/watch?v=McFKtdoVz7g)

The commands run in the screencast are:

    sudo apt-get install git-core
    sudo apt-get install build-essential
    sudo apt-get install python-dev

    git clone https://github.com/mixminion/mixminion.git
    cd mixminion/

    make download-openssl
    make build-openssl
    make
    make test
    sudo make install

    mixminion list-servers
    mixminion send -t sir@sirvaliance.com -i email.txt

