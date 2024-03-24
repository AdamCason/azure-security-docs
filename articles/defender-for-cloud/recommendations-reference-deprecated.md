---
title: Reference table for all deprecated security recommendations in Microsoft Defender for Cloud
description: This article lists all Microsoft Defender for Cloud deprecated security recommendations that help you harden and protect your resources.
author: dcurwin
ms.service: defender-for-cloud
ms.topic: reference
ms.date: 03/13/2024
ms.author: dacurwin
ms.custom: generated
ai-usage: ai-assisted
---

# Deprecated security recommendations

This article lists all the deprecated security recommendations in Microsoft Defender for Cloud. The recommendations that appear in your environment are based on the resources that you're protecting and on your customized configuration.


## Deprecated recommendations


### Over-provisioned identities in subscriptions should be investigated to reduce the Permission Creep Index (PCI)

**Description**: Over-provisioned identities in subscription should be investigated to reduce the Permission Creep Index (PCI) and to safeguard your infrastructure. Reduce the PCI by removing the unused high risk permission assignments. High PCI reflects risk associated with the identities with permissions that exceed their normal or required usage
(No related policy).

**Severity**: Medium

### Over-provisioned identities in accounts should be investigated to reduce the Permission Creep Index (PCI)

**Description**: Over-provisioned identities in accounts should be investigated to reduce the Permission Creep Index (PCI) and to safeguard your infrastructure. Reduce the PCI by removing the unused high risk permission assignments. High PCI reflects risk associated with the identities with permissions that exceed their normal or required usage.

**Severity**: Medium

### Access to App Services should be restricted

**Description & related policy**: Restrict access to your App Services by changing the networking configuration, to deny inbound traffic from ranges that are too broad.
(Related policy: [Preview]: Access to App Services should be restricted).

**Severity**: High

### The rules for web applications on IaaS NSGs should be hardened

**Description & related policy**: Harden the network security group (NSG) of your virtual machines that are running web applications, with NSG rules that are overly permissive with regard to web application ports.
(Related policy: The NSGs rules for web applications on IaaS should be hardened).

**Severity**: High

### Pod Security Policies should be defined to reduce the attack vector by removing unnecessary application privileges (Preview)

**Description & related policy**: Define Pod Security Policies to reduce the attack vector by removing unnecessary application privileges. It is recommended to configure pod security policies so pods can only access resources which they are allowed to access.
(Related policy: [Preview]: Pod Security Policies should be defined on Kubernetes Services).

**Severity**: Medium

### Install Azure Security Center for IoT security module to get more visibility into your IoT devices

**Description & related policy**: Install Azure Security Center for IoT security module to get more visibility into your IoT devices.

**Severity**: Low

### Your machines should be restarted to apply system updates

**Description & related policy**: Restart your machines to apply the system updates and secure the machine from vulnerabilities.
(Related policy: System updates should be installed on your machines).

**Severity**: Medium

### Monitoring agent should be installed on your machines

**Description & related policy**: This action installs a monitoring agent on the selected virtual machines. Select a workspace for the agent to report to.
(No related policy)

**Severity**: High

### Java should be updated to the latest version for web apps

**Description & related policy**: Periodically, newer versions are released for Java software either due to security flaws or to include additional functionality.
Using the latest Java version for web apps is recommended to benefit from security fixes, if any, and/or new functionalities of the latest version.
(Related policy: Ensure that 'Java version' is the latest, if used as a part of the Web app).

**Severity**: Medium

### Python should be updated to the latest version for function apps

**Description & related policy**: Periodically, newer versions are released for Python software either due to security flaws or to include additional functionality.
Using the latest Python version for function apps is recommended to benefit from security fixes, if any, and/or new functionalities of the latest version.
(Related policy: Ensure that 'Python version' is the latest, if used as a part of the Function app).

**Severity**: Medium

### Python should be updated to the latest version for web apps

**Description & related policy**: Periodically, newer versions are released for Python software either due to security flaws or to include additional functionality.
Using the latest Python version for web apps is recommended to benefit from security fixes, if any, and/or new functionalities of the latest version.
(Related policy: Ensure that 'Python version' is the latest, if used as a part of the Web app).

**Severity**: Medium

### Java should be updated to the latest version for function apps

**Description & related policy**: Periodically, newer versions are released for Java software either due to security flaws or to include additional functionality.
Using the latest Java version for function apps is recommended to benefit from security fixes, if any, and/or new functionalities of the latest version.
(Related policy: Ensure that 'Java version' is the latest, if used as a part of the Function app).

**Severity**: Medium

### PHP should be updated to the latest version for web apps

**Description & related policy**: Periodically, newer versions are released for PHP software either due to security flaws or to include additional functionality.
Using the latest PHP version for web apps is recommended to benefit from security fixes, if any, and/or new functionalities of the latest version.
(Related policy: Ensure that 'PHP version' is the latest, if used as a part of the WEB app).

**Severity**: Medium

### [Endpoint protection health issues on machines should be resolved](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/37a3689a-818e-4a0e-82ac-b1392b9bb000)

**Description**: Resolve endpoint protection health issues on your virtual machines to protect them from latest threats and vulnerabilities. See the documentation for the [endpoint protection solutions supported by Defender for Cloud](/azure/defender-for-cloud/supported-machines-endpoint-solutions-clouds#supported-endpoint-protection-solutions-) and the [endpoint protection assessments](/azure/defender-for-cloud/endpoint-protection-recommendations-technical).
(No related policy)

**Severity**: Medium

### [Endpoint protection should be installed on machines](https://portal.azure.com/#blade/Microsoft_Azure_Security/RecommendationsBlade/assessmentKey/4fb67663-9ab9-475d-b026-8c544cced439)

**Description**: To protect machines from threats and vulnerabilities, install a supported endpoint protection solution.
Learn more about how endpoint protection for machines is evaluated in [Endpoint protection assessment and recommendations in Microsoft Defender for Cloud](/azure/defender-for-cloud/endpoint-protection-recommendations-technical).
(No related policy)

**Severity**: High

## Related content

- [What are security policies, initiatives, and recommendations?](security-policy-concept.md)
- [Review your security recommendations](review-security-recommendations.md)
