## AnonCreds Revocation Data Flow

AnonCreds includes a mechanism that supports the revocation of verifiable
credentials. This mechanism includes:

- An [[ref: issuer]] setting up to issue revocable credentials.
- An [[ref: issuer]] issuing revocable credentials.
- An [[ref: issuer]] activating or revoking issued credentials.
- A [[ref: verifier]] requesting a presentation to include a non-revocation proof
  for one or more revocable credentials.
- A [[ref: holder]] generating based on the presentation request of the verifier a
  non-revocation proof for attributes derived from revocable credentials.
- A [[ref: verifier]] verifying a non-revocation proof included in a
  presentation from a [[ref: holder]].

A fundamental goal of AnonCreds is to not provide a correlatable identifier for
either a [[ref: holder]] or a credential as part of generation and verification
of an AnonCreds presentation. Applying that goal to revocation means that the
revocation mechanism must support the [[ref: holder]] proving a credential used
in generating a presentation is not revoked without providing a correlatable
identifier for that credential or the holder itself. As such, an AnonCreds
revocation mechanism must use some sort of Zero Knowledge Proof (ZKP) that allows the [[ref:
holder]] to prove a credential they hold is not revoked without revealing an
identifier for their credential or for the holder themselves.

In the following, we describe the basic flow that must be supported by any
revocation scheme suitable for AnonCreds.

### AnonCreds Issuer Setup With Revocation

Before issuing credentials of a given type with AnonCreds, an issuer must create
and publish a Credential Definition ("CredDef") that is flagged to be revocable.
In addition, the issuer must also create a Revocation Registry, which holds the
revocation status of a set of issued credentials. A Revocation Registry is
linked to a given Credential Definition. Each Revocation Registry may be of a
limited size (number of tracked credentials). If so, an Issuer **MUST** be able
to create additional Revocation Registries when needed so as to have an
effectively unlimited supply of credentials of a given type.

### AnonCreds Issuance with Revocation

When an Issuer issues a credential to a holder, they identify both the
Revocation Registry in which they will track the revocation status of the
credential, and an identifier (index) within the Revocation Registry for the
specific credential. We'll call the combination of the identifier for the
Revocation Registry, and the identifier for a given credential within the
Revocation Registry the RevocationId. The RevocationId is tracked by the issuer,
correlated with the holder, such that when necessary, the Issuer can revoke the
credential. The RevocationID is shared with the holder. The holder will use that
information when needed for creating a proof that their credential has not been
revoked. The RevocationID is a linkable identifier for the credential and so
**MUST** not be shared with verifiers.

### AnonCreds Credential Activation/Revocation and Publication

When an [[ref: issuer]] decides to revoke one or more issued credentials, they
do so by publishing an update to the state of the Revocation Registry, updating
(from the last state) the status of each revoked credential. Where the
RevRegEntry is published depends on the revocation scheme--it might be to a
Verifiable Data Registry (VDR), it might be to a Revocation Manager service, or it
might be to a file on a web server.

The [[ref: holder]] is not involved in the process of revoking a credential.
There is no technical requirement for an [[ref: issuer]] to notify the [[ref:
holder]] that a credential they were issued has been revoked. That said, it is a
courtesy that may improve the user experience of the [[ref: holder]]. [Aries RFC
0183 Revocation
Notification](https://github.com/hyperledger/aries-rfcs/tree/main/features/0183-revocation-notification)
is an example of how that can be done. Even if not notified by the [[ref:
issuer]] of the revocation of a credential, the [[ref: holder]] can detect their
credential has been revoked when they retrieve a RevRegEntry, attempt to create
a proof that their credential is not revoked, and discover they are unable to do
so.

### AnonCreds Presentation Request with Revocation

Carrying out an AnonCreds presentation with revocation is a two-step process, beginning with a
request from the [[ref: verifier]] asking the [[ref: holder]] to include a
non-revocation proof (NRP) in the presentation, and then the [[ref: holder]]
creating the NRP and including it in the presentation sent to the [[ref:
verifier]]. The NRP must include a cryptographic link to the source verifiable credential
to which it is related.

- A holder **MUST** not be able to produce a valid proof that their credential
  has not been revoked based on a RevRegEntry in which their credential is
  marked as revoked.


### AnonCreds Verification with Revocation

A [[ref: verifier]] receives the presentation from the [[ref: holder]] and
processes the [non-revocation-related parts of the presentation](#verify-presentation) and
the [revocation-related parts of the presentation](#verify-non-revocation-proof)
(if any) in the presentation. The resulting status of the presentation combines the
verification outcomes from processing all proofs within the presentation. If
verification of one or more of the embedded proofs is unsuccessful, the
presentation is rejected as unverifiable.
