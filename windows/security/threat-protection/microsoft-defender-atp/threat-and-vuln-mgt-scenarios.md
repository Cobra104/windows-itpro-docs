---
title: Threat & Vulnerability Management scenarios
description: Learn how Threat & Vulnerability Management can be used to help security admins, IT admins, and SecOps collaborate in defending against security threats.
keywords: mdatp-tvm scenarios, mdatp, tvm, tvm scenarios, reduce threat & vulnerability exposure, reduce threat and vulnerability, improve security configuration, increase configuration score, increase threat & vulnerability configuration score, configuration score, exposure score, security controls 
search.product: eADQiWindows 10XVcnh
search.appverid: met150
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
ms.author: ellevin
author: levinec
ms.localizationpriority: medium
manager: dansimp
audience: ITPro
ms.collection: M365-security-compliance 
ms.topic: article
---

# Threat & Vulnerability Management scenarios

**Applies to:**

- [Microsoft Defender Advanced Threat Protection (Microsoft Defender ATP)](https://go.microsoft.com/fwlink/p/?linkid=2069559)

>Want to experience Microsoft Defender ATP? [Sign up for a free trial.](https://www.microsoft.com/microsoft-365/windows/microsoft-defender-atp?ocid=docs-wdatp-portaloverview-abovefoldlink)

[!include[Prerelease information](../../includes/prerelease.md)]

## Use advanced hunting query to search for machines with High active alerts or critical CVE public exploit

1. Go to **Advanced hunting** from the left-hand navigation pane of the Microsoft Defender Security Center.

2. Scroll down to the TVM advanced hunting schemas to familiarize yourself with the column names.

3. Enter the following queries:

```kusto
// Search for machines with High active alerts or Critical CVE public exploit
DeviceTvmSoftwareInventoryVulnerabilities
| join kind=inner(DeviceTvmSoftwareVulnerabilitiesKB) on CveId
| where IsExploitAvailable == 1 and CvssScore >= 7
| summarize NumOfVulnerabilities=dcount(CveId),
DeviceName=any(DeviceName) by DeviceId
| join kind =inner(DeviceAlertEvents) on DeviceId  
| summarize NumOfVulnerabilities=any(NumOfVulnerabilities),
DeviceName=any(DeviceName) by DeviceId, AlertId
| project DeviceName, NumOfVulnerabilities, AlertId  
| order by NumOfVulnerabilities desc

```

## Find and remediate software or software versions which have reached end-of-support (EOS)

End-of-support (otherwise known as end-of-life) for software or software versions means that they will no longer be supported or serviced, and will not receive security updates. When you use software or software versions which have reached end-of-support, you're exposing your organization to security vulnerabilities, legal, and financial risks.

It is crucial for Security and IT Administrators to work together and ensure that the organization's software inventory is configured for optimal results, compliance, and a healthy network ecosystem. They should examine the options to remove or replace apps that have reached end of support, and update versions that have reached end of support. It is best to create and implement a plan **before** the end of support dates.

To find software or software versions which have reached end-of-support:

1. From the Threat & Vulnerability Management menu, navigate to **Security recommendations**.
2. Go to the **Filters** panel and look for the tags section. Select one or more of the EOS tag options. Then **Apply**.

    ![Screenshot tags that say EOS software, EOS versions, and Upcoming EOS versions](images/tvm-eos-tag.png)

3. You will see a list recommendations related to software that is end of support, software versions that are end of support, or upcoming end of support versions. These tags are also visible in the [software inventory](tvm-software-inventory.md) page.

    ![Screenshot tags that say EOS software, EOS versions, and Upcoming EOS versions](images/tvm-eos-tags-column.png)

After you have identified which software and software versions are vulnerable due to its end-of-support status, remediate them to lower your organizations exposure to vulnerabilities and advanced persistent threats. See [Remediation and exception](tvm-remediation.md) for details.

## Use APIs

Threat and vulnerability management supports multiple APIs. See the following topics for related APIs:

- [Machine APIs](machine.md)
- [Recommendation APIs](vulnerability.md)
- [Score APIs](score.md)
- [Software APIs](software.md)
- [Vulnerability APIs](vulnerability.md)

## Related topics

- [Supported operating systems and platforms](tvm-supported-os.md)
- [Risk-based Threat & Vulnerability Management](next-gen-threat-and-vuln-mgt.md)
- [Threat & Vulnerability Management dashboard overview](tvm-dashboard-insights.md)
- [Exposure score](tvm-exposure-score.md)
- [Configuration score](configuration-score.md)
- [Security recommendations](tvm-security-recommendation.md)
- [Remediation and exception](tvm-remediation.md)
- [Software inventory](tvm-software-inventory.md)
- [Weaknesses](tvm-weaknesses.md)
- [Advanced hunting overview](overview-hunting.md)
- [All advanced hunting tables](advanced-hunting-reference.md)
- [Configure data access for Threat & Vulnerability Management roles](user-roles.md#create-roles-and-assign-the-role-to-an-azure-active-directory-group)
