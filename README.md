# Ansible Role: Iptables

[![Build Status](https://travis-ci.org/cguillerminet/ansible-role-iptables.svg?branch=master)](https://travis-ci.org/cguillerminet/ansible-role-iptables)

Installs a simple iptables-based firewall for RHEL/CentOS or Debian/Ubunty systems. It uses iptables-save format to define rules.

## Requirements

None.

## Role Variables

For a complete list of variables, see `default/main.yml`.

The three main variables are:

    iptables_mangle_rules:
      - comment: "Prioritize packets to begin tcp connections (those with SYN flag set)"
        rules:
          - "-I PREROUTING -p tcp -m tcp --tcp-flags SYN,RST,ACK SYN -j MARK --set-mark 0x1"
          - "-I PREROUTING -p tcp -m tcp --tcp-flags SYN,RST,ACK SYN -j RETURN"
    iptables_nat_rules:
      - comment: "Change destination address of web traffic to localhost"
        rules:
          - "-A PREROUTING -p tcp --dport 80 -i eth0 -j DNAT --to 127.0.0.1"
    iptables_filter_rules:
      - comment: "Open http port"
        rules:
          - "-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT"

They are used to inject rules into the tables.

## Dependencies

None.

## Example Playbook

## License

MIT / BSD

## Author Information

This role was created in 2016 by [ALG](https://www.attestationlegale.fr), based on a fork of [ansible-role-firewall](https://github.com/geerlingguy/ansible-role-firewall) by [Jeff Geerling](http://jeffgeerling.com/).
