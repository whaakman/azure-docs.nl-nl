---
title: Het wijzigen van de levensduur van tokens standaard ingesteld voor een aangepaste toepassing | Microsoft Docs
description: Het bijwerken van beleid voor de levensduur van Token voor uw toepassing die u op Azure AD ontwikkelt
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: celested
ms.custom: seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 04abdedf5ac19be3d5a43e7502cbc97f8f5fee43
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59360305"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Het wijzigen van de levensduur van vernieuwingstoken standaardwaarden voor een aangepaste toepassing

Dit artikel leest hoe u Azure AD PowerShell gebruiken om in te stellen van een beleid voor de levensduur van tokens. Azure AD Premium biedt app-ontwikkelaars en tenantbeheerders het configureren van de levensduur van tokens die zijn uitgegeven voor niet-vertrouwelijke clients. Levensduur van vernieuwingstoken beleidsregels zijn ingesteld op basis van de tenant-brede of de resources waartoe toegang wordt verkregen.

1. Als u wilt een levensduur van token instellen, moet u voor het downloaden van de [Azure AD PowerShell-Module](https://www.powershellgallery.com/packages/AzureADPreview).
1. Voer de **Connect-AzureAD-bevestigen** opdracht.

    Hier volgt een voorbeeldbeleid waarmee het vernieuwingstoken dat voor de maximale leeftijd één factor wordt ingesteld. Het beleid maken:
  ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Volgende stappen

* Zie [configureerbare levensduur van tokens in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) voor informatie over het configureren van de levensduur van tokens dat is uitgegeven door Azure AD, inclusief het instellen van de levensduur van tokens voor alle apps in uw organisatie, voor een app met meerdere tenants of voor een bepaalde service principal in uw organisatie. 
* [Naslaginformatie over Azure AD-Token](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

