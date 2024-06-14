OpenPLC Suite -- Debian 12 + XFCE + OpenPLC + OpenPLC Editor + ScadaBR + FUXA
=============================================

## SYNOPSIS

This is a collection of scripts that helps you create an OpenPLC based OS.  
A one-size-fits all PLC OS based on Debian 12 with XFCE.  
It includes OpenPLC, OpenPLC Editor, ScadaBR and FUXA.

It exists because it is really hard to get all these applications to install
and play nice together.

## USAGE

Default web-application credentials:

| Service   | URL:Port               | User    | Password |
|-----------|------------------------|---------|----------|
| OpenPLC 3 | localhost:8080         | openplc | openplc  |
| Tomcat    | localhost:8081         | tomcat  | tomcat   | 
| ScadaBR   | localhost:8081/ScadaBR | admin   | admin    |
| FUXA      | localhost:1881         | N/A     | N/A      | 

## BUILD REQUIREMENTS

Gather all required files by downloading them from the following locations.  
See "Version" column for the correct version.

| Tools          | Download                                                       | Version |
|----------------|----------------------------------------------------------------|---------|
| OpenPLC 3      | https://github.com/ServerMonkey/OpenPLC_v3_Debian/releases     | 240610  |
| OpenPLC Editor | https://github.com/ServerMonkey/OpenPLC_Editor_Debian/releases | 240610  |
| ScadaBR        | https://github.com/ScadaBR/ScadaBR/releases                    | 1.2     |
| FUXA           | https://github.com/frangoteam/FUXA/releases                    | 1.1.119 |

Or use fastpkg:

    sudo fastpkg -p 'openplc-3 openplc-editor scadabr fuxa' download

## BUILD OS

To create your own custom OS follow these steps:

Install Debian 12 with XFCE and become root.

Extract all above applications into the /opt folder. Including this project.

### Prepare OS

    # go to the OpenPLC Suite folder in /opt
    # and run the prepare script
    sh prepare-os

### Install OpenPLC

    # go to the OpenPLC folder in /opt
    # and run the install script
    bash install.sh linux

### Install OpenPLC Editor

    # add deadsnakes PPA to apt
    # because OpenPLC Editor requires python3.9
    KEY="/usr/share/keyrings/launchpad-ppa-deadsnakes.gpg"
    gpg --homedir /tmp --no-default-keyring \
        --keyring "$KEY" --keyserver keyserver.ubuntu.com \
        --recv-keys F23C5A6CF475977595C89F51BA6932366A755776
    URL="https://ppa.launchpadcontent.net/deadsnakes/ppa/ubuntu"
    echo "deb [arch=amd64 signed-by=$KEY] $URL jammy main" \
        >/etc/apt/sources.list.d/launchpad-ppa-deadsnakes.list
    apt-get -qq update

    # go to the OpenPLC Editor folder in /opt
    # and run the install script
    bash install.sh
    # after the installation, a link to the OpenPLC Editor will be created
    # in the start menu under "Development".

### Install ScadaBR

    # go to the ScadaBR source folder in /opt
    # and run the install script
    bash install_scadabr.sh
    # during the installation, you will be asked to set the tomcat port,
    # set it to 8081. Same for user and password, set it to 'tomcat'.

### Install FUXA

    # install required packages
    apt-get -yqq install npm
    # install fuxa via precise npm version
    npm install -g --unsafe-perm @frangoteam/fuxa
    # go to the OpenPLC Suite folder in /opt
    # add fuxa as a systemd service
    sh fuxa-as-systemd-service

Now you are done, all services should be running on boot.
