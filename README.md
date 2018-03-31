# About
There is a lot of interest in interoperability of Open Badges and Educational/Occupational Verifiable Credentials. This repo contains our goals, design drafts, and other research related to achieving interoperabilty between the relevant specifications and services operating in this space.

Work on describing credentials is occurring within the IMS Global Open Badges Workgroup, W3C Verifiable Credentials Working Group, W3C Credentials Commiunity Group, Educational and Occupational Credentials in Schema.org Community Group, and Credential Engine/CTDL.

This effort will result in requirements, use cases, and/or draft specs to be formalized in the appropriate standards group. For now, our priorities are:
- exploring the problem space
- creating specification/schema drafts and prototypes
- getting broader community feedback
- drafting initial use cases

## Goals, use cases, and deliverables:
1. **VC Envelope**: Develop a proof of concept that uses Verifiable Credentials as a verification method for an Open Badges Assertion
  - Wrap and sign an assertion with verifiable claims envelope and linked data signature
    - See draft: [Open Badge Assertions as Verifiable Credentials](open_badge_assertions_as_verifiable_credentials.md)
  - Update an experimental fork of the [Open Badges Validator](https://github.com/IMSGlobal/openbadges-validator-core) to support the VC envelope JSON string as an input format, including support for verification of at least one flavor of Linked Data Signature.
  - Accurately describe capabilities and limitations of verification functionality in terms of what signature suites are supported in prototype.
2. **Blockcerts**: Develop a proof of concept that uses a [Blockcerts](https://www.blockcerts.org/) signature-and-blockchain-proof-of-existence Linked Data signature prototype using the above VC envelope. 
  - Add support to experimental fork of the Open Badges Validator that can verify the integrity of this signature and verify proof of existence on at least one blockchain.
3. **DID Recipient Identification**: Develop a proof of concept that uses a DID as Assertion recipient identifier
  - This is fairly trivial. Open Badges may use "id" as a recipient type. `"recipient": {"type": "id", "identity": "did:example:abc123", "hashed": false}` See [Profile Identifier Properties](https://www.imsglobal.org/sites/default/files/Badges/OBv2p0/index.html#ProfileIdentifierProperties).
  - Update the experimental fork of the Open Badges Validator to support this use case and add relevant tests. 
4. **DID Issuer Identification and Signing**: Develop a proof of concept that uses a DID as an Issuer id.
  - Implement support for at least one did resolver method in the experimental fork of the Open Badges Validator to discover a relevant signing key and verify Linked Data Signature (at least one suite)
5. **BadgeClass Flexibility**: Develop a proof of concept that uses the Open Badges Assertion class to assert issuance of an achievement defined by Credential Engine Registry data.
 - See: [BadgeClass Flexibility](badgeclass_flexibility_cer.md)
  - Example: an Open Badge could fully defined BadgeClass properties, link to CER, or both
  - Reference: http://credentialfinder.org/credential/1/21st_Century_Skills_for_Workplace_Success
  - Add issuer property to the Assertion class to examine use cases that break assumption that whoever created the badge is the entity that issued it
6. **Expressiveness**: Explore use cases for expanding the flexibility of the Open Badges Specification
  - Amending an assertion with further evidence after original issue
    - A "Living Badge"
    - Making evidence verifiable as well
    - Identification of evidence provider separate from issuer and/or recipient
  - Credentials that aren't considered valid until recipient has counter-signed
    - May improve prospects for compliance with GDPR and other privacy

## Related discussion questions
  - Is type in assertion or is it part of data that supports the assertion?
    - Should badges always be microcredentials?





