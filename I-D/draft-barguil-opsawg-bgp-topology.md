---
coding: utf-8

title: A YANG Data Model for Border Gateway Protocol (BGP)

abbrev: BGP Topology YANG
docname: draft-barguil-opsawg-bgp-topology-00
workgroup: OPSAWG Working Group
category: std
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs, comments]

author:
  -
    name: Samier Barguil Giraldo
    org: Telefonica
    email: samier.barguilgiraldo.ext@telefonica.com
  -
    name: Oscar González de Dios
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com
  -
    name: Victor Lopez
    org: Nokia
    email: victor.lopez@nokia.com

#contributor:
#  -


--- abstract

This document defines a YANG data model for representing an abstract view of the provider network topology that contains Border Gateway Protocol (BGP) information. This document augments the 'ietf-network' data model by adding BGP concepts. The aim of the model is to, from a SDN controller perspective, obtain iBGP and MP-BGP topology information from the network and to export it towards NBI interface to the service orchestration layer.

The YANG data model defined in this document conforms to the Network Management Datastore Architecture (NMDA).

--- middle

# Introduction

Topology collection is a critical use case for the network operators because the network topology is an abstract representation of the physical nodes, links and network interconnections. 

Exchanging BGP information between a service orchestration layer and a SDN controller to create services is desirable. The deployment of L3 services with the Layer 3 VPN Network Model (L3NM) {{!RFC9182}} is more accurate if the SDN controller can export BGP topological information, so the customer uses the information of the controller instead of getting it from inventory information. 

The main idea of this model is to represent some of the iBGP and MP-BGP topology attributes. Taking as main consideration all the IETF network characteristics:
  *  Areas can be explicit, depending on which IGP protocol is used.
  *  Metrics can be provided to the operations team for greater control of the network.
  *  A view of the topology of the network built on the basis of the neighbors can be presented
  *  Will allow to inform higher layers e.g. service orchestration layer, about existing connections between domains.

This document defines a YANG data model for representing, managing and controlling the BGP topology. The data model augments ietf-network module {{!RFC8345}} by adding the BGP information.

This document explains the scope and purpose of the BGP  topology model and how the topology and service models fit together.

The YANG data model defined in this document conforms to the Network Management Datastore Architecture {{!RFC8342}}.

## Terminology and Notations 

This document assumes that the reader is familiar with the contents of {{!RFC8345}}, . The document uses terms from those documents.

The terminology for describing YANG data models is found in {{!RFC7950}}, {{!RFC8795}} and {{!RFC8346}}.

Following terms are used for the representation of this data model.

TBD

## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in  {{!RFC2119}}, {{!RFC8174}} when, and only when, they appear in all capitals, as shown here.

## Tree Diagram

Authors include a simplified graphical representation of the data model is used in {{ietf-l3-bgp-topology-tree}} of this document.
The meaning of the symbols in these diagrams is defined in {{!RFC8340}}.

## Prefix in Data Node Names

  In this document, names of data nodes and other data model objects are prefixed using the standard prefix associated with the corresponding YANG imported modules, as shown in the following table.

| Prefix | Yang Module            | Reference    |
| ------ | ---------------------- | ------------ |
| bgpnt  | ietf-l3-bgp-topology      | RFCXXX       |
| yang   | ietf-yang-types        | {{!RFC6991}} |
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document.
Please remove this note.

# YANG Data Model for BGP Topology

## YANG Model Overview

The abstract (base) network data model is defined in the "ietf-network" module of {{!RFC8345}}. The BGP-topology builds on the network data model defined in the "ietf-network" module {{!RFC8345}}, augmenting the nodes with bgp information, which anchor the links and are contained in nodes).

The set of parameters and augmentations are included just a node level. Each parameter and its xpath and description are detailed following:

* Network-types /restconf/data/ietf-network:networks/network/network-types: Its presence identifies the BGP topology type. Thus, the network type MUST be bgp-topology.
+ Local-as /restconf/data/ietf-networks/network/node/l3t:l3-node-attributes/bgp:local-as: Identifies the Local-AS configured in the Network-Element.
+ Neighbors /restconf/data/ietf-network:networks/network/node/l3t:l3-node-attributes/bgp:neighbours: list of Neighbors of the Node. Each Neighbor has the same set of parameters to describe the BGP session.
+ Neighbour neighbour is identified by the IP address. Description for troubleshooting purposes.
+ Peer-As attributes/bgp:neighbours/bgp:peer-as: Autonomous System of the peer. In case of iBGP sessions the Local -As and Peer-As is the same.
- Address-Family: Address Families shared by the nodes in the session. This is a leaf-list, because more than one AFI + SAFI address family can be shared. The options are alligned to the ones available in the IETF-BGP-TYPES (ipv4-unicast, ipv6-unicast, ipv4-labeled-unicast, ...)


{: #ietf-l3-bgp-topology-tree}

# BGP Topology Tree Diagram

{{fig-ietf-l3-bgp-topology-tree}} below shows the tree diagram of the YANG data model defined in module ietf-l3-bgp-topology.yang ({{ietf-l3-bgp-topology-yang}}). 

~~~~
{::include ../Yang/ietf-l3-bgp-topology.tree}
~~~~
{: #fig-ietf-l3-bgp-topology-tree title="BGP Topology tree diagram"}

{: #ietf-l3-bgp-topology-yang}

# YANG Model for BGP topology

This module imports types from {{!RFC8343}} and {{!RFC8345}}.

~~~~
<CODE BEGINS> file "ietf-l3-bgp-topology@2022-03-07.yang"
{::include ../Yang/ietf-l3-bgp-topology.yang}<CODE ENDS>
~~~~
{: #fig-ietf-bgp-topolopy-yang title="BGP Topology YANG module"}

# Security Considerations

    The YANG module specified in this document defines a schema for
    data that is designed to be accessed via network management
    protocols such as NETCONF {{!RFC6241}} or RESTCONF 
    {{!RFC8040}}.  The lowest NETCONF layer is the secure transport
    layer, and the mandatory-to-implement secure transport is Secure
    Shell (SSH) {{!RFC6242}}. The lowest RESTCONF layer is HTTPS, and
    the mandatory-to-implement secure transport is TLS {{!RFC5246}}.

   The NETCONF access control model {{!RFC6536}} provides the means to restrict access for particular NETCONF or RESTCONF users to a    preconfigured subset of all available NETCONF or RESTCONF protocol operations and content.

   There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., config true, which is the default).  These data nodes may be considered sensitive or vulnerable   in some network environments.  Write operations (e.g., edit-config) to these data nodes without proper protection can have a negative effect on network operations.

# IANA Considerations

  This document registers the following namespace URIs in the IETF XML registry {{!RFC3688}}:

    --------------------------------------------------------------------
    URI: urn:ietf:params:xml:ns:yang:ietf-l3-bgp-topology
    Registrant Contact: The IESG.
    XML: N/A, the requested URI is an XML namespace.
    --------------------------------------------------------------------

   This document registers the following YANG module in the YANG Module Names registry {{!RFC6020}}:

    --------------------------------------------------------------------
    name:         ietf-l3-bgp-topology
    namespace:    urn:ietf:params:xml:ns:yang:ietf-l3-bgp-topology
    maintained by IANA: N
    prefix:       ietf-l3-bgp-topology
    reference:    RFC XXXX
    --------------------------------------------------------------------

# Implementation Status

This section will be used to track the status of the implementations of the model. It is aimed at being removed if the document becomes RFC.

--- back

{: numbered="false"}

# Acknowledgments

The authors of this document would like to thank XXX.

This document was prepared using kramdown.
