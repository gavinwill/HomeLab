# HomeLab

This repo is used by myself to deploy my home lab / home network. This is mainly a resource for myself however it may also assist other people looking for ideas for their home lab.

## Requirements

- Everything should be made as simple as possible, but not simpler.
- Low energy and footprint. Aiming for around ~20 watts during normal operation for networking and compute. Development server should be around ~15 watts.
- Cheap capital expenditure and operational expenditure.
- Automate where possible but not to the detriment of simplicity and maintainability. See [XKCD: Automation](https://xkcd.com/1319/).
- Manual once off task(s) are potentially acceptable compromises. See [XKCD: Is It Worth the Time?](https://xkcd.com/1205/)
- Stateless so can easily rebuild and not worry about lost data.
- KVM, Docker and Kubernetes Support.
- Where possible or applicable configure HA between the prod and dev node for services.

## Limitations

- Everything running on a machine with relative low resources for dev and prod.

Given the nature this is just for home internet access, local development and is stateless the limitations are deemed acceptable.

## Production Server Requirements

- Virtualised pfSense Router to supply internet access (in HA Pair with CARP).
- Home Assistant VM to provide basic home automation.
- Unifi Controller for configuration of Unifi Access Points.

## Development Server Requirements

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

- [Unifi EdgeRouter X](https://store.ui.com/collections/operator-edgemax-routers/products/edgerouter-x) switch:

  - Ports: `5`
  - Speed: `1000Mbps`
  - Routing disabled and running as switch with vlans only

- [Unifi UAP-AC-Lite](https://eu.store.ui.com/products/unifi-ac-lite) Wifi Accesspoint

### Bare Metal Provisioning

With running everything from one box at the very start there is no storage or network environment setup that could be used for something like pxe boot to provision the bare metal host. In this instance I decided on using 2 x USB memory sticks to provision an Ubuntu 22.04 LTS Bare Metal machine with cloud-init and autoinstall config. This enables me to get the Bare Metal machine "online" with a basic configuration for network connectivity along with users setup and configured with SSHs keys.

The basics of the install are:

- Create a customised live-server ISO with a modified grub.cfg to allow autoinstall

- Rebuild the ISO with [livefs-editor](https://github.com/mwhudson/livefs-editor) to include the customised grub.cfg

```
wget "https://releases.ubuntu.com/22.04/ubuntu-22.04.1-live-server-amd64.iso"
export ORIG_ISO="ubuntu-22.04.1-live-server-amd64.iso"
mount -o loop $ORIG_ISO mnt
cp --no-preserve=all mnt/boot/grub/grub.cfg /tmp/grub.cfg
sed -i 's/linux \/casper\/vmlinuz ---/linux \/casper\/vmlinuz autoinstall quiet ---/g' /tmp/grub.cfg
sed -i 's/timeout=30/timeout=1/g' /tmp/grub.cfg
export MODDED_ISO="${ORIG_ISO::-4}-modded.iso"
livefs-edit ../$ORIG_ISO ../$MODDED_ISO --cp /tmp/grub.cfg new/iso/boot/grub/grub.cfg
```

- Copy to the ISO using dd, etcher or imaging software of choice to a USB stick.

- Format a second USB stick as FAT32 and label the volume CIDATA. This is used for cloud-init config

- Create an empty file called meta-data (cloud init will not work if meta-data is missing. An empty file meets the requirement) on the cloud-init USB stick.

- create a file called user-data and populate it with options you require and save to the cloud-init USB stick. [The official documentation has a few examples that can be used as reference.](https://ubuntu.com/server/docs/install/autoinstall)

Now its time to install onto baremetal

- Boot from the USB (wait for cloud-init to trigger autoinstall)
- After it shuts off, unplug the USB
- Boot again (wait for the second cloud-init to trigger host first-run provisioning)
- After it shuts off, provisioning is complete
- Boot for the last time

At this point further configuration is passed to ansible. A quick test to confirm connectivity is all good.

```
ansible all -m ping
```
