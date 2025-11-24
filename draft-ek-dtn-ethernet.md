---
title: >
  Assignment of Ethernet Parameters for Bundle Transfer Protocol -
  Unidirectional (BTP-U) over Ethernet
abbrev: "BTPU-over-Ethernet"
category: info

docname: draft-ek-dtn-ethernet-latest
submissiontype: IETF
number:
date:
consensus: false
v: 3
area: "Internet"
workgroup: "Delay/Disruption Tolerant Networking"
keyword:
 - Delay and Distruption Tolerant Networking
 - DTN
 - Bundle Protocol
 - BP
 - BTP-U
 - Ethernet
 - BTPoE
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
    email: ek.ietf@gmail.com

normative:
informative:
  BPv7: RFC9171
  BTP-U: I-D.ietf-dtn-btpu
#  IANA-IEEE802:
#    author:
#     -
#        org: IANA
#    title: "IEEE 802 Numbers",
#    target: http://www.iana.org/assignments/ieee-802-numbers
#  IEEE-ETHERTYPES:
#    author:
#     -
#        org: IEEE
#    title: "EtherType"
#    target: http://standards-oui.ieee.org/ethertype/eth.txt

#  DVB-GSE:
#    author:
#     -
#        org: DVB
#    title: "TODO"
#    target: http://todo.example/find-suitable-biblio-link
  DGRAMCL: RFC7122
  TCPCL: RFC9174
  RFC9542:
#  IEEE802dot3:
#    author:
#      -
#        org: IEEE
#    title: "IEEE Standard for Ethernet, IEEE Std 802.3-2018 (or latest edition)"
#    target: https://standards.ieee.org/standard/802_3-2018.html

--- abstract

This memo requests Ethernet parameters for Bundle Transfer Protocol -
Unidirectional {{BTP-U}} for use on Ethernet and Ethernet-like links.  This
is not intended to replace existing IP-based Convergence Layer (CLs).
Rather this makes it possible to use Ethernet with BTP-U as a
CL in environments where Ethernet-like operation better matches the
network infrastructure or operational constraints.

--- middle

<!--
-->

# Introduction

When two Bundle nodes are connected by an Ethernet link, or by a technology
that emulates Ethernet, it may be possible for a Bundle Protocol
Agent to transmit Bundles in the payload of an Ethernet frame without
higher layer protocol Convergence Layer (CL) overhead.  Examples of
"Ethernet-like" Technologies include Digital Video Broadcast Generic
Stream Encapsulation ([DVB-GSE]).

<!--
TODO: find some more biblio references other than just GSE for Ethernet-like
links.
-->

This memo recommends use of Bundle Transfer Protocol - Unidirectional
[BTP-U] for this purpose and requests some Ethernet parameters to support
this.
Specifically, it requests:
an EtherType for identifying frames carrying BTP-U payloads ({{<ethertype}})
and a multicast MAC address, for indicating the frame is addressed to all
BTP-U capable receivers ({{<multicast_mac}}).

While hypothetically applicable to a physical Ethernet LAN, it may be better
utilized within Virtual Private Cloud (VPC) networks, which allow novel
software-define connectivity among a set of cooperating Bundle processing
cloud compute nodes (i.e. VMs).  Such deployments can be useful for mission
modeling, testbed activities, and may also integrate well with some
Ground-Station-as-a-Service (GSaaS) network infrastructure.

