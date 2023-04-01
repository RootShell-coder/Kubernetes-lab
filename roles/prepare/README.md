# Prepare Role

## Role Variables

| Name                     | Description                                 | Default                                                                   | Required |
| ------------------------ | ------------------------------------------- | ------------------------------------------------------------------------- | :------: |
| `variable hosts_entries` | [ip hostname+domain hostname]               |                                                                           |          |
|                          |                                             | `- { ip: '192.168.0.2', hostname: 'k8s0-master', domain: 'example.com' }` |    X     |
| `listв dict`             | file /etc/hosts                             | `- { ip: '192.168.0.3', hostname: 'k8s1-master', domain: 'example.com' }` |    X     |
|                          |                                             | `- { ip: '192.168.0.4', hostname: 'k8s2-master', domain: 'example.com' }` |    X     |
|                          |                                             | `- { ip: '192.168.0.5', hostname: 'k8s3-node', domain: 'example.com' }`   |    X     |
|                          |                                             | `- { ip: '192.168.0.6', hostname: 'k8s4-node', domain: 'example.com' }`   |    X     |
|                          |                                             | `- { ip: '192.168.0.7', hostname: 'k8s5-node', domain: 'example.com' }`   |    X     |
| backup_hosts_file        | backup file hosts.uuuu.YYYY-MM-DD@HH:MM:SS~ | `true`                                                                    |    X     |

`cat /etc/hosts`

```ini
# Ansible managed

127.0.0.1     localhost
    {% for i in ansible_play_batch %}
      - "host_{{loop.index}}:{{ hostvars[i]['ansible_facts']['default_ipv4']['address'] }}"
    {% endfor %}
192.168.0.2   k8s0-master.example.com  k8s0
192.168.0.3   k8s1-master.example.com  k8s1
192.168.0.4   k8s2-master.example.com  k8s2
192.168.0.5   k8s3-node.example.com  k8s3
192.168.0.6   k8s4-node.example.com  k8s4
192.168.0.7   k8s5-node.example.com  k8s5

# The following lines are desirable for IPv6 capable hosts
# ::1     localhost ip6-localhost ip6-loopback
# ff02::1 ip6-allnodes
# ff02::2 ip6-allrouters
```
