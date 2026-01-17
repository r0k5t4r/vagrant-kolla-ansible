# OpenStack Lab on VMware ESXi with Vagrant

This repository provides a **multi-node OpenStack lab environment** deployed via **Vagrant on VMware ESXi**.
Virtual machines are cloned from a **CentOS Stream 8 template** and prepared for use as seed, control, and compute nodes.

This setup is intended for **testing, development, automation, and learning OpenStack internals**.
It is **not intended for production use**.

---

## 🧱 Architecture Overview

```
                       Management / Public Network
                           192.168.2.0/24
 ┌──────────────────────────────────────────────────────────┐
 │                                                          │
 │   seed        control01   control02   control03          │
 │   .210        .211        .212        .213               │
 │                                                          │
 │   compute01   compute02                                 │
 │   .214        .215                                     │
 │                                                          │
 └──────────────────────────────────────────────────────────┘
              │
              │
              ▼
        Private / Internal Network
           192.168.45.0/24
```

### Node Roles

| Hostname   | Role              | Nested Virt |
|------------|-------------------|-------------|
| seed       | Deployment / Ansible | No |
| control01  | OpenStack Control   | No |
| control02  | OpenStack Control   | No |
| control03  | OpenStack Control   | No |
| compute01  | OpenStack Compute   | Yes |
| compute02  | OpenStack Compute   | Yes |

---

## 📦 Requirements

### Software
- Vagrant ≥ 2.x
- Ruby
- VMware ESXi
- `vagrant-vmware-esxi` plugin
- `vagrant-hostmanager` plugin

Install required plugins:
```bash
vagrant plugin install vagrant-vmware-esxi
vagrant plugin install vagrant-hostmanager
```

If you encounter SSH permission issues:
```bash
export VAGRANT_PREFER_SYSTEM_BIN=0
```

---

## 🖥️ ESXi Prerequisites

### ESXi Host
- Address: `192.168.2.10`
- SSH enabled

### Template VM
A **pre-existing CentOS Stream 8 template** is required:

| Setting              | Value |
|----------------------|-------|
| Template name        | `template-centos8stream` |
| OS                   | CentOS Stream 8 |
| SSH enabled          | Yes |
| Network              | VM Network |
| Disk                 | Single system disk |

### ESXi Configuration Used
- Datastore: `truenas_nvme_01`
- Network: `VM Network`
- NIC type: `vmxnet3`
- Resource pool: `/`

Credentials are read securely using:
```
esxi.esxi_password = 'file:'
```

---

## ⚙️ Node Configuration

Nodes are defined in the `nodes` array in the `Vagrantfile`.

Example:
```ruby
{
  :hostname => 'compute01',
  :ip => '192.168.45.214',
  :public_ip => '192.168.2.214',
  :clone_from => 'template-centos8stream',
  :cpus => 2,
  :ram => 4096,
  :hv => 'yes',
  :esxi => 'yes'
}
```

### Supported Parameters

| Parameter     | Description |
|--------------|-------------|
| `hostname`   | VM hostname |
| `ip`         | Internal / private IP |
| `public_ip`  | Public / management IP |
| `clone_from` | ESXi template name |
| `cpus`       | Number of vCPUs |
| `ram`        | Memory in MB |
| `hv`         | Enable nested virtualization |
| `esxi`       | Must be `yes` |

---

## 🌐 Networking

Each VM uses **three NICs**:

1. Auto-assigned network (Vagrant / ESXi)
2. Management / Public network (`192.168.2.0/24`)
3. Internal / Private network (`192.168.45.0/24`)

This layout matches common **OpenStack reference architectures**.

---

## 🧠 Provisioning

This Vagrantfile focuses on **infrastructure deployment only**.

Optional provisioning hooks (currently commented):
- Root SSH access setup
- System updates

Provisioning is typically handled later via:
- Ansible
- Kolla-Ansible
- Manual bootstrap from `seed` node

---

## 🌍 Hostname Resolution

The setup uses **vagrant-hostmanager**:

- Guest `/etc/hosts` is automatically updated
- Host system is not modified
- IP resolution happens dynamically via SSH

---

## 🚀 Usage

Start the full lab:
```bash
vagrant up
```

Start a single node:
```bash
vagrant up compute01
```

SSH into seed node:
```bash
vagrant ssh seed
```

Destroy everything:
```bash
vagrant destroy -f
```

---

## ✅ Post-Deployment Checks

Verify networking:
```bash
ip a
ip route
```

Verify nested virtualization (compute nodes):
```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

Should return a value > 0.

---

## ⚠️ Notes & Warnings

- Lab environment only
- Nested virtualization enabled on compute nodes
- Static IPs must be unused
- No security hardening applied
- No backups configured

---

## 🎯 Intended Use

- OpenStack testing & learning
- Kolla-Ansible deployments
- Network and compute experimentation
- ESXi-based lab automation

---

Happy hacking ☁️🚀
