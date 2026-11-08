# Recommended test scenario: PLAT on the CPE

This is the test I recommend until upstream providers deploy a PLAT. It needs no help from the ISP. Any MikroTik owner with a public IPv4 address can run it once RouterOS has a PLAT.

This page is a specification. RouterOS cannot run it yet. The current baseline is my own network, measured on a dual-stack MikroTik: about 80% of traffic is native IPv6 on average, up from 67%, with peaks beyond 90%. The remainder is "four cameras and a caching proxy" (my ISP's), and excluding that floor the ratio holds above 90% ([Setting Up Akvorado, Six Months On](https://medium.com/@terrysweetser_90287/setting-up-akvorado-six-months-on-4565b46dfc84)).

Requirement IDs refer to [requirements.md](requirements.md).

## Setup

| Part | What it does | RouterOS today |
|---|---|---|
| WAN | Dual stack: a public static IPv4 address and an IPv6 prefix delegation | present |
| PLAT on the CPE | Stateful NAT64 from the LAN's IPv6 to the CPE's public IPv4, using 64:ff9b::/96 or a /96 from the delegated prefix ([P1, P3](requirements.md#plat)) | missing |
| PREF64 on the LAN | Router Advertisements carry the NAT64 prefix ([C7](requirements.md#clat), [RFC 8781](https://www.rfc-editor.org/rfc/rfc8781.html)) | present since 7.8 |
| Option 108 on the LAN | The DHCPv4 server tells hosts that support it to go IPv6-only ([C8](requirements.md#clat), [RFC 8925](https://www.rfc-editor.org/rfc/rfc8925.html)) | present since 7.25 |
| DNS64, optional | AAAA synthesis for IPv6-only hosts without their own CLAT, behind a validating DNSSEC resolver ([D1 to D3](requirements.md#dns64-optional)) | missing |
| LAN | IPv6-mostly or IPv6-only ([draft-ietf-v6ops-6mops](https://datatracker.ietf.org/doc/draft-ietf-v6ops-6mops/)) | present |

On an IPv6-mostly LAN:

- Hosts with their own CLAT see option 108 and PREF64. They give up their IPv4 address and reach IPv4 destinations through their CLAT and the CPE's PLAT.
- IPv4-only devices, such as cameras, ignore option 108. They keep a DHCPv4 lease and reach the Internet through ordinary NAT44.

On an IPv6-only LAN there is no DHCPv4, so IPv4-only devices cannot connect.

## Path to IPv6-only

1. PLAT on the CPE (this test). The WAN is still dual stack. The LAN moves to IPv6-mostly.
2. The ISP deploys a PLAT and withdraws native IPv4 from the access link. The CPE stops its own PLAT and passes the ISP's PREF64 to the LAN ([C4, C7](requirements.md#clat)). It runs CLAT for the IPv4-only devices ([C1, C2](requirements.md#clat)). This stage assumes no native IPv4 on the WAN, because a CLAT turns off when native IPv4 appears ([N2](requirements.md#clat-node-behaviour)).
3. IPv6-only access and LAN, with IPv4 delivered as a service by the ISP's PLAT and the CPE's CLAT.

Each stage uses a different part of the same software on the same CPE, so the request covers CLAT and PLAT together.

## What to measure

I measure with NetFlow from the MikroTik into Akvorado, as described in [Setting Up Akvorado](https://medium.com/@terrysweetser_90287/setting-up-akvorado-a-netflow-analyser-for-your-ipv6-first-network-608882bc8445). Akvorado splits traffic by EType, which separates IPv4 (0x800) from IPv6 (0x86DD).

- On the LAN, the share of bytes that is IPv6. With hosts using their own CLAT, this should approach 100% apart from the IPv4-only devices.
- On the WAN, filtered to `InIfBoundary = external`, the IPv6 share of all Internet traffic, compared with the baseline above.
- Whether hosts that support option 108 drop their IPv4 address and still reach IPv4-only sites.
- Whether IPv4-only devices still work through NAT44 on the IPv6-mostly LAN.

The [Project IPv6-First case study](https://medium.com/@terrysweetser_90287/project-ipv6-first-a-case-study-in-achieving-an-80-native-ipv6-soho-network-34cb2912923d) describes the method I used to find and remove IPv4 traffic before this stage.
