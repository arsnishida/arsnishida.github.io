---
layout: post
title:  "Mamba Setup"
date:   2022-12-05
parent: ravnica
nav_order: 2
---

1. Initialize mamba: `/home/groups/ravnica/src/mamba/mambaforge/bin/mamba init bash`
2. Restart your shell.
3. Check `mamba info`. It should look something like this with your username replacing mine 'nishidaa'.
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
4. Try loading an environment: `mamba activate scitools`.

5. Deactivate: `mamba deactivate`.
