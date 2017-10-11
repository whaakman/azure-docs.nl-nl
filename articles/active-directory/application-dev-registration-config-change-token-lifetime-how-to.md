---
title: Het wijzigen van de levensduur van token standaardinstellingen voor een toepassing ontwikkelde aangepaste | Microsoft Docs
description: Het bijwerken van de levensduur van Token beleidsregels voor uw toepassing die u voor Azure AD ontwikkelt
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: a28eacd820ed28a6470992ce86b060e886c00bcb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Het wijzigen van de standaardinstellingen van de levensduur van token voor een toepassing ontwikkelde aangepaste

Azure AD Premium kunt app-ontwikkelaars en tenantbeheerders voor het configureren van de levensduur van tokens die zijn uitgegeven voor niet-vertrouwelijke clients. Levensduur van token beleidsregels zijn ingesteld op basis van de tenant wide of de bronnen die worden geopend.

 * Als u wilt een levensduur van token instellen, moet u voor het downloaden van de [Azure AD PowerShell-Module](https://www.powershellgallery.com/packages/AzureADPreview).

 * Voer de **Connect-AzureAD-bevestigen** opdracht.

 * Hier volgt een voorbeeldbeleid dat het vernieuwingstoken dat maximale leeftijd één factor ingesteld. Het beleid maken:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Bekijk de [configureren levensduur van token](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) document voor meer informatie over het maken van andere aangepaste.

## <a name="next-steps"></a>Volgende stappen
[Levensduur van Token configureren](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Verwijzing naar Azure AD-Token](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)

