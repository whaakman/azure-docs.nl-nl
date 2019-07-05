---
title: Uw Azure Active Directory Domain Services beheerde domein beveiligen | Microsoft Docs
description: Uw beheerde domein beveiligen
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 6b4665b5-4324-42ab-82c5-d36c01192c2a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: e94cd9ca049cfdfd2321ce046714506ed1f23390
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483278"
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

> [!IMPORTANT]
> Gebruikers (en serviceaccounts) uitvoeren niet eenvoudige LDAP-bindingen als u NTLM-wachtwoord-hashsynchronisatie hebt uitgeschakeld op uw Azure AD Domain Services-exemplaar.  Lees voor meer informatie over het uitschakelen van NTLM-wachtwoord-hashsynchronisatie [beveiligen van uw Azure AD DOmain Services beheerde domein](secure-your-domain.md).
>
>

## <a name="next-steps"></a>Volgende stappen
* [Inzicht in synchronisatie in Azure AD Domain Services](synchronization.md)
