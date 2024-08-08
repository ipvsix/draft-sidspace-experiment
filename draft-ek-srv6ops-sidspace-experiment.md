---
title: "SID Space (5f00::/16) Experiment"
abbrev: "SID Space Exp."
category: exp

docname: draft-ek-srv6ops-sidspace-experiment-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "SRv6 Operations"
keyword:
 - Segment Routing for IPv6 Segment Identifiers
 - SRv6 SIDs
venue:
  group: "SRv6 Operations"
  type: "Working Group"
  mail: "srv6ops@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/srv6ops/"
  github: "ipvsix/draft-sidspace-experiment"
  latest: "https://ipvsix.github.io/draft-sidspace-experiment/draft-ek-srv6ops-sidspace-experiment.html"

author:
 -
    fullname: Erik Kline
    organization: Aalyria Technologies, Inc.
    email: ek.ietf@gmail.com
 -
    fullname: Nick Buraglio
    organization: Energy Sciences Network
    email: buraglio@forwardingplane.net

normative:
  I-D.ietf-6man-sids:
    target: https://datatracker.ietf.org/doc/draft-ietf-6man-sids/
    title: "SRv6 Segment Identifiers in the IPv6 Addressing Architecture"
  IANA-IPv6Special:
    target: https://www.iana.org/assignments/iana-ipv6-special-registry/iana-ipv6-special-registry.xhtml
    title: "IANA IPv6 Special-Purpose Address Registry"
  IANA-ASNs:
    target: https://www.iana.org/assignments/as-numbers/as-numbers.xhtml
    title: "Autonomous System (AS) Numbers"

informative:
  RFC1897:
  RFC8402:
  RFC8754:
  RFC5398:
  RFC6996:
  RFC8799:
  draft-bdmgct-spring-srv6-security:
    target: https://datatracker.ietf.org/doc/draft-bdmgct-spring-srv6-security/
    title: SRv6 Security Considerations

--- abstract

This specification proposes an experimental structure for use of the SRv6 SIDs prefix in support of Inter-domain SRv6 networks.
The core of the proposal is to structure the address space by Autonomous System Number (ASN).

Use of this proposed structure is entirely voluntary.
The goal of this experiment is to aid SRv6 operations while preserving the ability to use this prefix across cooperating SRv6 domains, but not across the general Internet.

--- middle

# Introduction

[I-D.ietf-6man-sids] requested of IANA a dedicated prefix for Segment Routing over IPv6 [RFC8402] Segment Identifiers (SRv6 SIDs), with the aim of "improv\[ing\] security by making it simpler to filter traffic at the edge of the SR domains."
The prefix 5f00::/16 was allocated for this purpose [IANA-IPv6Special].
No requirements were placed on the use of this prefix nor any recommendations made for structured use of this prefix.

This specification proposes an experimental structure for use of the SRv6 SIDs prefix in support of Inter-domain SRv6 networks.
The core of the proposal is to structure the address space by Autonomous System Number (ASN).

Use of this proposed structure is entirely voluntary.
The goal is to aid SRv6 operations while preserving the ability to use this prefix across cooperating SRv6 domains, but not across the general Internet.

The SID space prefix was allocated to improve ease of filtering.
Where SRv6 traffic using these prefixes may be shared with cooperating partner networks,
this proposal makes it easier to craft filters that permit only SRv6 traffic from identified ASNs.

As a point of historical interest, this proposal contains echos of the structure of the original 6bone test allocation [RFC1897].

# Proposed Structure

The recommendation of this specification is for SRv6 domains to allocate SIDs from prefixes that are concatenations of the SRv6 SID prefix (5f00::/16) and an applicable ASN.
Assuming 32-bit ASNs, this yields a /48 per ASN in use within an SRv6 domain, i.e. 5f00:as.hi16:as.lo16::/48.

## Generation of ASN derived SRv6 prefix SID

Each unique ASN generates a prefix from the IANA allocation by converting mutually agreed upon ASNs to hexidecimal, and inserting this hex into a /48 prefix.

### SRv6 SID Documentation Prefixes

