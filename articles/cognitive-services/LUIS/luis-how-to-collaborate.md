---
title: Werk samen met andere inzenders aan apps van LUIS
titleSuffix: Azure Cognitive Services
description: Eigenaar van een app kunt deelnemers toevoegen aan de app. Deze medewerkers kunnen wijzigen het model te trainen en publiceren van de app.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: b7d108bfc0c4283e7856b93daba3f4f92af4cc5b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042190"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Auteurs en deelnemers beheren 

Eigenaar van een app kunt deelnemers toevoegen aan de app. Deze medewerkers kunnen wijzigen het model te trainen en publiceren van de app. 

<a name="owner-and-collaborators"></a>

## <a name="add-collaborator"></a>De samenwerker toevoegen

Een app kunt heeft een enkel auteur, de eigenaar, maar veel deelnemers. Als u wilt toestaan dat de medewerkers om uw LUIS-app te bewerken, moet u het e-mailbericht dat ze gebruiken voor toegang tot de portal LUIS aan de lijst met deelnemers toevoegen. Zodra ze zijn toegevoegd, ziet u de app in hun LUIS-portal.

1. Selecteer **beheren** vanuit het menu rechtsboven op, selecteer **Samenwerkers** in het menu links.

2. Selecteer **Samenwerker toevoegen** via de werkbalk.

    [![](./media/luis-how-to-collaborate/add-collaborator.png "De samenwerker toevoegen")](./media/luis-how-to-collaborate/add-collaborator.png#lightbox)

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

Als de tenant-beheerder wil dat alleen bepaalde gebruikers LUIS gebruiken, verwijzen naar dit [identiteit blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Gebruikersaccounts met meerdere e-mailberichten voor de medewerkers

Als u de medewerkers aan een LUIS-app toevoegt, geeft u de exacte e-mailadres moet een samenwerker LUIS gebruikt als samenwerker. Azure Active Directory (Azure AD) kunt u een enkele gebruiker meer dan één e-mailaccount door elkaar gebruikt, is de gebruiker zich aanmelden met het e-mailadres dat is opgegeven in de lijst van de samenwerker door LUIS vereist.

