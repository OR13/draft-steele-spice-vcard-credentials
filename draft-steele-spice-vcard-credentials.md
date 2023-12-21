---
title: "vCard Credentials"
abbrev: "SpicyVc"
category: info

docname: draft-steele-spice-vcard-credentials-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Secure Patterns for Internet CrEdentials"
keyword:
 - vcard
 - contacts
 - digital credentials
venue:
  group: "Secure Patterns for Internet CrEdentials"
  type: "Working Group"
  mail: "spice@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/spice/"
  github: "OR13/draft-steele-spice-vcard-credentials"
  latest: "https://OR13.github.io/draft-steele-spice-vcard-credentials/draft-steele-spice-vcard-credentials.html"

author:
 -
    fullname: "Orie Steele"
    organization: Transmute
    email: "orie@transmute.industries"

normative:
  RFC3986: URI
  RFC6350: VCARD
  RFC7516: JWE
  RFC9052: COSE
  RFC9285: BASE45

informative:
  IANA.media-types: MEDIA-TYPES

  RFC9278: JWK-THUMBPRINT-URI

  I-D.draft-ietf-oauth-selective-disclosure-jwt: SD-JWT
  I-D.draft-ietf-oauth-sd-jwt-vc: SD-JWT-VC

  CWE-502:
    title: Deserialization of Untrusted Data
    target: https://cwe.mitre.org/data/definitions/502.html
    date: false

  W3C.VC-DATA-MODEL-2.0:
    title: Verifiable Credentials Data Model v2.0
    target: https://www.w3.org/TR/vc-data-model-2.0/
    date: false



--- abstract

vCard is a file format for digital business cards.
This document enables {{-VCARD}} to be used as a transport for digital credentials.


--- middle

# Introduction

Public key based identity systems require users to obtain cryptographic keys, and use them to verify digital signatures produced by a private key, or decrypt data with a private key that was encrypted to a public key.

The public key cryptographic operations are a foundational building block for building secure systems capable of providing confidentiality, integrity and authenticity.

Applications supporting protocols such as OAUTH, OpenSSH, and OpenPGP rely on different public key formats.

To support interoperability public key formats are registered at {{-MEDIA-TYPES}}.

A common challenge in working with applications that require key management is obtaining cryptographic keys.

This document describes how {{-VCARD}} can address this challenge, for keys that are embedded by value or by reference in digital business cards.

# Terminology

{::boilerplate bcp14-tagged}

# Transports

vCARDs can be transported over various channels, including through NFC and QR Codes, as decribed in {{-BASE45}}.

## Optical

QR Codes are well supported in modern smartphones and frequently displayed on physical products, providing additional information to consumers.

Anyone who can see a QR Code can access the data encoded in it.

Sensitive plaintext data MUST NOT be encoded in vCARD QR Codes.

Encryption formats, such as {{-JWE}} MAY be be used to secure confidential credentials.

QR Codes can be removed, altered, or replaced on physical products.

Additional security checks MUST be performed before accepting any data transported by QR Codes.

For example, confirming serial numbers or other presented identifiers identifiers are consistent with the credentials presented through the vCard QR Code.

