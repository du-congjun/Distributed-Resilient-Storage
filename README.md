Distributed-Resilient-Storage
=============================

The aim of this project is to build on top of the DIRAC client to provide easy distributed storage capability for the DIRAC file catalog using Reed-Solomon erasure coding.

##Included scripts
**add-ec.py**: Uploads a single file to a location on the catalogue. If the file is named test.txt it will be turned into multiple smaller files whilch will then be uploaded to a directory called _test.txt in the user defined directory, ie /gridpp/username.

**get-ec.py**: Downloads the erasure coded files. The input could be:
* A *directory*, eg. `/gridpp/username/_test.txt`
* The *original filename*, eg. `test.txt` - then get-ec.py uses metadata to search for the file

**se-check-cli.py**: Tests on which storage elements shown by `dirac-dms-show-se-status` the user has write privileges.

##Installing the DIRAC client
Scientific linux 6.5 under VirtualBox was used for this project. Previously Ubuntu 14.04 was tried, but it had a problem with lcg_utils. The supported versions of lcg_utils and glibc are listed [here](http://lhcbproject.web.cern.ch/lhcbproject/dist/DIRAC3/lcgBundles/). The commands for an installation in the home directory are as follows:
```
$ cd
$ mkdir dirac
$ cd dirac
$ wget -np -O dirac-install http://lhcbproject.web.cern.ch/lhcbproject/dist/Dirac_project/dirac-install
$ chmod +x dirac-install
$ ./dirac-install -V gridpp -r v6r11p3 -g 2014-06-27
$ source bashrc 
```
Here,  `-V gridpp` indicates that the VO is gridpp, `-r v6r11p3` is the version of DIRAC to be installed, and `-g 2014-06-27` is the version of lcg_utils to be used as seen on the link above. These values may have to be changed depending on your system.

Next, the installation has to be configured, so make sure you have you keys in `.globus/`. Then run the following commands:
```
$ dirac-proxy-init -x
$ dirac-configure defaults-gridpp.cfg
```
This concludes the DIRAC client installation, but keep in mind that the `$ dirac-proxy-init` command has to be ran occasionally to generate a new proxy.

##Installing the erasure coding scripts
The only required module is the [zfec](https://pypi.python.org/pypi/zfec) extention for python. This could be installed by running:
````
$ easy_install zfec
```
Then clone this repository:
```
$ git clone https://github.com/ptodev/Distributed-Resilient-Storage.git
$ cd Distributed-Resilient-Storage
```
Now you could either use the scripts from this folder, or try to integrate them with DIRAC, which has not been tested and could have unexpected consequences:
```
$ cp add-ec.py ~/dirac/DIRAC/DataManagementSystem/scripts/dirac-dms-add-file-ec.py
$ cp get-ec.py ~/dirac/DIRAC/DataManagementSystem/scripts/dirac-dms-get-file-ec.py
$ cp se-check.py ~/dirac/DIRAC/DataManagementSystem/scripts/dirac-dms-se-check.py
$ dirac-deploy-scripts
```
