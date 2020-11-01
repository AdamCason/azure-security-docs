---
title: Troubleshoot issues
description: Trouble shooting guide to the commonly observed issues
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: reference
ms.date: 07/20/2020
ms.author: mbaldwin


---

# Microsoft Azure Attestation troubleshooting guide

Error handling in Azure Attestation is implemented following [Microsoft REST API guidelines](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#7102-error-condition-responses). The error response returned by Azure Attestation APIs contains HTTP status code and name/value pairs with the names “code” and “message”. The value of “code” is human-readable and is an indicator of the type of error. The value of “message” intends to aid the user and provides error details.

If your issue is not addressed in this article, you can also submit an Azure support request on the [Azure support page](https://azure.microsoft.com/support/options/).

Below are some examples of the errors returned by Azure Attestation:

## HTTP – 401 : Unauthorized exception

### HTTP status code
401

#### Error code
Unauthorized

#### Scenarios
Examples:
  - Attestation failure if the user is not assigned with Attestation Reader role
  - Unable to manage attestation policies as the user is not assigned with appropriate roles
  - Unable to manage attestation policy signers as the user is not assigned with appropriate roles
  
### Message
User with Reader role trying to edit an attestation policy in PowerShell 

  ```powershell
  Set-AzAttestationPolicy : Operation returned HTTP Status Code 401
At line:1 char:1
+ Set-AzAttestationPolicy -Name $attestationProvider -ResourceGroupName ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzAttestationPolicy], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.SetAzureAttestationPolicy
  ```

#### Troubleshooting steps

In order to view attestation policies/policy signers, an Azure AD user requires the permission for "Actions":
- Microsoft.Attestation/attestationProviders/attestation/read

  This permission can be assigned to an AD user through a role such as "Owner" (wildcard permissions) or "Reader" (wildcard permissions) or "Attestation Reader" (specific         permissions for Azure Attestation only).

In order to add/delete policy signers or to configure policies, an Azure AD user requires the following permissions for "Actions":
- Microsoft.Attestation/attestationProviders/attestation/write
- Microsoft.Attestation/attestationProviders/attestation/delete

  These permissions can be assigned to an AD user through a role such as "Owner" (wildcard permissions), "Contributor" (wildcard permissions) or "Attestation Contributor"         (specific permissions for Azure Attestation only).

Customers can choose to use the default provider for attestation, or create their own providers with custom policies. "Owner" (wildcard permissions) or "Reader" (wildcard permissions) or "Attestation Reader" role is required to send attestation requests to custom providers. The default providers are accessible by any Azure AD user.

#### PowerShell

To verify the roles in PowerShell, please run below:
- Launch PowerShell and log into Azure via the "Connect-AzAccount" cmdlet
- Verify your RBAC role assignment settings. 

  ```powershell
  $c = Get-AzContext
  Get-AzRoleAssignment -ResourceGroupName $attestationResourceGroup -ResourceName $attestationProvider -ResourceType Microsoft.Attestation/attestationProviders -SignInName $c.Account.Id
  ```

  You should see something like this:

  ```
  RoleAssignmentId   :/subscriptions/subscriptionId/providers/Microsoft.Authorization/roleAssignments/roleAssignmentId
  
  Scope              : /subscriptions/subscriptionId
  
  DisplayName        : displayName
  
  SignInName         : signInName
  
  RoleDefinitionName : Reader
  
  RoleDefinitionId   : roleDefinitionId
  
  ObjectId           : objectid
  
  ObjectType         : User
  
  CanDelegate        : False
 
  ```

If you don't find an appropriate role assignment in the list, follow the instructions in [here](/azure/role-based-access-control/role-assignments-powershell)

## HTTP – 400 errors

### HTTP status code
400

There are different reasons why a request may return 400. Below are some examples of errors returned by Azure Attestation APIs:

### Attestation failure due to policy evaluation errors

Attestation policy includes authorization rules and issuance rules. Enclave evidence is evaluated based on the authorization rules. Issuance rules define the claims to be included in attestation token. If claims in enclave evidence do not comply with authorization rules, attest calls will return policy evaluation error. 

#### Error code
PolicyEvaluationError

#### Examples
```
Native operation failed with 65518: G:\Az\security\Attestation\src\AttestationServices\Instance\NativePolicyWrapper\NativePolicyEngine.cpp(168)\(null)!00007FF801762308: (caller: 00007FF80143DCC8) Exception(0) 83FFFFEE Policy Evaluation Error has occurred Msg:[Policy Engine Exception: A Deny claim was issued, authorization failed.]

G:\Az\security\Attestation\src\AttestationServices\Instance\Enclave\api.cpp(840)\(null)!00007FF801739FF3: (caller: 00007FF801232801) LogHr(0) 83FFFFEE Policy Evaluation Error has occurred Msg:[Unhandled Enclave Exception: "Policy Evaluation Error has occurred"]

```

#### Troubleshooting steps
Users can evaluate enclave evidence against an SGX attestation policy before configuring the same.

Send a request to attest API by providing policy text in “draftPolicyForAttestation” parameter. The AttestSgxEnclave API will use this policy document during the attest call and this can be used to test attestation policies before they are consumed. Note that the MAA token generated when this field is present will be unsecured.

See [attestation policy examples ](/azure/attestation/policy-examples)

### Attestation failure due to invalid input

#### Error code
InvalidParameter

#### Examples
SGX attestation failure due to invalid input

Below are some of the examples for error messages:
The specified quote was invalid due to an error in the quote collateral 
The specified quote was invalid because the device on which the quote was generated does not meet the azure baseline requirements
The specified quote was invalid because the TCBInfo or QEID provided by the PCK Cache Service was invalid

#### Troubleshooting steps

Microsoft Azure Attestation supports attestation of SGX quotes generated by Intel SDK and Open Enclave SDK.

Refer to [code samples](https://docs.microsoft.com/en-us/samples/browse/?expanded=azure&terms=attestation) for performing attestation using Open Enclave SDK/ Intel SDK

### Invalid certificate chain error while uploading policy/policy signer

#### Error code
InvalidParameter

#### Examples
**Upload policy/policy signer signed with an invalid certificate chain**

```
Native operation failed with 65529: C:\source\src\AttestationServices\Instance\SgxPal\sgxcert.cpp(1074)\(null)!00007FFA285CDAED: (caller: 00007FFA285C36E8) Exception(0) 83FFFFF9 The requested item is not found    Msg:[Unable to find issuer certificate CN=attestationsigningcert]
C:\source\src\AttestationServices\Instance\Enclave\api.cpp(618)\(null)!00007FFA286DCBF8: (caller: 00007FFA285860D3) LogHr(0) 83FFFFF9 The requested item is not found    Msg:[Unhandled Enclave Exception: "The requested item is not found"]
At line:1 char:1
+ Set-AzAttestationPolicy -Name "testpolicy1" -ResourceGroupName "BugBa ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzAttestationPolicy], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.SetAzureAttestationPolicy

```

#### Troubleshooting steps
The root certificate must be flagged as being issued by a CA (the X.509 basic constraints), else it will not be considered as a valid certificate. 

Ensure that the Basic Constraints extension of the root certificate is set to indicate that Subject Type = CA

Else the certificate chain is considered to be invalid. See [policy signer](https://docs.microsoft.com/en-us/azure/attestation/policy-signer-examples) and [policy](https://docs.microsoft.com/en-us/azure/attestation/policy-examples) examples 

### Add/Delete policy signer failure

#### Error code
InvalidOperation

#### Examples

**when user uploads invalid signer**

```
Add-AzAttestationPolicySigner : Operation returned HTTP Status Code 400
Code: InvalidOperation
Message: Native operation failed with 74: ..\Shared\base64url.h(226)\(null)!: (caller: ) Exception(0) 83FF004A Bad
message    Msg:[Unknown base64 character: 41 (')')]
..\Enclave\api.cpp(496)\(null)!: (caller: ) LogHr(0) 83FF004A Bad message    Msg:[Unhandled Enclave Exception: "Bad
message"]
At line:1 char:1
+ Add-AzAttestationPolicySigner -Name $attestationProvider -ResourceGro ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Add-AzAttestationPolicySigner], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.AddAzureAttestationPolicySigner

```

**when user uploads JWS without "maa-policyCertificate" claim**

```
Add-AzAttestationPolicySigner : Operation returned HTTP Status Code 400
Code: InvalidOperation
Message: Native operation failed with 74: ..\Enclave\enclave.cpp(2213)\(null)!: (caller: ) Exception(0) 83FF004A Bad
message    Msg:[Could not find "maa-policyCertificate" claim in policy token]
..\Enclave\api.cpp(496)\(null)!: (caller: ) LogHr(0) 83FF004A Bad message    Msg:[Unhandled Enclave Exception: "Bad
message"]
At line:1 char:1
+ Add-AzAttestationPolicySigner -Name $attestationProvider -ResourceGro ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Add-AzAttestationPolicySigner], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.AddAzureAttestationPolicySigner

```

**when user does not upload a certificate in JWS format**

```

Add-AzAttestationPolicySigner : Operation returned HTTP Status Code 400
Code: InvalidOperation
Message: Native operation failed with 74: ..\JsonWebToken\jsonwebtoken.cpp(375)\(null)!: (caller: ) LogHr(0) 83FF004A
Bad message    Msg:[RETURN_IF_TRUE('(firstPeriod == std::string::npos)') failed with 0x4a: Malformed JWT: Could not
find first period in the token.]
..\Enclave\enclave.cpp(2106)\(null)!: (caller: ) LogHr(0) 83FF004A Bad message
Msg:[THROW_IF_ERROR('DecomposeJsonWebSignature(&policyJws, encodedJoseHeader, encodedJwsBody, jwsSignature)') failed
with 0x4a: 'Bad message']
..\Enclave\enclave.cpp(2106)\(null)!: (caller: ) Exception(0) 83FF004A Bad message
..\Enclave\api.cpp(496)\(null)!: (caller: ) LogHr(0) 83FF004A Bad message    Msg:[Unhandled Enclave Exception: "Bad
message"]
At line:1 char:1
+ Add-AzAttestationPolicySigner -Name $attestationProvider -ResourceGro ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Add-AzAttestationPolicySigner], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.AddAzureAttestationPolicySigner
```

#### Trouble shooting steps
To add/delete a new policy signer certificate, use RFC7519 JSON Web Token (JWT) with a claim named "x-ms-policyCertificate". Value of the claim is a RFC7517 JSON Web Key which contains the certificate to be added. JWT must be signed with private key of any of the valid policy signer certificates associated with the provider. See policy signer examples

## Az.Attestation installation issues in PowerShell

Unable to install Az or Az.Attestation modules in PowerShell

### Error

WARNING: Unable to resolve package source 'https://www.powershellgallery.com/api/v2' 
PackageManagement\Install-Package : No match was found for the specified search criteria and module name

### Troubleshooting steps

PowerShell Gallery has deprecated Transport Layer Security (TLS) versions 1.0 and 1.1. 

TLS 1.2 or a later version is recommended. 

To continue to interact with the PowerShell Gallery, run the following command before the Install-Module commands

**[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12**

## Policy access/configuration issues in PowerShell

User assigned with appropriate roles. But facing authorization issues while managing attestation policies through PowerShell.

### Error
The client with object id <object Id>  does not have authorization to perform action Microsoft.Authorization/roleassignments/write over scope ‘subcriptions/<subscriptionId>resourcegroups/secure_enclave_poc/providers/Microsoft.Authorization/roleassignments/<role assignmentId>’ or the scope is invalid. If access was recently granted, please refresh your credentials

### Troubleshooting steps

Minimum version of Az modules required to support attestation operations are the below: 

 **Az 4.5.0** 
 
 **Az.Accounts 1.9.2**
 
 **Az.Attestation 0.1.8** 

Run the below command to verify the installed version of all Az modules 

```powershell
Get-InstalledModule 
```

If the versions are not matching with the minimum requirement, run Update-Module commands

e.g. - Update-Module -Name Az.Attestation
