---
title: De instellingen van uw account in LUIS beheren | Microsoft Docs
description: LUIS website gebruiken voor het beheren van instellingen van uw account.
titleSuffix: Azure
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2018
ms.author: v-geberr
ms.openlocfilehash: 1f22112a38bf32af03ffaf0493db16839b3fe794
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36749960"
---
# <a name="manage-your-luis-account"></a>Uw account LUIS beheren
De twee belangrijkste stukjes informatie voor een account LUIS zijn het gebruikersaccount en de sleutel ontwerpen. Uw aanmeldingsgegevens wordt beheerd op [account.microsoft.com](https://account.microsoft.com). Uw ontwerp sleutel wordt beheerd vanaf de [LUIS][LUIS] website **instellingen** pagina. 

## <a name="authoring-key"></a>Sleutel ontwerpen

Deze enkele, regiospecifieke authoring sleutel op de **instellingen** pagina kunt u de auteur van uw apps uit de [LUIS][LUIS] website, evenals de [ API's ontwerpen](https://aka.ms/luis-authoring-api). Voor uw gemak de authoring sleutel mag een [beperkt](luis-boundaries.md) eindpunt aantal query's per maand. 

![Pagina LUIS-instellingen](./media/luis-how-to-account-settings/account-settings.png)

De authoring sleutel wordt gebruikt voor alle apps die u bezit, evenals alle apps die u als een medewerker die worden vermeld.

## <a name="authoring-key-regions"></a>Belangrijke gebieden ontwerpen
De sleutel authoring is specifiek voor de [authoring regio](luis-reference-regions.md#publishing-regions). De sleutel werkt niet in een andere regio. 

## <a name="reset-authoring-key"></a>Authoring sleutel opnieuw instellen
Als uw ontwerp sleutel is geknoeid, opnieuw de sleutel. De sleutel wordt opnieuw ingesteld voor uw apps in de [LUIS] website. Als u uw apps via de API's voor ontwerpen ontwerpt, moet u de waarde van wijzigen `Ocp-Apim-Subscription-Key` aan de nieuwe sleutel. 

## <a name="delete-account"></a>Account verwijderen
Zie [gegevensopslag en verwijdering](luis-concept-data-storage.md#accounts) voor meer informatie over welke gegevens worden verwijderd wanneer u uw account verwijdert. 

## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory-tenant-gebruiker
LUIS gebruikt standaard Azure Active Directory (Azure AD) toestemming stroom. 

De tenantbeheerder moet samenwerken met de gebruiker die toegang te krijgen tot het gebruik van LUIS in de Azure AD nodig heeft. 

Eerst de gebruiker zich aanmeldt bij LUIS en ziet een pop-upvenster hoeven admin goedkeuring. De gebruiker contact op met de tenantbeheerder voordat u doorgaat. 

Ten tweede de tenantbeheerder zich aanmeldt bij LUIS en ziet een toestemming stroom pop-upvenster. Dit is het dialoogvenster dat de beheerder moet de machtiging voor de gebruiker. Zodra de beheerder de machtiging accepteert, kan de gebruiker om door te gaan met LUIS.

Als de tenantbeheerder niet bij LUIS aanmelden wordt, de beheerder toegang tot [toestemming](https://account.activedirectory.windowsazure.com/r#/applications) voor LUIS. 

![Azure active directory-machtigingen door app-website](./media/luis-how-to-account-settings/tenant-permissions.png)

Als de tenantbeheerder wil dat alleen bepaalde gebruikers LUIS gebruiken, verwijzen naar dit [identiteit blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Gebruikersaccounts met meerdere e-mailberichten voor deelnemers
Als u deelnemers aan een app LUIS toevoegt, geeft u het exacte e-mailadres moet een samenwerker LUIS gebruiken als een samenwerker. Terwijl Azure Active Directory (Azure AD) kan een enkele gebruiker meer dan één e-mailaccount uitwisselbaar, vereist LUIS dat gebruikers zich aanmelden met het e-mailadres dat is opgegeven in de lijst van de medewerker. 


## <a name="next-steps"></a>Volgende stappen

Meer informatie over uw [authoring sleutel](luis-concept-keys.md#authoring-key). 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