~~~aasvg
 ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ ◌ * ◌ * ◌ ◌ * * ◌ * * ◌ * * ◌ ◌ * * ◌ * ◌ ◌ ◌ * * * ◌ * * ◌ ◌ ◌ * ◌ ◌ ◌ ◌ * * * ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ ◌
 ◌ * * * * * ◌ * ◌ * * ◌ ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * * ◌ * * ◌ * ◌ ◌ ◌ * * ◌ * * * * * * * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * ◌ * * * * * ◌
 ◌ * ◌ ◌ ◌ * ◌ * * ◌ * * * ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ * ◌ * ◌ * * * ◌ ◌ * * * ◌ ◌ ◌ * * * * ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ ◌ ◌ * ◌
 ◌ * ◌ ◌ ◌ * ◌ * * * ◌ * ◌ * * * * * ◌ * * ◌ * ◌ ◌ ◌ * ◌ ◌ ◌ * * ◌ * ◌ ◌ ◌ * * ◌ * ◌ * ◌ * ◌ ◌ ◌ * * ◌ * ◌ * ◌ * ◌ ◌ ◌ * ◌
 ◌ * ◌ ◌ ◌ * ◌ * ◌ * ◌ * ◌ ◌ ◌ ◌ * * ◌ * ◌ ◌ * ◌ * * * ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ * * * * * ◌ * * * ◌ * * ◌ ◌ ◌ ◌ ◌ * * ◌ * ◌ ◌ ◌ * ◌
 ◌ * * * * * ◌ * ◌ * ◌ * ◌ * ◌ ◌ ◌ * * ◌ * ◌ ◌ ◌ ◌ * ◌ ◌ ◌ * * * ◌ ◌ * * ◌ * * * ◌ * ◌ * ◌ ◌ * * ◌ ◌ ◌ * * * ◌ * * * * * ◌
 ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ ◌
 * * * * * * * * * * ◌ * ◌ ◌ * * * * ◌ ◌ ◌ ◌ ◌ * * * ◌ ◌ ◌ * * * ◌ ◌ * ◌ ◌ * ◌ * ◌ ◌ ◌ * ◌ ◌ * ◌ * * ◌ * * * * * * * * * *
 ◌ ◌ ◌ ◌ * ◌ ◌ * * ◌ * ◌ * ◌ ◌ * ◌ * * * * * * ◌ ◌ ◌ * * ◌ ◌ ◌ ◌ ◌ ◌ * * ◌ * ◌ * * * * ◌ * ◌ ◌ * ◌ ◌ ◌ ◌ * ◌ * ◌ ◌ * * ◌ ◌
 * ◌ ◌ ◌ ◌ * * ◌ * * ◌ ◌ * ◌ ◌ * ◌ ◌ * ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ ◌ * ◌ ◌ * * ◌ * * ◌ ◌ * ◌ ◌ * ◌ * ◌ ◌ ◌ * ◌ * ◌ * * ◌ ◌ * ◌ ◌ ◌ *
 ◌ ◌ ◌ * * * ◌ ◌ * * ◌ ◌ ◌ ◌ * ◌ ◌ ◌ * * * ◌ ◌ ◌ ◌ * ◌ * * ◌ ◌ ◌ * * ◌ * ◌ * * ◌ ◌ * * ◌ * * ◌ ◌ ◌ * ◌ ◌ ◌ * * * ◌ ◌ ◌ ◌ ◌
 ◌ ◌ ◌ * ◌ ◌ * ◌ ◌ * ◌ ◌ * * ◌ ◌ * ◌ * ◌ * * * ◌ * * ◌ * ◌ * ◌ ◌ ◌ * ◌ ◌ * ◌ * * * ◌ ◌ ◌ ◌ ◌ * * ◌ * ◌ * ◌ ◌ * * ◌ ◌ ◌ ◌ *
 ◌ ◌ ◌ * ◌ * ◌ * ◌ * * * ◌ * ◌ * * ◌ * * * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * ◌ ◌ * ◌ * ◌ * * * * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ * ◌ * ◌ * ◌ * * ◌ ◌
 * * * ◌ ◌ * * * * * ◌ ◌ * * ◌ * * ◌ ◌ * * ◌ * * ◌ * ◌ * * * * * ◌ ◌ ◌ * ◌ * ◌ ◌ * * * * * ◌ ◌ ◌ * * * * * * * * ◌ ◌ ◌ * *
 ◌ ◌ ◌ * * ◌ ◌ * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * * * ◌ * * * * ◌ * * ◌ ◌ ◌ * ◌ ◌ * ◌ ◌ * * * ◌ * * * ◌ ◌ * * ◌ * ◌ * ◌ * * ◌ ◌ ◌ *
 * ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * * ◌ ◌ * ◌ * * * * ◌ ◌ ◌ ◌ * * * ◌ * * ◌ * ◌ * * ◌ * ◌ * * * ◌ ◌ * ◌ ◌ * ◌ * ◌ ◌ * ◌ * * ◌
 * * ◌ * * * ◌ * ◌ * ◌ * ◌ * ◌ ◌ * ◌ * * ◌ * ◌ ◌ ◌ * * * ◌ * ◌ * * * ◌ ◌ ◌ * ◌ ◌ ◌ * * * ◌ * ◌ * * ◌ * * ◌ * ◌ * ◌ * * * ◌
 * ◌ ◌ * ◌ ◌ * ◌ ◌ * * * * ◌ ◌ * ◌ * ◌ * ◌ ◌ ◌ * * ◌ * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ * * ◌ ◌ * * * * ◌ ◌ * ◌ * ◌ * ◌ ◌ ◌ * * ◌ * * *
 * ◌ ◌ * * * ◌ ◌ * ◌ ◌ * ◌ * ◌ ◌ ◌ * ◌ * ◌ * * * ◌ ◌ * ◌ ◌ * ◌ ◌ ◌ * * * ◌ * ◌ ◌ * ◌ * ◌ ◌ * * ◌ ◌ * ◌ * ◌ * * * ◌ ◌ * ◌ ◌
 ◌ ◌ ◌ * ◌ ◌ * ◌ * ◌ ◌ * * * ◌ ◌ * * ◌ ◌ * ◌ ◌ ◌ ◌ * * ◌ ◌ * * * ◌ * ◌ * * ◌ ◌ * * ◌ ◌ * * * * ◌ * * ◌ ◌ ◌ * * * ◌ * * ◌ ◌
 ◌ ◌ * ◌ ◌ * ◌ * ◌ ◌ * * ◌ ◌ ◌ ◌ * ◌ * * * ◌ * * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * * ◌ * * ◌ ◌ * * * ◌ ◌ * * ◌ * ◌ * ◌ ◌ * ◌ * ◌ ◌ *
 * * ◌ * * ◌ * * ◌ * ◌ ◌ ◌ * ◌ * * ◌ ◌ * * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * * ◌ ◌ * * * * ◌ * * ◌ * * * ◌ ◌ ◌ * * ◌ ◌ * ◌ * ◌ * * * * ◌ ◌
 * * ◌ ◌ ◌ ◌ ◌ * * ◌ ◌ ◌ * * ◌ ◌ * * ◌ * * ◌ * * * ◌ * ◌ * * * * ◌ * ◌ * * ◌ ◌ * * ◌ ◌ * * ◌ ◌ ◌ * * * ◌ * ◌ * * * ◌ ◌ * *
 ◌ * * * ◌ * * ◌ * * ◌ * ◌ * ◌ ◌ ◌ * * ◌ ◌ * ◌ * * ◌ * * ◌ * ◌ * * ◌ ◌ * ◌ * * * * ◌ * ◌ * ◌ * ◌ ◌ * * ◌ ◌ * ◌ ◌ * ◌ ◌ * *
 * * ◌ * ◌ * ◌ ◌ * ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ * * * ◌ ◌ * * ◌ ◌ ◌ * ◌ * * ◌ * ◌ ◌ * ◌ * * * ◌ * ◌ * ◌ * ◌ ◌ ◌ * * * ◌ ◌ * * ◌ * * *
 ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * ◌ * ◌ * * ◌ * ◌ * * * ◌ ◌ * * ◌ * * * * * ◌ * ◌ * ◌ ◌ ◌ * ◌ ◌ * ◌ ◌ * * * ◌ * * ◌ ◌ ◌ * ◌ * * ◌ ◌ ◌ *
 * ◌ ◌ * * ◌ ◌ * ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ * ◌ * ◌ * ◌ ◌ * ◌ ◌ ◌ ◌ * * ◌ ◌ * * * ◌ * * * ◌ ◌ * * ◌ * * * ◌ * ◌ * ◌ * ◌ ◌ * ◌ * * ◌
 ◌ * * ◌ ◌ * * ◌ ◌ * ◌ * * ◌ * * * * ◌ * * * * * ◌ ◌ * ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ * ◌ ◌ * ◌ * * * ◌ ◌ ◌ * ◌ ◌ * * * ◌ ◌ * *
 * * * * ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ ◌ * ◌ ◌ ◌ * ◌ * * * ◌ * * ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ * ◌ * *
 * * * ◌ ◌ * * * ◌ * ◌ * ◌ * * ◌ ◌ ◌ * * * ◌ * ◌ ◌ * ◌ * ◌ * * * ◌ * * ◌ ◌ * ◌ * ◌ * * * ◌ * ◌ * ◌ * ◌ * ◌ * * * ◌ * * * *
 ◌ ◌ * * ◌ * ◌ * ◌ ◌ * * * ◌ * * ◌ ◌ * * ◌ ◌ * * * ◌ ◌ * ◌ * ◌ * ◌ * ◌ ◌ * ◌ ◌ * ◌ ◌ ◌ * ◌ * ◌ * ◌ * * * ◌ * ◌ * ◌ ◌ * * ◌
 * * * * ◌ * * * ◌ ◌ ◌ * * * ◌ * ◌ * ◌ ◌ ◌ ◌ * ◌ * * * ◌ ◌ * * * ◌ ◌ * ◌ * * ◌ ◌ ◌ * * * * * * ◌ ◌ * * ◌ ◌ * * * ◌ * ◌ ◌ *
 ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ * * * ◌ ◌ ◌ * * * ◌ * * * ◌ * * ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * * * ◌ ◌ * * ◌ ◌ * ◌ * * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * * ◌
 * * * ◌ ◌ * * ◌ ◌ ◌ * * ◌ ◌ ◌ * ◌ ◌ * ◌ ◌ * * ◌ * ◌ ◌ * * ◌ ◌ * * ◌ ◌ * * ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ ◌ * * ◌ ◌ ◌ ◌ ◌ ◌
 * ◌ * * ◌ * ◌ ◌ ◌ * ◌ ◌ * * ◌ * * * * ◌ * * ◌ ◌ * * ◌ ◌ * * ◌ ◌ * * * * ◌ ◌ ◌ * * * ◌ * * * ◌ * * * * ◌ * * ◌ ◌ ◌ * ◌ ◌ *
 ◌ * ◌ * ◌ ◌ * * ◌ * * * * ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ * * * ◌ ◌ * * * ◌ * ◌ * * ◌ ◌ ◌ ◌ * * * * ◌ ◌ ◌ ◌ * ◌ * ◌ ◌ * * ◌ ◌ * ◌ ◌
 * * * * ◌ * ◌ * ◌ ◌ ◌ * ◌ ◌ * ◌ ◌ * * * ◌ * ◌ ◌ ◌ * * * * * ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * * ◌ ◌ * ◌ ◌ * ◌ * ◌ ◌ * * ◌ ◌ * * *
 * * ◌ * ◌ ◌ * * * * ◌ ◌ ◌ ◌ * * * ◌ ◌ ◌ * ◌ * ◌ * * ◌ ◌ * ◌ * * ◌ * * * ◌ * * ◌ ◌ * * * ◌ ◌ * * * * * * * * * * ◌ ◌ ◌ ◌ ◌
 ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ * ◌ ◌ * * * ◌ ◌ ◌ ◌ ◌ * * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ * ◌ * ◌ * * ◌ ◌ ◌ ◌ * ◌ * ◌ ◌ ◌ * ◌ * ◌ * ◌ ◌ ◌ ◌ ◌ *
 ◌ ◌ ◌ * * * * ◌ ◌ * ◌ ◌ ◌ ◌ * ◌ ◌ ◌ ◌ * ◌ ◌ ◌ ◌ * * ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * * ◌ * ◌ * ◌ ◌ * ◌ ◌ * * ◌ * ◌ * * ◌
 * * ◌ ◌ ◌ * ◌ * * * ◌ * * ◌ * ◌ * * * ◌ * * ◌ * * ◌ * * * ◌ ◌ ◌ * ◌ ◌ ◌ * ◌ ◌ ◌ * * * * * ◌ ◌ ◌ * ◌ * ◌ * ◌ ◌ ◌ * ◌ * * *
 ◌ * * * ◌ * * ◌ ◌ ◌ * ◌ * * ◌ * ◌ ◌ * * * ◌ ◌ * * ◌ ◌ ◌ * ◌ * ◌ ◌ * * * * * * * ◌ ◌ ◌ ◌ * ◌ ◌ ◌ * ◌ ◌ ◌ * ◌ * * ◌ ◌ ◌ * ◌
 ◌ * * * ◌ ◌ ◌ * ◌ ◌ * * * ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * * ◌ ◌ * * ◌ ◌ * * * ◌ ◌ * ◌ ◌ ◌ * ◌ ◌ * * ◌ ◌ ◌ ◌ * ◌ ◌ ◌ * ◌ * * * ◌ * * ◌
 * ◌ ◌ ◌ * * * * * * * * * ◌ * * * ◌ * * * ◌ ◌ * * ◌ * * ◌ * ◌ ◌ * * * ◌ * * * * ◌ * ◌ * ◌ * * * * ◌ * ◌ * * * * ◌ ◌ * ◌ *
 ◌ ◌ ◌ * ◌ ◌ ◌ * ◌ * * * ◌ * ◌ ◌ ◌ * * ◌ ◌ ◌ * * ◌ * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * * * ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ ◌ * ◌ ◌ * * ◌
 * ◌ * ◌ ◌ * * * ◌ ◌ * ◌ * ◌ ◌ ◌ * * * * * * * * * ◌ ◌ * * ◌ ◌ * * * ◌ ◌ * ◌ ◌ * * * * ◌ ◌ ◌ * * * * * * ◌ ◌ ◌ ◌ * ◌ * * *
 * ◌ ◌ * * * ◌ ◌ * * * * ◌ * * * ◌ * * * ◌ * * ◌ * * * * * * ◌ ◌ * * * * ◌ * ◌ ◌ ◌ * * * ◌ * * * ◌ * * * * * * * ◌ * * ◌ ◌
 * ◌ ◌ * * * * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ ◌ * * ◌ * * ◌ ◌ * ◌ ◌ * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * * * * * * ◌ * * ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * *
 * * * * * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ * ◌ * ◌ ◌ ◌ * * ◌ ◌ * ◌ * ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * ◌ * ◌ ◌ ◌ * ◌ * * ◌ ◌ * * ◌ ◌ * * ◌ ◌
 ◌ ◌ * ◌ ◌ * * * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * * ◌ * * ◌ ◌ * * * ◌ * * ◌ * * ◌ * ◌ * * ◌ * * ◌ ◌ ◌ ◌ ◌ ◌ * * ◌ * * ◌ ◌ * ◌ * ◌ ◌ *
 * * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * * * ◌ ◌ * ◌ ◌ ◌ ◌ * ◌ * * * ◌ * * ◌ ◌ ◌ ◌ * ◌ ◌ * * ◌ * * ◌ ◌ * * * ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ ◌ * * ◌ * * *
 ◌ ◌ ◌ * ◌ * * ◌ * ◌ ◌ * ◌ * ◌ ◌ * * * * ◌ ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ * * ◌ ◌ * * ◌ * * * * * * ◌ ◌ ◌ * * * ◌ * ◌ * * ◌
 ◌ ◌ ◌ ◌ * * ◌ * ◌ * ◌ * ◌ * * ◌ * * ◌ * * * * ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * * ◌ ◌ * * ◌ ◌ ◌ * ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ ◌ ◌ *
 * * * * * * * * * ◌ ◌ * ◌ ◌ ◌ * ◌ ◌ * * ◌ ◌ * ◌ ◌ ◌ ◌ * ◌ * * * ◌ * * ◌ * * * ◌ * ◌ * * * ◌ * * ◌ * ◌ * ◌ * * * ◌ * * * *
 ◌ ◌ ◌ ◌ ◌ ◌ ◌ * * ◌ * * * ◌ ◌ * ◌ ◌ ◌ ◌ * ◌ ◌ * * ◌ * * ◌ * ◌ * ◌ ◌ * ◌ * ◌ * ◌ * ◌ * ◌ ◌ ◌ * * ◌ * * ◌ ◌ * ◌ * ◌ ◌ * * *
 ◌ * * * * * ◌ * ◌ ◌ ◌ * * * ◌ * * ◌ ◌ ◌ ◌ * * ◌ * ◌ * ◌ ◌ * * * ◌ * * * ◌ ◌ * ◌ ◌ * ◌ ◌ * ◌ * ◌ * * * ◌ ◌ * * * ◌ * ◌ ◌ ◌
 ◌ * ◌ ◌ ◌ * ◌ * * * * ◌ ◌ * * ◌ ◌ * ◌ ◌ ◌ ◌ * * ◌ * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ * ◌ * * * ◌ * ◌ * ◌ ◌ ◌ * ◌ * * ◌ ◌ ◌ ◌ ◌ * * ◌ *
 ◌ * ◌ ◌ ◌ * ◌ * ◌ * * ◌ ◌ * ◌ ◌ * ◌ ◌ * * ◌ * * * * ◌ ◌ ◌ ◌ * ◌ * * ◌ ◌ ◌ ◌ * * * ◌ ◌ * * ◌ * * * * * * ◌ * * * * * * ◌ *
 ◌ * ◌ ◌ ◌ * ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * ◌ ◌ * * ◌ ◌ * * * ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ ◌ * * ◌ * * * ◌ ◌ ◌ * ◌ ◌ ◌ ◌ ◌ * ◌ ◌ ◌ * * * ◌ ◌
 ◌ * * * * * ◌ * ◌ * * ◌ * * * ◌ ◌ ◌ ◌ * * ◌ ◌ * ◌ * * ◌ * * * ◌ * ◌ * ◌ * * ◌ * ◌ ◌ ◌ * * ◌ ◌ * * ◌ * ◌ ◌ ◌ ◌ * ◌ * ◌ ◌ ◌
 ◌ ◌ ◌ ◌ ◌ ◌ ◌ * ◌ ◌ * ◌ ◌ * * ◌ ◌ * * ◌ ◌ * * * * ◌ * * * ◌ * ◌ ◌ * * * ◌ * * * ◌ ◌ * * ◌ * * * * * * * ◌ * * * ◌ ◌ * * ◌
