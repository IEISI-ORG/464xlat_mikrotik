# Who is affected

## Networks that already run IPv6-only with 464XLAT

On these networks, a MikroTik router gets an IPv6-only connection. Phones and recent laptops bring their own CLAT. Devices behind the router that only speak IPv4 get nothing, because RouterOS has no CLAT ([requirements C1](requirements.md#clat)).

| Network | What it does | Source |
|---|---|---|
| T-Mobile US (mobile) | IPv6-only with 464XLAT since 2014 | [Internet Society case study, 2014](https://www.internetsociety.org/deploy360/2014/case-study-t-mobile-us-goes-ipv6-only-using-464xlat/) |
| Telstra (mobile, Australia) | IPv6-only with 464XLAT, DNS64 and NAT64 since February 2020 | [SIDN, 2020](https://www.sidn.nl/en/news-and-blogs/australias-telstra-switches-mobile-users-to-ipv6-only) |
| Deutsche Telekom (mobile, Germany) | 5G standalone requires the IPv6-only APN, with DNS64 and NAT64 in the network | [Forum post 24, Jan 2026](https://forum.mikrotik.com/t/feature-request-464xlat/263487/24) |
| EE (mobile, UK) | EE's community manager: "Our mobile network uses IPv6, and 464XLAT for IPv4 traffic" | [EE Community](https://community.ee.co.uk/t5/Mobile-Broadband/IPv6-on-EE-Mobile-not-landline/td-p/1361112) |

Reports from MikroTik users on these networks:

- A Chateau 5G R17 on T-Mobile US gives no IPv4 to an average user. Its owner gets by with an L2TP VPN ([post 32](https://forum.mikrotik.com/t/feature-request-464xlat/263487/32), [post 39](https://forum.mikrotik.com/t/feature-request-464xlat/263487/39)).
- Behind a Chateau 5G R17 ax on Deutsche Telekom 5G SA, IPv4-only devices get no IPv4. Its owner tunnels IPv4 over WireGuard to a rented server ([post 24](https://forum.mikrotik.com/t/feature-request-464xlat/263487/24)).

## MikroTik hardware and the container workaround

The only way to run a CLAT on RouterOS today is a third-party container, such as [antongrizli/tayga](https://github.com/antongrizli/tayga). The [Container page](https://help.mikrotik.com/docs/display/ROS/Container) says the container package "is compatible with arm, arm64 and x86 architectures". Turning it on needs a device-mode change, confirmed by pressing the reset button. The page recommends an external disk.

MIPS and TILE devices cannot run containers at all. A native CLAT in RouterOS would reach them, because RouterOS is one operating system across the range.

Architecture and CPU are from each product page on mikrotik.com, read on 8 November 2026.

| Model | Architecture | CPU | Containers | Source |
|---|---|---|---|---|
| hEX (RB750Gr3) | MMIPS | MT7621A | no | [product page](https://mikrotik.com/product/RB750Gr3) |
| hEX S | MMIPS | MT7621A | no | [product page](https://mikrotik.com/product/hex_s) |
| hAP lite (RB941-2nD) | SMIPS | QCA9533 | no | [product page](https://mikrotik.com/product/RB941-2nD) |
| hAP ac lite (RB952Ui-5ac2nD) | MIPSBE | QCA9531 | no | [product page](https://mikrotik.com/product/RB952Ui-5ac2nD) |
| LtAP LTE6 kit | MMIPS | MT7621A | no | [product page](https://mikrotik.com/product/ltap_lte6_kit) |
| LtAP mini LTE kit | MIPSBE | QCA9531 | no | [product page](https://mikrotik.com/product/ltap_mini_lte_kit) |
| wAP LTE kit | MIPSBE | QCA9531 | no | [product page](https://mikrotik.com/product/wap_lte_kit) |
| CCR1036-8G-2S+EM | TILE | TLR4-03680 | no | [product page](https://mikrotik.com/product/CCR1036-8G-2SplusEM) |
| hEX refresh (E50UG) | ARM 32bit | EN7562CT | arm32v5 images only | [product page](https://mikrotik.com/product/hex_2024), [Container page](https://help.mikrotik.com/docs/display/ROS/Container) |
| hAP ac2 | ARM 32bit | IPQ-4018 | yes | [product page](https://mikrotik.com/product/hap_ac2) |
| wAP ac LTE kit | ARM 32bit | IPQ-4018 | yes | [product page](https://mikrotik.com/product/wap_ac_lte_kit) |
| Chateau 5G | ARM 32bit | IPQ-4019 | yes | [product page](https://mikrotik.com/product/chateau_5g) |
| Chateau LTE12 | ARM 32bit | IPQ-4019 | yes | [product page](https://mikrotik.com/product/chateau_lte12) |
| hAP ax lite LTE6 | ARM 32bit | IPQ-5010 | yes | [product page](https://mikrotik.com/product/hap_ax_lite_lte6) |
| Chateau LTE6 ax | ARM 64bit | IPQ-6010 | yes | [product page](https://mikrotik.com/product/chateau_lte6_ax) |
| Chateau 5G ax | ARM 64bit | IPQ-6010 | yes | [product page](https://mikrotik.com/product/chateau_5g_ax) |
| Chateau 5G R17 ax | ARM 64bit | IPQ-6010 | yes | [product page](https://mikrotik.com/product/chateau_5g_r17_ax) |
| RB5009UG+S+IN | ARM 64bit | 88F7040 | yes | [product page](https://mikrotik.com/product/rb5009ug_s_in) |

Where a container can run, it costs CPU:
- One user reports about 250 Mbit/s through a Tayga NAT64 container on an RB5009 with a 910 Mbit/s WAN ([post 9, Oct 2026](https://forum.mikrotik.com/t/tayga-nat64-official-support-for-routeros/267504/9)).
- Tayga's maintainer writes that "Packets will flow through RouterOS twice, once as IPv4 and once as IPv6", and that packet marks do not survive the trip ([post 1](https://forum.mikrotik.com/t/tayga-nat64-official-support-for-routeros/267504/1)).
- Tayga's own [RouterOS guide](https://github.com/apalrd/tayga/blob/main/docs/container/mikrotik.md) covers NAT64 only, and says CLAT "is not completely documented yet".
