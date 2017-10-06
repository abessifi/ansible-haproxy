# Ansible HAProxy Role

Installs HAProxy on RedHat/CentOS GNU/Linux servers with high availability support.

**Note**: This role supports HAProxy versions `1.4` or `1.5`. Future versions may require some rework.

## Requirements

None.

## Dependencies

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

### HAProxy configuration

- **`haproxy_socket`**: The socket through which HAProxy can communicate for admin purposes or statistics (default: `/var/lib/haproxy/stats`).

- **`haproxy_chroot`**: The jail directory where chroot() will be performed before dropping privileges (default: `/var/lib/haproxy`).

- Default user and group under which HAProxy should run:

```
    haproxy_user: haproxy
    haproxy_group: haproxy
```

- **`haproxy_enable_stats`**: HAProxy stats is disabled by default. To enable stats, set `haproxy_enable_stats` to `true` (default: `false`).

- **`haproxy_stats_username / haproxy_stats_password`**: Stats access would be protected by login/pass. Set the role parameters to enforce authentication (default: empty).

- Default HAProxy frontend configuration directives:

```
  haproxy_frontend_name: 'hafrontend'
  haproxy_frontend_bind_address: '*'
  haproxy_frontend_port: 80
  haproxy_frontend_mode: 'http'
```

- Default HAProxy backend configuration directives:

```
  haproxy_backend_name: 'habackend'
  haproxy_backend_mode: 'http'
  haproxy_backend_balance_method: 'roundrobin'
  haproxy_backend_httpchk: 'HEAD / HTTP/1.1\r\nHost:localhost'
```

- A list of backend servers (name and address) to which HAProxy will distribute requests (default: `[]`). Example:

```
  haproxy_backend_servers:
    - name: srv-1
      address: 192.168.0.1:80
    - name: srv-2
      address: 192.168.0.2:80
```

- A list of extra global variables to add to the global configuration section inside `haproxy.cfg` (default: `[]`). Example:

```
  haproxy_extra_global_vars:
    - 'ssl-default-bind-ciphers ABCD+KLMJ:...'
    - 'ssl-default-bind-options no-sslv3'
```

### Keepalived configuration

- **`haproxy_enable_ha`**: Configure HA for HAProxy instances using Keepalived (default: `false`).

- **`haproxy_keepalived_bind_interface`**: The interface to monitor by Keepalived (default: `eth0`).

- **`haproxy_keepalived_vip`**: The virtual IP address to configure by Keepalived (default: empty).

- **`haproxy_keepalived_vrrp_name`**: A unique, by NIC, virtual instance name to configure by Keepalived (default: `VI_1`)

- **`haproxy_keepalived_instance_state`**: The virtual instance type. Could be "MASTER|BACKUP|FAULT" (default: `MASTER`).

- **`haproxy_keepalived_instance_priority`**: The instance priority. For backup instances this __MUST__ be lower that the priority value of the master instance (default of master: `200`).

## Usage

```yaml

    - hosts: balancer
      become: yes
      roles:
        - role: ansible-haproxy
```

## License

MIT

## Author Information

This role was created by [Ahmed Bessifi](https://www.linkedin.com/in/abessifi), a DevOps enthusiast.
