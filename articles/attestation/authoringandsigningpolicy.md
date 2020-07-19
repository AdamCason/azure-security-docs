---
title: Microsoft Azure Attestation 
description: XXX
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin


---
# Authoring and signing attestation policy for Microsoft Azure Attestation

Attestation provider gets created with a default policy for each TEE type. For default policy content, see [examples of attestation policy](policy-samples.md). If the default TEE policy of attestation provider meets the requirements, skip this section.

Else custom policy can be configured for any of the supported TEE types. The policy can be uploaded in a Azure Attestation specific policy format. Alternatively, an encoded version of the policy, in JWT, can also be uploaded. The policy administrator is responsible for writing the attestation policy. In most attestation scenarios, the relying party acts as the policy administrator. The client making the attestation call sends evidence which Azure Attestation  parses and converts into incoming claims (set of properties, value). Azure Attestation then processes the claims, based on what’s defined in the policy, and returns the computed result.

The policy contains rules that determine the authorization criteria, properties and the contents of the attestation token. Structure of policy file looks as below:

```Policy
version=1.0;
authizationrules
{
};

issuancerules
{
};
```

A policy file has 3 segments as seen above:
- Version
- Authorizationrules
- Issuancerules

**Version**: The version is the version number of the grammar that is followed.

```
Version=MajorVersion.MinorVersion	
```

Currently the only version supported is version 1.0.

**Authorizationrules**: The authorization rules are a collection of claim rules that will be checked first, to determine if Azure Attestation should proceed to issuancerules. The claim rules apply in the order they are defined.

**Issuancerules**: The issuance rules are a collection of claim rules that will be evaluated to add additional information to the attestation result as defined in the policy. The claim rules apply in the order they are defined and are also optional.

For policy samples, see [examples of attestation policy](policy-samples.md).
See [claim and claim rules](claimrulegrammar.md).to understand how to create rules in policy.

## Drafting the policy file
1. Create a new file.
2. Add version to the file.

  ```
  Version=1.0;
  ```
3. Add sections for authorizationrules and issuancerules

  ```
  Version=1.0;
  
  authorizationrules={
  =>deny();
  };
  
  issuancerules={
  };
  ```
 
  The authorization rules contains the deny() action without any condition, this is to make sure no issuance rules are processed. Alternatively, the authorization rule can also contain permit() action to allow processing of issuance rules.
4. Add claim rules to the authorization rules

  ```
  Version=1.0;
  
  authorizationrules={
  [type=="secureBootEnabled", value==true, issuer=="AttestationService"]=>permit();
  };
  
  issuancerules={
  };
  ```

  If the incoming claim set contains a claim which matches the type, value and issuer, the permit() action will indicate to the policy engine to process the issuancerules.
5. Add claim rules to issuancerules

  ```
  Version=1.0;
  
  authorizationrules={
  [type=="secureBootEnabled", value==true, issuer=="AttestationService"]=>permit();
  };
  
  issuancerules={
  =>issue(type="SecurityLevelValue", value=100)
  };
  ```
  
  The outgoing claim set will contain a claim with:

  ```
  [type="SecurityLevelValue", value=100, valueType="Integer", issuer="AttestationPolicy"]
  ```

  Complex policies can be crafted in a similar manner. 
6. Save file.

## Creating the policy file in JSON Web Token format

After creating a policy file, to upload a policy in JWT format, follow the below steps.
1. Generate the JWT with policy (utf-8 encoded) as the payload
  - The payload identifier for the Base64Url encoded policy should be “AttestationPolicy”.
  
  ``Sample JWT
  Header: {"alg":"none"}
  Payload: {“AttestationPolicy”:” Base64Url (policy)”}
  Signature: {}

  JWT format: eyJhbGciOiJub25lIn0.XXXXXXXXX.
``

2. Optionally to sign the policy, currently Azure Attestation supports the following algorithms: 
  a. None – When you don’t want to sign the policy payload
  b. RS256 – Supported algorithm to sign the policy payload

3. Upload the JWT and validate the policy (See “Policy management” section of this document)
  a. If the policy file is free of syntax errors the policy file gets accepted by the service.
  b. If the policy file contains syntax errors the policy file will be rejected by the service.

## Signing the policy

Below is a sample Python script on how to perform policy signing operation. See [benefits of polisy signing](basic-concepts.md##Benefits of policy signing) for more information.

```python
from OpenSSL import crypto
import jwt
import getpass
       
def cert_to_b64(cert):
              cert_pem = crypto.dump_certificate(crypto.FILETYPE_PEM, cert)
              cert_pem_str = cert_pem.decode('utf-8')
              return ''.join(cert_pem_str.split('\n')[1:-2])
       
print("Provide the path to the PKCS12 file:")
pkcs12_path = str(input())
pkcs12_password = getpass.getpass("\nProvide the password for the PKCS12 file:\n")
pkcs12_bin = open(pkcs12_path, "rb").read()
pkcs12 = crypto.load_pkcs12(pkcs12_bin, pkcs12_password.encode('utf8'))
ca_chain = pkcs12.get_ca_certificates()
ca_chain_b64 = []
for chain_cert in ca_chain:
   ca_chain_b64.append(cert_to_b64(chain_cert))
   signing_cert_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM, pkcs12.get_privatekey())
signing_cert_b64 = cert_to_b64(pkcs12.get_certificate())
ca_chain_b64.insert(0, signing_cert_b64)

print("Provide the path to the policy text file:")
policy_path = str(input())
policy_text = open(policy_path, "r").read()
encoded = jwt.encode({'text': policy_text }, signing_cert_pkey, algorithm='RS256', headers={'x5c' : ca_chain_b64})
print("\nAttestation Policy JWS:")
print(encoded.decode('utf-8'))
```

## Next steps
- Quickstart: [Set up Microsoft Azure Attestation using PowerShell](quickstart-powershell.md)

