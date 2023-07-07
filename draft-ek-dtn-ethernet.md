---
title: "Support for the Delay- and Disruption-Tolerant Networking (DTN) Bundle Protocol and Licklider Transmission Protocol (LTP) Datagrams over Ethernet"
abbrev: "DTN-over-Ethernet"
category: exp

docname: draft-ek-dtn-ethernet-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Transport"
workgroup: "Delay/Disruption Tolerant Networking"
keyword:
 - Delay and Distruption Tolerant Networking
 - DTN
 - Bundle Protocl
 - BP
 - Ethernet
venue:
  group: "Delay/Disruption Tolerant Networking"
  type: "Working Group"
  mail: "dtn@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/dtn/"
  github: "ekline/draft-dtn-ethernet"
  latest: "https://ekline.github.io/draft-dtn-ethernet/draft-ek-dtn-ethernet.html"

author:
 -
    fullname: Erik Kline
    organization: Aalyria Technologies, Inc.
    email: ek@aalyria.com

normative:

informative:

--- abstract

When two Bundle nodes are connected by an Ethernet link, or by a logical
link that emulates Ethernet, it may be possible for a Bundle Protocol
Agent to transmit bundles directly over Ethernet, without higher layer
Convergence-layer Adaptator (CLA) overhead.

This memo specifies a CLA protocol for the encapsulation of Bundles and
Licklider Transmission Protocol (LTP) segments over Ethernet, provides
recommendations for operational considerations, and requests an
EtherType for this protocol.

--- middle

*[BPv6]: RFC5050
*[BPv7]: RFC9171

# Introduction

When two Bundle nodes are connected by an Ethernet link, or by a logical
link that emulates Ethernet, it may be possible for a Bundle Protocol
Agent to transmit bundles directly over Ethernet, without higher layer
Convergence-layer Adaptator (CLA) overhead.

This memo specifies a CLA protocol for the encapsulation of Bundles and
Licklider Transmission Protocol (LTP) segments over Ethernet, provides
recommendations for operational considerations, and requests an
EtherType for this protocol.

The protocol described here is another datagram CLA, like those
specified in {{!RFC7122}}. It is in essence a reduced overhead version
of {{!RFC7122}}'s IP/UDP-based CLA (UDPCL), suitable for use only
among directly-connected nodes (i.e. on-link communications only).

# Conventions and Definitions

<!-- {::boilerplate bcp14-tagged} -->

This specification is experimental and does not make use of normative
language.

# Specification

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      Type     |    Reserved   |            Checksum           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

# Operational Considerations

## Fragmentation and Reassembly

## Congestion Control

## Checksums

# Security Considerations

TODO Security

Bundles transmitted over any link may be subject to observation and alteration.
Security must be applied to the bundle itself, either in the form of BPSec or
at a layer below the transmition of bundles, e.g. 802.1AE MACsec.

# IANA Considerations

## Type field

TODO: IANA registry for Type field.

## Reserved field

No IANA registry is requested for Reserved field values.  This is
deferred to a future specification wishing to utilize this field.

## EtherType

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.

--- fluff

# check for existing EtherType allocations for BP/LTP

[Issue 1](https://github.com/ekline/draft-dtn-ethernet/issues/1) records
the links checked (IANA and IEEE) and the absence of any clear
allocation specifically for use by BP/LTP.
