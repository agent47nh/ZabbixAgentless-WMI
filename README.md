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

## Usage

```python-usage
usage: zabbix_wmi [-h] [-v] -c Win32_LogicalDisk [-lp /tmp/zabbix_wmi.log] -t 192.168.75.34 [-a single] [-n //./root/cimv2] [-kf Name] -qf Name [-f Name<>FooBar LIKE %10] [-i C:] [-zsrv 127.0.0.1] [-zsnd /usr/bin/zabbix_sender]
                  [-zhost Windows 2019 Host] -atype userpass [-afile /etc/zabbix/wmi.cnf] [-dc-ip 192.168.56.1] [-rpc-auth [default]] [-user WMIUser1] [-pass *********] [-dom MyNTDoamin]

An implementation of Zabbix Agentless host for Windows.

optional arguments:
  -h, --help            show this help message and exit
  -v, --version         show program's version number and exit
  -c Win32_LogicalDisk, --wmi-class Win32_LogicalDisk
                        WMI class
  -lp /tmp/zabbix_wmi.log, --log-path /tmp/zabbix_wmi.log
                        Log file path (default: /tmp/zabbix_wmi.log
  -t 192.168.75.34, --target-host 192.168.75.34
                        Target host
  -a single, --action single
                        Action to perform Choices: ['single', 'multiple', 'json', 'discover', 'discover_n_multiple'] - (default: single)
  -n //./root/cimv2, --namespace //./root/cimv2
                        WMI Namespace (default: //./root/cimv2)
  -kf Name, --key-field Name
                        Key Fields to query, for multiple fields, separate by comma (default: Name)
  -qf Name, --query-fields Name
                        Fields to query, for multiple fields, separate by comma
  -f Name<>FooBar LIKE %10, --filter Name<>FooBar LIKE %10
                        WQL Style WHERE clause (default: )
  -i C:, --item C:      Item to query about. (default: )

Zabbix related parameters:
  -zsrv 127.0.0.1, --zabbix-server 127.0.0.1
                        Zabbix server address. (default: 127.0.0.1)
  -zsnd /usr/bin/zabbix_sender, --zabbix-sender /usr/bin/zabbix_sender
                        Zabbix Sender Path. (default: /usr/bin/zabbix_sender)
  -zhost Windows 2019 Host, --zabbix-hostname Windows 2019 Host
                        Registered name of Host in Zabbix. Required when using multiple or discover_n_multiple action.

Windows WMI Authentication Parameters:
  -atype userpass, --auth-type userpass
                        Select an authentication method. Choices: ['userpass', 'file'] - (default: userpass)
  -afile /etc/zabbix/wmi.cnf, --credentials-file-path /etc/zabbix/wmi.cnf
                        Credentials file path. Requiredonly if auth type file is selected.
  -dc-ip 192.168.56.1, --domain-controller-ip 192.168.56.1
                        IP Address of the domain controller. Use only if it was omitted in the domain part in target parameter.
  -rpc-auth [default], --rpc-authentication-level [default]
                        WMI RPC Authentication level. (default: default)
  -user WMIUser1, --wmi-username WMIUser1
                        Enter a Username login with. Required if 'userpass' method is selected.
  -pass *********, --wmi-password *********
                        Enter a Password to login with. Required if 'userpass' method is selected.
  -dom MyNTDoamin, --domain MyNTDoamin
                        Enter the Windows Domain. Required if 'userpass' method is selected.
```

## WMI Authentication

### Credentials stored in a file

For authentication via file to the Windows host, use '`-atype file`'.  
To specify a file path other than the default path, use '`-afile <ABSOLUTE_FILE_PATH>`'.  
Default location is '`/etc/zabbix/wmi.cnf`'.  
The format of the file is as follows,

```wmi.cnf
myWMIUser
myWMIPassword
myWMIDomain
```

The first line should contain the username, second line should contain the password and the third line should have the domain name.

### Credentials passed directly via arguments

For authentication via command-line arguments, use '`-atype userpass`' flag, and the following argument parameters must be set.  

- Username: '`-user <USERNAME>`' or '`--wmi-username <USERNAME>`'
- Password: '`-pass <PASSWORD`' or '`--wmi-password <PASSWORD>`'
- Domain: '`-dom <DOMAINNAME>`' or '`--domain <DOMAINNAME>`'

## Installation Instructions

- Step 1: Ensure WMI Queries are working on the target system.
- Step 2: Copy `'zabbix_wmi'` to the external scripts folder of the Zabbix server or proxy `(default location: /usr/lib/zabbix/externalscripts/)`.
- Step 3: Import the template provided in the repository into your Zabbix system via frontend.
- Step 4: Link the template to the agentless hosts.
- Step 5: Change the these macros `WMIC_USER`, `WMIC_PASS`, and `WMIC_DOMAIN` in the host.

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
