![OpenWrt logo](include/logo.png)

## OpenWrt with TROPIC01 Support

This fork of OpenWrt adds support for the **TROPIC01** secure element, enabling
secure storage and hardware-based cryptographic operations on OpenWrt-compatible
devices. The TROPIC01 is an auditable secure element / hardware root-of-trust
designed by Tropic Square that provides:

- Hardware-accelerated cryptographic operations
- True random number generation (TRNG)
- Secure data, credentials and keys storage
- Cryptographic-based identification and authentication of embedded devices

This fork includes the **libtropic** library and **lt-util** command-line utility,
allowing OpenWrt devices to leverage TROPIC01's secure element capabilities through
both SPI (via MikroBus on OpenWrt One) and USB dongle interfaces.

**Use cases:** Secure IoT deployments and supply chains protection, hardware-backed
data signing, secure boot and secure update, cryptographic keys management,
and applications requiring certified random number generation.

---

OpenWrt Project is a Linux operating system targeting embedded devices. Instead
of trying to create a single, static firmware, OpenWrt provides a fully
writable filesystem with package management. This frees you from the
application selection and configuration provided by the vendor and allows you
to customize the device through the use of packages to suit any application.
For developers, OpenWrt is the framework to build an application without having
to build a complete firmware around it; for users this means the ability for
full customization, to use the device in ways never envisioned.

Sunshine!

## Download

Built firmware images are available for many architectures and come with a
package selection to be used as WiFi home router. To quickly find a factory
image usable to migrate from a vendor stock firmware to OpenWrt, try the
*Firmware Selector*.

* [OpenWrt Firmware Selector](https://firmware-selector.openwrt.org/)

If your device is supported, please follow the **Info** link to see install
instructions or consult the support resources listed below.

## 

An advanced user may require additional or specific package. (Toolchain, SDK, ...) For everything else than simple firmware download, try the wiki download page:

* [OpenWrt Wiki Download](https://openwrt.org/downloads)

## Development

To build your own firmware you need a GNU/Linux, BSD or macOS system (case
sensitive filesystem required). Cygwin is unsupported because of the lack of a
case sensitive file system.

### Requirements

You need the following tools to compile OpenWrt, the package names vary between
distributions. A complete list with distribution specific packages is found in
the [Build System Setup](https://openwrt.org/docs/guide-developer/build-system/install-buildsystem)
documentation.

```
binutils bzip2 diff find flex gawk gcc-6+ getopt grep install libc-dev libz-dev
make4.1+ perl python3.7+ rsync subversion unzip which
```

### Quickstart

1. Run `./scripts/feeds update -a` to obtain all the latest package definitions
   defined in feeds.conf / feeds.conf.default

2. Run `./scripts/feeds install -a` to install symlinks for all obtained
   packages into package/feeds/

3. Run `make menuconfig` to select your preferred configuration for the
   toolchain, target system & firmware packages.

4. Run `make` to build your firmware. This will download all sources, build the
   cross-compile toolchain and then cross-compile the GNU/Linux kernel & all chosen
   applications for your target system.

### Enabling MicroE TROPIC01 Support for OpenWrt One

The OpenWrt One router supports the MicroE TROPIC01 board. To enable this support in your build:

1. Run `make menuconfig` to open the configuration menu

2. Navigate to **Target System** and select your appropriate target (e.g., MediaTek ARM)

3. Navigate to **Subtarget** and select **Filogic 8x0 (MT798x)**

4. Navigate to **Target Profile** and select **OpenWrt One**

5. Navigate to **Kernel modules** → **SPI Support** and enable:
   - `kmod-spi-dev` - User mode SPI device driver (required for TROPIC01 SPI communication)

6. (Optional) For USB dongle with TROPIC01, navigate to **Kernel modules** → **USB Support** and enable:
   - `kmod-usb-acm` - USB Abstract Control Model (CDC ACM) support for TROPIC01 USB dongle

7. Navigate to **Libraries** and enable:
   - `libtropic` - MicroE TROPIC01 library for secure element communication

8. Navigate to **Utilities** and enable:
   - `lt-util` - TROPIC01 utility for cryptographic operations and device management

9.  (Optional) For development and debugging, navigate to **Utilities** and enable:
   - `spidev-test` - SPI device testing utility

10. Save your configuration and exit

11. Build the firmware with `make`

The libtropic and lt-util packages provide userspace tools to interface with the
TROPIC01 secure element for cryptographic operations, key management, and random number
generation. The kmod-spi-dev module enables userspace access to the SPI bus for
communication with TROPIC01 via the MikroBus connector on OpenWrt One. For USB dongles,
the kmod-usb-acm module provides the necessary USB CDC ACM driver support.

### Related Repositories

The main repository uses multiple sub-repositories to manage packages of
different categories. All packages are installed via the OpenWrt package
manager called `opkg`. If you're looking to develop the web interface or port
packages to OpenWrt, please find the fitting repository below.

* [LuCI Web Interface](https://github.com/openwrt/luci): Modern and modular
  interface to control the device via a web browser.

* [OpenWrt Packages](https://github.com/openwrt/packages): Community repository
  of ported packages.

* [OpenWrt Routing](https://github.com/openwrt/routing): Packages specifically
  focused on (mesh) routing.

* [OpenWrt Video](https://github.com/openwrt/video): Packages specifically
  focused on display servers and clients (Xorg and Wayland).

## Support Information

For a list of supported devices see the [OpenWrt Hardware Database](https://openwrt.org/supported_devices)

### Documentation

* [Quick Start Guide](https://openwrt.org/docs/guide-quick-start/start)
* [User Guide](https://openwrt.org/docs/guide-user/start)
* [Developer Documentation](https://openwrt.org/docs/guide-developer/start)
* [Technical Reference](https://openwrt.org/docs/techref/start)

### Support Community

* [Forum](https://forum.openwrt.org): For usage, projects, discussions and hardware advise.
* [Support Chat](https://webchat.oftc.net/#openwrt): Channel `#openwrt` on **oftc.net**.

### Developer Community

* [Bug Reports](https://bugs.openwrt.org): Report bugs in OpenWrt
* [Dev Mailing List](https://lists.openwrt.org/mailman/listinfo/openwrt-devel): Send patches
* [Dev Chat](https://webchat.oftc.net/#openwrt-devel): Channel `#openwrt-devel` on **oftc.net**.

## License

OpenWrt is licensed under GPL-2.0
