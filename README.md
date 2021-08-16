# Monitor Agentless hosts on Zabbix using WMI

## Dependencies

### System Packages

- Python3
- Python3-pip
- Python3-dev

### Python Packages

- Impacket >=0.9.23
- asn1 >= 2.4.1
- six >= 1.15.0
- pycryptodome >= 3.10.1

## Installation Instructions

- Step 1: Ensure WMI Queries are working on the target system.
- Step 2: Copy `'zabbix_wmi'` to the external scripts folder of the Zabbix server or proxy `(default location: /usr/lib/zabbix/externalscripts/)`.
- Step 3: Import the template provided in the repository into your Zabbix system via frontend.
- Step 4: Link the template to the agentless hosts.

Please note: If you are facing script timeout error, then increase the timeout to 30 seconds in your zabbix_server.conf or zabbix_proxy.conf

```zabbix_conf
### Option: Timeout
#       Specifies how long we wait for agent, SNMP device or external check (in seconds).
#
# Mandatory: no
# Range: 1-30
# Default:
# Timeout=3

Timeout=30
```

## Important Notes

This script was tested on Zabbix 5.2.5, should work on higher versions without any issues.
If you face any issues please open an issue from the issues tab on top.

You're welcome to contribute on this project. :)
