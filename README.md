# zapache 1.5 - Apache monitoring script for Zabbix

The script version 1.4 and template are taken from https://www.zabbix.com/wiki/templates/apache#method_3

## What's new

### Version 1.5

* Zapache would cache received apache status page for 60 seconds by default. This eliminates the need to query apache for every item collected.
* Added worker threads graph to a template
* Added new items: status, ping, BusyWorkers, CPULoad
* Added trigger for Apache status to a template
* Ability to specify status page URL as a parameter to the script
* Better error handling
* Added sample conf files for Apache and Zabbix agent
* Added measurement units to a template
* Requires Zabbix version 2.0 and later.

## Installation

### CentOS 6.4/Zabbix 2.0 from EPEL repo

#### On the apache server:

##### Install files
	sudo install -o root -g root -m 0755 zapache /var/lib/zabbixsrv/externalscripts/zapache
	sudo install -d /etc/zabbix_agentd.conf.d
	echo "Include=/etc/zabbix_agentd.conf.d/" | sudo tee -a /etc/zabbix_agentd.conf
	sudo install -o root -g root -m 0644 userparameter_zapache.conf.sample /etc/zabbix_agentd.conf.d/userparameter_zapache.conf
	sudo install -o root -g root -m 0644 httpd-server-status.conf.sample /etc/httpd/conf.d/httpd-server-status.conf
##### Restart
	sudo service httpd restart
	sudo service zabbix-agent restart
##### Check if it's working
	sudo -u zabbix /var/lib/zabbixsrv/externalscripts/zapache Uptime
	sudo -u zabbix zabbix_agentd -p | grep ^zapache
	sudo -u zabbix zabbix_get -s localhost -k zapache[Uptime]

#### On Zabbix server

Now import zapache-template.xml and zapache-template-active.xml on Zabbix server and bind "Template App Apache Web Server zapache" OR "Template App Apache Web Server zapache Agent Active" template to Apache host.
