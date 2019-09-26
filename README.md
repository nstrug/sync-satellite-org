# sync-satellite-org
Project to clone metadata from an organisation to a disconnected satellite via a generated playbook

The project depends on APYPIE. Following are instructions on how to install it on RHEL 7:

1] Find out default python version
[user@server.example.com ~]$ env python
Python 2.7.5 (default, Jun 11 2019, 14:33:56)

2] Install software collections alternative version of pip (step assumes software collections are enabled)
sudo -i yum install python27-python-pip-8.1.2-3.el7.noarch

3] Enable given python version of scl
sudo scl enable python27 bash

4] install apypie with pip tool
sudo pip install apypie
