---
layout: post
title: "Genie installation guide"
date: 2024-05-245 12:00:00 -0000
categories: work
---

# GENIE installation guide
https://hep.ph.liv.ac.uk/~costasa/genie/get_started.html

OS: Windows Linux subsystem with Ubuntu 22.04

### get GENIE/Generator source code 
``` 
$ cd /your/software/area
$ git clone https://github.com/GENIE-MC/Generator.git
```


### install prerequisite packages: 
* log4cpp 
* libxml2


```$ sudo apt-get install liblog4cpp5-dev libxml2 ```

* LHAPDF

```
$ wget https://lhapdf.hepforge.org/downloads/?f=LHAPDF-6.5.4.tar.gz -O LHAPDF-6.5.4.tar.gz
$ tar xf LHAPDF-6.5.4.tar.gz
$ cd LHAPDF-6.5.4
$ ./configure --prefix=/path/for/installation
$ make
$ sudo make install
```

if configure step can't find python because only python3 is installed, install python-is-python3 package

* PYTHIA6
GENIE Generator source code ships with a script that automatically downloads and installs the latest version of PYTHIA6

```
cd /your/software/area 
source your-path-to-/Generator/src/scripts/build/ext/build_pythia6.sh
```
* ROOT6 with Pythia6 support

install ROOT prerequisite packages https://root.cern/install/dependencies/ , don't forget fortran 

```
$ git clone --branch latest-stable --depth=1 https://github.com/root-project/root.git root_src
$ mkdir root_build root_install && cd root_build
$ cmake -Dpythia6=ON -DPYTHIA6_DIR=/path-to-pythia-dir/lib  -DCMAKE_INSTALL_PREFIX=../root_install ../root_src
$ cmake --build . -- install -j8 # if you have 8 cores available for compilation

```

### prepare GENIE environment

create a setup script and execute it before using GENIE.

```
#!/bin/bash
export GENIE=/path/to/genie/top/directory
export ROOTSYS=/path/to/root/top/directory
export LHAPATH=/path/to/lhapdf/PDFSets/
export PATH=$PATH:\
$ROOTSYS/bin:\
$GENIE/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:\
/path/to/log4cpp/library:\
/path/to/libxml2/library:\
/path/to/lhapdf/libraries:\
/path/to/pythia6/library:\
$ROOTSYS/lib:\
$GENIE/lib
```

assuming the above script is named "genie_setup", execute it with: ```$ source genie_setup```


### 