---
title: Samen werken met anderen-LUIS
titleSuffix: Azure Cognitive Services
description: Eigenaar van een app kunt deelnemers toevoegen aan de app. Deze medewerkers kunnen wijzigen het model te trainen en publiceren van de app.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: dbc27176e7a300c0799e326acb10b99ac663fa89
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638121"
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

Als u [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (Azure AD) in uw organisatie gebruikt, moet language UNDERSTANDING (Luis) toestemming hebben voor de informatie over de toegang van gebruikers wanneer ze Luis willen gebruiken. De resources die vereist dat LUIS zijn minimaal. 

De gedetailleerde beschrijving ziet u wanneer u probeert aan te melden met een account met toestemming van een beheerder is of geen toestemming van een beheerder, zoals administrator toestemming vereist:

* Hiermee kunt u zich aanmelden bij de app met uw organisatieaccount en kan uw profiel te lezen. Ook kunnen de app die basisgegevens over het bedrijfsinformatie te lezen. Dit geeft LUIS-machtiging voor het lezen van basis profiel gegevens, zoals gebruikers-ID, e-mail, naam
* Hiermee kan de app weergeven en bijwerken van uw gegevens, zelfs wanneer u de app momenteel niet worden gebruikt. De machtiging is vereist voor het vernieuwen van het toegangs token van de gebruiker.


## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory-tenant-gebruiker

LUIS gebruikt standaard instemmingsstroom van Azure Active Directory (Azure AD). 

De tenantbeheerder kunnen rechtstreeks met de gebruiker die toegang krijgen tot LUIS in de Azure AD gebruiken nodig heeft. 

* Eerst de gebruiker zich aanmeldt bij LUIS en ziet de pop-upvenster hoeven beheerder goedkeuring. De gebruiker neemt contact op met de tenantbeheerder voordat u doorgaat. 
* Ten tweede tenant-beheerder zich aanmeldt bij LUIS en een toestemming stroom pop-upvenster ziet. Dit is het dialoogvenster dat de beheerder moet de machtiging voor de gebruiker. Zodra de beheerder de machtiging accepteert, kan de gebruiker om door te gaan met LUIS. Als de Tenant beheerder zich niet bij LUIS aanmeldt, heeft de beheerder toegang tot [toestemming](https://account.activedirectory.windowsazure.com/r#/applications) voor Luis, zoals weer gegeven in de volgende scherm afbeelding. U ziet dat de lijst is gefilterd op items `LUIS`die de naam bevatten.

![Azure active directory-machtigingen per app-website](./media/luis-how-to-collaborate/tenant-permissions.png)

Als de Tenant beheerder alleen bepaalde gebruikers in staat wilt stellen om LUIS te gebruiken, zijn er enkele mogelijke oplossingen:
* De "toestemming van de beheerder" (toestemming geven aan alle gebruikers van de Azure AD), maar vervolgens ingesteld op "ja" de "gebruikers toewijzing vereist" onder eigenschappen van bedrijfs toepassing en uiteindelijk alleen de gewenste gebruikers aan de toepassing toewijzen/toevoegen. Met deze methode wordt de beheerder nog steeds ' toestemming van de beheerder ' gegeven aan de app, maar het is mogelijk om de gebruikers te beheren die er toegang toe hebben.
* Een tweede oplossing is het gebruik van [Azure AD Graph API](https://docs.microsoft.com/graph/azuread-identity-access-management-concept-overview) om toestemming te geven aan elke specifieke gebruiker. 

Meer informatie over Azure Active Directory-gebruikers en-toestemming: 
* [Uw app beperken](../../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md) tot een set gebruikers

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Gebruikersaccounts met meerdere e-mailberichten voor de medewerkers

Als u de medewerkers aan een LUIS-app toevoegt, geeft u de exacte e-mailadres moet een samenwerker LUIS gebruikt als samenwerker. Azure Active Directory (Azure AD) kunt u een enkele gebruiker meer dan één e-mailaccount door elkaar gebruikt, is de gebruiker zich aanmelden met het e-mailadres dat is opgegeven in de lijst van de samenwerker door LUIS vereist.

