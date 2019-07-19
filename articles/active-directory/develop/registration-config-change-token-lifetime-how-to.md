---
title: De standaard waarden voor de levens duur van tokens wijzigen voor een aangepaste toepassing | Microsoft Docs
description: De levens duur van tokens bijwerken voor uw toepassing die u ontwikkelt op Azure AD
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: ryanwi
ms.custom: aaddev, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8dd69ff41c890c654fc41bb601b17d135ff0e984
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320929"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>De standaard waarden voor de levens duur van tokens wijzigen voor een aangepaste toepassing

In dit artikel wordt beschreven hoe u Azure AD Power shell gebruikt om een beleid voor levens duur van tokens in te stellen. Met Azure AD Premium kunnen app-ontwikkel aars en Tenant beheerders de levens duur configureren van tokens die zijn uitgegeven voor niet-vertrouwelijke clients. Het token levensduur beleid wordt ingesteld op basis van de Tenant of de bronnen waartoe toegang wordt verkregen.

1. Als u een token levensduur beleid wilt instellen, moet u de [Azure AD Power shell-module](https://www.powershellgallery.com/packages/AzureADPreview)downloaden.
1. Voer de opdracht **Connect-AzureAD-confirm** uit.

    Hier volgt een voor beeld van een beleid waarmee het maximale leeftijds token voor het vernieuwen van één factor wordt ingesteld. Het beleid maken:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Volgende stappen

* Zie [Configureer bare token levensduur in azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) voor meer informatie over het configureren van de levens duur van tokens die zijn uitgegeven door Azure AD, inclusief het instellen van de levens duur van tokens voor alle apps in uw organisatie, voor een multi tenant-app of voor een specifieke Service-Principal in uw organisatie. 
* [Azure AD-token referentie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

