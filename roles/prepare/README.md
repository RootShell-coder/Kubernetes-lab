# Prepare Role

disable swap, edit hosts and hostname

## Role Variables

| Name                   | Description                                     | Default                                                                     | Required |
| ---------------------- | ----------------------------------------------- | --------------------------------------------------------------------------- | :------: |
| variable hosts_entries | [ip hostname+domain hostname] lists dict        | `- { ip: '192.168.0.2', hostname: 'k8s0-master', domain: 'example.local' }` |    X     |
|                        |                                                 | `- { ip: '192.168.0.3', hostname: 'k8s1-master', domain: 'example.local' }` |    X     |
|                        |                                                 | `- { ip: '192.168.0.4', hostname: 'k8s2-master', domain: 'example.local' }` |    X     |
|                        |                                                 | `- { ip: '192.168.0.5', hostname: 'k8s3-node', domain: 'example.local' }`   |    X     |
|                        |                                                 | `- { ip: '192.168.0.6', hostname: 'k8s4-node', domain: 'example.local' }`   |    X     |
|                        |                                                 | `- { ip: '192.168.0.7', hostname: 'k8s5-node', domain: 'example.local' }`   |    X     |
| backup_hosts_file      | backup file hosts.uuuu.YYYY-MM-DD@HH:MM:SS~     | `true`                                                                      |    X     |
| backup_hostsname_file  | backup file hostsname.uuuu.YYYY-MM-DD@HH:MM:SS~ | `true`                                                                      |    X     |

`cat /etc/hosts`

```ini
# Ansible managed

127.0.0.1     localhost

192.168.0.2   k8s0-master.example.local  k8s0
192.168.0.3   k8s1-master.example.local  k8s1
192.168.0.4   k8s2-master.example.local  k8s2
192.168.0.5   k8s3-node.example.local  k8s3
192.168.0.6   k8s4-node.example.local  k8s4
192.168.0.7   k8s5-node.example.local  k8s5

# The following lines are desirable for IPv6 capable hosts
# ::1     localhost ip6-localhost ip6-loopback
# ff02::1 ip6-allnodes
# ff02::2 ip6-allrouters
```
