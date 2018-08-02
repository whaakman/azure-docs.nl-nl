---
title: Werk samen met andere inzenders aan apps in Azure LUIS | Microsoft Docs
description: Meer informatie over het samenwerken met andere inzenders aan Language Understanding (LUIS)-toepassingen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/31/2018
ms.author: diberry
ms.openlocfilehash: 99f37cb6dc5e05fc5eb4bde09685435ee57fecc6
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397782"
---
# <a name="how-to-manage-authors-and-collaborators"></a>Auteurs en deelnemers beheren 

U kunt samenwerken met anderen in uw LUIS-app samen. 

## <a name="owner-and-collaborators"></a>Eigenaar en samenwerkers

Een app kunt heeft een enkel auteur, de eigenaar, maar veel deelnemers. 

## <a name="add-collaborator"></a>De samenwerker toevoegen

Om toe te staan samenwerkers om te bewerken van uw LUIS-app op de **instellingen** pagina van uw LUIS-app, voer de e-mailadres van de samenwerker en klik op **toevoegen samenwerker**. Deelnemers kunnen aanmelden en uw LUIS-app op hetzelfde moment als die u in de app werkt bewerken.

![De samenwerker toevoegen](./media/luis-how-to-collaborate/add-collaborator.png)

## <a name="transfer-of-ownership"></a>Overdracht van eigendom

Hoewel LUIS momenteel geen overdracht van eigendom ondersteunt, kunt u uw app exporteren en een andere LUIS gebruiker de app kunt importeren. Er zijn kleine verschillen in LUIS scores tussen de twee toepassingen. 

## <a name="azure-active-directory-tenant-user"></a>Azure Active Directory-tenant-gebruiker

LUIS gebruikt standaard instemmingsstroom van Azure Active Directory (Azure AD). 

De tenantbeheerder kunnen rechtstreeks met de gebruiker die toegang krijgen tot LUIS in de Azure AD gebruiken nodig heeft. 

Eerst de gebruiker zich aanmeldt bij LUIS en ziet de pop-upvenster hoeven beheerder goedkeuring. De gebruiker neemt contact op met de tenantbeheerder voordat u doorgaat. 

Ten tweede tenant-beheerder zich aanmeldt bij LUIS en een toestemming stroom pop-upvenster ziet. Dit is het dialoogvenster dat de beheerder moet de machtiging voor de gebruiker. Zodra de beheerder de machtiging accepteert, kan de gebruiker om door te gaan met LUIS.

Als de tenantbeheerder niet bij LUIS aanmelden wordt, de beheerder toegang tot [toestemming geven](https://account.activedirectory.windowsazure.com/r#/applications) voor LUIS. 

![Azure active directory-machtigingen per app-website](./media/luis-how-to-account-settings/tenant-permissions.png)

Als de tenant-beheerder wil dat alleen bepaalde gebruikers LUIS gebruiken, verwijzen naar dit [identiteit blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/).

### <a name="user-accounts-with-multiple-emails-for-collaborators"></a>Gebruikersaccounts met meerdere e-mailberichten voor de medewerkers

Als u de medewerkers aan een LUIS-app toevoegt, geeft u de exacte e-mailadres moet een samenwerker LUIS gebruikt als samenwerker. Azure Active Directory (Azure AD) kunt u een enkele gebruiker meer dan één e-mailaccount door elkaar gebruikt, is de gebruiker zich aanmelden met het e-mailadres dat is opgegeven in de lijst van de samenwerker door LUIS vereist.