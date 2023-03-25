# HomeLab

This repo is used by myself to deploy my home lab / home network. This is mainly a resource for myself however it may also assist other people looking for ideas for their home lab.

## Requirements

- Everything should be made as simple as possible, but not simpler.
- Low energy and footprint. Aiming for around ~20 watts during normal operation for networking and compute.
- Cheap capital expenditure and operational expenditure.
- Automate where possible but not to the detriment of simplicity and maintainability. See [XKCD: Automation](https://xkcd.com/1319/).
- Manual once off task(s) are potentially acceptable compromises. See [XKCD: Is It Worth the Time?](https://xkcd.com/1205/)
- Stateless so can easily rebuild and not worry about lost data.
- KVM, Docker and Kubernetes Support in Development environment.

## Limitations

- Everything running on a single machine for 'production' services
- Everything running on a machine with relative low resources for dev and prod.

Given the nature this is just for home internet access, local development and is stateless the limitations are deemed acceptable.

## Production Requirements

- Virtualised pfSense Router to supply internet access.
- Home Assistant VM to provide home automation.
- Unifi Controller for Unifi Access Points.

## Development Requirements

- Wake on Lan as not powered 24/7.
- Flexibility

### Hardware

- 1 'Production' × [Intel Nuc](https://ark.intel.com/content/www/us/en/ark/products/83256/intel-nuc-kit-nuc5i3ryk.html) `NUC5i3RYK` :

  - CPU: `Intel i3-5010U CPU @ 2.10GHz`
  - RAM: `8GB`
  - SSD: `120GB`
  - Cost: £100 (ebay)

- 1 × 'Development' [Gigabyte Brix](https://www.gigabyte.com/uk/Mini-PcBarebone/GB-BSCEA-3955-rev-10#ov) `GB-BSCEA-3955` :

  - CPU: `Intel(R) Celeron(R) CPU 3955U @ 2.00GHz`
  - RAM: `16GB`
  - SSD: `240GB`
  - Cost: £100 (ebay)

- [Unifi EdgeRouter X](https://store.ui.com/collections/operator-edgemax-routers/products/edgerouter-x) ` switch:

  - Ports: `5`
  - Speed: `1000Mbps`
  - Routing disabled and running as switch only

- [Unifi UAP-AC-Lite](https://eu.store.ui.com/products/unifi-ac-lite) Wifi Accesspoint

### Bare Metal Provisioning

With running everything from one box at the very start there is no storage or network environment setup that could be used for something like pxe boot to provision the bare metal host. In this instance, instead of say pxe booting with a dhcp server from my laptop, I decided on using USB memory sticks to provision an Ubuntu 22.04 LTS Bare Metal machine with cloud-init and autoinstall config. This enables me to get the Bare Metal machine "online" with a basic configuration for network connectivity and users setup with SSHs keys. At this point configuration and management of the box is passed onto other automation tools.
