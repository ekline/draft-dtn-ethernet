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

  DGRAMCL: RFC7122

  UDPCLv2: I-D.ietf-dtn-udpcl

  TCPCL: RFC9174

  RFC9542:

  IANA-IEEE802:
    title: "IEEE 802 Numbers"
    author:
      org: IANA
    target: https://www.iana.org/assignments/ieee-802-numbers/

  IEEE802dot3:
    title: "IEEE Standard for Ethernet"
    author:
      org: IEEE
    date: 2018-08
    seriesinfo:
      IEEE: Std 802.3-2018
    target: https://standards.ieee.org/standard/802_3-2018.html

  IEEE802dot1X:
    title: "IEEE Standard for Local and Metropolitan Area Networks--Port-Based Network Access Control"
    author:
      org: IEEE
    date: 2020-02
    seriesinfo:
      IEEE: Std 802.1X-2020

  IEEE802dot1AE:
    title: "IEEE Standard for Local and Metropolitan Area Networks--Media Access Control (MAC) Security"
    author:
      org: IEEE
    date: 2018-12
    seriesinfo:
      IEEE: Std 802.1AE-2018

  DVB-GSE:
    title: "Digital Video Broadcasting (DVB); Generic Stream Encapsulation (GSE) Protocol"
    author:
      org: ETSI
    date: 2007-10
    seriesinfo:
      ETSI: TS 102 606

  3GPP-TS-23.501:
    title: "System Architecture for the 5G System (5GS)"
    author:
      org: 3GPP
    date: 2018-06
    seriesinfo:
      3GPP: TS 23.501 V15.2.0
    target: https://www.3gpp.org/ftp/Specs/archive/23_series/23.501/

  SDA-OCT:
    title: "Optical Communications Terminal (OCT) Standard"
    author:
      org: Space Development Agency
    date: 2024-07
    seriesinfo:
      SDA: OCT Standard Version 4.0.0
    target: https://www.sda.mil/wp-content/uploads/2024/07/SDA_OCT_Standard_4.0.0_final-20240701.pdf

--- abstract

This memo requests allocation of an EtherType and multicast MAC address
for Bundle Transfer Protocol - Unidirectional (BTP-U) to enable its use
as a Convergence Layer on Ethernet networks. This provides an alternative
to IP-based convergence layers for environments where Ethernet forwarding
is operationally feasible but IP routing is unavailable or operationally
undesirable.

--- middle

<!--
-->

# Introduction

This memo requests Ethernet parameters to enable BTP-U [BTP-U] as a
Convergence Layer for environments where Bundle Protocol nodes are
connected by Ethernet or Ethernet-like technologies. Specifically:

- an EtherType to identify frames carrying BTP-U payloads
  ({{<ethertype}})
- a multicast MAC address for neighbor discovery and announcements
  ({{<multicast_mac}})

This convergence layer is applicable to:

- Physical Ethernet LANs
- Virtual Private Cloud (VPC) networks connecting bundle agents
- Ground-Station-as-a-Service (GSaaS) infrastructure
- Technologies supporting Ethernet framing, e.g., DVB-GSE ({{DVB-GSE}}),
  3GPP 5G Ethernet PDU Session types ({{3GPP-TS-23.501}} §5.6.10.2), and
  the US Space Development Agency's Optical Communications Terminal
  standard ({{SDA-OCT}} §3.4.8).

Primary use cases include mission modeling, testbed environments, and
deployments where IP routing is unavailable or adds unnecessary complexity.

# Applicability and Limitations

## BTP-U Protocol Compliance

This document specifies Ethernet encapsulation for {{BTP-U}}. All protocol
requirements, features, and recommendations defined in {{BTP-U}} apply to
this Ethernet profile.

Implementations MUST implement all mandatory {{BTP-U}} features and SHOULD
implement all recommended features, including {{BTP-U}} Segmentation for
handling bundles that exceed the Ethernet MTU.

