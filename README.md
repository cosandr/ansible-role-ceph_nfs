# ansible-role-ceph_nfs

![GitHub](https://img.shields.io/github/license/cosandr/ansible-role-ceph_nfs) ![GitHub last commit](https://img.shields.io/github/last-commit/cosandr/ansible-role-ceph_nfs) ![GitHub issues](https://img.shields.io/github/issues-raw/cosandr/ansible-role-ceph_nfs)

**Ansible role for setting up configuring NFS on Ceph.**

## Supported Platforms

| OS Family | Distribution  | Latest | Supported Version(s) | Comment |
|-----------|---------------|--------|----------------------|---------|
| RedHat    | RHEL          | :heavy_check_mark: | 9 | |
|           | RockyLinux    | :heavy_check_mark: | 8, 9 | |
|           | AlmaLinux     | :heavy_check_mark: | 8, 9 | |
|           | Fedora        | :heavy_check_mark: | 35, 36, 37 | |

## Requirements

Ansible 2.12 or higher. Tasks must run on Ceph host.

## Variables

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `ceph_nfs_clusters` | [] | Configures NFS clusters, see example playbook. |
| `ceph_nfs_exports` | [] | Configures NFS exports, see example playbook. |


## Dependencies

Ceph cluster.

## Example Playbook

```yaml
---

- hosts: all
  become: true
  gather_facts: true
  roles:
    - role: ceph_nfs
      vars:
        ceph_nfs_clusters:
          - name: "cluster"
            port: 2150
            count: 3
            config: |
              NFSV4 {
                  Allow_Numeric_Owners = true;
                  Only_Numeric_Owners = true;
              }
            ingress:
              - count: 2
                port: 2050
                monitor_port: 9050
                vip: "10.1.2.200/24"
        ceph_nfs_exports:
          - path: "/"
            cluster_id: "{{ ceph_nfs_clusters[0].name }}"
            pseudo: "/example"
            clients:
              - addresses:
                  - "10.1.2.3/32"
```

## Author

[Andrei Costescu](https://github.com/cosandr/)
