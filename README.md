# Augur

branch | status
   --- | ---
master | [![Build Status](https://travis-ci.org/chaoss/augur.svg?branch=master)](https://travis-ci.org/chaoss/augur)
   dev | [![Build Status](https://travis-ci.org/chaoss/augur.svg?branch=dev)](https://travis-ci.org/chaoss/augur)

## What we expect to deliver

Overall, we plan to provide a comprehensive scoring system that uses metrics in 3 different groups: Code development, issue resolution, and community growth.

To businesses, who are most likely looking to adopt a new project, we expect to deliver a way for them to see the consistency of a repositories score overtime.

To community managers, who are most likely looking to check up on recent activity of a project they oversee, we expect to deliver a way for them to see how recent score/activity changes compare to the repo's usual activity.

To individual contributors/maintainers, who are most likely looking to inspect their individual contributions, we expect to deliver a way for them to see how their contributions are impacting the repository's overall score.

## About Augur

Augur is focused on prototyping open source software metrics. 

Functionally, Augur is a prototyped implementation of the Linux Foundation's [CHAOSS Project](http://chaoss.community) on [open source software metrics](https://github.com/chaoss/metrics). Technically, Augur is a [Flask web application](http://augurlabs.io), [Python library](http://augur.augurlabs.io/static/docs/) and [REST server](http://augur.augurlabs.io/static/api_docs/) that presents metrics on open source software development project health and sustainability. 


## Getting Started 
-------------------
### Vagrant
**The quickest way to get started working on Augur is by using [Vagrant](https://www.vagrantup.com/)** to spin up a virtual machine (VM) that comes with Augur already installed. We'll do all the work of setting up and installing dependencies, leaving you free to jump right into making something awesome. 

*Caveat: if you’re a super nerd who likes to have total control over your development environment, there’s a local installation link at the bottom of this page. For the rest of you, Vagrant is the way to go, especially if you've had trouble getting all the dependcies installed locally, are not comfortable installing them yourself, or are using an OS for which we don't currently support local installation. **We currently only support local installation for macOS and most flavors of Linux**.*

Windows installation instructions using Vagrant can be found [here](docs/python/source/windows-install.md).

Dependencies
------------

-   [Git
    client](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
-   [Vagrant](https://www.vagrantup.com/)
-   [Virtualbox](https://www.virtualbox.org/)
-   [GitHub Access Token](https://github.com/settings/tokens) (no write
    access required)

1. Clone the repository and boot up the VM.

```bash
# on your local machine
git clone https://github.com/chaoss/augur.git
cd augur
make vagrant
```

Note: you'll probably see a fair bit of errors during this provisioning process as Augur is getting installed. Don't worry about them, most of them are harmless. *Probably.*

2. Log in as `root` and navigate to `/vagrant/augur`. This folder is synced with your local clone of `augur`, meaning you'll be able to use your preferred local editor and just use the VM to run augur.  
```bash
# inside the vagrant VM
sudo su -
cd /vagrant/augur

# due to vagrant weirdness, we have to manually install the python packagew (this might take a while)
$AUGUR_PIP install --upgrade .
```

3. Add your GitHub API key to the `augur.config.json` file under the
section `GitHub`. 

4. Start both the backend and frontend servers with `make dev`.

```bash
make dev
```

5. When you're done working in the VM, type `exit` twice: once to log out of `root`, and another to log out of the VM. Don't forget to shut down the VM with `vagrant halt`.

If you're interested in adding a new plugin, data source, or metric, check out the [backend development guide](http://augur.augurlabs.io/static/docs/dev-guide/3-backend.html). If new visualizations are more your speed, you'll want the [frontend development guide](http://augur.augurlabs.io/static/docs/dev-guide/4-frontend.html\).

### TL;DR

```bash
# on your local machine

# using your Git client: 
git clone https://github.com/chaoss/augur.git

# using Command Prompt
cd augur
vagrant up
vagrant ssh

# inside the vagrant VM
sudo su -
cd /vagrant/augur

# due to vagrant weirdness, we have to manually install the python packages
$AUGUR_PIP install --upgrade .

# add your GitHub personal access token to augur.config.json

# start the frontend and backend servers
make dev
# full steam ahead!
```

### Local Installation

2.1.1. 1. Install Dependencies (OS Specific Instructions Below)
Anaconda
NodeJS Version 8.0 or newer, which includes npm

Also remember the database dependency in the README.


Dependency Installation for Ubuntu
Dependency Installation for Fedora
Dependency Installation for OS X
Install Augur



2.1.1.1. Ubuntu Dependency Installation Instructions
```
# Install NodeSource
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -

# Install NodeJS
sudo apt-get install -y nodejs

# Install MariaDB (if needed on the same machine for the GHTorrent/msr14 dataset)
sudo apt-get install -y mariadb-server

# Install Anaconda
curl https://repo.anaconda.com/archive/Anaconda3-5.1.0-Linux-x86_64.sh > Anaconda.sh
chmod +x Anaconda.sh

# You must agree to Anaconda's license terms to proceed
./Anaconda.sh -b
rm Anaconda.sh

# [Install Augur](#Install)
```







2.1.1.2. Fedora Dependency Installation Instructions
```
# Install NodeSource
curl -sL https://rpm.nodesource.com/setup_10.x | sudo -E bash -
sudo yum install -y nodejs


# Install NodeJS
sudo apt-get install -y nodejs

# Install MariaDB (if needed on the same machine for the GHTorrent/msr14 dataset)
sudo apt-get install -y mariadb-server

# Install Anaconda
curl https://repo.anaconda.com/archive/Anaconda3-5.1.0-Linux-x86_64.sh > Anaconda.sh
chmod +x Anaconda.sh

# You must agree to Anaconda's license terms to proceed
./Anaconda.sh -b
rm Anaconda.sh

# [Install Augur](#Install)
```







2.1.1.3. Mac OSX Dependency Installation Instructions
```
# Install Homebrew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# Install NodeJS
brew install wget node

# Install MariaDB (if needed on the same machine for the GHTorrent/msr14 dataset)
brew install wget mariadb

# Install Anaconda
cd ~/Downloads
wget https://repo.anaconda.com/archive/Anaconda3-5.1.0-MacOSX-x86_64.pkg
open Anaconda3-5.1.0-MacOSX-x86_64.pkg

# [Install Augur](#Install)
```










2.2. macOS High Sierra (and possibly older OS X Versions) Errata:
If you check the logs/frontend.log and find that “brunch” was not
found:

```
brew install npm
npm install -g brunch
brew install yarn
```





If the logs look good but the webpage at localhost:3333 is empty, it
could be that Yarn installed the wrong version of some libraries. In
that case:

```
cd frontend
npm install
```









2.3. Augur Installation Instructions
Clone the repo and install the libraries and tools needed by Augur

```
git clone https://github.com/chaoss/augur/

## Assume you are in the root from which you cloned augur

cd augur ## To get to augur root, where the make files live

# If you are going to do active development, please use the dev branch
git checkout dev

# Install the Python and Node tools and libraries needed
sudo make install-dev # some libraries require a root install.

# Ignore node-pre-gyp install errors asking for cairo library or install cairo library. Augur works either way.
```




Make sure you have a database user that has select access to the
database where you installed `GHTorrent <http://ghtorrent.org/>`__ and
all priviledges on another database for Augur.

```
CREATE USER 'augur'@'localhost' IDENTIFIED BY 'password';
GRANT SELECT ON ghtorrent.* TO 'augur'@'localhost';

CREATE DATABASE augur;
GRANT ALL PRIVILEDGES ON augur.* TO 'augur'@'localhost';
```




Augur runs in an Anaconda environment. To get started, activate the environment and then
run augur.

```
conda activate augur
augur
```




After you run the augur command for the first time, a configuration file called augur.config.json will automatically be generated.


Reference the sample configuration file (sample.config.json) on how to
set up the server, development, and cache options, as well as the plugin connections.


For all the API’s and visualiazations to work, you will need to include:


A GitHub API Key,
A connection to a Facade database,
A connection to a GHTorrent database.

For local API testing, you will need a Postman API key.


You’re ready to rock! To start both the frontend and backend, run:
make dev





## Guidelines
To contribute to Augur, please check out our [development guide](http://augur.augurlabs.io/static/docs/dev-guide/1-overview.html) and [notes on making contributions](CONTRIBUTING.md). Also, please note our [code of conduct](CODE_OF_CONDUCT.md). We want Augur to be a welcoming development community that is open to everyone. 

## Roadmap
Our technical, outreach, and academic goals [roadmap](https://github.com/chaoss/augur/wiki/Release-Schedule).

## License and Copyright
Copyright © 2018 University of Nebraska at Omaha, University of Missouri and CHAOSS Project at the Linux Foundation

Augur is free software: you can redistribute it and/or modify it under the terms of the MIT License as published by the Open Source Initiative. See the file [LICENSE](LICENSE) for more details.

(This work has been funded through the Alfred P. Sloan Foundation)
