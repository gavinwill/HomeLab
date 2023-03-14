# HomeLab
This repo is used by myself to deploy my home lab / home network. It can change quite often depending on my requirements and needs.
The requirements I have for my home lab are quite simple:

* Everyting should be made as simple as possible, but not simpler.
* Low energy and footprint (In all senses of the word).
* Cheap capital expenditure and operational expenditure.
* Automate where possible but not to the detriment of simplicity.
* Manual once off task(s) are potentially acceptable compromises for this situation.
* Stateless so can easily rebuild and not worry about lost data.
* KVM, Docker and Kubernetes Support.
* Vlan support.

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
