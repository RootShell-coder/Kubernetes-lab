# Common Role

- install: `net-tools` `nfs-common` `tcpdump` `iptables` `iptables-persistent` `chrony` `htop` `sudo` `python3` `python3-pip`
- add ansible_ssh_user sudo
- disable ipv6 sshd, chrony
- configure nproc soft/hard limits, ntp client sudoers (NOPASSWD), and sysctl variable

## Role Variables

| Name                 | Description    | Default     | Required |
| -------------------- | -------------- | ----------- | :------: |
| security soft limits | soft nproc     | `128000`    |    X     |
| security hard limits | hard nproc     | `128000`    |    X     |
| ntp_server           | server ntp     | `ntp.ix.ru` |    X     |
| ntp_type             | server or pool | `server`    |    X     |