## Congestion Control
{: #cc}

BTP-U lacks a congestion control mechanism and presumes sending rate is
externally managed. Ethernet flow control mechanisms exist but, may not be
operationally applicable in all situations (e.g. high delay links).

For deployments where congestion control cannot be managed by a mechanism
outside of BTP-U, network operators MUST consider alternate
Convergence Layers.

## Relationship to IP-based Convergence Layers

IP-based convergence layers (TCPCL {{TCPCL}}, UDPCLv2 {{UDPCLv2}}) remain
recommended where IP infrastructure exists. This Ethernet convergence
layer addresses scenarios where:

- no operational IP addressing or routing is available
- only IPv4 or IPv6 link-local addresses exist and peer discovery is
  unspecified
- direct Ethernet operation simplifies deployment and management

Header overhead savings (28-48+ bytes) are secondary to operational
utility in non-IP environments.

<!-- XXX -->

# Conventions and Definitions

{::boilerplate bcp14-tagged}

As this memo is Informational it uses BCP14 langauge only for clarity.

# Assignment Considerations

Allocation of the following Ethernet parameters is requested.

## IEEE Assignment Considerations

### EtherType
{: #ethertype}

(per {{RFC9542}})
The IESG is requested to approve applying to the IEEE
Registration Authority for an EtherType for BTP-U.  (The IESG
should communicate its approval to IANA and to those concerned
with this document.  IANA will forward the IESG Approval to the
registry expert of the "EtherType" registry from the "IEEE 802
Numbers" registry group who will make the application to the
IEEE Registration Authority, keeping IANA informed.)

(if approved)
The following entry has been added to the "ETHER TYPES" subregistry
of the "IEEE 802 Numbers" registry {{IANA-IEEE802}}:

   Ethertype (decimal): YYYY

   Ethertype (hex): YYYY

   Exp. Ethernet (decimal): -

   Exp. Ethernet (octal): -

   Description: BTP-U payloads

   References: RFC ZZZZ (this document)

## IANA Considerations

### Multicast MAC Address
{: #multicast_mac}

One multicast MAC address is requested to enable neighbor discovery and
bundle availability announcements within a broadcast domain. This allows
BTP-U senders to reach all capable receivers without prior knowledge of
individual MAC addresses.

Following the recommended format given as the EUI-48 Identifier template
in {{RFC9542}}:

Applicant Name: IETF DTN Working Group

Applicant Email: dtn@ietf.org

Applicant Telephone: (none)

Use Name: Bundle Transfer Protocol - Unidirectional

Document: {{BTP-U}}

This memo is an application for one multicast EUI-48 identifier.

# Operational Considerations

## Checksums

To reiterate the observation in §3.5 of {{DGRAMCL}}, the Bundle Protocol
specifications assume that Bundles "are transmitted over an erasure
channel, i.e., a channel that either delivers packets correctly or not at
all".

Ethernet's Frame Check Sequence (FCS) minimally meets this requirement to
ensure Bundles are not corrupted in transmission.  Use of stronger integrity
checks are left to BTP-U and its extensions.

## BTP-U Virtual Channels

{{BTP-U}} Transfer Numbers are unique within a "virtual channel." For
Ethernet, the virtual channel is identified by:

- Source MAC address (6 octets)
- Destination MAC address (6 octets)
- C-VLAN ID (12 bits, if 802.1Q tag present)

Each unique combination defines a separate virtual channel with
independent Transfer Number sequencing.

Other technologies that support Ethernet-like framing and use of this
EtherType ({{<ethertype}}) but which lack the above virtual channel
identifers MUST define the equivalent virtual channel identifiers,
e.g. technology-specific source and/or destination as well as any
protocol-specific channel discriminators.

When using the multicast MAC address for transmitting to receivers
whose MAC addresses have not yet been learned, all receivers in the
broadcast domain share the same destination address (and therefore the
same virtual channel). Implementations relying on multicast SHOULD use
additional mechanisms, i.e. the Bundle destination EID, to determine
whether received bundles are intended for local processing. Once a
sender learns a peer's MAC address, implementations SHOULD switch to
unicast transmission.

## MTU and Jumbo Frames

Implementations MUST support transmission and reception of frames with
payload sizes up to 1500 octets (standard Ethernet MTU minus Ethernet
header), as required by {{IEEE802dot3}}.

Implementations MAY support jumbo frames with payload sizes up to 9000
octets or larger, but SHOULD only enable this capability when explicitly
configured where operators have verified that the network path supports
the larger frame size.

Since {{BTP-U}} has no path MTU discovery mechanism and Ethernet networks
silently drop oversized frames, implementations SHOULD default to 1500
octets. Link Layer Discovery Protocol (LLDP) MAY be used to discover
directly-connected link and neighbor parameters.

# Security Considerations

This document requests assignment of an EtherType and Multicast MAC address
for BTP-U datagrams.  It has no incremental implications for security
beyond those already documented in {{BPv7}} and {{BTP-U}}.

## Denial of Service

BTP-U assumes the sending rate is controlled by a mechanism out of scope for
the protocol and has no built-in mechanism for identifying or mitigating any
congestion a sender might cause. Use of this protocol on some networks,
a shared LAN segment for example, may cause a Denial-of-Service by
flooding Ethernet switches and stations.

## Link-layer Security

Any attacker with access to the link, or with sufficient knowledge of local
Bundle forwarding configuration so as to inject BTP-U frames and cause them
to be sent to an Ethernet peer, may overwhelm the receiver to the point of
Denial of Service to other onlink senders.

IEEE standards include several security mechanisms that may be used in
Ethernet networks.  Examples of recommended Ethernet-level security
mechanisms a network might deploy include: IEEE 802.1X ({{IEEE802dot1X}}),
which may be used restrict access to the link to authorized participants,
and IEEE 802.1AE ({{IEEE802dot1AE}}), which offers confidentiality of
the entire BTP-U payload.

## Filtering

A common security paradigm is to "default deny" all traffic patterns that,
broadly, do not conform to operator expectations.  In such environments the
BTP-U EtherType needs to be explicitly permitted to be used on a given
Ethernet segment before BTP-U messages can be successfully transmitted.

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

