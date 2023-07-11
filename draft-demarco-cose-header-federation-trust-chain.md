---
title: "COSE Header Parameter for Carrying OpenID Federation 1.0 Trust Chains"
abbrev: "COSE Trust Chains"
category: std

docname: draft-demarco-cose-header-federation-trust-chain-latest
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "CBOR Object Signing and Encryption"
keyword:
 - OpenID Connect Federation
 - COSE
venue:
  group: "CBOR Object Signing and Encryption"
  type: "Working Group"
  mail: "cose@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/cose/"
  github: "peppelinux/draft-demarco-cose-header-federation-trust-chain"

author:
 -
    fullname: Giuseppe De Marco
    organization: independent
    email: demarcog83@gmail.com

normative:
  RFC7517: RFC7517
  RFC7519: RFC7519
  RFC8152: RFC8152
  RFC8610: RFC8610
  RFC8949: RFC8949
  RFC9052: RFC9052
  RFC9053: RFC9053
  RFC9360: RFC9360

  OIDC-FED:
    title: "OpenID Connect Federation 1.0"
    author:
      -
        ins: R. Hedberg
        name: Roland Hedberg
      -
        ins: M.B. Jones
        name: Michael Jones
      -
        ins: A.Å. Solberg
        name: Andreas Solberg
      -
        ins: J. Bradley
        name: John Bradley
      -
        ins: G. De Marco
        name: Giuseppe De Marco
      -
        ins: V. Dzhuvinov
        name: Vladimir Dzhuvinov

informative:


--- abstract

The CBOR Object Signing and Encryption (COSE) [RFC9053]
message structure uses message headers to give references to elements
that are needed for the security and verifiability of the message, such as
algorithms and keys.

OpenID Connect Federation 1.0 [OIDC-FED] is a general purpose
attestation mechanism to obtain verifiable
metadata and cryptographic keys.

This document defines a new COSE header parameter to
identify and transport an OpenID Federation 1.0 Trust Chain.

--- middle

# Introduction

The Internet Standards [RFC8152] and [RFC9052] defines how to transport
symmetric keys in the COSE headers, and are extended by [RFC9360]
to transport X.509 certificates for the requirements
of identification and cryptographic key attestation
of a third party.

There are some cases where obtaining proof of a third party's identity
through key attestation and cryptographic signature verification
is not enough, cases where the solution requirements include
attestation of metadata, proofs of compliance and policies.

In these cases, it would be necessary to extend the X.509 certificates
with policies, metadata and other information required by the
interoperability schemes or by a trust framework.

OpenID Connect Federation 1.0 [OIDC-FED]
standard allows the exchange of metadata, roles, trust marks, policies
and public keys, in a certifiable and non-repudiable secure way.

OIDC Federation 1.0 [OIDC-FED] allows the construction of a trust
infrastructure in which even X.509 certificates can be published
within the Entity Statements that make up
the federation Trust Chain. This flexibility allows an infrastructure
based on OIDC Federation 1.0 to guarantee the security of the
solutions, the historical verifiability of the signatures, and
the revocation mechanisms without
the requirement to implement
CRL or OCSP technologies, where X.509 requires it.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Audience Target audience/Usage

The audience of the document is implementers that require a high level
of security for the attestations of metadata, cryptographic keys
and policies.

# Scope

This specification defines how a [OIDC-FED]
Trust Chain is made available within the COSE headers.

## Out of Scope

The following items are out of scope for the current version of this document:

* X.509 publication over a [OIDC-FED] Infrastructure, this can be achieved
  using `x5c` or `x5u` as defind in [RFC7517]
* Metadata schemas, OIDC Federation allows the definition of custom
  metadata schemas even for entities not belonging
  to OAuth and OpenID ecosystems.

# Terminology

This specification uses the terms "Trust Chain", "Trust Anchor",
"Intermediate", "Trust Mark" and "Entity Statement" as defined in [OIDC-FED].

# The Scope of Trust Chain COSE Header Parameter

The use of OIDC Federation Trust Chain enables a trust infrastructure
with full suites of Trust Anchors, Intermediates, status and revocation
checking, Trust Marks and metadata policies that have been defined
in [OIDC-FED].

The Concise Binary Object Representation (CBOR) key
structures [RFC8949] and Header Parameters for Carrying and
Referencing X.509 Certificates [RFC9360] that have been defined
in COSE currently do not
support all the properties made available in [OIDC-FED].

# Requirements

If the application cannot establish trust to the cryptographic keys
or metadata made available and verified within the Trust Chain, the public
key and the metadata MUST NOT be used.

When Trust Chain parameter is used, the parameter KID defined in [RFC9052]
MUST be used. KID allows an efficient matching to the key to be used for
signature verification.

# Trust Chain COSE Header Parameter

The header parameter defined is `trustchain`, described below:

trustchain:  This header parameter contains an ordered array of strings,
representing federation Entity Statements encoded as signed
Json Web Tokens [RFC7519]. How the Entity Statements are ordered is defined
in [OIDC-FED].

The trust mechanism used to process any Entity Statements
is defined in [OIDC-FED].

The header parameter can be used in the following locations:

COSE_Signature and COSE_Sign1 objects:  In these objects, the
  parameters identify the Trust Chain to be used for obtaining the
  key needed for validating the signature, any needed metadata for
  interoperability purpose, any metadata policy
  and any required Trust Marks for administrative compliaces.

The labels assigned to the header parameter can be found in Table 1.

     +=============+=======+=================+=====================+
     | Name        | Label | Value Type      | Description         |
     +=============+=======+=================+=====================+
     | trustchain  | 27    | COSE_TRUSTCHAIN | OpenID Connect      |
     |             |       |                 | Federation 1.0      |
     |             |       |                 | Trust Chain         |
     +-------------+-------+-----------------+---------------------+

               Table 1: TRUST CHAIN COSE Header Parameters

Below is an equivalent Concise Data Definition Language (CDDL)
description (see [RFC8610]) of the text above.

```
COSE_TRUSTCHAIN = [ N * jws :bstr ]
```

The variable N represent the number of Entity Statements
that a Trust Chain can contain.
The contents of "bstr" are the bytes representing a JWS.

# IANA Considerations

TBD

--- back

# Acknowledgments
{:numbered="false"}

TBD
