# What "done" looks like

These are behaviours a user could observe on a RouterOS device once 464XLAT is in place. They describe what a user sees. How MikroTik exposes the settings is MikroTik's choice. Requirement IDs refer to [requirements.md](requirements.md).

## CLAT

1. An IPv4-only device on the LAN reaches an IPv4-only Internet host when the WAN has IPv6 only and the network provides a PLAT ([C1, C2](requirements.md#clat)).
2. A program on the LAN that connects to an IPv4 address literal, with no DNS lookup, also succeeds ([C1](requirements.md#clat)).
3. The router learns the NAT64 prefix from a PREF64 option in a Router Advertisement received on the WAN, with no manual prefix setting ([C4](requirements.md#clat)).
4. With no PREF64 on the WAN, the router can learn the prefix by resolving `ipv4only.arpa` through the provider's DNS64 ([C5](requirements.md#clat)).
5. The router sends the learned prefix to the LAN in its own Router Advertisements ([C7](requirements.md#clat)).
6. CLAT starts by itself when the prefix is learned and the router has no IPv4 address ([N1](requirements.md#clat-node-behaviour)).
7. CLAT stops when a native IPv4 default gateway appears on the WAN, unless the administrator chose to prefer CLAT ([N2](requirements.md#clat-node-behaviour)).
8. IPv4 packets near the MTU pass without fragmentation, because the router offers an IPv4 MTU 28 bytes below the IPv6 MTU ([N6](requirements.md#clat-node-behaviour)).
9. The administrator can turn CLAT on and off in WinBox or WebFig, and in the CLI or API ([C10](requirements.md#clat)).
10. CLAT runs on every architecture RouterOS supports, MIPS and TILE included, without containers ([who is affected](who-is-affected.md#mikrotik-hardware-and-the-container-workaround)).

## PLAT

1. An IPv6-only host on the LAN reaches an IPv4-only Internet host through the router, over TCP, UDP and ICMP ([P1](requirements.md#plat)).
2. The PLAT works with the well-known prefix 64:ff9b::/96 and with a network-specific prefix that the administrator chooses ([P3](requirements.md#plat)).
3. Two IPv6 hosts behind the same PLAT can reach each other through it ([P2](requirements.md#plat)).
4. The prefix in use is advertised to hosts with PREF64 ([P4](requirements.md#plat)).
5. The same feature runs on a hEX and on a CCR, and throughput rises with the hardware (this repo's request).
6. Firewall rules and packet marks apply to translated traffic in a single pass (this repo's request; the container workaround loses marks, see [who is affected](who-is-affected.md#mikrotik-hardware-and-the-container-workaround)).

## DNS64 (optional)

1. When DNS64 is on, a AAAA query for a name that has only A records returns an address built from the NAT64 prefix ([D1](requirements.md#dns64-optional)).
2. When DNS64 is on, its answers are validated with DNSSEC before synthesis, either by RouterOS or by a validating upstream resolver, and a failed validation returns SERVFAIL ([D2, D3](requirements.md#dns64-optional)).
3. DNS64 is off by default. A network using only PREF64 and CLAT needs no DNS64.

## Test scenario

The [recommended test scenario](test-scenario.md) runs on one device: PLAT on the CPE with a public static IPv4, and an IPv6-mostly LAN.
