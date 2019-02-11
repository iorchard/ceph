Debian Build (Experimental)
============================

This document is about building ceph debian packages on debian stretch.

To build ceph, gcc should be 7+ but gcc on debian stretch is 6.3.0.
So you need to install gcc from buster(testing) on debian stretch.

Build env
----------

* CPU: The more, The better. 8+ cores recommended.
* RAM: 16 GiB minimum. 32 GiB recommended.
* Disk: 50+ GiB minimum.

Build
------

Install gcc from buster on stretch build machine.::

    jijisa@shaq:~$ echo "deb http://debian-archive.trafficmanager.net/debian/ \
                    buster main" | sudo tee /etc/apt/sources.list.d/buster.list
    jijisa@shaq:~$ echo 'APT::Default-Release "stable";' | \
                    sudo tee /etc/apt/apt.conf.d/stable-as-default
    jijisa@shaq:~$ sudo apt update
    jijisa@shaq:~$ sudo apt install -t buster g++

Download the latest ceph source tarball.::

    orchard@deployer:~/build$ wget \
                    https://download.ceph.com/tarballs/ceph-13.2.4.tar.gz

Untar it.::

    orchard@deployer:~$ mkdir -p build/ceph
    orchard@deployer:~$ cd build/ceph
    orchard@deployer:~/build/ceph$ tar xzf ceph-13.2.4.tar.gz 

Copy install-deps-stretch.sh into the source directory.
install-deps-stretch.sh file is on iorchard forked ceph repository.::

    orchard@deployer:~/build/ceph$ cd ceph-13.2.4
    orchard@deployer:~/build/ceph/ceph-13.2.4$ wget \
        https://raw.githubusercontent.com/iorchard/ceph/master/install-deps-stretch.sh

Build prerequisites.::

    orchard@deployer:~/build/ceph/ceph-13.2.4$ ./install-deps-stretch.sh

Build ceph debian package.::

    orchard@deployer:~/build/ceph/ceph-13.2.4$ dpkg-buildpackage -uc -us

If everything goes well, there will be ceph debian packages in ~/build/ceph
directory.

