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
  RFC6996:

informative:
  RFC1897:
  RFC8402:
  RFC8754:
  RFC5398:

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

## SRv6 SID Documentation Prefixes

Using 16-bit and 32-bit ASNs reserved for documentation purposes [IANA-ASNs] yields several SRv6 SID prefixes that might be used for SRv6 documentation purposes.
These prefixes presently include:
...
\[include references from\] [RFC5398].

## SRv6 SID Private Use Prefixes

Using 16-bit and 32-bit ASNs reserved for private use purposes [IANA-ASNs] yields several SRv6 SID prefixes for private use.
These prefixes presently include:
...
\[include references from\] [RFC6996].

# Evaluating the Experiment

Evaluation of the results will determine the ease of deployment and operation, or lack thereof and will inform if further work can be performed to improve ease of implementation and operation.

A survey of experiences of those SRv6 domain operators that opted to participate should be undertaken, and results published.
Feedback highlighting ease of SID allocation or management, improved security through ease of filtering, etc. may all point towards a successful experiment.

As use of the structure proposed here is entirely voluntary, lack of adoption will clearly indicate a failed experiment.
Understanding the reasons for lack of adoption may prove helpful should any further experiments of this sort be undertaken.

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