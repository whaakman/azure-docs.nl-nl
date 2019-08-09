---
title: Uw door Azure Active Directory Domain Services beheerd domein beveiligen | Microsoft Docs
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
ms.openlocfilehash: 923ecae9dc649b8f5cdcfd447b78fdec0805927a
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879153"
---
# <a name="secure-your-azure-ad-domain-services-managed-domain"></a>Uw door Azure AD Domain Services beheerd domein beveiligen
Dit artikel helpt u bij het beveiligen van uw beheerde domein. U kunt het gebruik van zwakkere coderings suites uitschakelen en NTLM-referentie-hash-synchronisatie uitschakelen.

## <a name="install-the-required-powershell-modules"></a>De vereiste Power shell-modules installeren

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD Power Shell installeren en configureren
Volg de instructies in het artikel om [de Azure AD Power shell-module te installeren en verbinding te maken met Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell installeren en configureren
Volg de instructies in het artikel om [de Azure PowerShell-module te installeren en verbinding te maken met uw Azure-abonnement](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).


## <a name="disable-weak-cipher-suites-and-ntlm-credential-hash-synchronization"></a>Zwakke coderings suites en hash-synchronisatie van NTLM-referenties uitschakelen
Gebruik het volgende Power shell-script om:

1. Ondersteuning van NTLM v1 voor het beheerde domein uitschakelen.
2. Schakel de synchronisatie van NTLM-wachtwoord-hashes uit vanuit uw on-premises AD.
3. TLS v1 voor het beheerde domein uitschakelen.

Als er een fout bericht wordt weer `Get-AzResource` gegeven met de opdracht dat de resource *micro soft. Aad/DomainServices* niet bestaat, [moet u uw toegang verhogen om alle Azure-abonnementen en-beheer groepen te beheren](../role-based-access-control/elevate-access-global-admin.md).

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
> Gebruikers (en service accounts) kunnen geen LDAP-eenvoudige bindingen uitvoeren als NTLM-wachtwoord synchronisatie is uitgeschakeld op uw Azure AD Domain Services-exemplaar.  Lees [uw door Azure AD DOmain Services beheerde domein beveiligen](secure-your-domain.md)voor meer informatie over het uitschakelen van NTLM-wachtwoord synchronisatie.
>
>

## <a name="next-steps"></a>Volgende stappen
* [Synchronisatie begrijpen in Azure AD Domain Services](synchronization.md)
