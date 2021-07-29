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

There are currently two current or proposed methods for establishing initial trust, which do not require registrant use of a UI or API.
Those are RFC8078, and draft-thomassen-dnsop-dnssec-bootstrapping.

## RFC8078
RFC8078 proposes several models that a parent may use for managing initial trust. These models assume that it is the parent which will observe any CDS/CDNSKEY signals and it will also be the parent which manages the initial trust solution. 

When a Registry does not support {{!RFC8078}} a Registrar can instead carry out the operations described in {{!RFC8078}} section 3. 

### Accept Policy via Authenticated Channel
The Registrar can use an authenticated channel to receive a notice that a CDS/CDNSKEY exists. Once the notice is received, the process documented in {{!RFC8078}} section 3.1 should be followed.

### Accept with Extra Checks
TODO

### Accept after Delay
TODO

### Accept with Challenge
TODO

### Accept from Inception
TODO

### Draft-thomassen-dnsop-dnssec-bootstrapping

[comment]: # (Depending on the progress of the bootstrap draft we might reduce this text to a short summary)

This draft supports the "bootstrap" method where a DNS operator, to whom the Registrant's domain is delegated, is able to publish the CDS/CDNSKEY records in the Registrant's domain (which will then become DNSSEC signed), and to also publish this initial CDS/CDNSKEY in a special zone operated by the DNS operator, specifically for the purpose of establishing this initial trust.

### Bootstrap Details

The bootstrap zone is a child of the zone of the DNS operator's name servers. It is intended for use exclusively for bootstrapping any domain operated by the DNS operator.

When the Registrant (who is a DNS customer of the operator) decides to enable DNSSEC, the DNS operator's sytems manage all the details. The KSK and ZSK are generated, the zone is signed (by the ZSK), the DNSKEY set is signed (by the KSK), and the CDS and/or CDNSKEY records are created. This is standard RFC7344 processing.

The next step is what differentiates draft-thomassen-dnsop-dnssec-bootstrapping from RFC7344: new entries in the bootstrap zone are created, to associate the CDS and/or CDNSKEY records' RDATA with a hash of the zone name (needed for ensuring total FQDN length is not exceeded), and to map the hashed name back to the original zone name via PTR record.

Since the DNS operator's bootstrap zone is DNSSEC signed, all of the records can be validated.
Since the DNS delegation to the operator's name servers already exists, the trust relationship can be verified at the parent side.
The remaining step is for the Registrar to poll the bootstrap zone periodically, validate the records (including the PTR which contains the domain name), and transform the bootstrap records into the corresponding EPP commands to add DNSSEC for the domain to the corresponding TLD.

Once the initial trust exists, all the rest of RFC7344 can manage key rollovers and even, when appropriate, going insecure.

### Bootstrapping Multi-Signer DNSSEC

The interesting consequence of bootstrap is that a second DNS provider can be added at the Registrar (and submitted as an insecure delegation via EPP), and then the new DNS provider's bootstrap can be recognized to add a second secure delegation, to enable multi-signer DNSSEC.

[comment]: # (I don't quite follow the comments regarding 'insecure delegation' and 'secure delegation' if a single DS is present its secure regardless of whether the second provider's DS is present yet or not.)


Each DNS operator would have its own KSK, and would need to maintain the full set of ZSKs for both providers, in order for both providers' zone instances to pass the DNSSEC validation logic.

[comment]: # (The domain owner can still generate the dnskey rrset on behalf of each operator, so 2x KSK isnt strictly required. although perhaps you wouldnt bother with this bootstrap method if you had that capability)

Thus, the RFC7344 requirements would need to ensure that adding the new DS record (via CDS/CDNSKEY plus bootstrap) only occurs after both DNS operator's zone instances contain both sets of ZSKs, and are signed with their respective KSKs.

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
