# 464XLAT requirements and RouterOS 7.25.1

This table lists what the standards ask of a router that provides IPv4 over an IPv6-only network, and what RouterOS does today.

RouterOS version: 7.25.1, the stable release as of 8 November 2026.

Method: every RouterOS changelog from 7.1 to 7.26beta5 (305 files, including betas and release candidates) was fetched from `upgrade.mikrotik.com/routeros/<version>/CHANGELOG` and searched for NAT64, CLAT, 464XLAT, PREF64, DNS64, DNSSEC, PCP, DS-Lite, MAP-E, MAP-T and option 108. The [MikroTik help pages](https://help.mikrotik.com/docs/display/ROS/RouterOS) for IPv6 Neighbor Discovery, DNS, DHCP and Container were read for the same terms.

Status words:

- **present**: RouterOS has it, with the version that added it.
- **missing**: no changelog entry, help page or configuration option exists for it.
- **not documented**: behaviour RouterOS could perform without a visible setting (reading an option from a received Router Advertisement, validating DNSSEC inside the resolver), where nothing in the changelogs or help pages says it does and no hardware test has confirmed that it does not.
- **not applicable**: the requirement depends on a protocol RouterOS does not implement.

Sources:

- The CLAT rows follow [RFC 8585](https://www.rfc-editor.org/rfc/rfc8585.html) (Informational, May 2019) as amended by [draft-ietf-v6ops-claton-16](https://datatracker.ietf.org/doc/draft-ietf-v6ops-claton/). As of 8 November 2026, that draft is approved by the IESG and in the RFC Editor queue as a Proposed Standard.
- Its [section 9.2](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-9.2) changes 464XLAT-4 and 464XLAT-6 in RFC 8585 and adds 464XLAT-0 and 464XLAT-7.
- The translation itself is SIIT, [RFC 7915](https://www.rfc-editor.org/rfc/rfc7915.html), which both the CLAT and the PLAT use.

## CLAT

RFC 8585 [section 3.2.1](https://www.rfc-editor.org/rfc/rfc8585.html#section-3.2.1) says a CE router for the retail market MUST support CLAT.

| ID | Requirement | Source | RouterOS 7.25.1 | Evidence |
|---|---|---|---|---|
| C1 | MUST: CLAT, stateless IPv4 to IPv6 translation on the router | [RFC 6877](https://www.rfc-editor.org/rfc/rfc6877.html), [RFC 7915](https://www.rfc-editor.org/rfc/rfc7915.html), [RFC 8585 s3.2.1](https://www.rfc-editor.org/rfc/rfc8585.html#section-3.2.1) | missing | IPv6 NAT exists only within IPv6 ([7.1](https://upgrade.mikrotik.com/routeros/7.1/CHANGELOG), [7.4](https://upgrade.mikrotik.com/routeros/7.4/CHANGELOG)); nothing translates between IPv4 and IPv6 |
| C2 | 464XLAT-1, MUST: NAT44 the LAN's IPv4 traffic before CLAT translation, unless a dedicated /64 is available | [RFC 8585 s3.2.1](https://www.rfc-editor.org/rfc/rfc8585.html#section-3.2.1) | missing | Depends on C1 |
| C3 | 464XLAT-2, SHOULD: UPnP IGD to PCP interworking; 464XLAT-3 and 464XLAT-5: MUSTs that apply if PCP is implemented | [RFC 8585 s3.2.1](https://www.rfc-editor.org/rfc/rfc8585.html#section-3.2.1) | not applicable | RouterOS has no Port Control Protocol; every "PCP" changelog entry refers to the VLAN priority field |
| C4 | 464XLAT-4, MUST: learn the NAT64 prefix from PREF64 in Router Advertisements received on the WAN | [claton s9.2](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-9.2), [RFC 8781](https://www.rfc-editor.org/rfc/rfc8781.html) | not documented | The [Neighbor Discovery page](https://help.mikrotik.com/docs/display/ROS/IPv6+Neighbor+Discovery) covers sending PREF64 only |
| C5 | 464XLAT-4, SHOULD: learn the NAT64 prefix through DNS (`ipv4only.arpa`) | [claton s9.2](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-9.2), [RFC 7050](https://www.rfc-editor.org/rfc/rfc7050.html) | missing | No changelog entry; this needs an active query, which no setting provides |
| C6 | 464XLAT-6, MUST: when several prefix sources exist, prefer PCP, then PREF64, then DNS | [claton s9.2](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-9.2), [RFC 7225](https://www.rfc-editor.org/rfc/rfc7225.html) | missing | Depends on C4 and C5 |
| C7 | 464XLAT-7, SHOULD: send PREF64 "containing the PLAT prefix" in Router Advertisements on the LAN | [claton s9.2](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-9.2), [RFC 8781](https://www.rfc-editor.org/rfc/rfc8781.html) | present since 7.8, static value | [7.8 changelog](https://upgrade.mikrotik.com/routeros/7.8/CHANGELOG): "added "pref64" option configuration for RA". Passing on a prefix learned on the WAN depends on C4 |
| C8 | 464XLAT-7, SHOULD: as DHCPv4 server, process option 108, with a setting to turn it off | [claton s9.2](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-9.2), [RFC 8925](https://www.rfc-editor.org/rfc/rfc8925.html) | present since 7.25 | [7.25 changelog](https://upgrade.mikrotik.com/routeros/7.25/CHANGELOG): "add ipv6-only-preferred parameter to respect option 108". [7.10](https://upgrade.mikrotik.com/routeros/7.10/CHANGELOG) only named the option in debug logs. The default is not stated on the [DHCP page](https://help.mikrotik.com/docs/display/ROS/DHCP) |
| C9 | CONFIG-3, MUST: if a NAT64 prefix is configured, enable 464XLAT | [RFC 8585 s3.2](https://www.rfc-editor.org/rfc/rfc8585.html#section-3.2) | missing | Depends on C1 |
| C10 | TRANS-2, MUST: a GUI, and a CLI or API, to turn each transition mechanism on and off | [RFC 8585 s3.2](https://www.rfc-editor.org/rfc/rfc8585.html#section-3.2) | missing for 464XLAT | Depends on C1 |

## CLAT node behaviour

These rules apply to any node running CLAT, including a router. Sections are from [draft-ietf-v6ops-claton-16](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html).

| ID | Requirement | Source | RouterOS 7.25.1 | Evidence |
|---|---|---|---|---|
| N1 | MUST enable CLAT if no non-link-local IPv4 address exists when the NAT64 prefix is discovered, unless configured otherwise; SHOULD do so as soon as a PREF64 option arrives | [claton s4](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-4) | missing | Depends on C1 and C4 |
| N2 | MUST disable CLAT when a native IPv4 default gateway appears, unless configured to prefer CLAT. Section 11 describes the same rule as a SHOULD | [claton s5](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-5), [s11](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-11) | missing | Depends on C1 |
| N3 | SHOULD use addresses from 192.0.0.0/29 with a /32 mask for the CLAT's local IPv4 | [claton s6.1](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-6.1) | missing | Depends on C1 |
| N4 | MUST, when extending IPv4 to a LAN: either translate into a dedicated prefix (for example from DHCPv6-PD), or NAT44 to one IPv4 address and translate that to one dedicated IPv6 address. Without a prefix, MUST use a dedicated IPv6 address for CLAT only | [claton s6.2.1](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-6.2.1) | missing | Depends on C1 |
| N5 | MUST treat the CLAT IPv6 address like any other, including Duplicate Address Detection | [claton s6.2.2](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-6.2.2) | missing | Depends on C1 |
| N6 | SHOULD give IPv4 applications an MTU 28 bytes below the IPv6 MTU | [claton s8](https://www.ietf.org/archive/id/draft-ietf-v6ops-claton-16.html#section-8) | missing | Depends on C1 |

## PLAT

The PLAT is stateful NAT64. It is needed at the provider and, for the [recommended test scenario](test-scenario.md), on the CPE itself.

| ID | Requirement | Source | RouterOS 7.25.1 | Evidence |
|---|---|---|---|---|
| P1 | Stateful NAT64 for UDP, TCP and ICMP | [RFC 6146 s3.5](https://www.rfc-editor.org/rfc/rfc6146.html#section-3.5), [RFC 7915](https://www.rfc-editor.org/rfc/rfc7915.html) | missing | IPv6 NAT exists only within IPv6 ([7.1](https://upgrade.mikrotik.com/routeros/7.1/CHANGELOG), [7.4](https://upgrade.mikrotik.com/routeros/7.4/CHANGELOG)) |
| P2 | Hairpinning between two IPv6 hosts behind the same NAT64 | [RFC 6146 s3.8](https://www.rfc-editor.org/rfc/rfc6146.html#section-3.8) | missing | Depends on P1 |
| P3 | MUST support the RFC 6052 address algorithm, with the well-known prefix 64:ff9b::/96 and network-specific prefixes | [RFC 6146 s3.5.4](https://www.rfc-editor.org/rfc/rfc6146.html#section-3.5.4), [RFC 6052 s2](https://www.rfc-editor.org/rfc/rfc6052.html#section-2) | missing | Depends on P1 |
| P4 | Tell hosts which prefix the NAT64 uses | [RFC 8781](https://www.rfc-editor.org/rfc/rfc8781.html) | present since 7.8 | Same feature as C7 ([7.8 changelog](https://upgrade.mikrotik.com/routeros/7.8/CHANGELOG)) |

## DNS64 (optional)

DNS64 is optional. Hosts that learn the NAT64 prefix through PREF64 run their own CLAT and do not need synthesised AAAA records.

| ID | Requirement | Source | RouterOS 7.25.1 | Evidence |
|---|---|---|---|---|
| D1 | Synthesise AAAA records from A records for names that have no AAAA | [RFC 6147 s5.1.7](https://www.rfc-editor.org/rfc/rfc6147.html#section-5.1.7) | missing | No changelog entry; no setting on the [DNS page](https://help.mikrotik.com/docs/display/ROS/DNS) |
| D2 | RFC rule for a validating DNS64: MUST validate the negative AAAA answer before it synthesises, and MUST return SERVFAIL when validation fails | [RFC 6147 s5.5](https://www.rfc-editor.org/rfc/rfc6147.html#section-5.5) | missing | Depends on D1 |
| D3 | This repo's requirement: DNSSEC validation runs upstream of the DNS64 function, in RouterOS or in a resolver RouterOS forwards to | [RFC 6147 s5.5](https://www.rfc-editor.org/rfc/rfc6147.html#section-5.5) | not documented | No DNSSEC entry in any v7 changelog; no DNSSEC setting on the [DNS page](https://help.mikrotik.com/docs/display/ROS/DNS) |

RFC 6147 allows a DNS64 that does not validate. This repo asks for more than that.
- RFC 6147 section 5.5 says that, for a validating DNS64, skipping the D2 step "would allow an attacker to use DNS64 as a mechanism to circumvent DNSSEC".
- If RouterOS adds DNS64, validation has to happen before synthesis, either in its own resolver or in a validating resolver upstream.

## Context: other transition mechanisms

| ID | Requirement | Source | RouterOS 7.25.1 | Evidence |
|---|---|---|---|---|
| O1 | DS-Lite, lw4o6, MAP-E and MAP-T, with DHCPv6 priority options (TRANS-1) | [RFC 8585 s3.2](https://www.rfc-editor.org/rfc/rfc8585.html#section-3.2) | missing | No changelog names them. DS-Lite and MAP-E can be built by hand from `ipip6` tunnels and NAT rules |

This repo asks for CLAT and PLAT, with optional DNS64. O1 is listed for completeness.
