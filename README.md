# Open Badges and Verifiable Claims integration

## Background

Kim and Nate Otto sketched out an [integration approach](https://github.com/WebOfTrustInfo/rebooting-the-web-of-trust-fall2017/blob/master/topics-and-advance-readings/open-badges-as-verifiable-claims.md) but we had many open questions.

Dave Longley described an approach that addresses concerns about integrating Open Badges and VC verification. 

## Approach

Nate, Kim, and Dave's approaches all use the basic structure of a Verifiable Claim, with an Open Badge Assertion embedded.

Kim and Nate fleshed out the inner Assertio, and it looked like the following. Note that the claim has a term `earnedAssertion` (we made this up on the fly), where the Open Badge assertion goes.

The highlight from Dave is in the new `verification` type, which I've called `useVerificationOfVerifiableClaim` as a placeholder. This solves the biggest duplication concern we had, about how Open Badges verification and VC verification integrate. This effectively says -- back out a level to use Verifiable Claims verification.


```
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
      "earnedAssertion": {
        "type": "Assertion",
        "badge": {
          "type": "BadgeClass",
          "id": "https://some.university.edu/badges/6415",
          "name": "Certificate of Accomplishment",
          "image": "data:image/png;base64,...",
          "description": "Lorem ipsum dolor sit amet, mei docendi concludaturque ad, cu nec   partem graece. Est aperiam consetetur cu, expetenda moderatius neglegentur ei nam, suas dolor laudem eam an.",
          "criteria": {
            "narrative": "Nibh iriure ei nam, modo ridens neglegentur mel eu. At his cibo mucius."
          }
        },
        ...
        "verification": "useVerificationOfVerifiableClaim"           <<<<<<<<<<<<
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


## Analysis

- Dave concluded (similarly to Kim and Nate) that some duplication (e.g. the Issuer type) might be unavoidable, but is likely the best path forward for backcompat
- The duplication that would cause the most problem in the future is verification, which this handles nicely by delegating to VC verification. Open Badges verifier should be able to reuse the LD verification libraries.
- Note for Nate: If we like this approach, we should rethink adding Blockcerts as an IMS extension in the previously-described way. The approach sketched out here allows Open Badges to get Blockcerts (and more) for free. We could prototype and start using it on the Blockcerts side as a v3 opt-in feature.