Using 16-bit and 32-bit ASNs reserved for documentation purposes [IANA-ASNs] yields several SRv6 SID prefixes that might be used for SRv6 documentation purposes.
These prefixes presently include ASNs in the range of 64496-64511 as defined in [RFC5398]:

~~~~~~
5f00:0:fbf0::/48
...
5f00:0:fbff::/48
~~~~~~

or any /48 prefix between these.

It should be noted that 32-but ASNs do not have a specific range dedicated for documentation but do have a private use block as defined in [RFC6996].

### SRv6 SID Private Use Prefixes

Using 16-bit and 32-bit ASNs reserved for private use purposes [IANA-ASNs] and defined by yields several SRv6 SID prefixes for private use.
These prefixes are defined by RFC 6996 and presently include:

| ASN size | Private Use Range     |
|----------|-----------------------|
| 16-bit   | 64512-65534           |
| 32-bit   | 4200000000-4294967294 |

yielding:

~~~~~~
5f00:0:fc00::/48
...
5f00:0:fffe::/48
~~~~~~

and

~~~~~~
5f00:fa56:ea00::/48
...
5f00:ffff:fffe::/48
~~~~~~

or any /48 prefix between these, as private use ASN-derived SID prefixes.

# Routing and Filtering

As noted in [draft-bdmgct-spring-srv6-security], it is assumed that each ASN participating in the SRv6 SID space experiment has deployed their respective SRv6 implementations within a limited domain [RFC8799] with appropriate filtering at the domain boundaries. Because this is a shared space experiment, the requisite filtering exceptions must be made between each limited domain to allow for the desired inter-domain communication to occur. Care should be taken to allow only the desired and necessary communication between each limited domain. The mechanisms used should be conformant with the limited domain security policy and may include, but are not limited to:
* routing filters such as BGP prefix-lists, route-maps, route-policies, or other analogous mechanisms, or
* access control filters at the domain edge

# Example test case

One possible test case is the exchange of the IPv6 prefix SID between two autonomous systems with independent management domains. In this example, AS4294967294 exchanges their SRv6 SID prefix (5f00:ffff:fffe::/48) with AS4200000000 who announces their ASN derived SRv6 SID prefix (5f00:fa56:ea00::/48).

~~~~~~
  ┌─────────────────────────────────┐           ┌──────────────────────────────────┐
  │                                 │           │                                  │
  │                                 │           │                                  │
  │                  eBGP speaker   │           │   eBGP speaker                   │
  │           5f00:ffff:fffe::/48   │           │   5f00:fa56:ea00::/48            │
  │   ┌─────┐               ┌────┐  │           │  ┌────┐                ┌─────┐   │
  │   │     ├──────┐        │    ├──┼───────────┼──┤    │        ┌───────┤     │   │
  │   │     │      │        │    │  │           │  │    │        │       │     │   │
  │   └─────┘   ┌──┴──┐     └─┬──┘  │           │  └──┬─┘     ┌──┴──┐    └─────┘   │
  │             │     │       │     │           │     │       │     │              │
  │             │     ├───────┘     │           │     └───────┤     │              │
  │             └─────┘             │           │             └─────┘              │
  │                                 │           │                                  │
  │                                 │           │                                  │
  │                                 │           │                                  │
  │ AS4294967294                    │           │                      AS4200000000│
  └─────────────────────────────────┘           └──────────────────────────────────┘
~~~~~~

Within this structure, appropriate and agreed upon policy may be shared between the partner ASNs. Defining the policy or use cases is outside of the scope of this document.

# Evaluating the Experiment

A survey of participants in the experiment and subsequent evaluation of the results will determine the ease of deployment and operation, or lack thereof, and will inform if further work can be performed to improve ease of implementation and operation.

# Security Considerations

This document does not alter the inherent security posture of SRv6 [RFC8402], [RFC8754].
The SID space prefix was allocated to improve ease of filtering.
Where SRv6 traffic using these prefixes may be shared with cooperating partner networks,
this proposal makes it easier to craft filters that permit only SRv6 traffic from identified ASNs.

# IANA Considerations

This document has no IANA actions.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
