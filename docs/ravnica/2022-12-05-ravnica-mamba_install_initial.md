---
layout: post
title:  "Mamba Install - Initial"
date:   2022-12-05
parent: ravnica
nav_order: 2
---

Install mambaforge
```
cd /home/groups/ravnica/src/mamba
bash Mambaforge-Linux-x86_64.sh
conda config --set auto_activate_base false
# restart shell
# set envs_dirs, pkgs_dirs, and channel and other stuff in global mambarc
mamba update --all
mamba info # check this
```

`mamba info` should look like this:
```
     active environment : None
            shell level : 0
       user config file : /home/users/nishidaa/.condarc
 populated config files : /home/groups/ravnica/src/mamba/mambaforge/.condarc
          conda version : 22.9.0
    conda-build version : not installed
         python version : 3.10.7.final.0
       virtual packages : __linux=5.15.0=0
                          __glibc=2.35=0
                          __unix=0=0
                          __archspec=1=x86_64
       base environment : /home/groups/ravnica/src/mamba/mambaforge  (writable)
      conda av data dir : /home/groups/ravnica/src/mamba/mambaforge/etc/conda
  conda av metadata url : None
           channel URLs : https://conda.anaconda.org/conda-forge/linux-64
                          https://conda.anaconda.org/conda-forge/noarch
                          https://conda.anaconda.org/bioconda/linux-64
                          https://conda.anaconda.org/bioconda/noarch
                          https://repo.anaconda.com/pkgs/main/linux-64
                          https://repo.anaconda.com/pkgs/main/noarch
                          https://repo.anaconda.com/pkgs/r/linux-64
                          https://repo.anaconda.com/pkgs/r/noarch
          package cache : /home/groups/ravnica/src/mamba/mambaforge/pkgs
       envs directories : /home/groups/ravnica/env
                          /home/groups/ravnica/src/mamba/mambaforge/envs
                          /home/users/nishidaa/.conda/envs
               platform : linux-64
             user-agent : conda/22.9.0 requests/2.28.1 CPython/3.10.7 Linux/5.15.0-52-generic ubuntu/22.04.1 glibc/2.35
                UID:GID : 5417:3010
             netrc file : None
           offline mode : False
```

Set up scitools py env in a slightly older version of python so all the libraries are actually found.
```
mamba create --name scitools python="3.9"
mamba activate scitools
mamba install numpy
mamba install scipy
mamba install pandas
mamba install jupyter
mamba install git
mamba install matplotlib
mamba install exrex
mamba install seaborn
mamba install pysam
mamba install Cython
mamba install zarr
mamba install pybedtools
mamba install bx-python
mamba install tensorflow
mamba install snakemake
mamba install macs2
mamba update --all
```
