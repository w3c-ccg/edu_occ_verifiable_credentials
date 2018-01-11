# Open Badges Assertions as Verifiable Credentials

> We may want to break this up; i.e. work on DIDs after Verifiable Credentials

## Goals
1. Express Open Badge Assertions as Verifiable Credentials
2. Allow the Open Badge Validator to be aware of LD Signature Verification (including Blockcerts)
3. Allow recipients and issuers to be identified by DIDs

## Approach

The basic structure is a Verifiable Credentials with an Open Badge Assertion embedded in the `claim`. In this example, the issuer and recipient are identified by DIDs.

The new `verification` type is called `VerifiableClaim2017`. It allows us to bridge the Open Badges validator to Verifiable Credentials verification. The Open Badge validator will respond to this by delegating signature verification to an LD signature/verification library. 

This example adds existing Open Badges data for recipient and issuer, even though this is duplicative. The recipient data could probably be dropped without much harm. If the DID spec and at least one DID Method Spec are ready enough for primetime, perhaps we could hydrate the issuer data from a resolved identity document. At least for now, OB spec and implementations expect id, type, name, url, and email values to exist for issuers, so this avoids a breaking change.

```json
{
  "id": "https://some.university.edu/credentials/9732",
  "type": [
    "Credential",
    "OpenBadgeCredential"
  ],
  "issuer": "did:example:issuer_did",
  "issued": "2017-06-29T14:58:57.461422+00:00",
  "claim": [
    {
      "id": "did:example:recipient_did",
      "type": "Assertion",
      "recipient": {
        "type": "id",
        "identity": "did:example:recipient_did",
        "hashed": false
      },
      "badge": {
        "type": "BadgeClass",
        "id": "https://some.university.edu/badges/6415",
        "name": "Certificate of Accomplishment",
        "image": "data:image/png;base64,...",
        "description": "Lorem ipsum dolor sit amet, mei docendi concludaturque ad, cu nec   partem graece. Est aperiam consetetur cu, expetenda moderatius neglegentur ei nam, suas dolor laudem eam an.",
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
        "type": "VerifiableClaim2017"
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

## Questions
- Are other changes related to Recipient and Issuer use of DIDs? I.e. recipient `type` and `identity` fields; can we just use `@id` instead?

