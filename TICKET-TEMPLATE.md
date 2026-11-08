# Support ticket template

MikroTik reads support tickets at [help.mikrotik.com/servicedesk](https://help.mikrotik.com/servicedesk/). If you need 464XLAT in RouterOS, open a ticket and paste the text below. If you only need one of the two features, delete the other. Fill in the three fields first. A ticket that describes a real network carries more weight than a forum reply.

---

**Subject:** Feature request: CLAT for CPEs and PLAT (NAT64) in RouterOS

**Body:**

Please add these two features to RouterOS.

1. A CLAT (RFC 6877) for CPEs. Behind an IPv6-only WAN, IPv4-only devices get no IPv4 without one. RFC 8585 says a CE router for the retail market MUST support CLAT, and draft-ietf-v6ops-claton, in the RFC Editor queue as a Proposed Standard, specifies how a CLAT behaves. Containers do not run on MIPS or TILE devices. Requirements C1 to C10 and N1 to N6.

2. A PLAT, stateful NAT64 (RFC 6146), for ISP routers and for CPEs with a public IPv4 address. RouterOS already sends PREF64 (7.8) and handles DHCP option 108 (7.25), but it has no PLAT for hosts to use. Requirements P1 to P4. DNS64 (RFC 6147) is optional, with DNSSEC validation before synthesis: requirements D1 to D3.

The requirements and their status in RouterOS 7.25.1 are listed here:
https://github.com/IEISI-ORG/464xlat_mikrotik/blob/main/docs/requirements.md

My network:
- Network type (ISP, mobile, enterprise, home): ____
- MikroTik models: ____
- Number of devices: ____

---

Thank you. Background and sources are in the [repository README](README.md).
