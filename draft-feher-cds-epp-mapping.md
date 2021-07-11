---
title: "CDS to EPP Mapping"
abbrev: "cdsEPPMapping"
docname: draft-feher-cds-epp-mapping-latest
category: info

ipr: trust200902
area: General
workgroup: TODO Working Group
keyword: Internet-Draft

stand_alone: yes
smart_quotes: no
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: K. Feher
    name: Kal Feher
    organization: None
    email: ietf@feherfamily.org

normative:
  RFC2119:

informative:



--- abstract

TODO Abstract

--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in BCP 14 {{RFC2119}} {{!RFC8174}}
when, and only when, they appear in all capitals, as shown here.

# Background

{{!RFC7344}} specifies how DNS trust can be maintained across key rollovers in-band between parent and child. {{!RFC8078}} added inband signalling DNSSEC status changes and documented 3 use cases. This document describes how the parent zone changes required by those use cases might be implemented via EPP. This document seeks to map the three use cases found in {{!RFC8078}} section 2, to EPP operations.

## Registries and Registrars
Support for CDS/CDNSKEY amongst Internet Registries is likely to remain inconsistent for some time. Therefore practices for updating the parent zone for a registered domain should allow for either the Registry or the sponsoring Registrar to observe a domain's CDS records and subsequent state changes. In most TLDs both the Registrar and Registry are capable of making the changes signaled by CDS records, to the parent zone.

Some Registrars may support CDS even if the Registry does not.

Some Registries may use EPP as the method by which CDS signals are applied to the registry system for subsequent entry into the registered domain's parent zone.


# Registrar Initial Trust Models
RFC8078 proposes several models that a parent may use for managing initial trust. These models assume that it is the parent which will observe any CDS/CDNSKEY signals and it will also be the parent which manages the initial trust solution. 

When a Registry does not support {{!RFC8078}} a Registrar can instead carry out the operations described in {{!RFC8078}} section 3. 

## Accept Policy via Authenticated Channel
The Registrar can use an authenticated channel to receive a notice that a CDS/CDNSKEY exists. Once the notice is received, the process documented in {{!RFC8078}} section 3.1 should be followed.

## Accept with Extra Checks
TODO

## Accept after Delay
TODO

## Accept with Challenge
TODO

## Accept from Inception
TODO
# EPP Commands for CDS/CDNSKEY Use Cases

## Enable DNSSEC Validation
Regardless of the method used to determine that the initial CDS/CDNSKEY signal is trustworthy, the Registrar or Registry SHOULD use the EPP \<update\> command with the secDNS extension documented in {{!RFC5910}} in order to add the DS to the parent. A \<secDNS:add\> sub-element to \<secDNS:update\> MUST be used to indicate that security information is being added.
In the case of a CDS RRset the DS Data Interface MUST be used by including the \<secDNS:dsData\> child element to \<secDNS:update\>.
In the case of a CDNSKEY RRset, the Key Data Interface MUST be used by including the \<secDNS:keyData\> child element to \<secDNS:update\>.
If both CDS and CDNSKEY RRsets are present then a Registrar SHOULD choose the RRset based on the server policy.

TODO EPP Examples

## Roll over the KSK
TODO

## Turn off DNSSEC validation
TODO

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.