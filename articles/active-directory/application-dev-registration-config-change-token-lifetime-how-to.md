---
title: Het wijzigen van de levensduur van tokens standaard ingesteld voor een aangepaste toepassing | Microsoft Docs
description: Het bijwerken van beleid voor de levensduur van Token voor uw toepassing die u op Azure AD ontwikkelt
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 0ecb1f55309901abd2c623d2c3d23ef717fb176b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365099"
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