~~~
{: #vcard-public-key-by-reference-qrcode align="left" title="A vCard containing a public key by reference"}

# Key Transparency

In cases where a key is communicated by reference, and a service is used to dereference the key material,
key transparency can mitigate threats related to duplicity, censorship, and consistency.

In the context of this document, key references are URIs.

Any key transparency system capable of deliverying key material for a {{-URI}} MAY be used.

## Public Keys

Section 6.8.1 of {{-VCARD}} defines how to embed a public key in a vCARD.

The following informative example is provided:

~~~
BEGIN:VCARD
VERSION:4.0
CATEGORIES:IETF,SPICE
UID:urn:uuid:f8127e4e-4599-4672-8722-61cef672004a
FN:Mister Person
KEY;MEDIATYPE=application/jwk-set+json:https://key-trans.example/urn:ietf:p
 arams:oauth:jwk-thumbprint:sha-256:2oKUD95AGfItMsBbaFvzjHV06kH9YlGclYSMoFh0
 95Y
KEY:data:application/jwk+json;base64,eyJrdHkiOiJFQyIsIngiOiJfZjFoOUlCTnVTNU
 pSZ0FtdkZRTEpPcDV2VGdoYnFNbmlLWkZQYWJFeERHbUNjR19rSU1hRmt0T3UxN2Z6cl83Iiwie
 SI6IjB1VHI5LTNpamJBZlc4a0JHcmdMbHZCZnJsSmJzQ29EcHl3WEZkd3JFcklkN25tb1JHSEhh
 QUdUMjNWV1pVNm4iLCJjcnYiOiJQLTM4NCJ9
END:VCARD

~~~
{: #vcard-public-keys align="left" title="A vCard containing public keys"}

In this example, the fields "CATEGORIES", "UID" and "FN" are not integrity protected, software systems MAY leverage this property to alter these values, will preserving the integrity protected values.

The credentials are encoded by value or by reference, in this example the same key is expressed encoded by value and encoded by reference using a URI built from a thumbprint using {{-JWK-THUMBPRINT-URI}}.

## Digital Credentials

Section 6.8.1 of {{-VCARD}} defines how to embed a public key in a vCARD.

A verifiable credential, as defined in {{-SD-JWT}} and {{-SD-JWT-VC}}, is a special kind of public key, that a holder can prove possession of in order to convince a verifier, that an issuer has asserted attributes about a subject.

## Verifiable Credentials

A verifiable credential, is a special kind of digital credential, defined in {{W3C.VC-DATA-MODEL-2.0}}.

The following informative example is provided:

~~~
BEGIN:VCARD
VERSION:4.0
CATEGORIES:IETF,SPICE
UID:urn:uuid:c1f938a8-7241-4e09-886a-00a61f8b43f6
FN:Mister Person
KEY:data:application/vc+ld+json+sd-jwt,eyJhbGciOiJFUzM4NCIsImtpZCI6Im95NjU2
 SUNPb1phU1RUXy1tLTkwb3VtWGVGRmwyNDY4MURyTlljazl0QjgifQ.eyJjbmYiOnsiamt0Ijoi
 S2Z0VTVaMGRMbWVZUWV5WU1KbGxRNDlsUG5qVklfVEd2aGVGdGpvb0Q1YyJ9LCJfc2RfYWxnIjo
 ic2hhLTI1NiIsIkBjb250ZXh0IjpbImh0dHBzOi8vd3d3LnczLm9yZy9ucy9jcmVkZW50aWFscy
 92MiIsImh0dHBzOi8vd3d3LnczLm9yZy9ucy9jcmVkZW50aWFscy9leGFtcGxlcy92MiJdLCJpZ
 CI6Imh0dHA6Ly91bml2ZXJzaXR5LmV4YW1wbGUvY3JlZGVudGlhbHMvMzczMiIsInR5cGUiOlsi
 VmVyaWZpYWJsZUNyZWRlbnRpYWwiLCJFeGFtcGxlRGVncmVlQ3JlZGVudGlhbCJdLCJpc3N1ZXI
 iOnsiaWQiOiJodHRwczovL3VuaXZlcnNpdHkuZXhhbXBsZS9pc3N1ZXJzLzU2NTA0OSIsIm5hbW
 UiOlt7InZhbHVlIjoidGVzdCB2YWx1ZSAwIiwibGFuZyI6ImVuIn0seyIuLi4iOiJlVnBPNm8tc
 UFyVm1icXdFaERSV21sUHhwVnROU04weFY2SldtUzgzUUZjIn0seyJ2YWx1ZSI6InRlc3QgdmFs
 dWUgMiIsImxhbmciOiJlbiJ9LHsiLi4uIjoia0hHYjZ6eUxiMzZ2cmxzVEZNZk12UzBlelRIUVl
 6b1VPdnlEUGN6T3ItYyJ9LHsidmFsdWUiOiJ0ZXN0IHZhbHVlIDQiLCJsYW5nIjoiZW4ifV19LC
 J2YWxpZEZyb20iOiIyMDE1LTA1LTEwVDEyOjMwOjAwWiIsImNyZWRlbnRpYWxTY2hlbWEiOlt7I
 mlkIjoiaHR0cHM6Ly92ZW5kb3IuZXhhbXBsZS9zY2hlbWFzL0V4YW1wbGVEZWdyZWVDcmVkZW50
 aWFsLmpzb24iLCJ0eXBlIjoiSnNvblNjaGVtYSJ9XSwiY3JlZGVudGlhbFN0YXR1cyI6W3siaWQ
 iOiJodHRwczovL3ZlbmRvci5leGFtcGxlL3N0YXR1cy1saXN0L3Vybjp1dWlkOmQzMWFkYTVkLT
 FkM2QtNGY2OC04NTg3LThmZjliYjMwMzhkNiMwIiwidHlwZSI6IlN0YXR1c0xpc3QyMDIxRW50c
 nkiLCJzdGF0dXNQdXJwb3NlIjoicmV2b2NhdGlvbiIsInN0YXR1c0xpc3RJbmRleCI6IjAiLCJz
 dGF0dXNMaXN0Q3JlZGVudGlhbCI6Imh0dHBzOi8vdmVuZG9yLmV4YW1wbGUvc3RhdHVzLWxpc3Q
 vdXJuOnV1aWQ6ZDMxYWRhNWQtMWQzZC00ZjY4LTg1ODctOGZmOWJiMzAzOGQ2In1dLCJjcmVkZW
 50aWFsU3ViamVjdCI6eyJpZCI6InVybjp1dWlkOnV1aWQ6MmM2MTQyMDctNjRiNi00NTgzLTgxZ
 mEtYmQ1OWEwZjE4ZGMxIiwiZGVncmVlIjp7InR5cGUiOiJFeGFtcGxlQmFjaGVsb3JEZWdyZWUi
 LCJzdWJ0eXBlIjoiQmFjaGVsb3Igb2YgU2NpZW5jZSBhbmQgQXJ0cyJ9fX0.ULlb13aEPgqhE2G
 Xo8ErTFSQ6FV8n_XmeQYGuXcnCDwXXI8Df4s1RcUk0dg8P82bb-DphW3x3yPa_UOVi7nq2_Qv-E
 eZSf0jWq4NgUHOX97h68Y-Vb9qhYZiCLcXW2eF~WyJJSTktR084VU1sVGk4Z2o0UlV5WFNnIiwg
 eyJ2YWx1ZSI6ICJ0ZXN0IHZhbHVlIDEiLCAibGFuZyI6ICJlbiJ9XQ~WyIzb19MZDIyU0xUdFla
 YkNxbUtRcV9nIiwgeyJ2YWx1ZSI6ICJ0ZXN0IHZhbHVlIDMiLCAibGFuZyI6ICJlbiJ9XQ
END:VCARD

~~~
{: #vcard-vc align="left" title="A vCard containing Verifiable Credentials"}

In this example, all fields except for "KEY" are not protected.

KEY encodes a verifiable credential expressing a university degree.

Public credentials are often shared on social, or professional networks, however, they may still contain sensitive information which SHOULD NOT be disclosed such as student identification numbers, or other details about the recipient.

This example is for demonstration purposes only.

# Security Considerations

TODO Security

## Deserialization of Untrusted Data

Several layers of application processing occure before a integrity protected data can be verified.

The QR Code processing layer could have vulnerabilities that are exploitable before the vCard layer is even reached.

The vCard processing layer could have vulnerabilities that are exploitable before the credential can be selected.

The credential processing layer might require deserialization of parts or all of the encoded credentials before the verification process can occur.

After verification has occurred, the credential structure can be validated further, for example checking the lengths of strings, or ranges on integers expressing date times.

After all this validation, processing, verification and processing has occurred, the end result is the credential data as the issuer intended for it to be delivered to the verifier.

Implementations should avoid exposing unverified, unvalidated fields to users, because they can be altered without detection.

In cases were data is repeated without protection outside the credential, the guidance provided regarding unprotected headers in {{-COSE}} and {{-JWE}} MUST be followed.


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
