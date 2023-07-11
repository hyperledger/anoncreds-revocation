## Abstract

This paper discusses the requirements for a revocation mechanism in AnonCreds, a system that supports the issuance and verification of verifiable credentials. The revocation mechanism aims to allow the holder of a credential to prove its non-revocation without revealing any correlatable identifiers. This paper presents the data flow involved in the revocation process, including issuer setup, credential issuance, activation/revocation, presentation request, and verification. With each part of the process are defined the privacy, security, and practical requirements of a suitable AnonCreds revocation scheme.

The paper covers the following elements of verifiable credential revocation.

**1. Introduction**
   - Background and motivation
   - Objectives of an AnonCreds revocation mechanism

**2. AnonCreds Issuer Setup With Revocation**
   - Creation and publication of a revocable Credential Definition (CredDef)
   - Establishment of a Revocation Registry for tracking revocation status
   - Ability to create additional Revocation Registries as needed

**3. AnonCreds Issuance with Revocation**
   - Assignment of a RevocationId to each issued credential
   - Correlation of RevocationId with the holder for revocation purposes
   - Importance of not sharing RevocationId with verifiers

**4. AnonCreds Credential Activation/Revocation and Publication**
   - Revocation process initiated by the issuer
   - Update of the Revocation Registry to mark revoked credentials
   - Publication of RevRegEntry depending on the revocation scheme
   - No technical requirement for issuer-to-holder notification of revocation
   - Holder's ability to detect revocation through proof creation

**5. AnonCreds Presentation Request with Revocation**
   - Request from verifier to holder for a non-revocation proof (NRP)
   - Creation of NRP by the holder, including cryptographic link to the source credential

**6. AnonCreds Verification with Revocation**
   - Verifier's processing of non-revocation-related parts of the presentation
   - Verifier's processing of revocation-related parts of the presentation
   - Combination of verification outcomes from all proofs in the presentation
   - Rejection of unverifiable presentations due to unsuccessful proof verification

**7. Conclusion**
   - Summary of the revocation data flow in AnonCreds
   - Implications and benefits of the revocation mechanism
   - Future research directions and potential enhancements

**References:**
Include all the references cited throughout the paper.

**Keywords:**
AnonCreds, verifiable credentials, revocation mechanism, issuer, holder, verifier, Credential Definition, Revocation Registry, RevocationId, non-revocation proof, proof verification.