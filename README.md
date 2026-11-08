# 464XLAT for MikroTik RouterOS

I am asking MikroTik to build 464XLAT into RouterOS: a CLAT (customer-side translator) and a PLAT (stateful NAT64), with DNS64 as an option. RouterOS is one operating system from the hEX to the CCR, so the same software serves CPEs and the high-end routers that would run the PLAT.

This repository holds the case: the standards, what RouterOS has today, who is affected, and how to test it. It contains no code.

## Why now

- **The IETF has approved it.** [draft-ietf-v6ops-claton](https://datatracker.ietf.org/doc/draft-ietf-v6ops-claton/) is approved by the IESG and in the RFC Editor queue, as of 8 November 2026. It updates RFC 8585 so that a CE router supporting 464XLAT MUST learn the NAT64 prefix from PREF64 in Router Advertisements ([RFC 8781](https://www.rfc-editor.org/rfc/rfc8781.html)).
- **RouterOS already has the edges.** It sends PREF64 (7.8) and handles DHCP option 108 (7.25). It cannot translate between IPv4 and IPv6. See the [requirements table](docs/requirements.md).
- **The clients are ready.** Windows 11 made CLAT generally available in October 2026 ([Redmond Magazine](https://redmondmag.com/blogs/redmond-dispatch/2026/10/microsoft-makes-windows-clat-generally-available.aspx)), joining the phones and laptops that already had it. A router that sends PREF64 and option 108 lets those hosts drop IPv4. Devices that only speak IPv4 still need a CLAT on the router.
- **Large networks already run IPv6-only.** T-Mobile US, Telstra mobile, Deutsche Telekom 5G SA and EE deliver IPv4 through 464XLAT. MikroTik LTE and 5G routers on those networks give no IPv4 to the devices behind them ([who is affected](docs/who-is-affected.md)).

## My network

My home office runs dual stack on a MikroTik gateway. I measure it with NetFlow and Akvorado. About 80% of traffic is native IPv6 on average, up from 67%, with peaks beyond 90%. Once the IPv4-only devices are excluded, it is above 90%. The IPv4 that remains is "four cameras and a caching proxy". Only a CLAT on the router can move that traffic, and RouterOS does not have one.

- [Project IPv6-First: a case study in achieving an 80% native IPv6 SOHO network](https://medium.com/@terrysweetser_90287/project-ipv6-first-a-case-study-in-achieving-an-80-native-ipv6-soho-network-34cb2912923d)
- [Setting Up Akvorado: a NetFlow analyser for your IPv6-first network](https://medium.com/@terrysweetser_90287/setting-up-akvorado-a-netflow-analyser-for-your-ipv6-first-network-608882bc8445)
- [Setting Up Akvorado, Six Months On](https://medium.com/@terrysweetser_90287/setting-up-akvorado-six-months-on-4565b46dfc84) (source of the figures above)

## A live example

As of 8 November 2026, github.com publishes no AAAA record. A host on an IPv6-only network cannot open this page unless its network provides NAT64, or the host has a CLAT and the network a PLAT.

## Contents

| Document | What it covers |
|---|---|
| [Requirements](docs/requirements.md) | Each MUST and SHOULD from RFC 8585, the claton draft, RFC 6146 and RFC 6147, with RouterOS 7.25.1 status and sources |
| [Test scenario](docs/test-scenario.md) | The test I recommend until ISPs deploy PLAT: PLAT on the CPE, IPv6-mostly LAN |
| [Who is affected](docs/who-is-affected.md) | IPv6-only networks, and which MikroTik models can or cannot run a container |
| [Objections](docs/objections.md) | Dual stack, customer-side translation, containers, scale, DNS64, IPv4 prices |
| [Acceptance criteria](docs/acceptance.md) | What a user should be able to observe once 464XLAT ships |
| [History](docs/history.md) | Requests and MikroTik statements from 2010 to 2026 |
| [Ticket template](TICKET-TEMPLATE.md) | Text to paste into a MikroTik support ticket |

## How to help

Open a support ticket with MikroTik using the [ticket template](TICKET-TEMPLATE.md), and describe your own network. Discussion is in the forum thread [Feature Request: 464XLAT](https://forum.mikrotik.com/t/feature-request-464xlat/263487).

Corrections are welcome as GitHub issues or pull requests. Please include a source.

## Credits and licence

Me with AI Help.

Text copyright 2026 Terry Sweetser, licensed under [CC BY 4.0](LICENSE). Published by [IEISI](https://www.ieisi.org/).
