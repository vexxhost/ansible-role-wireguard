# WireGuard

## Dependencies
This role does not take care of generating keys for you.  You should be able to
take care of that yourself.

### RHEL/CentOS
This role depends on the fact that you have the EPEL repositories installed on
your machine.  It does not manage that for you.

## Usage
This example below should be pretty self-explanitory.

```yaml
---
- hosts: vpn1.internal
  roles:
    - wireguard
  vars:
    wireguard_links:
      - name: wg0
        address: 10.0.0.1
        port: 51820
        private_key: eCpvWOe8zI0HCj/KjK3TZP71kd+glDxvDo5JaQhw3mw=
        post_up: iptables ...
        post_down: iptables ...
        peers:
          - public_key: UB9Lhk0JgwAPFD8F3k3Dq9iS7r/jLD+oYMX98T+fmGw=
            endpoint: vpn2.internal:51820
            allowed_ips: 10.0.0.2

- hosts: vpn2.internal
  roles:
      - wireguard
  vars:
    wireguard_links:
      - name: wg0
        address: 10.0.0.2
        port: 51820
        private_key: aIPcdRd6ncwRa+DJLaiq0Jmbvx1FjtLnWE2EApgcr2E=
        post_up: iptables ...
        post_down: iptables ...
        peers:
          - public_key: E/MU5hNb1mQ55ww0H0luxigNTXNNo/x49MRm5AcHLSI=
            endpoint: vpn1.internal:51820
            allowed_ips: 10.0.0.1
```