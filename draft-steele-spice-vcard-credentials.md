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
  I-D.draft-ietf-mediaman-suffixes: MULTIPLE-STRUCTURED-SUFFIXES



informative:
  IANA.media-types: MEDIA-TYPES
  IANA.media-type-structured-suffix: MEDIA-TYPE-STRUCTURED-SUFFIX


  RFC9278: JWK-THUMBPRINT-URI
  RFC1952: GZIP
  RFC8878: ZSTD

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

This document enables vCards to be used as a transport for digital credentials.

--- middle

# Introduction

Public key based identity systems require users to obtain cryptographic keys, and use them to verify digital signatures produced by a private key, or decrypt data with a private key that was encrypted to a public key.

The public key cryptographic operations are a foundational building block for building secure systems capable of providing confidentiality, integrity and authenticity.

Applications supporting protocols such as OAUTH, OpenSSH, and OpenPGP rely on different public key formats.

To enable interoperability, useful public key formats have media types registered with IANA {{-MEDIA-TYPES}}.

A common challenge in working with applications that require key management is obtaining cryptographic keys.

vCards as decribed in {{-VCARD}} can address this challenge, for keys that are embedded by value or by reference in digital business cards.

# Terminology

{::boilerplate bcp14-tagged}

# Transports

vCARDs can be transported over various channels, including through NFC and QR Codes, as decribed in {{-BASE45}}.

## Optical

QR Codes are well supported in modern smartphones and frequently displayed on physical products, providing additional information to consumers.

Anyone who can see a QR Code can access the data encoded in it.

Sensitive plaintext data MUST NOT be encoded in vCARD QR Codes.

Encryption formats, such as JSON Web Encryption as decribed in {{-JWE}} MAY be used to secure confidential credentials.

QR Codes can be removed, altered, or replaced on physical products, and in video streams.

Additional security checks MUST be performed before accepting any data transported by QR Codes.

For example, confirming serial numbers or other presented identifiers are consistent with the claims in credentials presented through the vCard QR Code.

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

Any key transparency system capable of deliverying key material for a URI as described in {{-URI}} MAY be used.

## Public Keys

Section 6.8.1 of {{-VCARD}} defines how to embed a key in a vCARD.

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

In this example, the same credentials are encoded by value and by reference, and the fields "CATEGORIES", "UID" and "FN" are not integrity protected, software systems MAY leverage this property to alter these values, will preserving the integrity protected values.

The credential encoded by reference is using a URI built from a thumbprint using {{-JWK-THUMBPRINT-URI}}.

## Digital Credentials

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

KEY encodes a verifiable credential expressing a hypothetical university degree.

Public credentials are often shared on social, or professional networks, however, they may still contain sensitive information which SHOULD NOT be disclosed such as student identification numbers, or other details about the credential subject.

This example is for demonstration purposes only.

## Compression Suffixes

In order to reduce the size of Data URIs encoded in vCards, compression MAY be applied using structured suffixed, including using {{-MULTIPLE-STRUCTURED-SUFFIXES}}.

Any suffixes registered in {{-MEDIA-TYPE-STRUCTURED-SUFFIX}} MAY be used.

When encoding data formats, such as JSON Web Tokens, that are already URI safe, the base64 data encoding MUST be ommited.

The examples that follow have line wrapping for readability.

The following example is informative:

