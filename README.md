# 464XLAT for MikroTik RouterOS

I am asking MikroTik to add two features to RouterOS. They have different users and different costs.

## 1. A CLAT on CPEs

Behind an IPv6-only WAN, IPv4-only devices get no IPv4 unless the router translates for them, and RouterOS cannot. T-Mobile US, Telstra mobile, Deutsche Telekom 5G SA and EE run IPv6-only with 464XLAT today, and Chateau owners on T-Mobile US and Deutsche Telekom networks report the problem ([who is affected](docs/who-is-affected.md)). Containers do not run on MIPS or TILE models.

[RFC 8585](https://www.rfc-editor.org/rfc/rfc8585.html#section-3.2.1) says a CE router for the retail market MUST support CLAT. [draft-ietf-v6ops-claton](https://datatracker.ietf.org/doc/draft-ietf-v6ops-claton/), in the RFC Editor queue on 8 November 2026, specifies how a CLAT behaves.

## 2. A PLAT (stateful NAT64)

A PLAT is the bigger job: it keeps state, and at ISP scale it is a CGNAT. An ISP with a CCR or CHR at the edge has to buy other equipment to go IPv6-only without one. On a CPE with a public IPv4 address, the same feature serves one LAN and lets anyone run IPv6-mostly before their ISP deploys a PLAT ([test scenario](docs/test-scenario.md)).

RouterOS already tells hosts about a PLAT, with PREF64 since 7.8 and DHCP option 108 since 7.25, but it has no PLAT to point them at. Hosts with their own CLAT, including [Windows 11 since October 2026](https://redmondmag.com/blogs/redmond-dispatch/2026/10/microsoft-makes-windows-clat-generally-available.aspx), can then drop IPv4.

DNS64 is optional. Where it is used, DNSSEC validation has to run upstream of it.

## My network

My home office runs dual stack on a MikroTik gateway, as a stage on the way to IPv6-only. About 80% of traffic is native IPv6 on average, up from 67%, with peaks beyond 90%. The rest is "four cameras and a caching proxy" (the proxy is my ISP's), and excluding that floor the ratio holds above 90%. Only a CLAT on the router can move that traffic. Sources: [Six Months On](https://medium.com/@terrysweetser_90287/setting-up-akvorado-six-months-on-4565b46dfc84), [case study](https://medium.com/@terrysweetser_90287/project-ipv6-first-a-case-study-in-achieving-an-80-native-ipv6-soho-network-34cb2912923d), [Akvorado setup](https://medium.com/@terrysweetser_90287/setting-up-akvorado-a-netflow-analyser-for-your-ipv6-first-network-608882bc8445).

## How to help

Open a support ticket using the [ticket template](TICKET-TEMPLATE.md) and describe your network. Discussion is in the [forum thread](https://forum.mikrotik.com/t/feature-request-464xlat/263487). Corrections are welcome as GitHub issues, with a source.

Reference: [requirements](docs/requirements.md), [objections](docs/objections.md), [acceptance criteria](docs/acceptance.md), [history](docs/history.md).

## Credits and licence

Me with AI Help.

Text copyright 2026 Terry Sweetser, licensed under [CC BY 4.0](LICENSE). Published by [IEISI](https://www.ieisi.org/).
