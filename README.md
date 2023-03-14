# HomeLab
This repo is used by myself to deploy my home lab / home network. This is mainly a resource for myself however it may also assist other people looking at a very simple home lab setup.

## Requirements

* Everything should be made as simple as possible, but not simpler.
* Low energy and footprint. Aiming for around ~25 watts during normal operation for networking and compute.
* Cheap capital expenditure and operational expenditure.
* Automate where possible but not to the detriment of simplicity and maintainability.
* Manual once off task(s) are potentially acceptable compromises for this situation.
* Stateless so can easily rebuild and not worry about lost data.
* KVM, Docker and Kubernetes Support.
* Vlan support.

## Limitations

* Everything running on a single machine
* Everything running on a machine with relative low resources

Given the nature this is just for home internet access, local development and is stateless the limitations are deemed acceptable

### Hardware

- 1 × [Gigabyte Brix](https://www.gigabyte.com/uk/Mini-PcBarebone/GB-BSCEA-3955-rev-10#ov) `GB-BSCEA-3955` :
    - CPU: `Intel(R) Celeron(R) CPU 3955U @ 2.00GHz`
    - RAM: `16GB`
    - SSD: `240GB`
- [Unifi EdgeRouter X](https://store.ui.com/collections/operator-edgemax-routers/products/edgerouter-x) ` switch:
    - Ports: `5`
    - Speed: `1000Mbps`
    - Routing disabled and running as switch only
- [Unifi UAP-AC-Lite](https://eu.store.ui.com/products/unifi-ac-lite) Wifi Accesspoint
- UniFi Stand-Alone Cloud Key Controller (Gen 1)
    - Potential to be virtualised in future

### Bare Metal Provisioning

With running everything from one box at the very start there is no storage or network environment setup that could be used for something like pxe boot to provision the bare metal host. In this instance, instead of say pxe booting with a dhcp server from my laptop, I decided on using USB memory sticks to provision an Ubuntu 22.04 LTS Bare Metal machine with cloud-init and autoinstall config. This enables me to get the Bare Metal machine "online" with a basic configuration for network connectivity and users setup with SSHs keys. At this point configuration and management of the box is passed onto other automation tools.
