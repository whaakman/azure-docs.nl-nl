---
title: Het wijzigen van de levensduur van tokens standaard ingesteld voor een aangepaste toepassing | Microsoft Docs
description: Het bijwerken van beleid voor de levensduur van Token voor uw toepassing die u op Azure AD ontwikkelt
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 059920c710b202e22a22f8431c536c5dfa19f2b9
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724078"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Het wijzigen van de levensduur van vernieuwingstoken standaardwaarden voor een aangepaste toepassing

Azure AD Premium biedt app-ontwikkelaars en tenantbeheerders het configureren van de levensduur van tokens die zijn uitgegeven voor niet-vertrouwelijke clients. Levensduur van vernieuwingstoken beleidsregels zijn ingesteld op basis van de tenant-brede of de resources waartoe toegang wordt verkregen.

 * Als u wilt een levensduur van token instellen, moet u voor het downloaden van de [Azure AD PowerShell-Module](https://www.powershellgallery.com/packages/AzureADPreview).

 * Voer de **Connect-AzureAD-bevestigen** opdracht.

 * Hier volgt een voorbeeldbeleid waarmee het vernieuwingstoken dat voor de maximale leeftijd één factor wordt ingesteld. Het beleid maken: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Bekijk de [levensduur van tokens configureren](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) document voor meer informatie over het maken van andere aangepaste.

## <a name="next-steps"></a>Volgende stappen
[Levensduur van tokens configureren](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Naslaginformatie over Azure AD-Token](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

