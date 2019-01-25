---
title: Samenwerken met anderen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Eigenaar van een app kunt deelnemers toevoegen aan de app. Deze medewerkers kunnen wijzigen het model te trainen en publiceren van de app.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 6b9dd4beccbe8bc872c46976f490400daa200c3a
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54888835"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Auteurs en deelnemers beheren 

Eigenaar van een app kunt deelnemers toevoegen aan de app. Deze medewerkers kunnen wijzigen het model te trainen en publiceren van de app. 

<a name="owner-and-collaborators"></a>

## <a name="add-collaborator"></a>De samenwerker toevoegen

Een app kunt heeft een enkel auteur, de eigenaar, maar veel deelnemers. Als u wilt toestaan dat de medewerkers om uw LUIS-app te bewerken, moet u het e-mailbericht dat ze gebruiken voor toegang tot de portal LUIS aan de lijst met deelnemers toevoegen. Zodra ze zijn toegevoegd, ziet u de app in hun LUIS-portal.

1. Selecteer **beheren** vanuit het menu rechtsboven op, selecteer **Samenwerkers** in het menu links.

2. Selecteer **Samenwerker toevoegen** via de werkbalk.

    [![De samenwerker toevoegen](./media/luis-how-to-collaborate/add-collaborator.png "samenwerker toevoegen")](./media/luis-how-to-collaborate/add-collaborator.png#lightbox)

3. Voer het e-mailadres dat de samenwerker wordt gebruikt om u te melden bij de LUIS-portal.

    ![De samenwerker e-mailadres toevoegen](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)

## <a name="transfer-of-ownership"></a>Overdracht van eigendom

Hoewel LUIS momenteel geen overdracht van eigendom ondersteunt, kunt u uw app exporteren en een andere LUIS gebruiker de app kunt importeren. Er zijn kleine verschillen in LUIS scores tussen de twee toepassingen. 

## <a name="azure-active-directory-resources"></a>Azure Active Directory-resources

Als u Azure Active Directory (Azure AD) in uw organisatie gebruiken, LUIS heeft toestemming nodig om de toegang tot informatie over uw gebruikers wanneer ze willen LUIS gebruikt. De resources die vereist dat LUIS zijn minimaal. 

De gedetailleerde beschrijving ziet u wanneer u probeert aan te melden met een account met toestemming van een beheerder is of geen toestemming van een beheerder, zoals administrator toestemming vereist:

* Hiermee kunt u zich aanmelden bij de app met uw organisatieaccount en kan uw profiel te lezen. Ook kunnen de app die basisgegevens over het bedrijfsinformatie te lezen.
* Hiermee kan de app weergeven en bijwerken van uw gegevens, zelfs wanneer u de app momenteel niet worden gebruikt.

De eerste machtiging biedt een LUIS-machtiging voor het lezen van gegevens van de basisprofielgegevens, zoals gebruikers-ID, e-mailadres, naam. De tweede machtiging is vereist voor het vernieuwen van het toegangstoken van de gebruiker.

## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory-tenant-gebruiker

LUIS gebruikt standaard instemmingsstroom van Azure Active Directory (Azure AD). 

De tenantbeheerder kunnen rechtstreeks met de gebruiker die toegang krijgen tot LUIS in de Azure AD gebruiken nodig heeft. 

Eerst de gebruiker zich aanmeldt bij LUIS en ziet de pop-upvenster hoeven beheerder goedkeuring. De gebruiker neemt contact op met de tenantbeheerder voordat u doorgaat. 

Ten tweede tenant-beheerder zich aanmeldt bij LUIS en een toestemming stroom pop-upvenster ziet. Dit is het dialoogvenster dat de beheerder moet de machtiging voor de gebruiker. Zodra de beheerder de machtiging accepteert, kan de gebruiker om door te gaan met LUIS.

Als de tenantbeheerder niet bij LUIS aanmelden wordt, de beheerder toegang tot [toestemming geven](https://account.activedirectory.windowsazure.com/r#/applications) voor LUIS. 

![Azure active directory-machtigingen per app-website](./media/luis-how-to-collaborate/tenant-permissions.png)

Als de tenant-beheerder wil dat alleen bepaalde gebruikers LUIS gebruikt, moet u er een aantal mogelijke oplossingen zijn:
* Geeft de 'beheerder toestemming geven' (toestemming geven voor alle gebruikers van de Azure AD), maar wordt ingesteld op 'Ja' de 'Gebruikerstoewijzing vereist' onder de Enterprise-eigenschappen voor de toepassing en ten slotte toewijzen/alleen de gewenste gebruikers toevoegen aan de toepassing. Met deze methode 'beheerder toestemming geven' is nog steeds het leveren van de beheerder van de App, het is echter mogelijk om te bepalen welke gebruikers die toegang hebben tot deze.
* Een tweede oplossing, wordt met behulp van [Azure AD Graph API](https://docs.microsoft.com/graph/azuread-identity-access-management-concept-overview) voor toestemming voor elke specifieke gebruiker. 

Meer informatie over Azure active directory-gebruikers en toestemming: 
* [Uw app beperken](../../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md) aan een groep gebruikers

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Gebruikersaccounts met meerdere e-mailberichten voor de medewerkers

Als u de medewerkers aan een LUIS-app toevoegt, geeft u de exacte e-mailadres moet een samenwerker LUIS gebruikt als samenwerker. Azure Active Directory (Azure AD) kunt u een enkele gebruiker meer dan één e-mailaccount door elkaar gebruikt, is de gebruiker zich aanmelden met het e-mailadres dat is opgegeven in de lijst van de samenwerker door LUIS vereist.

