---
title: Beheren van de instellingen van uw account in LUIS | Microsoft Docs
description: LUIS-website gebruiken voor het beheren van instellingen van uw account.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/08/2018
ms.author: diberry
ms.openlocfilehash: 963a7f8c196702ea899ddfe31e6187a15eb5f683
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223205"
---
# <a name="manage-account-and-authoring-key"></a>Account en het ontwerpen van sleutel beheren
De twee belangrijkste stukjes informatie voor een LUIS-account zijn het gebruikersaccount en de sleutel schrijven. Uw aanmeldingsgegevens wordt beheerd op [account.microsoft.com](https://account.microsoft.com). Uw authoring sleutel wordt beheerd via de [LUIS](luis-reference-regions.md) website **instellingen** pagina. 

## <a name="authoring-key"></a>Sleutel ontwerpen

Deze één, specifiek ontwerpen sleutel, op de **instellingen** pagina, kunt u de auteur van al uw apps uit de [LUIS](luis-reference-regions.md) website, evenals de [API's ontwerpen](https://aka.ms/luis-authoring-api). Als uw gemak de authoring sleutel is toegestaan om een [beperkt](luis-boundaries.md) nummer van het eindpunt van query's per maand. 

![Pagina instellingen van LUIS](./media/luis-how-to-account-settings/account-settings.png)

De ontwerphandleiding sleutel wordt gebruikt voor alle apps die u bezit, evenals alle apps die u als samenwerker wordt vermeld.

## <a name="authoring-key-regions"></a>Belangrijkste regio's ontwerpen
De sleutel schrijven is specifiek voor de [ontwerpen regio](luis-reference-regions.md#publishing-regions). De sleutel werkt niet in een andere regio. 

## <a name="reset-authoring-key"></a>Het ontwerpen van de sleutel opnieuw instellen
Als uw authoring sleutel is geknoeid, opnieuw instellen van de sleutel. De sleutel wordt opnieuw ingesteld op al uw apps in de [LUIS](luis-reference-regions.md) website. Als u uw apps via de API's voor ontwerpen maken, moet u Wijzig de waarde van `Ocp-Apim-Subscription-Key` aan de nieuwe sleutel. 

## <a name="delete-account"></a>Account verwijderen
Zie [gegevensopslag en -verwijdering](luis-concept-data-storage.md#accounts) voor informatie over welke gegevens worden verwijderd wanneer u uw account verwijderen. 

## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory-tenant-gebruiker
LUIS gebruikt standaard instemmingsstroom van Azure Active Directory (Azure AD). 

De tenantbeheerder kunnen rechtstreeks met de gebruiker die toegang krijgen tot LUIS in de Azure AD gebruiken nodig heeft. 

Eerst de gebruiker zich aanmeldt bij LUIS en ziet de pop-upvenster hoeven beheerder goedkeuring. De gebruiker neemt contact op met de tenantbeheerder voordat u doorgaat. 

Ten tweede tenant-beheerder zich aanmeldt bij LUIS en een toestemming stroom pop-upvenster ziet. Dit is het dialoogvenster dat de beheerder moet de machtiging voor de gebruiker. Zodra de beheerder de machtiging accepteert, kan de gebruiker om door te gaan met LUIS.

Als de tenantbeheerder niet bij LUIS aanmelden wordt, de beheerder toegang tot [toestemming geven](https://account.activedirectory.windowsazure.com/r#/applications) voor LUIS. 

![Azure active directory-machtigingen per app-website](./media/luis-how-to-account-settings/tenant-permissions.png)

Als de tenant-beheerder wil dat alleen bepaalde gebruikers LUIS gebruiken, verwijzen naar dit [identiteit blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Gebruikersaccounts met meerdere e-mailberichten voor de medewerkers
Als u de medewerkers aan een LUIS-app toevoegt, geeft u de exacte e-mailadres moet een samenwerker LUIS gebruikt als samenwerker. Azure Active Directory (Azure AD) kunt u een enkele gebruiker meer dan één e-mailaccount door elkaar gebruikt, is de gebruiker zich kunnen aanmelden met het e-mailadres dat is opgegeven in de lijst van de samenwerker door LUIS vereist. 


## <a name="next-steps"></a>Volgende stappen

Meer informatie over uw [ontwerpen sleutel](luis-concept-keys.md#authoring-key). 

