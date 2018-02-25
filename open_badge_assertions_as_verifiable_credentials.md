# Open Badges Assertions as Verifiable Credentials Prototype
This document presents an example of what a prototype credential could look like in JSON-LD described using the Open Badges vocabulary and expressed in the Verifiable Credentials envelope. 

## Goals Explored in this  (From [README](./README.md))
- **VC Envelope**: Develop a proof of concept that uses Verifiable Credentials as a verification method for an Open Badges Assertion
- **DID Recipient Identification**: Develop a proof of concept that uses a DID as Assertion recipient identifier
- **DID Issuer Identification and Signing**: Develop a proof of concept that uses a DID as an Issuer id.


## Approach

The basic structure is a Verifiable Credentials with an Open Badge Assertion embedded in the `claim`. In this example, the issuer and recipient are identified by DIDs.

The new `verification` type is called `VerifiableClaim2018`. It allows us to bridge the Open Badges validator to Verifiable Credentials verification. The Open Badge validator will respond to this by delegating signature verification to an LD signature/verification library. 

This example adds existing Open Badges data for recipient and issuer, even though this is duplicative. The recipient data could probably be dropped without much harm. If the DID spec and at least one DID Method Spec are ready enough for primetime, perhaps we could hydrate the issuer data from a resolved identity document. At least for now, OB spec and implementations expect id, type, name, url, and email values to exist for issuers, so this avoids a breaking change.

```json
{
  "@context": "http://example.com/TBD-OB-VC-Context-V1",
  "id": "https://some.university.edu/credentials/9732",
  "type": [
    "Credential",
    "OpenBadgeCredential"
  ],
  "issuer": "did:example:issuer_did",
  "issued": "2018-02-28T14:58:57.461422+00:00",
  "claim": [
    {
      "id": "urn:uuid:437fc6ff-bb3c-4987-a4b7-be8661ff6f21",
      "type": "Assertion",
      "recipient": {
        "type": "id",
        "identity": "did:example:recipient_did",
        "hashed": false
      },
      "badge": {
        "type": "BadgeClass",
        "id": "urn:uuid:7aad3c57-3bfb-45ea-ae79-5a6023cc62e4",
        "name": "Certificate of Accomplishment",
        "image": "data:image/png;base64,...",
        "description": "Lorem ipsum dolor sit amet, mei docendi concludaturque ad, cu nec partem graece. Est aperiam consetetur cu, expetenda moderatius neglegentur ei nam, suas dolor laudem eam an.",
        "criteria": {
          "narrative": "Nibh iriure ei nam, modo ridens neglegentur mel eu. At his cibo mucius."
        },
        "issuer": {
          "type": "Profile",
          "id": "did:example:issuer_did",
          "name": "Example Issuer",
          "url": "http://example.com",
          "email": "test@example.com"
        }
      },
      "verification": {
        "type": "VerifiableClaim2018"
      }
    }
  ],
  "revocation": {
    "id": "http://example.gov/revocations/738",
    "type": "SimpleRevocationList2017"
  },
  "signature": {}
}
```

## TODOs
* Develop an actual prototype context file so that we can start conversation around what additions to the various vocabularies should be proposed (as well as gain the ability to actually produce a valid JSON-LD Signature)
* Reference an appropriate LD Signature suite and provide a dummy implementation. This should be appropriate to use with 
* Choose a DID resolver method and generate/publish the appropriate keys and DID document so it may be successfully resolved, and public key information may be discovered. 
  - Actually LD-sign the example data using the generated keypair. 
* Show implementation of the latest blockcerts draft in the LD signature

## Questions
- Are other changes related to Recipient and Issuer use of DIDs? I.e. recipient `type` and `identity` fields; can we just use `@id` instead?
  * We can consider `id` (aliased in OB)

