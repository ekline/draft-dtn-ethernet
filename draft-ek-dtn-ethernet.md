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

TODO Abstract

Where two Bundle Protocol endpoints are connected by an Ethernet link, or by
a link that emulates Ethernet, it may be possible to transmit bundles directly
without additional converge layer adaptator (CLA) overhead.  This memo requests
an EtherType for bundles and describes how bundles may be transmitted similar
to existing datagram CLAs.

--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# General Recommendation

## Fragmentations

## Congestion Control

## Checksums

# Security Considerations

TODO Security

Bundles transmitted over any link may be subject to observation and alteration.
Security must be applied to the bundle itself, either in the form of BPSec or
at a layer below the transmition of bundles, e.g. 802.1AE MACsec.

# IANA Considerations

This document has no IANA actions.

## EtherType

## Multicast MAC Address

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
