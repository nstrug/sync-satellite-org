# sync-satellite-org
Use this playbook to clone the structure of a Red Hat Satellite Organisation from one satellite to another satellite.
The playbook has the following features:
* Idempotent - run as a regular task to keep two or more satellites in sync
* Non-invasive - no changes are made on the source satellite
* Disconnected - can be use to sync multiple satellite by sneakernet

## How it works

The sync-satellite-org playbook connects to a satellite, the 'source' and reads metadata for the following objects:
* locations
* domains
* subnets
* lifecycle environments
* repositories
* content views
* activation keys
* partition tables
* templates
* operating systems
* hostgroups
* (more to add)

After reading this metadata, a new, portable playbook directory is created - we call this the execution playbook. This execution playbook can be executed against another organisation on any satellite (including the original satellite). The execution playbook will recreate the objects from the source organisation.

## Prerequisites

The following python packages need to be installed:

* apypie

The APYPIE library can be installed by following procedure

RHEL 7
1) Find Python version
[ec2-user@rhte-satellite ~]$ env python
Python 2.7.5 (default, Jun 11 2019, 14:33:56)

2) Install pip version for the default python version
sudo -i yum install python27-python-pip-8.1.2-3.el7.noarch

3) Enable relevant software collection
sudo scl enable python27 bash

4) Install APYPIE 
pip install apypie

Fedora
1) Install APYPIE
pip3 install apypie

## Running the Playbook

### Inventory

An example inventory is provided, the structure is as follows:

```
[satellite]
<FQDN of source satellite>

[satellite:vars]
<param1>=<val1>
<param2>=<val2>
<param3>=<val3>
```

**NB** Only ONE satellite should be specified.

The following parameters can be set:

| Parameter     | Default             | Notes                      |
|---------------|---------------------|----------------------------|
|foreman_organisation |Default_Organization |The organization on the source satellite to clone from |
|foreman_username|admin|The username to use when connecting to the source satellite|
|foreman_password|foobar|The password to use when connecting to the source satellite|
|foreman_server_url|https://satellite.example.com|The URL of the source satellite|
|foreman_verify_ssl|true|Whether to use strict SSL cert checking when connecting to the satellite|
|output_dir|/tmp/sync-satellite-org|The directory where the execution playbook will bre created|

## Running the execution playbook

The execution playbook will be created in ```output_dir``` on the ansible host.

### Inventory

A stub inventory is created for the execution playbook as follows:

```
[target]
<Enter target satellites here>

[target:vars]
target_foreman_username=<username to connect to target satellites>
target_foreman_password=<password to connect to target satellites>
foreman_verify_ssl=true
```

MULTIPLE target satellites can be specified, however the same username and password will be used on all of them.


