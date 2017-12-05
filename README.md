stefano-m.iptables
=========

Set up `iptables` rules and a `systemd` unit to ensure they are loaded at
boot. This role has been inspired by the Arch Linux `iptables` package.

At the moment, only `INPUT` rules can be configured.

The rules will always accept the following:

- input connections on the loopback interface
- ICMP echo requests (i.g. ping)
- incoming connections that are related and established

The rules default to the `ACCEPT` policy and always end with the following:

- reject all incoming TCP connections with TCP reset
- reject all incoming UDP connections with ICMP port unreachable
- reject everything else with ICMP protocol unreachable


Requirements
------------

`iptables` and `systemd` must be available on the remote hosts.


Role Variables
--------------

### Mandatory Variables

#### `iptables_directory`

The directory on the remote host where the iptables files will be copied,
e.g. `/etc/iptables`.


### Optional Variables

The below variables may be omitted.

#### `iptables_lan_cidr`

The LAN [CIDR block](https://en.wikipedia.org/wiki/CIDR) from which all
incoming connections will be accepted.

####  `iptables_accept_rules`

A list of dictionaries to specify accepted input rules. Each dictionary must
have the following keys:

- `protocol`: a network procotol, e.g. `tcp`
- `module`: an iptables module, e.g. `tcp`
- `dport`: a destination port, e.g. `22`

#### `iptables_free_form`

A free form list of iptables rules.


Dependencies
------------

No dependencies.

Example Playbook
----------------

``` yaml
- hosts: servers
  roles:
      - role: stefano-m.iptables
        iptables_dir: /etc/iptables
        iptables_accept_rules:
            - protocol: tcp
              module: tcp
              dport: 22
        iptables_lan_cidr: 10.0.0.0/16
```


License
-------

[Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0)
