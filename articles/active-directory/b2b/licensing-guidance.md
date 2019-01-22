---
title: Azure Active Directory B2B-samenwerking licentierichtlijnen | Microsoft Docs
description: Azure Active Directory B2B collaboration niet vereist-betaalde Azure AD-licenties, maar u kunt ook ophalen betaalde functies voor gastgebruikers voor B2B
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: mal
ms.openlocfilehash: d89fb86209af2074794d5a5abcdcebadec904799
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54434271"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Licentierichtlijnen voor B2B-samenwerking van Azure Active Directory

Met business-to-business (B2B)-samenwerking van Azure Active Directory (Azure AD), kunt u uitnodigen externe gebruikers (of "gastgebruikers") met uw betaalde Azure AD-services. Voor elke Azure AD-licentie die u aan een gebruiker betaalde toewijst, kunt u maximaal vijf gastgebruikers ook kunnen onder de aftrek van de externe gebruiker uitnodigen.

Een gastgebruiker is iemand die geen lid is van uw organisatie of een van de gelieerde ondernemingen van uw organisatie. Gastgebruikers ook kunnen worden gedefinieerd door hun relatie met uw organisatie, niet door de referenties die ze gebruiken voor het aanmelden. In feite kunt een gastgebruiker aan met een externe id of met de referenties die eigendom zijn van uw organisatie ondertekenen.

Hieronder vindt u *niet* gastgebruikers ook kunnen:
- Uw werknemers, op locatie opdrachtnemers of onsiteagents
- Werknemers, op locatie opdrachtnemers of onsiteagents van uw partners

Gebruikerslicenties voor B2B-Gast wordt automatisch berekend en gerapporteerd op basis van de hoogte-breedteverhouding 1:5. Het is momenteel niet mogelijk om toe te wijzen B2B Gast gebruikerslicenties rechtstreeks naar gastgebruikers.

Er zijn bepaalde situaties waar een gastgebruiker is niet gemeld met behulp van de limiet van 1:5 externe gebruiker. Als een gastgebruiker beschikt al over een betaald Azure AD-licentie in de organisatie van gebruiker, de gebruiker een van de gebruikerslicenties van uw B2B-Gast niet gebruiken. Bovendien gratis Gast kunnen gebruikers met Azure AD-functies met geen aanvullende licentievereisten te voldoen. Gastgebruikers hebben toegang tot Azure AD-functies gratis, zelfs als u geen hebt betaalde Azure AD-licenties. 

## <a name="examples-calculating-guest-user-licenses"></a>Voorbeelden: Berekenen van de Gast-gebruikerslicenties
Zodra u hoeveel gastgebruikers nodig bepalen voor toegang tot uw betaalde Azure AD-services, zorg ervoor dat u hebt voldoende Azure AD betaalde licenties voor gastgebruikers ook kunnen in de vereiste 1:5-verhouding. Hier volgen enkele voorbeelden:

- U wilt uitnodigen 100 gastgebruikers ook kunnen aan uw Azure AD-apps of services, en u wilt toewijzen, toegangsbeheer en inrichting voor alle gastgebruikers. U wilt ook vereisen dat MFA en voorwaardelijke toegang voor deze gastgebruikers 50. Hebt u voor deze combinatie van 10 licenties voor Azure AD Basic en 10 licenties voor Azure AD Premium P1 nodig. Als u van plan bent te Identity Protection-functies met uw gastgebruikers ook kunnen gebruiken, moet u Azure AD Premium P2-licenties in de dezelfde 1:5-verhouding tussen het betrekking hebben op de gastgebruikers.
- U wilt uitnodigen 60 gastgebruikers die alle MFA, vereist dus u moet ten minste 12 Azure AD Premium P1-licenties hebt. Hebt u 10 werknemers met Azure AD Premium P1-licenties kunnen maximaal 50 gastgebruikers ook kunnen onder de 1:5 verhouding-licentieverlening. U moet twee extra Premium P1-licenties voor 10 extra Gastgebruikers ook kunnen kopen.

## <a name="using-the-b2b-collaboration-api-to-invite-users-from-your-affiliates"></a>Met behulp van de B2B-samenwerking API om uit te nodigen gebruikers uit uw gelieerde ondernemingen

Een gastgebruiker B2B is per definitie een externe gebruiker u uitnodigen voor het gebruik van uw betaalde Azure AD-apps en services. Een werknemer, contractant op locatie of de agent op locatie van uw bedrijf of een van uw gelieerde ondernemingen wordt niet in aanmerking komen voor B2B-samenwerking, zelfs als de B2B-functies worden gebruikt. Hier volgen enkele voorbeelden: 
- U wilt gebruiken van externe referenties (bijvoorbeeld een sociale identiteit) om uit te nodigen van een gebruiker die een werknemer van uw organisatie. In dit scenario niet voldoen aan de licentievereisten duidelijk zijn en is niet toegestaan. Externe referenties maken niet van een werknemer een externe gebruiker.  
- U wilt de B2B-API's gebruiken om uit te nodigen van een gebruiker vanuit een van de partners van uw organisatie. B2B-API's in dit scenario wordt de gebruiker uitnodigen, niet wordt dit beschouwd als B2B-samenwerking. Het voldoen niet aan de licentievereisten duidelijk zijn omdat een gebruiker vanuit de partner niet een externe gebruiker. 

In beide scenario's, betere oplossing is de B2B-API gebruikt om uit te nodigen van de gebruikers als leden (invitedUserType lid =) en ze elk een Azure AD-licentie toewijzen. 

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen op Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [Veelgestelde vragen over Azure Active Directory B2B-samenwerking](faq.md)