~~~text
data:application/sd-jwt,eyJhbGciOiJFUzM4NCIsImtpZCI6IkRpV25vN2NUSTRUWlB\
QNmV5cWhicm1iZTVQbHo5WHlyVjd0TkNGTXVQUWsifQ.eyJjbmYiOnsiamt0IjoiQ1RFRGI\
zU1FSRVNxUlUzNWpmbWNxVVlXX0xiTU8xWUs3VEZmbTg4VC0zMCJ9LCJfc2RfYWxnIjoic2\
hhLTI1NiIsIkBjb250ZXh0IjpbImh0dHBzOi8vd3d3LnczLm9yZy9ucy9jcmVkZW50aWFsc\
y92MiIsImh0dHBzOi8vd3d3LnczLm9yZy9ucy9jcmVkZW50aWFscy9leGFtcGxlcy92MiJd\
LCJpZCI6Imh0dHA6Ly91bml2ZXJzaXR5LmV4YW1wbGUvY3JlZGVudGlhbHMvMzczMiIsInR\
5cGUiOlsiVmVyaWZpYWJsZUNyZWRlbnRpYWwiLCJFeGFtcGxlRGVncmVlQ3JlZGVudGlhbC\
JdLCJpc3N1ZXIiOnsiaWQiOiJodHRwczovL3VuaXZlcnNpdHkuZXhhbXBsZS9pc3N1ZXJzL\
zU2NTA0OSIsIm5hbWUiOlt7InZhbHVlIjoidGVzdCB2YWx1ZSAwIiwibGFuZyI6ImVuIn0s\
eyIuLi4iOiI1VGJBdzZLUDFxaGZCMk9fcExnNHBtYWZEMW5wWnZqOGpueHh6UGJYTzdzIn0\
seyJ2YWx1ZSI6InRlc3QgdmFsdWUgMiIsImxhbmciOiJlbiJ9LHsiLi4uIjoiMmNTcHRSLV\
BzLW9oeGcxbld1azFGMXA2Y09oZ3ljeUhJcFRVWkc2VUU1ZyJ9LHsidmFsdWUiOiJ0ZXN0I\
HZhbHVlIDQiLCJsYW5nIjoiZW4ifV19LCJ2YWxpZEZyb20iOiIyMDE1LTA1LTEwVDEyOjMw\
OjAwWiIsImNyZWRlbnRpYWxTY2hlbWEiOlt7ImlkIjoiaHR0cHM6Ly92ZW5kb3IuZXhhbXB\
sZS9zY2hlbWFzL0V4YW1wbGVEZWdyZWVDcmVkZW50aWFsLmpzb24iLCJ0eXBlIjoiSnNvbl\
NjaGVtYSJ9XSwiY3JlZGVudGlhbFN0YXR1cyI6W3siaWQiOiJodHRwczovL3ZlbmRvci5le\
GFtcGxlL3N0YXR1cy1saXN0L3Vybjp1dWlkOmQzMWFkYTVkLTFkM2QtNGY2OC04NTg3LThm\
ZjliYjMwMzhkNiMwIiwidHlwZSI6IlN0YXR1c0xpc3QyMDIxRW50cnkiLCJzdGF0dXNQdXJ\
wb3NlIjoicmV2b2NhdGlvbiIsInN0YXR1c0xpc3RJbmRleCI6IjAiLCJzdGF0dXNMaXN0Q3\
JlZGVudGlhbCI6Imh0dHBzOi8vdmVuZG9yLmV4YW1wbGUvc3RhdHVzLWxpc3QvdXJuOnV1a\
WQ6ZDMxYWRhNWQtMWQzZC00ZjY4LTg1ODctOGZmOWJiMzAzOGQ2In1dLCJjcmVkZW50aWFs\
U3ViamVjdCI6eyJpZCI6InVybjp1dWlkOnV1aWQ6MmM2MTQyMDctNjRiNi00NTgzLTgxZmE\
tYmQ1OWEwZjE4ZGMxIiwiZGVncmVlIjp7InR5cGUiOiJFeGFtcGxlQmFjaGVsb3JEZWdyZW\
UiLCJzdWJ0eXBlIjoiQmFjaGVsb3Igb2YgU2NpZW5jZSBhbmQgQXJ0cyJ9fX0.E2rHWUvBk\
q4aY0VvmdNH3jHD3TIw6fHFXbdAXfX2y1Jrj4lGWS44wTg0rv-Gv0aGI3bNJ5rBaq_daT1y\
hS07sg3FzFo7x6mdsKt6I4C4oQiJ10ooORZn6RJeepnD9z8e~WyJOTHpIV2pvbTRhNVlNNl\
JIRU1kZ2RBIiwgeyJ2YWx1ZSI6ICJ0ZXN0IHZhbHVlIDEiLCAibGFuZyI6ICJlbiJ9XQ~Wy\
J1Ny1PRHh0WHpyNFg4QWx3Wl9wbGxRIiwgeyJ2YWx1ZSI6ICJ0ZXN0IHZhbHVlIDMiLCAib\
GFuZyI6ICJlbiJ9XQ
~~~
{: #example-sd-jwt-data-uri align="left" title="A data uri for sd-jwt with no base64 encoding."}

When applying compression to content with a well defined media type, the result of the compression and decompression operations are bytes, which MUST be made URI safe through base64 encoding.

The following example used zstd as defined in {{-ZSTD}}, and is informative:

~~~text
data:application/sd-jwt+zstd;base64,KLUv/QBInScA2l/8ESbwRmo9iOJWn+zgzkG\
EOkM0UdZBdRlKpfn7wZ/YfsTDBp6BZ+CZ0hEBEgESAZzCWbRW/ijGvIwz8fb7H5feejaB6u\
Y3Pxr3GEUiSG/80SD3yZwuWMR6NJhgbHB7JVBXNtIrK7zrhBFrwQBpcuuzMJvs0aZq9COFE\
TcPn/1wngX/5kcKI+7PwZkk4Ny4fXOMuCGMmEWm13F3ddEWLTLgpfEAaXLrU3vclNuVJUHR\
3KExv/k2mVXdNmW/g5tdiLECStkFtKkMbaJRNnfAqHTPibt8Fg4HWNWH8rr4vaHFunvYjha\
3QnsphZNY43woh0qmd3BTPnr2Xyc91jVHxUWAqeA/bE33qh7bqKTGXZFDmOCA9HSr/Enfv+\
FNuN1OB44tqlJYVrERgnI9qjELs8neo89vNnYQjZZDX/r96I2KiwptKpPcgh/g/A5udgSMf\
bSpPyGgKe2VJX3p/4MqQ8WcWNZVdgNfytl0sxYcX7KDkZhiuKLHGbNE4zrcz8LB4DoeblVQ\
0K5sj97BTcjg639QZehrTl/63fUW9vnNjxgW1j6na4xp2ZzqZmFScfHJbo7aWGH0sQobtIB\
4j3Qgdxgk4CiRMfY4QLYLE7uAaURNfrLQQVg1j9qBPjeZL/3gK2SKi092E3L7LEwxQ5xHP/\
gTNChyEmJMOTTaeB/tHdzkF9Jb7qaAB5fewnoj9PlNhSQNlVWssY6UUeTvcOltuzKkAWme7\
TbPVwa5S404xg1SkOagL/2Es/866eeVE7tsjzZVo83Eb9/dfSiHNvVBjHlUpwM4ekzYbhTG\
wgiRCxPOkttJEHppkWnZXojreCFto5YtMk+G3C7tlUUGbhZms+BU15Tq5jdfR/ijbVbRyu3\
7HdzkdtEgvbTJXpp0Hm2kTf1GtyUBU+FEROK0bC+UfBamIoMc0h78CUZvG9sYgRGHPedy36\
q6MCo7kI2kYI99TErUcKrauBXWcHA0Tl1qcKwlHqfFu1Rpr+xLlkr4HaG65nnBhSnuhLowa\
Z0HFRdDFdjjPhlnuY7YTvEObvLDtZC0qT9Q9Oj1irg4ixrOiSvr6GDNNwUb56IbBJwVRqyF\
CNqSgrJ3cLPB+xTw4NInsOabglVhn998DBo4CYw4SUwgLpioamKoapJAROJ86aWixZzIXoi\
xERU+GmQ4XkI8DRiOFT0Iyw2iCiSiTaiIE0wXQ19z2hia+omGAf48UEpuJBY1KbeWX7tRlY\
54yp0eVElUxAkWA0YG0XmLBaUKXaYkh0Mm9CfHoBnMwoPAp/VCKMDhjM7C+ARIxSYmVaYZO\
JAdEBCo1ct8Mhl5AEQQSEYFDgYK/PABh4AmonIVXAZNXw5UUABntSAgboGs1iNXYtngQOMQ\
sTCEQEkEkFcBQ8Ut6TUxpq63gyJzBJw9q1gMTNGMibWRNvUtuujvPhnn9OkFeNzMD/4Moyc\
PElwGE1hKpo49Ej2HjKqOjMpK+fYpzj42IhKnKlKiHnXiojAbjbs9+7tYtKla0k1leqTRME\
ZPsdTzIz02QkoYVR1gXJayyeMUJSAwwhQjlQ/UQt6u8NP2EAgRR0B65LjDFDYfU/Y73OwSS\
64rlOF1jjHN9QeZTrtwmPPwnaEc6yHA2JC1c78POVAf8rmQ9tFCniL9scqj3JP3NyZ8dsOV\
5Dw8HmR2+flUVmfku2xfmMvAoAS6wI34FA==
~~~
{: #example-zstd-compressed-sd-jwt-data-uri align="left" title="A data uri for a zstd compressed sd-jwt with base64 encoding."}

The following example used gzip as defined in {{-GZIP}}, and is informative:

~~~text
data:application/sd-jwt+gzip;base64,eJyVVMuyozYU/KJMiYd9x0u/eAVEwCCBNik\
LbCMQmLnYBrSYb4+wfef6ppJKZeEFVVafPt19+jA6BTUz5jPHiIWnw7Xd2fWlJWt7bldhi9\
TZDaow3kVhjPkqgDWaZbhgWa0wEqGAWucZtviIyhxEFTSjBAUx7tgx+HYYnZLWKfObju3rC\
7DLMwuU0AhNW8SKsQsRHGIeC4jbmmI4IMSTBAwsir8POO40tCU1jU46WgPhrZ2Fu3aOmRoe\
Uzw0E1amFoUb2QpkknK1Kqk6AyQp5JyW2nUBcmslfPb9lmu55jaZcOvFSMbFNRsXZVajiuA\
Z2GOjk9+qN2H8vzf8YBqXzBz4472TS34P2e44y7k7LhRac5Ukjtgn4cytkZ5ipadmfEs1hx\
MTXXOTF9Tybp7IxJ1DE84yM2Y+7xiq0bjHpE2x05EYjgSHnDah/O6ZnGV8zA9N1EhuPHjFX\
D/4ZBpUSGI/PMDBZPM5t8I+E+ebq6HrPiE8a2CbW9VValfQZNWR3eL5zhGuiFUYLYG/m/SZ\
FRRP3C5vdkMkb8QnH3ITiXy9UqUvCtkte5v1jJrGlYyTFuhqN6A7jPbVZbqcbyvIdFa5IG6\
8MYa9SdZetThm26GB1uqSYrL18KzHDfnhm+31YBXz2HTSSOTigeM850jsJuSZFpzy2uhyHJ\
8eHg4Fre9x5pTJzFgdk3OvE0+vhlFmhTsXrYSLF+eDmQ2U58peGKaXLNUULM5E4+UhLpzMC\
BGuMhXFsULGB85zzoQtcwaBbT012ASTH12KZ/dcEqyzI1KmvE5cW7IlI1XBtPvobbaKGy3l\
b9ujzXb0S6/3y2WP79xfPR6iVC04xduH3jWvJuy9FYLM8qZsqTKLFdXsV9/E440hXPCRNXl\
FOJe4aPOaX7duBVX1iTc4JKu7j7sG3iiH5d5El3TnLJJdz77k1IAgTUIlk75i7Z/yRDitw1\
vGZr9uw9U+3ijdXmomMzfSslVyzCu/DoSHjSqNUOVGRuWpwQWaqeqvgQ6jk+ZGRU1KzlKpk\
SeKCjLvnq3c4v3df/7EBoPMayC1tYdQ7pc11bSXyE0D5AkM8sTpqQbvO0oNVKrCQu5zo/d7\
e8UIHcmfH6YbLpevGN7E/et9fdz5sy9kzom5GF9vXOIVuYVk1u78bpLH1W+QInWbk403pDg\
sIA4uHg4EWQNAylR3o5Pib7KLb5Laxw7zxFL4ZqDajTLd85cOijUke1UWr+Qi7+LRPc2Lvs\
9ZXu2pXjTpk11gGTLIAJD6CjlrIPX2ktaB4uNtT8qtTkxvmDQmz06RXfr2q5PYZ+cEtTHlp\
KOa88xX/NALf+bp8z/2iarpSTZJK7mXZLeSNxqcgsQBmbytYwK+bdV3C8e3VfVD36cA3eoc\
WlppbbTI7udHy0hovkyOiToqznupcxPvdL2PTuD99pt5A3vT1ih0Zu+r/Y8/832kjMUOvHU\
nzRDG+W2Y13n3+2Vu62v9HDBHAeezH5JmHjqHQ9tsFuL74SceHT+yWhup7Y1G0hjEIeSOHc\
ZKRdRwJUU5fSmf9d9LYCsFWH4W3/pRQEkwQStwVP4IrQJgqx2hcdIDPGiYL2ROhvC/ob1/g\
f4LKMyj6Q==
~~~
{: #example-zlib-compressed-sd-jwt-data-uri align="left" title="A data uri for a gzip compressed sd-jwt with base64 encoding."}

### Quid Pro Quo

Compression is a technique that reduces storage costs by increasing computation costs.

The best choice for a compression algorithm is use case specific.

Compression is often applied at the transport layer, making message level compression redundant in some cases.

For many use cases choosing no compression is the simplest, safest, and fastest solution.

A detailed analysis of compression algorithms is beyond the scope of this document.

In the examples in the section above, the storage costs are as follows:

| Media Type | Number of Bytes
|---
| application/sd-jwt    | 1981
| application/sd-jwt+zstd  | 1276
| application/sd-jwt+gzip  | 1204
{: #media-type-size-in-bytes align="left" title="Content length in bytes"}

# Security Considerations

TODO Security

## Deserialization of Untrusted Data

Several layers of application processing, relevant to {{CWE-502}} occur before a integrity protected data can be verified.

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
