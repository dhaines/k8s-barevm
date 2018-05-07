How to use:

In group\_vars/all, assign values thusly:

```yaml
domain: k8s.example.com
repository_host: repocache.example.com

networks:
  aio:
    cidr: 10.0.1.0/24
    gateway: 10.0.1.1
    start_index: 4
  node:
    cidr: 10.0.2.0/24
    gateway: 10.0.2.1
    start_index: 4
  nfs:
    cidr: 10.0.3.0/24
    mtu: 9000
    start_index: 4
  iscsia:
    cidr: 10.0.4.0/24
    mtu: 9000
    start_index: 4
  iscsib:
    cidr: 10.0.5.0/24
    mtu: 9000
    start_index: 4
```

'start\_index' is to account for existing infrastructure

'repository\_host' is an HTTP-accessible host with kickstarts available in /bootstrap and CentOS, Fedora, and EPEL mirrors available at /centos /fedora and /epel, accordingly (look at Trebuchet to see an Nginx-based caching proxy for mirrors.mit.edu that fits this spec.).

In host\_vars/aio.yml, assign values thusly:

```yaml
network_index: 0

host_networks:
  - network: aio
    device: enp6s0
    bridged: False
    allocate_ip: True
  - network: node
    device: enp7s0
    bridged: True
    allocate_ip: False
  - network: nfs
    device: enp9s0
    bridged: True
    allocate_ip: False
  - network: iscsia
    device: enp9s0.36
    bridged: True
    allocate_ip: False
    vlan: True
  - network: iscsib
    device: enp9s0.37
    bridged: True
    allocate_ip: False
    vlan: True
```

Put an aio in an inventory file (using ansible\_host as needed).

And do an `ansible-playbook -i inventory site.yml`
