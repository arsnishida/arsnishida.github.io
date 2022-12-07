---
layout: post
title:  "22-12-06, VENV for Macs3"
date:   2022-12-06
parent: ravnica
nav_order: 2
---

The beta version of MACS, 3.0.0b1, is held in a separate environment set up with `venv` to keep it away from macs2 and due to some different library requirements.

It can be accessed with `source /home/groups/ravnica/env/venv/macs3_env/bin/activate`.

It was setup as such:
```
cd /home/groups/ravnica/env/venv
/home/groups/ravnica/src/python/Python-3.9.16/bin/python3 -m venv macs3_env
source macs3_env/bin/activate
pip install macs3
```
