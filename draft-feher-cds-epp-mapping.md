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

RFC 7344 specifies how DNS trust can be maintained across key rollovers in-band between parent and child. RFC 8078 added inband signalling DNSSEC status changes and documented 3 use cases. This document describes how the parent zone changes required by those use cases might be implemented via EPP. This document seeks to map the three use cases found in RFC 8078 section 2, to EPP operations.

## Registries and Registrars
Support for CDS/CDNSKEY amongst Internet Registries is likely to remain inconsistent for some time. Therefore practices for updating the parent zone for a registered domain should allow for either the Registry or the sponsoring Registrar to observe a domain's CDS records and subsequent state changes. In most TLDs both the Registrar and Registry are capable of making the changes signaled by CDS records, to the parent zone.

Some Registrars may support CDS even if the Registry does not.

Some Registries may use EPP as the method by which CDS signals are applied to the registry system for subsequent entry into the registered domain's parent zone.


# Use Cases

## Enable DNSSEC Validation

### Accept Policy via Authenticated Channel
TODO

### Accept with Extra Checks
TODO

### Accept after Delay
TODO

### Accept with Challenge
TODO

### Accept from Inception
TODO

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