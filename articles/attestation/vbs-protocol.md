---
title: VBS attestation protocol 
description: VBS attestation protocol 
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: reference
ms.date: 07/20/2020
ms.author: mbaldwin


---

# VBS attestation protocol 

- Enclave starts the protocol by requesting a challenge from the service (shown as the Init message)
- Microsoft Azure Attestation generates a random challenge and creates a session context which contains such challenge and a timestamp. The session context is encrypted and       signed using K-Context **ENC_SIGN(K-Context)[Timestamp || Challenge]**.  Context is opaque to the client
- The enclave then:
    a.	Generates a key pair Kpriv/pub-Enclave
    b.	Retrieves the Boot/Resume logs and quote from the TPM using the challenge received from the service (notice that the enclave itself cannot call the TPM functions so it           calls back to VTL-0 to retrieve those values but this does not affect security as invalid/replayed logs or quotes will be detected)
    c.	Requests a signed report from VSM (EnclaveGetAttestationReport()) with the hash of the fields in the request
    d.	VSM Report is signed by Kpriv-IDKS (VSM Signing IDK) in the secure kernel
    e.	A message to be sent to the service is created with the following format:

    ```
    Signature algorithm || Request || SIGN(Kpriv-Enclave)[Request]

    Request = RP id || RP custom data || Challenge || Boot/Resume logs || AIK X.509 Certificate || TPM Quote || Kpub-Enclave || Custom claims || VSM Report || SIGN(Kpriv-IDKS)       [VSM Report] || Context

    ```

- Azure Attestation receives the message from the client and:
    a.	Verifies the signature and decrypts the session context. If the context has expired, the service fails the request
    b.	Verifies that the AIK X.509 Certificate is valid and trusted, i.e., issued by a trusted CA
    c.	Verifies the TPM quote was signed properly by a “known” TPM key (“known” in this context means that the TPM key used to sign the quote is indirectly trusted given that           the client uses AIK to sign the quote and the AIK Cert has already been verified). The quote must also include the same challenge as present in the context
    d.	Verifies the Boot/Resume logs against the PCRs in the TPM quote
    e.	Gets Kpub-IDKS from the now trusted Boot/Resume logs and verifies that the VSM Report was signed with it **(VERIFY(Kpub-IDKS)[VSM Report])**
    f.	Computes hash of the fields in the request and compares such hash against the hash present in the VSM Report, which is now trusted
    g.	Using the Kpub-Enclave received, confirm the client has possession of Kpriv-Enclave by verifying that the request was signed by such key **(VERIFY(Kpub-Enclave)                   [Request])**
    h.	Applies the policy:
        i.	Uses the authorization rules to make sure the client platform is trusted, e.g. expected enclave ID and SVN, enclave does not permit debugging, secure boot is                     enabled, test signing is disabled, etc.;
        ii.	Uses the issuance rules to add claims to the final report
    i.	Issues an attestation report signed by Kpriv-Attestation, the attestation Service key
- Client application can now request the attestation report from the enclave. The report can then be parsed and trusted by relying parties as it is signed by Azure Attestation       

The reason for sending the encrypted context to the client is to relieve the service from having to keep state (context) about an attestation session. Security in this case is therefore tied to the strength of K-Context. For increased security or other reasons, it is possible to change the protocol not to send the context to the client and instead keep state in the service on a memory cache, if the connection between the client and the service node is persistent. If the connection is not persistent, the state can be stored in a database shared by all nodes, however in this case, a session identifier must be sent to the client. In this case, special care has to be taken with the session identifier to prevent an attacker from using another session’s identifier.
