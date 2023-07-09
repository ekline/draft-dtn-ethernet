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

This memo describes a mechanism for the transmission of Bundle Protocol
(BP) Bundles over Ethernet links (BP-over-Ethernet), describes
limitations and operational considerations, and requests some dedicated
Ethernet parameters.

--- middle

# Introduction

When two Bundle nodes are connected by an Ethernet link, or by a logical
link that emulates Ethernet, it may be possible for a Bundle Protocol
Agent to transmit Bundles directly in the payload of an Ethernet frame,
without higher layer Convergence Layer (CL) overhead.

This memo describes a mechanism for the transmission of Bundle Protocol
(BP) Bundles over Ethernet links (BP-over-Ethernet), describes
limitations and operational considerations, and requests some dedicated
Ethernet parameters.

The mechanism described here acts like a datagram CL, specifically the
BP over UDP CL documented in section 3.2.2 of {{!DGRAMCL=RFC7122}},
ableit suitable for use only among directly connected nodes
(i.e. on-link communications only).

While hypothetically applicable to a physical Ethernet LAN, it may find
more use within Virtual Private Cloud (VPC) networks, which allow novel
software-define connectivity among a set of cooperating Bundle processing
cloud compute nodes (i.e. VMs).

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
CL encapsulations and instead place Bundles directly in the payload of
an Ethernet frame.  Section {{<ethertype}} lists the IEEE-assigned
EtherType used to indicate the Ethernet payload is a {{!BPv6}}
or {{!BPv7}} Bundle (or Bundle fragment; section {{<mtu}}).

This Ethernet Convergence Layer (ETHCL) avoids incurring the IP and UDP
header overhead (28 to 48 bytes, depending on Internet Protocol version
and assuming no other headers or options).  These savings may, however,
be offset by overhead introduced if Bundle fragmentation is necessary
(see sections {{<mtu}} and {{<fragmentation}}).

## Bundle Protocol Versions

A single EtherType suffices for both {{BPv6}} and {{BPv7}} payloads.
Current Bundle Protocol versions are readily distinguishable by the first
byte of the payload.

Encoding of {{BPv6}} bundles begins with the Version field of the Primary
Bundle Block, which has a fixed value of 0x06 (sections 4, 4.5.1 of {{BPv6}}).

Encoding of {{BPv7}} bundles "SHALL be a concatenated sequence of at least
two blocks, represented as a CBOR indefinite-length array..." (section 4.1 of
{{BPv7}}).  Per {{!CBOR=RFC8949}}, an indefinite-length array begins with the
octet value 0x9f.

All other first octet values indicate some other content.  Bundle Protocol
over Ethernet receivers MUST NOT attempt to interpret such payloads as bundles
and SHOULD log an error for administrator review.

## Destination MAC Address

When transmitting a Bundle directly in the payload of an Ethernet frame
a suitable destination MAC address must be selected.  Provisioning the
sending Bundle node with the correct destination MAC address of the
recipient Bundle node is out of scope for this document.  There is no
Bundle Protocol equivalent of {{?ARP=RFC0826}} or {{?IPv6ND=RFC4861}}.

It is possible for a sender to address all BP-over-Ethernet listeners
within the broadcast domain should the destination Bundle Endpoint ID
refer to "all of a group of nodes" (sections 3.2 and 3.4
of {{!ARCH=RFC4838}}).  How a sending Bundle node determines when this
is appropriate is out of scope of this document.

This document does not prohibit the use of the broadcast MAC address
for this function, but section {{<multicast_mac}} requests the
allocation of a multicast MAC address to represent "all Bundle Protocol
over Ethernet capable stations" within a given Ethernet broadcast
domain.  This may help reduce the number of stations awakened by
multicast BP-over-Ethernet frames.

## MTU
{: #mtu}

In the absence of Ethernet-layer fragmentation, no payload exceeding
the local Ethernet MTU can be transmitted.  Consequently, the contents
of the Ethernet payload MUST be a complete Bundle, employing Bundle
fragmention at the sender as necessary
({{!BPv6}} section 5.8, {{!BPv7}} section 5.8).

In practice the need for fragmentation may be reduced if the local
Ethernet MTU can be increased beyond the typical 1500 bytes, e.g. by
operator-configured use of "jumbo frames" or cloud management tuning of
a virtual Ethernet network.

How a sending Bundle node learns the size of the local Ethernet MTU connected
to a given interface is out of scope of this document.

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

TODO: same as UDPCL: non-existent.  Ethernet flow control mechanisms
exist but may not prevent the loss of Bundles.

## Checksums

TODO: Frame Check Sequence minimally meets the requirements to ensure
Bundles are not corrupted in transmission.  more checks at other
layers SHOULD be employed.

## Filtering

A common security paradigm is to "defaul deny" all traffic patterns that,
broadly, do not conform to operator expectations.  In such environments it
may be that this document's new EtherType needs to be added to an allowlist
or otherwise explicitly permitted to be used on a given Ethernet segment
before Bundles can be successfully delivered.

# Security Considerations

TODO Security

Bundles transmitted over any link may be subject to observation and alteration.
Security must be applied to the bundle itself, either in the form of BPSec or
at a layer below the transmission of bundles, e.g. 802.1AE MACsec.

# IANA Considerations

Allocation of the following Ethernet parameters is requested.

## EtherType
{: #ethertype}

IANA is requested to work its IEEE liaison magic to request allocation
of an EtherType for this document's description of Bundle Protocol over
Ethernet (BPoE).

## Multicast MAC Address
{: #multicast_mac}

In order to identify "all Bundle Protocol over Ethernet capable stations"
within the broadcast domain, IANA is requested to allocate one 48-bit
multicast MAC address, presumably from the 01‑00‑5E OUI.  The stated
Usage is "Bundle Protocol over Ethernet" and the Reference is this document.

--- back

# Acknowledgments
{:numbered="false"}

Thanks to Wes Eddy for advice and early reviews.

--- fluff

# check for existing EtherType allocations for BP/LTP

[Issue 1](https://github.com/ekline/draft-dtn-ethernet/issues/1) records
the links checked (IANA and IEEE) and the absence of any clear
allocation specifically for use by BP/LTP.
