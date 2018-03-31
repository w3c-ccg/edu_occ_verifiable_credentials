# BadgeClass Flexibility
Develop a proof of concept that uses the Open Badges Assertion class to assert issuance of an achievement defined by Credential Engine Registry data.
  - Example: an Open Badge could fully defined BadgeClass properties, link to CER, or both
  - Reference: http://credentialfinder.org/credential/1/21st_Century_Skills_for_Workplace_Success
  - Add issuer property to the Assertion class to examine use cases that break assumption that whoever created the badge is the entity that issued it

## Further Explanation

### Publish an assertion for a credential defined in the Credential Engine Registry
The [Credential Engine Registry (CER)](http://credreg.net/) is a database with open APIs that stores and shares metadata about credentials, credentialing organizations, quality assurance organizations, and competency frameworks. The Credential Transparency Description Language (CTDL) describes each credential and associated entities. Organizations publishing to the CER are human-verfied prior to publishing credential data.

Each credential stored in the CER contains a unique resource link which returns a JSON payload of the credential's properties as [required or reccomended] (http://credreg.net/registry/policy) by the CTDL. By referencing this resource link in an assertion the metadata for this credential is accessible as part of the verifiable credential. An example of a resource: 

```json
{
    "@id": "https://credentialengineregistry.org/resources/ce-c4ccf4c4-76d5-4a6e-9ca0-2cd7a8937ee6",
    "@type": "ceterms:DigitalBadge",
    "@context": "http://credreg.net/ctdl/schema/context/json",
    "ceterms:ctid": "ce-c4ccf4c4-76d5-4a6e-9ca0-2cd7a8937ee6",
    "ceterms:name": "Medical Assistant Patient Interviewing/Intake",
    "ceterms:keyword": ["Medical Assistant", "Patient Intake", "Patient Medical History"],
    "ceterms:ownedBy": [{
        "@id": "https://credentialengineregistry.org/resources/ce-67C45100-B6D7-413D-9924-F0D5ADB825F1"
    }],
    "ceterms:subject": [{
        "@type": "ceterms:CredentialAlignmentObject",
        "ceterms:targetNodeName": "Medical Assistant"
    }, {
        "@type": "ceterms:CredentialAlignmentObject",
        "ceterms:targetNodeName": "Patient Intake"
    }],
    "ceterms:isPartOf": [{
        "@id": "https://credentialengineregistry.org/resources/ce-4fa8dc4b-ef12-4a44-be7c-7397f8ef0421"
    }],
    "ceterms:offeredBy": [{
        "@id": "https://credentialengineregistry.org/resources/ce-67C45100-B6D7-413D-9924-F0D5ADB825F1"
    }],
    "ceterms:revokedBy": [{
        "@id": "https://credentialengineregistry.org/resources/ce-67C45100-B6D7-413D-9924-F0D5ADB825F1"
    }],
    "ceterms:inLanguage": ["English"],
    "ceterms:availableAt": [{
        "@type": "ceterms:Place",
        "ceterms:name": "Madison",
        "ceterms:latitude": 43.12171550000001,
        "ceterms:longitude": -89.3285843,
        "ceterms:postalCode": "53704",
        "ceterms:addressRegion": "WI",
        "ceterms:streetAddress": "1701 Wright St.",
        "ceterms:addressCountry": "United States",
        "ceterms:addressLocality": "Madison"
    }],
    "ceterms:description": "CAAHEP accreditation requires 100% of all medical assistant graduates to pass 100% of all competencies.  In addition to passing 100% of all competencies, to earn this badge a student performed a mock rooming exercise at an exceptional level.  Additionally the student successfully completed the psychomotor, affective, and cognitive domain assessments at a 93% or above.",
    "ceterms:dateEffective": "2016-09-01",
    "ceterms:occupationType": [{
        "@type": "ceterms:CredentialAlignmentObject",
        "ceterms:framework": "https://www.bls.gov/soc/",
        "ceterms:codedNotation": "31-9092.00",
        "ceterms:frameworkName": "Standard Occupational Classification",
        "ceterms:targetNodeName": "Medical Assistants",
        "ceterms:targetNodeDescription": "Perform administrative and certain clinical duties under the direction of a physician. Administrative duties may include scheduling appointments, maintaining medical records, billing, and coding information for insurance purposes. Clinical duties may include taking and recording vital signs and medical histories, preparing patients for examination, drawing blood, and administering medications as directed by physician."
    }],
    "ceterms:subjectWebpage": "https://www.youracclaim.com/org/madison-college-school-of-health-education/badge/medical-assistant-patient-interviewing-intake",
    "ceterms:copyrightHolder": [{
        "@id": "https://credentialengineregistry.org/resources/ce-67C45100-B6D7-413D-9924-F0D5ADB825F1"
    }],
    "ceterms:audienceLevelType": [{
        "@type": "ceterms:CredentialAlignmentObject",
        "ceterms:targetNode": "audLevel:AssociatesDegreeLevel",
        "ceterms:frameworkName": "AudienceLevel",
        "ceterms:targetNodeName": "Associates Degree Level"
    }],
    "ceterms:availableOnlineAt": ["https://madisoncollege.edu/program/medical-assistant"],
    "ceterms:estimatedDuration": [{
        "@type": "ceterms:DurationProfile",
        "ceterms:description": "This credential can be earned within one course within the Medical Assistant program.",
        "ceterms:maximumDuration": "P3M",
        "ceterms:minimumDuration": "P1M"
    }],
    "ceterms:availabilityListing": ["https://madisoncollege.edu/program/medical-assistant"],
    "ceterms:credentialStatusType": {
        "@type": "ceterms:CredentialAlignmentObject",
        "ceterms:targetNode": "credentialStat:Active",
        "ceterms:frameworkName": "CredentialStatus",
        "ceterms:targetNodeName": "Active"
    }
}
```

It is required that each credential contains at least an [`ceterms:ownedBy`] (http://credreg.net/ctdl/terms#ownedBy) or [`ceterms:offeredBy`] (http://credreg.net/ctdl/terms/offeredBy) property. A credential may contain multiples of each property. When the resource is linked to from an assertion,`ceterms:ownedBy` or `ceterms:offeredBy` data may be the verified issuer(s) of a credential. While it can't be assumed that `ceterms:ownedBy` is the "creator" of the credential. It is arguable that the organization(s) referenced in the `ceterms:ownedBy` property is the arbitrator of which organization(s) are referenced in the `ceterms:offeredBy` property.

At this stage of CER, organizations are the main providers of the credentials. However, the CER has made accomodations for individuals to be providers as well. 

## Questions/Considerations
- How will open badges validator correlate issuer property to `ceterms:ownedBy` and `ceterms:offeredBy`. Reference: [CER Organization property] (http://credreg.net/registry/policy#mindata_organization) CER resources may contain multiple values for these properties (confirm) whereas an open badge conatins a single issuer property.
- If a CER property is in an assertion as well as an issuer property, is a BadgeClass necessary? 
- Can the BadgeClass be optional? 
- If the BadgeClass is optional and the CER resource is not an Open Badge but another type of credential, is the verifiable credential still an Open Badge?

## Scenarios (assuming Issuer property is in the assertion, not in the badge class and that the assertion links to a CER resource)
- CER resource is [`ceterms:OpenBadge`] (http://credreg.net/ctdl/terms#OpenBadge); CER resource contains a single `ceterms:ownedBy` but not `ceterms:offeredBy`; Issuer property correlates with `ceterms:ownedBy`. BadgeClass is not present in the assertion.
- CER resource is [`ceterms:OpenBadge`] (http://credreg.net/ctdl/terms#OpenBadge); CER resource contains a single `ceterms:ownedBy` as well as a single `ceterms:offeredBy`; Issuer property correlates with `ceterms:offeredBy`. BadgeClass is not present in the assertion.
- CER resource is [`ceterms:OpenBadge`] (http://credreg.net/ctdl/terms#OpenBadge); CER resource contains multiple `ceterms:ownedBy` as well as a single `ceterms:offeredBy`; Issuer property does not correlates with `ceterms:ownedBy` or `ceterms:offeredBy`. BadgeClass is not present in the assertion.
- CER resource is [`ceterms:Certification`] (http://credreg.net/ctdl/terms#Certification); CER resource contains a single `ceterms:ownedBy` but not `ceterms:offeredBy`; Issuer property correlates with `ceterms:ownedBy`. BadgeClass is not present in the assertion.
- CER resource is [`ceterms:Certification`] (http://credreg.net/ctdl/terms#Certification); CER resource contains a single `ceterms:ownedBy` but not `ceterms:offeredBy`; Issuer property correlates with `ceterms:ownedBy`. BadgeClass is present in the assertion.
- CER resource is [`ceterms:OpenBadge`] (http://credreg.net/ctdl/terms#OpenBadge); CER resource contains a single `ceterms:ownedBy` but not `ceterms:offeredBy`; Issuer property correlates with `ceterms:ownedBy`. BadgeClass is present in the assertion but the badge class description does not match the CER resource `ceterms:description`] (http://credreg.net/ctdl/terms#description ). Note that description is a required property for both badge class and CER.