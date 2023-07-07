---
title: >
  Support for the Delay- and Disruption-Tolerant Networking (DTN)
  Bundle Protocol (BP) Datagrams over Ethernet
abbrev: "BP-over-Ethernet"
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
 - Bundle Protocol
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

This memo describes a mechanism for the transmission of Bundles over
Ethernet links, provides recommendations for operational considerations,
and requests some dedicated Ethernet parameters.

--- middle

# Introduction

When two Bundle nodes are connected by an Ethernet link, or by a logical
link that emulates Ethernet, it may be possible for a Bundle Protocol
Agent to transmit bundles directly over Ethernet, without higher layer
Convergence-layer (CL) overhead.

This memo describes a mechanism for the transmission of Bundles over
Ethernet links, provides recommendations for operational considerations,
and requests some dedicated Ethernet parameters.

The mechanism described here acts like a datagram CL, specifically the
BP over UDP CL documented in section 3.2.2 of {{!DGRAMCL=RFC7122}},
ableit suitable for use only among directly connected nodes
(i.e. on-link communications only).

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# General Recommendation

Paraphrasing {{!DGRAMCL}}, in order to transmit Bundles
({{!BPv6=RFC5050}}, {{!BPv7=RFC9171}})
across the Internet it is necessary to encapsulate them in a
Convergence Layer that utilizes one of the standard versions of the
Internet Protocols (e.g., {{?TCPCL=RFC9174}}).

When two Bundle nodes are directly connected via an Ethernet link,
however, it is possible for Bundle Protocol Agents to forego Internet
CL encapsulations and instead place Bundles directly in payload of
an Ethernet frame.  Section {{<ethertype}} lists the IEEE-assigned
EtherType used to indicate the Ethernet payload is a Bundle (or Bundle
fragment).

This Ethernet Convergence Layer (ETHCL) avoids incurring the IP and UDP
header overhead (28 to 48 bytes, depending on Internet Protocol version
and assuming no other headers or options).  These savings may, however,
be offset by overhead introduced if Bundle fragmentation is necessary
(see {{<mtu}}, {{<fragmentation}}).

## Destination MAC Address

When transmitting a Bundle directly in the payload of an Ethernet frame
a suitable destination MAC address must be selected.  Provisioning the
sending Bundle node with the correct destination MAC address of the
recipient Bundle node is out of scope for this document.  There is no
Bundle Protocol equivalent of {{?ARP=RFC0826}} or {{?ND=RFC4861}}.

It is possible for a sender to address all BP-over-Ethernet listeners
within the broadcast domain should the destination Bundle Endpoint ID
refer to all of a group of nodes (sections 3.2 and 3.4 of
{{!ARCH=RFC4838}}).  How a sending Bundle node determines when this is
appropriate is out of scope of this document.

This document does not prohibit the use of the broadcast MAC address
for this function, but section {{<multicast_mac}} requests allocation
of a multicast MAC address to represent "all Bundle Protocol capable
stations" within a given Ethernet broadcast domain.  This may help
reduce the number of stations awakened by multicast BP-over-Ethernet
frames.

## MTU
{: #mtu}

In the absence of Ethernet-layer fragmentation, no payload exceeding
the local Ethernet MTU can be transmitted.  Consequently, the contents
of the Ethernet payload MUST be complete Bundle, fragmenting at the
sender as necessary ({{!BPv6}} section 5.8, {{!BPv7}} section 5.8).

In practice the need for fragmentation may be reduced if the local
Ethernet MTU can be increased beyond the typical 1500 bytes (e.g., by
use of "jumbo frames").

How a sending Bundle node learns the size of local Ethernet MTU is out
of scope of this document.

# Operational Considerations

Conceptually, this Ethernet Convergence Layer (ETHCL) is analogous to
the BP over UDPCL in section 3.2.2 of {{DGRAMCL}}, with many of the
same limitations and considerations.

## Fragmentation and Reassembly
{: #fragmentation}

Transmission of Bundles exceeding the local Ethernet MTU MUST be
fragmented by the sending node ({{<mtu}}).  If excessive fragmentation
proves problematic, network operators may need to consider alternative
Convergence Layers.

TODO: expound.

## Congestion Control

TODO: same as UDPCL; Ethernet flow control mechanisms exist but may
not prevent the loss of Bundles.

## Checksums

TODO: Frame Check Sequence minimally meets the requirements to ensure
Bundles are not corrupted in transmission.  more checks at other
layers SHOULD be employed.

# Security Considerations

TODO Security

Bundles transmitted over any link may be subject to observation and alteration.
Security must be applied to the bundle itself, either in the form of BPSec or
at a layer below the transmission of bundles, e.g. 802.1AE MACsec.

# IANA Considerations

## Type field

TODO: IANA registry for Type field.

## Reserved field

No IANA registry is requested for Reserved field values.  This is
deferred to a future specification wishing to utilize this field.

## EtherType
{: #ethertype}

IANA is requested to work its IEEE liaison magic to request allocation
of an EtherType for this document's description of Bundle Protocol over
Ethernet (BPoE).

## Multicast MAC Address
{: #multicast_mac}

TODO: request a multicast MAC address for all Bundle Protocol capable
capable stations within the broadcast domain.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.

--- fluff

# check for existing EtherType allocations for BP/LTP

[Issue 1](https://github.com/ekline/draft-dtn-ethernet/issues/1) records
the links checked (IANA and IEEE) and the absence of any clear
allocation specifically for use by BP/LTP.
