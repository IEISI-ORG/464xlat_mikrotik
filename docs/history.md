# History of the request

Requests for IPv4 and IPv6 translation in RouterOS go back to 2010. This timeline lists the main forum threads, MikroTik staff statements and RouterOS releases, in date order. Staff statements are quoted as written.

## 2010 to 2011: NAT64 and DNS64 on the v5 roadmap

- **17 Jun 2010.** A user asks for NAT64 and DNS64 ([thread 38531](https://forum.mikrotik.com/t/nat64-and-dns64/38531)). MikroTik staff member janisk replies: "IPv6 does not have NAT as it is in IPv4" ([post 2](https://forum.mikrotik.com/t/nat64-and-dns64/38531/2)). The thread stays open until 2023, with 102 posts.
- **30 Dec 2010.** MikroTik staff member normis posts an IPv6 roadmap: "the list of features we have planned to complete within the next 6 months, for v5". The list includes DNS64 and NAT64 ([IPv6 TODO](https://forum.mikrotik.com/t/ipv6-todo/43429/1)).
- **23 Sep 2011.** An update to the same roadmap still lists DNS64 and NAT64 as not done ([post 46](https://forum.mikrotik.com/t/ipv6-todo/43429/46)). Neither appears in any v7 changelog (7.1 to 7.26beta5).

## 2014 to 2020: repeated requests

- **2014.** CLAT client and NAT64 request ([thread 81585](https://forum.mikrotik.com/t/464xlat-clat-client-and-or-nat64-capabilities/81585)).
- **2015 to 2017.** NAT64 and DNS64 request ([thread 85661](https://forum.mikrotik.com/t/features-request-nat64-dns64/85661)).
- **2015 to 2019.** SIIT (stateless translation) request ([thread 86659](https://forum.mikrotik.com/t/siit-support-thoughts/86659)).
- **Jan 2020.** Two new threads:
  - 464XLAT for CPEs, citing Vivo Brazil, Rogers and Deutsche Telekom ([thread 135906](https://forum.mikrotik.com/t/464xlat-support-mikrotik/135906));
  - NAT64 and DNS64 for carrier-grade NAT ([thread 136288](https://forum.mikrotik.com/t/feature-request-nat64-dns64-cgn/136288)). This one is still active in 2026, with 44 posts.

## 2021 to 2023: RouterOS v7 adds the edges

- **1 Dec 2021.** RouterOS 7.1 adds "support for IPv6 NAT" ([changelog](https://upgrade.mikrotik.com/routeros/7.1/CHANGELOG)). It works within IPv6 only.
- **19 Jul 2022.** RouterOS 7.4 adds IPv6 `action=src-nat` ([changelog](https://upgrade.mikrotik.com/routeros/7.4/CHANGELOG)).
- **20 Jan 2023.** The 7.8beta announcement lists "added "pref64" option configuration for RA" ([announcement](https://forum.mikrotik.com/t/v7-8beta-testing-is-released/163742/1)).
- **24 Feb 2023.** RouterOS 7.8 ships with PREF64 in Router Advertisements ([changelog](https://upgrade.mikrotik.com/routeros/7.8/CHANGELOG)).

## 2025 to 2026: the current request

- **31 Jul 2025.** "Feature Request: 464XLAT" asks for CLAT, PLAT and DNS64 ([thread 263487](https://forum.mikrotik.com/t/feature-request-464xlat/263487)).
- **Sep 2025.** "Please support NAT64&DNS64 as soon as possible", from a small provider in Vietnam ([thread 264846](https://forum.mikrotik.com/t/please-support-nat64-dns64-as-soon-as-possible/264846)). On 19 Sep 2025 normis, in a post he marked as not speaking for MikroTik, asks "why is dual stack not a solution?" ([post 3](https://forum.mikrotik.com/t/please-support-nat64-dns64-as-soon-as-possible/264846/3)). [Objections](objections.md#why-not-dual-stack) answers this.
- **11 Dec 2025.** A how-to for running Tayga in a container on an RB5009 ([thread 267046](https://forum.mikrotik.com/t/nat64-and-464xlat-on-ros-how-to/267046)).
- **2 Jan 2026.** Tayga's maintainer publishes a NAT64 container tested on RouterOS ([thread 267504](https://forum.mikrotik.com/t/tayga-nat64-official-support-for-routeros/267504), [docs](https://github.com/apalrd/tayga/blob/main/docs/container/mikrotik.md)).
- **Sep to Nov 2026.** A Tayga-based CLAT container for the Chateau 5G ax appears ([antongrizli/tayga](https://github.com/antongrizli/tayga)).
- **22 Oct 2026.** RouterOS 7.25 adds "ipv6-only-preferred parameter to respect option 108" to the DHCPv4 server ([changelog](https://upgrade.mikrotik.com/routeros/7.25/CHANGELOG)).
- **2 Nov 2026.** In thread 263487, an ISP that uses MikroTik CPEs says it will open a support ticket for CLAT ([post 42](https://forum.mikrotik.com/t/feature-request-464xlat/263487/42)), and another user notes 15 months without a reply from MikroTik ([page 3](https://forum.mikrotik.com/t/feature-request-464xlat/263487?page=3)).

## Status on 8 November 2026

RouterOS 7.25.1 is the stable release ([changelog](https://upgrade.mikrotik.com/routeros/7.25.1/CHANGELOG)). It has no CLAT, PLAT or DNS64; see [requirements](requirements.md).

[draft-ietf-v6ops-claton](https://datatracker.ietf.org/doc/draft-ietf-v6ops-claton/) is approved by the IESG and in the RFC Editor queue. It makes RFC 8781 PREF64 a MUST for CE routers that support 464XLAT.

MikroTik staff have not posted in thread 263487.
