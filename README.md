[English](README.md) | [Русский](README.ru.md)
# Xiaomi Mi Router 3 — Custom OpenWrt with BIRD, AmneziaWG and 600 MHz CPU

Custom OpenWrt firmware for the **Xiaomi Mi Router 3 (MiWiFi R3)**, based on **OpenWrt 25.12.6**.

The build includes BIRD for BGP routing, AmneziaWG (WireGuard) support, diagnostic tools, and a CPU frequency configuration targeting 600 MHz.

## Features

- **OpenWrt 25.12.6**
- **MediaTek MT7620A** platform
- CPU frequency configured for **600 MHz**
- **BIRD 3** for BGP routing
- **AmneziaWG (WireGuard)** support
- LuCI web interface
- `iperf3` for network throughput testing
- `htop` for process and resource monitoring
- `mtr`, `traceroute`, and DNS diagnostic utilities
- IPv6 disabled; IPv4-only configuration
- Software flow offloading enabled in the firewall
- Custom default network and wireless settings
- GitHub Actions workflow for automated firmware builds

## Included components

| Component | Purpose |
|---|---|
| BIRD 3 | BGP routing |
| AmneziaWG | VPN tunnel support |
| LuCI | Web administration interface |
| iperf3 | Network throughput testing (running as a service) |
| htop | Process and resource monitoring |
| curl | Transfer data and make HTTP/HTTPS requests from the command line |
| wget-ssl | Download files over HTTP/HTTPS with SSL/TLS support |
| mtr | Network path diagnostics |
| traceroute | Route tracing |
| host, dig | DNS troubleshooting |

## Default configuration

### Network

- Router hostname: `MIR3`
- LAN address: `172.16.1.1/24`
- LAN DHCP pool: `172.16.1.128`–`172.16.1.191`
- Local DNS domain: `mir3`
- WAN interface: obtains its IP address and network settings via DHCP from the ISP.

### Wireless

- Default SSID: `Mir3-2.4GHz/Mir3-5GHz`
- Default Wi-Fi password: `0123456789`
- To increase the permitted transmit power, the country code `PA` is used, with transmit power set to `21` for `radio0` (5 GHz) and `36` for `radio1` (2.4 GHz).

### Access

- Default root password: `0123456789`
- SSH port: `22`
- SSH access is allowed from LAN, WAN, and the AmneziaWG firewall zone.

**Security note:** These are default credentials intended for this custom build. Consider the security implications before exposing the router to untrusted networks.

## VPN over BGP

Documentation for configuring BIRD 3 with AmneziaWG 3.1 and routing VPN traffic over BGP:

- [English documentation](overlay/files/root/README-VPN-OVER-BGP.md)
- [Документация на русском](overlay/files/root/README-VPN-OVER-BGP.ru.md)

## BIRD and BGP

BIRD is included with a template configuration. It is not preconfigured with a local AS number or BGP peers.

Configure the local AS number, router ID, peer addresses, remote AS numbers, and routing policies for your own network before relying on BGP routes.

## AmneziaWG

AmneziaWG tools and LuCI protocol support are included.

The build initializes the `awg0` interface, but a working tunnel requires your own AmneziaWG private key, peer public key, endpoint, allowed IP ranges, and any required routing and firewall settings.

Do not publish private keys or other secrets in the repository.

## Repository structure

```text
.
├── .github/
│   └── workflows/
│       └── build.yml
├── config/
│   └── .config
├── overlay/
│   ├── files/
│   └── target/
├── patches/
│   └── 0001-mir3-customizations.patch
├── OPENWRT_COMMIT
├── README.md
└── sha256sums.txt
```

- `config/.config` — OpenWrt build configuration.
- `patches/` — patches applied to the OpenWrt source tree.
- `overlay/` — custom files copied into the OpenWrt source tree during the build.
- `OPENWRT_COMMIT` — the exact OpenWrt source revision used by the build.
- `.github/workflows/build.yml` — GitHub Actions workflow for building the firmware.
- `sha256sums.txt` — checksum file, when generated or maintained for the published firmware artifacts.

## Building the firmware

The repository uses GitHub Actions to automate the build.

1. Push the repository to GitHub.
2. Open the repository on GitHub.
3. Select the **Actions** tab.
4. Open the firmware build workflow.
5. Start the workflow manually if `workflow_dispatch` is enabled, or push a change to a path monitored by the workflow.
6. Wait for the workflow to finish.
7. Open the completed workflow run and download the firmware artifacts, if the workflow publishes them.

The workflow checks out the pinned OpenWrt revision, applies the project's patch and overlay, installs package feeds, loads the build configuration, and compiles the firmware.

For local builds, follow the same sequence used in `.github/workflows/build.yml`. Do not assume that a generic OpenWrt build procedure applies to this repository without its project-specific patch and overlay steps.

## Installing the firmware

Before flashing, verify that the image is intended for the **Xiaomi Mi Router 3 (MiWiFi R3)** and follow the installation method supported by your current firmware and recovery setup.

- Keep a copy of the current configuration if you need it.
- Verify the downloaded image and its checksum when a checksum is provided.
- Do not interrupt power during flashing.
- Have a recovery procedure available before upgrading.

Flashing the wrong image or interrupting the process can make the router unbootable.

## Firmware artifacts and checksums

When a build workflow publishes firmware files, download them from the corresponding GitHub Actions run or release.

If `sha256sums.txt` is provided, verify the downloaded firmware against it before flashing.

## Disclaimer

This project is provided **as is**, without any warranty. You are responsible for verifying the firmware image, understanding the configuration, and ensuring that it is suitable for your device and network.

Use at your own risk.