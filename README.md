ansible-systemd-resolved role
=========

This role manages configuration of systemd-resolved.

It creates directory `/etc/systemd/resolved.conf.d/` and places new configuration file, instead of overriding the default file `/etc/systemd/resolved.conf`. This is recommended by systemd-resolved documentation. It helps to track changes and custom settings. If needed, system default settings can be restored by removing all content of the *resolved.conf.d* directory.

Name of the file can be specified by the `systemd_resolved.config_file` variable. There can be more config files.

**There are no default variables (except config file name). If you run this role without defining any variable, config file called `ansible_config.conf` will be created but will have no effect on systemd-resolved settings. It is OK to use any subset of variables, e.g. if you use only `systemd_resolved.dnssec` the rest will be automatically "inhrited" from system default settings.**

Full documentation for the reolved.conf file can be found in [systemd documentation](https://www.freedesktop.org/software/systemd/man/resolved.conf.html).

You can always check systemd-resolved settings [using resolvectl](https://devopsadvocate.com/posts/2023/using-resolvectl-for-dns-debugging-querying/) utility.


Requirements
------------
None


Role Variables
--------------

| Variable          | Type | Possible values | Comments |
|-------------------|----------|---------|----------|
| systemd_resolved.config_file | String | File name | |
| systemd_resolved.dns | List, String | *One IP/List of IP addresses* | example: `[1.1.1.1, 8.8.8.8]` |
| systemd_resolved.fallback_dns | List, String | *One IP/List of IP addresses* | example: `[1.1.1.1, 8.8.8.8]` |
| systemd_resolved.domains | List, String | *One domain/List of domains* | |
| systemd_resolved.dnssec | Bool, String | `true`, `false`, `allow-downgrade` | |
| systemd_resolved.dns_over_tls | Bool, String | `true`, `false`, `opportunistic` | |
| systemd_resolved.multicast_dns | Bool, String | `true`, `false`, `resolve` | |
| systemd_resolved.llmnr | Bool, String | `true`, `false`, `resolve` | |
| systemd_resolved.cache | Bool, String | `true`, `false`, `no-negative` | |
| systemd_resolved.cache_from_localhost | Bool | `true`, `false` | |
| systemd_resolved.dns_stub_listener | Bool, String | `true`, `false`, `udp`, `tcp` | |
| systemd_resolved.dns_stub_listener_extra | String | IP address | |
| systemd_resolved.read_etc_hosts | Bool | `true`, `false` | |
| systemd_resolved.resolve_unicast | Bool | `true`, `false` | |
| systemd_resolved.stale_retention | Integer| Integer number | |




Example:

```
systemd_resolved:
  config_file: ansible_config.conf
  dns:
  - 8.8.8.8
  - 1.1.1.1
  fallback_dns:
  - 1.0.0.1
  dnssec: false
```


Dependencies
------------
None

Example Playbook
----------------

```yaml
- name: Manage systemd-resolved
  hosts: hosts
  vars:
    systemd_resolved:
      config_file: ansible_config.conf
      dns:
      - 8.8.8.8
      - 1.1.1.1
      fallback_dns:
      - 1.0.0.1
      dnssec: false
  roles:
    - { role: berkas1.ansible_systemd_resolved }
```
