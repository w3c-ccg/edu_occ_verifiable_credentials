# About
There is a lot of interest in interoperability of Open Badges and Educational/Occupational Verifiable Credentials. This repo contains our goals, design drafts, and other research related to achieving interoperabilty.

This effort will eventually result in requirements, use cases, and/or draft specs to be formalized in the appropriate standards group. For now, our priorities are:
- exploring the problem space
- creating specification/schema drafts and prototypes
- getting broader community feedback

# Goals and use cases
- Unify Open Badges and Verifiable Claims schema
  - Wrap and sign an assertion with verifiable claims linked data signature
  - [Open Badge Assertions as Verifiable Credentials](open_badge_assertions_as_verifiable_credentials.md)
  - Use DIDs for recipient and issuer id
  - LD signature compatibility
- Credential Engine Registry integration
  - Ability to link to CER credentials
  - Example: an Open Badge could fully defined BadgeClass properties, link to CER, or both
  - Reference: http://credentialfinder.org/credential/1/21st_Century_Skills_for_Workplace_Success
- Move issuer property into the assertion to break assumption that whoever created the badge is the one that issued it
- Expressiveness
  - Amending an assertion with further evidence
    - Living Badge
    - Making evidence verifiable as well
    - Identification of evidence provider
  - Credentials that aren't considered valid until recipient has counter-signed
    - GDPR compliance
    - TODO: Kim
- Discussion
  - Is type in assertion or is it part of data that supports the assertion?
    - Should badges always be microcredentials?





