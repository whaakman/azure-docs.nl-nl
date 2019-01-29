---
title: Uw Azure Active Directory Domain Services beheerde domein beveiligen | Microsoft Docs
description: Beveiligen van uw beheerde domein
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 6b4665b5-4324-42ab-82c5-d36c01192c2a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: ergreenl
ms.openlocfilehash: 3797c76f1537f86357f7ca68ffed4758eb1bdc9a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173759"
---
# <a name="secure-your-azure-ad-domain-services-managed-domain"></a>Beveiligen van uw Azure AD Domain Services beheerde domein
Dit artikel helpt u uw beheerde domein beveiligen. U kunt het gebruik van zwakke coderingssuites en hash-synchronisatie van NTLM-referenties uitschakelen.

## <a name="install-the-required-powershell-modules"></a>De vereiste PowerShell-modules installeren

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD PowerShell installeren en configureren
Volg de instructies in het artikel [installeren van de Azure AD PowerShell-module en maak verbinding met Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell installeren en configureren
Volg de instructies in het artikel [installeren van de Azure PowerShell-module en maak verbinding met uw Azure-abonnement](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).


## <a name="disable-weak-cipher-suites-and-ntlm-credential-hash-synchronization"></a>Uitschakelen van zwakke coderingssuites en hash-synchronisatie van NTLM-referenties
Gebruik de volgende PowerShell-script voor:
1. Ondersteuning van NTLM v1 voor het beheerde domein uitschakelen.
2. Uitschakelen van de synchronisatie van wachtwoord-hashes voor NTLM vanuit uw on-premises AD.
3. TLS v1 voor het beheerde domein uitschakelen.

```powershell
// Login to your Azure AD tenant
Login-AzAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// 1. Disable NTLM v1 support on the managed domain.
// 2. Disable the synchronization of NTLM password hashes from
//    on-premises AD to Azure AD and Azure AD Domain Services
// 3. Disable TLS v1 on the managed domain.
$securitySettings = @{"DomainSecuritySettings"=@{"NtlmV1"="Disabled";"SyncNtlmPasswords"="Disabled";"TlsV1"="Disabled"}}

// Apply the settings to the managed domain.
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $securitySettings -Verbose -Force
```

## <a name="next-steps"></a>Volgende stappen
* [Inzicht in synchronisatie in Azure AD Domain Services](active-directory-ds-synchronization.md)
