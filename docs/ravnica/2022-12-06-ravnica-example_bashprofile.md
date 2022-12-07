---
layout: post
title:  "22-12-06, Example Bash Profile"
date:   2022-12-06
parent: ravnica
nav_order: 2
---

`~/.bash_profile`: This should get added onto your bash profile. The only big change is that we’re asking for umask 002 now instead of umask 007 but either is still good. The module versions are different on the new clusters so change those around accordingly and you can add the shared R library path and auto-start the shared scitools python environment at startup if you want but that’s all optional.

```sh
if [ $HOSTNAME == "mays" ] || [ $HOSTNAME == "mccovey" ] || [ $HOSTNAME == "bonds" ] || [ $HOSTNAME == "bonds" ] || [ $HOSTNAME == "nissa" ] || [ $HOSTNAME == "liliana" ] || [ $HOSTNAME == "ajani" ] || [ $HOSTNAME == "teferi" ]
then
	# set default file permissions (REQUIRED)
	umask 002
fi

if [ $HOSTNAME == "nissa" ] || [ $HOSTNAME == "liliana" ] || [ $HOSTNAME == "ajani" ] || [ $HOSTNAME == "teferi" ]
then
	# add modules at start up (RECOMMENDED)
	module load bedops/2.4.41
	module load bedtools/2.29.1
	module load samtools/1.16.1
	module load scitools/1.0
	module load R/4.2.2
	module load slack/0.1

	# add R library path (OPTIONAL)
	#export R_LIBS_USER='/home/groups/ravnica/env/R_library_backups/R-4.2.2_copy'	

	# activate scitools env (OPTIONAL)
	#mamba activate scitools
fi
```

---

`.bashrc`: You should delete the older conda paths between the code blocks within >>> conda initalize <<<. When you initialize mamba via, https://arsnishida.github.io/docs/ravnica/2022-12-05-ravnica-mamba_setup.html, it should put the newer paths on it. You should not add them manually, just delete the old conda paths. At the end it should look like this...
```sh
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
	. /etc/bashrc
fi

# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/groups/ravnica/src/mamba/mambaforge/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/groups/ravnica/src/mamba/mambaforge/etc/profile.d/conda.sh" ]; then
        . "/home/groups/ravnica/src/mamba/mambaforge/etc/profile.d/conda.sh"
    else
        export PATH="/home/groups/ravnica/src/mamba/mambaforge/bin:$PATH"
    fi
fi
unset __conda_setup

if [ -f "/home/groups/ravnica/src/mamba/mambaforge/etc/profile.d/mamba.sh" ]; then
    . "/home/groups/ravnica/src/mamba/mambaforge/etc/profile.d/mamba.sh"
fi
# <<< conda initialize <<<
```