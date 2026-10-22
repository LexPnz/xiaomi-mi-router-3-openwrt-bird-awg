# Xiaomi Mi Router 3 — OpenWrt BIRD + WireGuard

Custom OpenWrt build for **Xiaomi Mi Router 3 (R3)**,
based on OpenWrt 25.12.6.

## Build information

- Device: Xiaomi Mi Router 3 (R3)
- Board: `xiaomi,miwifi-r3`
- Target: `ramips/mt7620`
- Architecture: `mipsel_24kc`
- OpenWrt version: 25.12.6
- Base source revision: see [`OPENWRT_COMMIT`](OPENWRT_COMMIT)
- CPU frequency: 600 MHz
- IPv6: disabled in this build configuration
- Routing: BIRD 3
- VPN: WireGuard tools and kernel support
- Web interface: LuCI with WireGuard protocol support

## Included components

The build configuration selects BIRD 3, WireGuard, LuCI WireGuard
protocol support, iperf3, htop, mtr, traceroute, and DNS diagnostic
utilities. The exact package selection is in [`config/.config`](config/.config).

BIRD is included with a template configuration. It is not preconfigured
with a local AS number or BGP peers.

WireGuard support and a `wg0` interface setup are included, but a usable
tunnel requires the user's own peer configuration and keys.

## Default settings and security warning

**Do not expose this firmware to an untrusted network without reviewing
its security settings.**

The current build sets the following defaults:

- Root password: `0123456789`
- Wi-Fi password: `0123456789`
- SSH access is allowed from LAN, WAN, and the WireGuard firewall zone.

These are intentionally documented to match the published build, but
they are not secure deployment settings. Change the root password and
Wi-Fi password immediately after installation. Restrict SSH from WAN
unless remote access is specifically required, and review firewall rules
before connecting the router to the Internet.

The default WireGuard setup does not provide a working tunnel by itself.
Do not publish private keys, preshared keys, or personal peer credentials.

## Repository layout

- `OPENWRT_COMMIT` — exact base OpenWrt Git revision
- `config/.config` — build configuration
- `patches/0001-mir3-customizations.patch` — modifications to tracked
  OpenWrt source files
- `overlay/` — additional source files to copy into the OpenWrt tree
- `sha256sums.txt` — firmware checksum information

## Reproducing the build

Use a Linux build environment supported by OpenWrt. Building OpenWrt
requires the standard host build dependencies documented by the OpenWrt
project.

1. Obtain the OpenWrt source tree.
2. Check out the exact revision recorded in `OPENWRT_COMMIT`.
3. Apply `patches/0001-mir3-customizations.patch`.
4. Copy the contents of `overlay/` into the root of the OpenWrt source tree.
5. Copy `config/.config` to the source tree as `.config`.
6. Run `make defconfig`, download the required sources, and build the image.

Example commands, starting in the directory where the source tree should
be created:

```sh
git clone https://git.openwrt.org/openwrt/openwrt.git openwrt
cd openwrt
git checkout "$(cat ../xiaomi-mi-router-3-openwrt-bird-wg/OPENWRT_COMMIT)"
git apply ../xiaomi-mi-router-3-openwrt-bird-wg/patches/0001-mir3-customizations.patch
cp -a ../xiaomi-mi-router-3-openwrt-bird-wg/overlay/. .
cp ../xiaomi-mi-router-3-openwrt-bird-wg/config/.config .config
make defconfig
make download
make -j"$(nproc)"
```

The example assumes the repository is located next to the new `openwrt`
directory. Adjust the paths if your directories differ.

A clean-tree build has not yet been independently verified. Confirm that
the patch applies and that the resulting image matches the intended
device before distributing it.

## Installation

Use the sysupgrade image only on the intended Xiaomi Mi Router 3 (R3)
device and verify the image checksum before flashing.

A firmware upgrade can render a device unusable if the wrong image is
selected or the flash process is interrupted. Keep a recovery method
available and back up any configuration you need.

## Firmware download

Prebuilt firmware images should be published under GitHub Releases,
rather than committed to the source repository. Each release should
identify the exact image filename and provide its SHA-256 checksum.

## Disclaimer

This is a community custom build, not an official Xiaomi firmware.
Use it at your own risk. Review the upstream OpenWrt licensing and notices
for the source code and included components.
