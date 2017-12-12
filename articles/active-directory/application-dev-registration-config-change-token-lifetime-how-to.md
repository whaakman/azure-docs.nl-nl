---
title: Het wijzigen van de levensduur van token standaardinstellingen voor een toepassing ontwikkelde aangepaste | Microsoft Docs
description: Het bijwerken van de levensduur van Token beleidsregels voor uw toepassing die u voor Azure AD ontwikkelt
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 8067ecf3e274f65abe2c82f20dd2f4469344f3b6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Het wijzigen van de standaardinstellingen van de levensduur van token voor een toepassing ontwikkelde aangepaste

Azure AD Premium kunt app-ontwikkelaars en tenantbeheerders voor het configureren van de levensduur van tokens die zijn uitgegeven voor niet-vertrouwelijke clients. Levensduur van token beleidsregels zijn ingesteld op basis van de tenant wide of de bronnen die worden geopend.

 * Als u wilt een levensduur van token instellen, moet u voor het downloaden van de [Azure AD PowerShell-Module](https://www.powershellgallery.com/packages/AzureADPreview).

 * Voer de **Connect-AzureAD-bevestigen** opdracht.

 * Hier volgt een voorbeeldbeleid dat het vernieuwingstoken dat maximale leeftijd één factor ingesteld. Het beleid maken:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Bekijk de [configureren levensduur van token](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) document voor meer informatie over het maken van andere aangepaste.

## <a name="next-steps"></a>Volgende stappen
[Levensduur van Token configureren](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Verwijzing naar Azure AD-Token](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

