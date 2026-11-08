# Support ticket template

MikroTik reads support tickets at [help.mikrotik.com/servicedesk](https://help.mikrotik.com/servicedesk/). If you need 464XLAT in RouterOS, open a ticket and paste the text below. Fill in the three fields first. A ticket that describes a real network carries more weight than a forum reply.

---

**Subject:** Feature request: 464XLAT (CLAT and PLAT) in RouterOS

**Body:**

Please add 464XLAT to RouterOS: a CLAT (RFC 6877) and a PLAT, stateful NAT64 (RFC 6146), with optional DNS64 (RFC 6147). Where DNS64 is used, DNSSEC validation needs to run before synthesis.

Why now: draft-ietf-v6ops-claton is approved by the IESG and in the RFC Editor queue. It updates RFC 8585 so that a CE router supporting 464XLAT MUST learn the NAT64 prefix from PREF64 in Router Advertisements (RFC 8781).

RouterOS already sends PREF64 (7.8) and handles DHCP option 108 (7.25), but it cannot translate between IPv4 and IPv6. Containers do not run on MIPS or TILE devices.

The requirements and their status in RouterOS 7.25.1 are listed here, with IDs C1 to C10, N1 to N6, P1 to P4 and D1 to D3:
https://github.com/IEISI-ORG/464xlat_mikrotik/blob/main/docs/requirements.md

My network:
- Network type (ISP, mobile, enterprise, home): ____
- MikroTik models: ____
- Number of devices: ____

---

Thank you. Background and sources are in the [repository README](README.md).
