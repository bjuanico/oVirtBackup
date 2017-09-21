# THIS TOOL IS BASED ON THE SDK3 WHICH IS DEPRECATED AND WILL BE REMOVED IN THE NEXT VERSION 4.2
# IT MAY OR MAY NOT GET UPDATED 
## This may be used as an alternative https://github.com/openbacchus/bacchus



# oVirtBackup

This is a tool, written in Python, to make **online** fullbackup's of a VM which runs in an oVirt environment.

## Requirements

It is necessary to install the oVirt Python-sdk.
In addition when running Pyhon < 2.7 you need you install argparse.

[http://www.ovirt.org/Python-sdk](http://www.ovirt.org/Python-sdk)

## Usage

Take a look at the usage text.

    backup.py -h

## Configuration

Take a look at the example "config_example.cfg"

## Workflow

* Create a snapshot
* Clone the snapshot into a new VM (posibly in a different storage domain)
* Delete the snapshot
* Two differet types of backup (can use either one or both)
  * Create a template from the cloned VM (in the same storage domain) 
  * Export the VM to the NFS share
* Delete the VM

Backup to template can be more flexible in large instalations because you can have multiple storage domains containig backed up hosts. 
With export based backups you can only have one export domain per datacenter

## Useful tips

### crontab

	00  20  *   *   *   /home/backup/oVirtBackup.git/backup.py -c /home/backup/oVirtBackup.git/config_webserver.cfg -d >> /var/log/oVirtBackup/webserver.log 

Logrotate: /etc/logrotate.d/oVirtBackup

	/var/log/oVirtBackup/* {
    	daily
    	rotate 14
    	compress
    	delaycompress
    	missingok
    	notifempty
	}
	
### Security

Set permissions to config.cfg only for the needed user (chmod 600 config.cfg).

## TODO's

* When the ovirtsdk supports exporting a snapshot directly to a domain, the step of a VM creation can be removed to save some disk space during backup

## Useful links:

* [http://www.ovirt.org/REST-Api](http://www.ovirt.org/REST-Api)
* [http://www.ovirt.org/Python-sdk](http://www.ovirt.org/Python-sdk)
* [http://www.ovirt.org/Testing/PythonApi](http://www.ovirt.org/Testing/PythonApi)
* [https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Virtualization/3.1/html-single/Developer_Guide/files/ovirtsdk.infrastructure.brokers.html#VMSnapshot](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Virtualization/3.1/html-single/Developer_Guide/files/ovirtsdk.infrastructure.brokers.html#VMSnapshot)

## Running tests

Install [tox](http://tox.readthedocs.io/en/latest/index.html).

```sh
$ tox
```
