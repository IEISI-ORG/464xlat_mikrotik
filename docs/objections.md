# Objections and answers

These objections have come up in the forum threads. Each is quoted with a link, then answered with sources.

## Why not dual stack?

> "why is dual stack not a solution?"
> normis, 19 Sep 2025, in a post he marked as not speaking for MikroTik ([post 3](https://forum.mikrotik.com/t/please-support-nat64-dns64-as-soon-as-possible/264846/3))

Dual stack is one step on the way to IPv6-only. It still needs an IPv4 address for every customer, so an operator short of addresses ends up running CGNAT anyway. Most mobile devices already sit behind CGNAT, and Starlink charges for a public IPv4 address ([Amm0, post 23](https://forum.mikrotik.com/t/feature-request-464xlat/263487/23)).

Large networks have already moved past dual stack: T-Mobile US, Telstra mobile, Deutsche Telekom 5G SA and EE run IPv6-only with 464XLAT ([who is affected](who-is-affected.md)).

Geoff Huston describes the result. Networks see no clear gain from running dual stack, so each waits for the others, and "there is no common sense of urgency" ([APNIC blog, Oct 2024](https://blog.apnic.net/2024/10/22/the-ipv6-transition/)). A home network on a dual-stack MikroTik shows how far IPv6 alone can go. It carries "just about 80% of traffic" as native IPv6 on average, up from 67%, with peaks beyond 90%. The remainder is "four cameras and a caching proxy", and excluding that floor the ratio holds above 90% ([Setting Up Akvorado, Six Months On](https://medium.com/@terrysweetser_90287/setting-up-akvorado-six-months-on-4565b46dfc84)). The remaining IPv4 needs a CLAT to move, and RouterOS has none.

## Why translate at the customer side?

> "I can't see the need for any Customer side translation to make ipv4 external resources available over ipv6."
> DuctView, 6 Jan 2026 ([post 21](https://forum.mikrotik.com/t/feature-request-464xlat/263487/21))

DNS64 alone helps only applications that look up a name and accept an AAAA answer. RFC 6877 names the cases it leaves out, "IPv4 literals or IPv4-only sockets", and gives them to the CLAT ([RFC 6877 s5.2](https://www.rfc-editor.org/rfc/rfc6877.html#section-5.2)).

On a home network those cases are real devices. In the case study behind the figures above, the residual IPv4 included four IPv4-only cameras ([Project IPv6-First](https://medium.com/@terrysweetser_90287/project-ipv6-first-a-case-study-in-achieving-an-80-native-ipv6-soho-network-34cb2912923d)). No DNS change reaches them. A CLAT on the router does, which is why RFC 8585 makes CLAT a MUST for retail CE routers ([requirements C1](requirements.md#clat)).

## Containers already do this

A Tayga container gives RouterOS NAT64, and a community fork adds CLAT ([sirbryan, post 14](https://forum.mikrotik.com/t/feature-request-464xlat/263487/14)). The work is useful, and it shows demand. It cannot stand in for a native feature:

- Containers run only on ARM, ARM64 and x86. The hEX, hAP ac lite, LtAP and wAP LTE are MIPS and cannot run them ([who is affected](who-is-affected.md#mikrotik-hardware-and-the-container-workaround)).
- Turning containers on needs a device-mode change confirmed with physical access, and MikroTik recommends an external disk ([Container page](https://help.mikrotik.com/docs/display/ROS/Container)).
- Packets pass through RouterOS twice and packet marks are lost ([Tayga maintainer, post 1](https://forum.mikrotik.com/t/tayga-nat64-official-support-for-routeros/267504/1)).
- One user reports about 250 Mbit/s on an RB5009 with a 910 Mbit/s WAN ([post 9](https://forum.mikrotik.com/t/tayga-nat64-official-support-for-routeros/267504/9)).

## Will it scale?

> "does the open source implementation of this can scale well or comparable to proprietary solutions?"
> loloski, 1 Aug 2025 ([post 5](https://forum.mikrotik.com/t/feature-request-464xlat/263487/5))

The CLAT's translation step is stateless ([RFC 6877](https://www.rfc-editor.org/rfc/rfc6877.html), [RFC 7915](https://www.rfc-editor.org/rfc/rfc7915.html)). On a router it sits behind a NAT44 ([requirements C2, N4](requirements.md#clat)), which is the same NAT44 RouterOS already runs on every CPE today.

The PLAT is stateful NAT64 ([RFC 6146](https://www.rfc-editor.org/rfc/rfc6146.html)). It keeps connection state, as RouterOS's NAT44 already does. RouterOS is one operating system from the hEX to the CCR, so one implementation can run on every device, and the hardware sets the scale.

## Why PLAT on RouterOS?

> "But the PLAT can live on other gear, the CLAT can't."
> Larsa, 2 Nov 2026 ([post 50](https://forum.mikrotik.com/t/feature-request-464xlat/263487/50))

The CLAT is the part only the CPE can provide, and it is the most urgent. A PLAT in RouterOS matters in two places.

The first is the CPE itself. With a PLAT on the CPE and a public IPv4 address on the WAN, anyone can run an IPv6-mostly LAN today, without waiting for an ISP ([test scenario](test-scenario.md)).

The second is ISPs that already run MikroTik routers. Leaving the PLAT to other gear assumes, in Amm0's words, "that an ISP does not want to use a MikroTik routers, only their CPEs" ([post 49](https://forum.mikrotik.com/t/feature-request-464xlat/263487/49)). An ISP with a CCR or CHR at the edge would have to buy something else to run IPv6-only.

## DNS64 is the hard part

> "And it's the DNS64 part that often the objection"
> Amm0, 2 Nov 2026 ([post 47](https://forum.mikrotik.com/t/feature-request-464xlat/263487/47))

DNS64 is optional in this request. Hosts that learn the NAT64 prefix from PREF64, which RouterOS already sends ([requirements C7](requirements.md#clat)), run their own CLAT and need no synthesised AAAA records.

Where DNS64 is used, DNSSEC validation has to run upstream of it. RFC 6147 says a validating DNS64 that skips this step "would allow an attacker to use DNS64 as a mechanism to circumvent DNSSEC" ([RFC 6147 s5.5](https://www.rfc-editor.org/rfc/rfc6147.html#section-5.5), [requirements D1 to D3](requirements.md#dns64-optional)).

## IPv4 addresses are getting cheaper

IPv4 prices are falling. An analysis of Hilco Global sales data projects that prices will approach zero in 2027, starting with larger blocks, and reports that hyperscalers have stopped buying ([IPv4 Sale Price Trends, June 2026](https://medium.com/@terrysweetser_90287/ipv4-sale-price-trends-update-june-2026-adcfc27b8662)).

A cheaper address does not remove the cost of running IPv4. An operator still has to scale CGNAT and keep port-block logs to meet retention rules, as the author of this repository argued in the thread (tcsweetser, [post 31](https://forum.mikrotik.com/t/feature-request-464xlat/263487/31)). Each flow that moves to IPv6 is one the CGNAT does not carry. 464XLAT lets the core and access network become IPv6-only, with IPv4 delivered as a service at the edge.