## Congestion Control
{: #cc}

BTP-U lacks a congestion control mechanism and presumes the sending rate is
controlled by a lower layer or mechanism otherwise out of scope for BTP-U.

Ethernet flow control mechanisms exist but, even if in use, they may not be
sufficient to avoid significant loss of BTP-U frames in all situations.
Additionally, a Bundle Protocol Agent may not be able to easily determine
whether any Ethernet-level flow control mechanism is available.

For deployments where congestion control cannot be managed by a mechanism
outside of BTP-U, network operators must consider alternate
Convergence Layers.

## Relationship to Other Convergence Layers

This "Ethernet Convergence Layer" is not intended to replace IP-based CLs
where their use would be more appropriate. While use of direct encapsulation
within an Ethernet frame avoids incurring some IP and UDP/TCP header
overhead (28 to 48 bytes, or more, depending on Internet Protocol version
and other options).  These savings, however, are not the primary motivation.
Rather, some Bundle Protocol deployments utilize links which may not have
any operational IP addressing or routing.

Convergence Layers like {{TCPCL}} and {{DGRAMCL}} have many useful features
and remain recommended wherever deployable.  This may include some links
where only IPv6 Link-Local Addresses are available, though how a Bundle
Protocol Agent obtains the IPv6 Link-Local Address of a peer is a
deployment-specific matter.

<!--
# Conventions and Definitions

{::boilerplate bcp14-tagged}
-->

# Assignment of an EtherType by IEEE

The IESG is requested to approve applying to the IEEE Registration Authority
for an EtherType for BTP-Y.  The IESG should communicate its approval to
IANA and to those concerned with this document.  IANA will forward the IESG
Approval to the registry expert of the "EtherType" registry from the "IEEE
802 Numbers" registry group who will make the application to the IEEE
Registration Authority, keeping IANA informed.

# IANA Considerations

Allocation of the following Ethernet parameters is requested.

## EtherType
{: #ethertype}

(if approved)
The following entry has been added to the "ETHER TYPES" subregistry
of the "IEEE 802 Numbers" registry [IANA-IEEE802]:

   Ethertype (decimal): YYYY
   Ethertype (hex): YYYY
   Exp. Ethernet (decimal): -
   Exp. Ethernet (octal): -
   Description: BTP-U payloads
   References: RFC ZZZZ (this document)

## Multicast MAC Address
{: #multicast_mac}

In order to identify "all Bundle Transfer Protocol - Unicast over Ethernet
capable receivers" within a broadcast domain, IANA is requested to allocate
one Multicast MAC address.

Following the recommended format given as the EUI-48 Identifier template
in [RFC9542]:

Applicant Name: IETF DTN Working Group

Applicant Email: dtn@ietf.org

Applicant Telephone: (none)

Use Name: Bundle Transfer Protocol - Unidirectional

Document: {{BTP-U}}

This memo is an application for one multicast EUI-48 identifier.

# Operational Considerations

In addition to issue around congenstion control and lack of feedback about
excess sending rate noted above ({{<cc}}), some addition operational
considerations are noted below.

## Checksums

To reiterate the observation in §3.5 of {{DGRAMCL}}, the Bundle Protocol
specifications assume that Bundles "are transmitted over an erasure
channel, i.e., a channel that either delivers packets correctly or not at
all".

Ethernet's Frame Check Sequence (FCS) minimally meets this requirement to
ensure Bundles are not corrupted in transmission.  Use of stronger integrity
checks are left to BTP-U.

<!--
## Session Identification

given a datagram in which the BTP-U Ethertype address appears, a session
is identified by the logical source and destination associated with that
datagram.  In the case of an Ethernet frame, the Source MAC, Destination
MAC, and optional C-VLAN ID (if present) comprise the identification of
a unique Convergence Layer session.

Other situations in which an Ethertype might appear are out of scope of
this document.  Nevertheless, a CL session is conceptually uniquely
identified by the session-identifying attributes of the context in which
the Ethernet type appears. For example: GRE (1701)...
-->

## MTU and Jumbo Frames

Implementations must support transmission and reception of frames with
payload sizes up to 1500 octets (standard Ethernet MTU minus Ethernet
header), as required by [IEEE802dot3].
Implementations may support jumbo frames with payload sizes up to 9000
octets or larger, but should only enable this capability when explicitly
configured by operators who have verified that the network path supports
the larger frame size.

MTU mismatches in Ethernet networks result in frame drops,
and {{BTP-U}} does not have any mechanism to probe for Path MTU.
Implementations may use Ethernet-specific protocols, like
Link Layer Discovery Protocol (LLDP),
to discover supported frame sizes on directly connected links, but should
default to conservative MTU values (1500 octets).

## Fragmentation and Segmentation

When transmitting Bundles that exceed the Ethernet CL's MTU, a BP agent
must decide how to break up a Bundle into multiple transmissible frames.
Both {{BPv7}} Fragmentation and {{BTP-U}} Segmentation may be viable
options.  However, Bundle Fragmentation may not always result in a
transmissible frame: Bundle Processing Control Flags may prohibit
fragmentation, and Block Processing Control Flags may require extension
blocks to be replicated with every fragment, either of which may result
in Bundle Fragments that exceed the Ethernet MTU.
For this reason, {{BTP-U}} Segmentation is recommended.

## Filtering

A common security paradigm is to "default deny" all traffic patterns that,
broadly, do not conform to operator expectations.  In such environments it
may be that the BTP-U EtherType needs to be added to an allowlist
or otherwise explicitly permitted to be used on a given Ethernet segment
before BTP-U messages can be successfully delivered.

# Security Considerations

This document requests assignment of an EtherType and Multicast MAC address
for BTP-U datagrams.  It has no incremental implications for security
beyond those in the relevant protocols.

BTP-U assumes the sending rate is controlled by a mechanism out of scope for
the protocol and has no builtin mechanism for identifying or mitigating any
congestion a sender might cause. Use of this protocol on some networks,
a shared LAN segment for example, may cause a Denial-of-Service by
flooding Ethernet switches and stations.

Any attacker with access to the link, or with sufficient knowledge of local
Bundle fordwarding configuration so as to inject BTP-U frames and cause them
to be sent to an Ethernet peer, may overwhelm the receiver to the point of
Denial of Service to other onlink senders.

IEEE standards include several security mechanisms that may be used in
Ethernet networks.  Examples of recommended Ethernet-level security
mechanisms a network might deploy include: IEEE 802.1X (TODO: reference),
which may be used restrict access to the link to authorized participants,
and IEEE 802.1AE (TODO: reference), which would offers confidentiality of
the entire BTP-U payload.

--- back

# Acknowledgments
{:numbered="false"}

Thanks to
Wes Eddy,
Jeorg Ott,
Brian Sipos,
and
Rick Taylor
for numerous discussions and contributions.

