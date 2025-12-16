---
title: "OAuth 2.1 Government Content Access Control"
abbrev: "OAuth 2.1 GCAC"
category: info

docname: draft-fx-oauth-government-content-access-control-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: AREA
workgroup: oauth
keyword:
 - government
 - age restriction
 - content control
venue:
  group: fxco
  type: Working Group
  mail: standard@fxco.ca
  arch: gcac.fxco.ca
  github: FxMorin/government-content-access-control
  latest: gcac.fxco.ca/LATEST

author:
 -
    fullname: Francois-Xavier Morin
    organization: FXCO Ltd.
    email: fx@fxco.ca

normative:
 - RFC2119
 - RFC6749
 - RFC7636
 - RFC8705
 
informative:
 - RFC6819
 - RFC9101

...

--- abstract

This document defines an OAuth 2.1 profile that enables a government authority to enforce age-based and content-based access restrictions for online services while preserving user privacy. The protocol allows relying parties to request government-defined regulatory scopes (such as pornography or social media access) and receive cryptographically verifiable eligibility decisions without disclosing user identity, exact age, or personally identifiable information. The profile constrains OAuth features to prevent abuse, cross-service correlation, and unauthorized token issuance.

--- middle


# Introduction

Governments increasingly require online services to restrict access to certain categories of content based on the age or legal status of users. Existing approaches frequently rely on disclosure of personal data, third-party identity providers, or proprietary mechanisms that enable tracking across services.

This document specifies **OAuth 2.1 Government Content Access Control (GCAC)**, an OAuth 2.1 profile in which a government-operated authorization server evaluates user eligibility for regulated content categories and issues privacy-preserving attestations to relying parties. GCAC is designed to answer narrowly scoped regulatory questions (e.g., whether a user may access a category of content) while minimizing data disclosure and preventing correlation across services.

GCAC is not an identity system and MUST NOT be used for authentication, user login, or personalization.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

The following additional terms are used:

* **Government Content Control Authority (GCCA)**:  
  A government-operated OAuth Authorization Server responsible for identity verification, age evaluation, and policy enforcement.

* **Relying Party (RP)**:  
  An OAuth client requesting eligibility decisions for regulated content.

* **Scope**:  
  A government-defined content access category (e.g., `pornography`, `social_media`, `gambling`, `alcohol`, `firearms`, `vpn`, `proxy`).

* **Person Key**:  
  A government-internal, pseudonymous identifier derived from a national identity record and never exposed outside the GCCA.

* **Site Token**:  
  An RP-scoped, non-reversible token representing a government eligibility attestation.


# Goals and Non-Goals

## Goals

The goals of this specification are:

* Enable government-enforced content access controls
* Support multiple regulatory scopes defined by government policy
* Prevent disclosure of identity, date of birth, or exact age
* Prevent cross-RP correlation of users
* Leverage existing OAuth 2.1 security mechanisms

## Non-Goals

This specification explicitly does not attempt to:

* Provide user authentication or login
* Expose personal attributes or identity claims
* Enable cross-service user identification
* Replace digital identity or credential systems


# Architecture Overview

GCCA uses the OAuth 2.1 Authorization Code flow with mandatory security extensions and additional semantic constraints.

```
User → Relying Party → Government Content Control Authority
↑                     ↓
└──── OAuth 2.1 ───────┘
```

The GCCA operates as a constrained OAuth Authorization Server, and the RP operates as a confidential OAuth client.


# Scope Model

## Government-Defined Scopes

Scopes represent legally regulated content categories. Examples include:

```
pornography
social_media
gambling
alcohol
firearms
vpn
proxy
```

Each scope:

* MUST be defined and governed by the GCCA
* MUST correspond to a legal or regulatory access rule
* MUST prevent scope definitions or combinations thereof that would allow an RP to infer a user’s exact age or approximate age range through multiple eligibility queries. Scopes MUST be coarse-grained and legally motivated, and MUST NOT be parameterized by numeric age values.

## Scope Evaluation Semantics

For each requested scope, the GCCA determines whether the user satisfies the applicable legal requirement. The RP receives only a boolean eligibility result per scope.

# Client Registration

Relying parties MUST register with the GCCA prior to using GCAC.

During registration, the RP MUST provide:

* Legal entity identification
* Intended use and justification for requested scopes
* One or more redirect URIs
* A client authentication method (mutual TLS or `private_key_jwt`)

The GCCA MAY restrict which scopes an RP is authorized to request.


# Protocol Flow

## Authorization Request

The RP initiates an OAuth authorization request:

```

GET /authorize
?response_type=code
&client_id=client_id
&redirect_uri=registered_uri
&scope=pornography social_media
&state=random_nonce
&code_challenge=pkce_value

````

The GCCA MUST reject requests that include unregistered redirect URIs or unauthorized scopes.

## User Authentication and Evaluation

The GCCA authenticates the user using government-controlled mechanisms and evaluates eligibility for each requested scope.

## Authorization Response

Upon successful evaluation, the GCCA redirects the user back to the RP with an authorization code.

## Token Request and Response

The RP exchanges the authorization code at the token endpoint using client authentication and PKCE. The GCCA responds with a site-scoped eligibility attestation:

```json
{
  "site_token": "opaque_string",
  "scope_results": {
    "pornography": true,
    "social_media": false
  },
  "expires_at": "timestamp"
}
````


# Token Derivation

The GCCA MUST derive an internal person key as follows:

```
person_key = HMAC(master_secret_key, national_person_id)
```

The site token MUST be derived deterministically:

```
site_token = HMAC(person_key, client_id)
```

The person key and national identifiers MUST NOT be exposed outside the GCCA.


# Re-Verification

Relying parties MUST provide a user-accessible mechanism to re-initiate the GCAC flow. Re-verification MUST follow the same protocol as the initial authorization.


# Security Considerations

GCAC relies on OAuth 2.1 security best practices, including authorization code flow, PKCE, redirect URI allowlists, and strong client authentication. Leaked client identifiers alone do not enable token issuance. Tokens are RP-scoped and non-transferable, preventing cross-service correlation and replay attacks.


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

The author would like to acknowledge ongoing discussions within the OAuth and digital privacy communities that informed the design principles of this specification.
