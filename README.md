# ipset support for the R7000

Packages and kernel modules for ipset support.

## Introduction

`ipset` support is a somewhat challenging on DD-WRT because several key components for it are not built into the firmware. While DD-WRT can be extended by compiling userland tools and rolling your own firmware builds, this is a rather complex and overkill approach for adding additional packages. This repository is a collection of additional ipk packages and kernel modules for ipset support on DD-WRT.

**Note:** These packages and modules are provided as is with no warranty/support. They have been tested on a R7000 running DD-WRT firmware, other router models may vary. 

## Packages

The additional packages are mostly taken from the Entware-ng project compiled with a compatible ARM toolchain that works on DD-WRT as well. You will need several additional packages for `ipset`. They are:

* ipset (static binary)
* dnsmasq-full
* iptables

While DD-WRT comes with both `dnsmasq` and `iptables` already, the `dnsmasq` version is compiled without `ipset` support, in addition the iptables version is a custom 1.3.7 version which is too old for some ipset related sets.

Default compile time options with dnsmasq on DD-WRT:

```
Compile time options: IPv6 GNU-getopt no-RTC no-DBus no-i18n no-IDN DHCP DHCPv6 no-Lua no-TFTP no-conntrack no-ipset no-auth DNSSEC loop-detect no-inotify
```

`ipset` is compiled using the build system in Entware-ng (which use OpenWRT) but with DD-WRT kernel sources to be compatible.

## Kernel module

In order for ipset and iptables to work together the `xt_set.ko` kernel module is needed. Chances are, this will not be present in any DD-WRT build currently. This is compiled using the DD-WRT kerenel sources and matches the kernel branch of the R7000.

## Installation

All ipk packages need to be installed via `opkg` if you already use Entware-ng or similar, you can install the packages straight to /opt.

The kernel module can be placed within /jffs/usr/lib/modules and be loaded at boot time.

JFFS is better for kernel modules as its a storage partition available early in the boot process. Alternatively you can also use /opt but may have to delay executing code related to this module till a bit later on in the boot process, to ensure the USB device holding /opt is available.

### Example tutorials using ipset

Once you've got `ipset` support setup on DD-WRT, you'll now what to start using it. Here's a few tutorials on what you can do:

* [Basic ipset tutorial](https://www.dd-wrt.com/phpBB2/viewtopic.php?t=279586)

