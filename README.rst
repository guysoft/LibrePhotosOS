LibrePhotosOS
=============
An out of the box `Raspberry Pi <http://www.raspberrypi.org/>`_ distro with `LibrePhotos <https://github.com/LibrePhotos>`_ installed. 


Where to get it?
----------------

You can use the `pi-imager <https://github.com/guysoft/pi-imager/releases>`_ commuity raspberrypi imager here, unofficial section.

Or download directly form the official mirror `here <http://unofficialpi.org/Distros/LibrePhotosOS>`_

How to use it?
--------------

#. Unzip the image and install it to an SD card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Configure your WiFi by editing ``LibrePhotosOS-wpa-supplicant.txt`` at the root of the flashed card when using it like a flash drive
#. Boot the Pi from the SD card
#. Hostname is ``librephotosos`` (not ``raspberrypi`` as usual), username: ``ubuntu`` and inital password is: ``ubuntu``
#. After a few mintues you should be able to access ``http://librephotosos.local/`` or ``http://librephotosos.lan/``
#. You can change the settings of the docker settings are located at ``/boot/docker-compose/librephotos/``


Requirements
------------
* Raspberrypi 3 and above, needs a 64bit capable pi
* 2A power supply

Features
--------

* `LibrePhotos <https://github.com/LibrePhotos>`_
* Automatically mounts hard drive attached to Pi and links librephotos to the "librephotos" folder on that drive


Developing
----------

Requirements
~~~~~~~~~~~~

#. Docker or Vagrant, docker recommended
#. Docker-compose - recommended if using docker build method, instructions assume you have it
#. Downloaded `Raspbian Lite <https://downloads.raspberrypi.org/raspbian_lite/images/>`_ image.
#. Root privileges for chroot
#. Bash
#. sudo (the script itself calls it, running as root without sudo won't work)

Build LibrePhotosOS
~~~~~~~~~~~~~~~~~~~

LibrePhotosOS can be built using docker running either on an intel or RaspberryPi (supported ones listed).
Build requires about 4.5 GB of free space available.
You can build it assuming you already have docker and docker-compose installed issuing the following commands::

    
    git clone https://github.com/guysoft/LibrePhotosOS.git
    cd LibrePhotosOS/src/image
    wget -c --trust-server-names 'https://cdimage.ubuntu.com/releases/20.04.4/release/ubuntu-20.04.4-preinstalled-server-arm64+raspi.img.xz'
    cd ..
    sudo docker-compose up -d
    sudo docker exec -it librephotosos-build build
    
Building LibrePhotosOS Variants
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

LibrePhotosOS supports building variants, which are builds with changes from the main release build. An example and other variants are available in the folder ``src/variants/example``.

To build a variant use::

    sudo docker exec -it LibrePhotosOS-build build [Variant]
    
Building Using Vagrant
~~~~~~~~~~~~~~~~~~~~~~
There is a vagrant machine configuration to let build LibrePhotosOS in case your build environment behaves differently. Unless you do extra configuration, vagrant must run as root to have nfs folder sync working.

To use it::

    sudo apt-get install vagrant nfs-kernel-server
    sudo vagrant plugin install vagrant-nfs_guest
    sudo modprobe nfs
    cd LibrePhotosOS/src/vagrant
    sudo vagrant up

After provisioning the machine, its also possible to run a nightly build which updates from devel using::

    cd LibrePhotosOS/src/vagrant
    run_vagrant_build.sh
    
To build a variant on the machine simply run::

    cd LibrePhotosOS/src/vagrant
    run_vagrant_build.sh [Variant]

Usage
~~~~~

#. If needed, override existing config settings by creating a new file ``src/config.local``. You can override all settings found in ``src/config``. If you need to override the path to the Raspbian image to use for building LibrePhotosOS, override the path to be used in ``ZIP_IMG``. By default, the most recent file matching ``*-raspbian.zip`` found in ``src/image`` will be used.
#. Run ``src/build_dist`` as root.
#. The final image will be created in ``src/workspace``

Code contribution would be appreciated!
