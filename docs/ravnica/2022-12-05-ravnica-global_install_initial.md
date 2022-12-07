---
layout: post
title:  "22-12-05, Global Installs - Initial"
date:   2022-12-05
parent: ravnica
nav_order: 2
---

Install major base installs (C libs, etc.) individually with sudo on ajani, liliana, nissa, and teferi.
```sh
# general stuff, most for specific other software downstream
sudo apt-get install emacs autoconf automake make gcc perl zlib1g-dev libbz2-dev liblzma-dev libcurl4-gnutls-dev libssl-dev gfortran tcl-dev libssl-dev libbz2-dev liblzma-dev libncurses5-dev libhdf5-dev xorg-dev git build-essential libreadline-dev libxml2-dev libgsl-dev libtiff-dev libharfbuzz-dev libfribidi-dev libopenblas-dev libcairo2-dev

# set python as python3 symlink
sudo apt install python-is-python3

# update
sudo apt-get update
```

Install Modules.
```sh
curl -LJO https://github.com/cea-hpc/modules/releases/download/v5.2.0/modules-5.2.0.tar.gz
tar xfz modules-5.2.0.tar.gz
mkdir modules-5.2.0-install
cd modules-5.2.0/
./configure --prefix=/home/groups/ravnica/src/Modules/modules-5.2.0-install --modulefilesdir=/home/groups/ravnica/modulefiles
```

Setup Modules individually on ajani, liliana, nissa, and teferi. This lets each of them find Modules.
```sh
sudo ln -s /home/groups/ravnica/src/Modules/modules-5.2.0-install/init/profile.sh /etc/profile.d/modules.sh
sudo ln -s /home/groups/ravnica/src/Modules/modules-5.2.0-install/init/profile.csh /etc/profile.d/modules.csh
```

Setup cron on teferi only. This seems already go to go on Ubuntu relative to centOS.
```sh
# use sudo to add users to /etc/cron.allow: root, nishidaa, adey
# but everybody seems allowed natively?
```
