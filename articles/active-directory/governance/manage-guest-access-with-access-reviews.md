---
title: Gasttoegang beheren met Azure AD toegangsbeoordelingen | Microsoft Docs
description: Gastgebruikers beheren als leden van een groep of toegewezen aan een toepassing met Azure Active Directory-toegangsbeoordelingen
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 12/13/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: a7d5f6c478776a58c9a4f9d92c5c5999addcca5d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55154215"
---
# <a name="manage-guest-access-with-azure-ad-access-reviews"></a>Gasttoegang beheren met Azure AD toegangsbeoordelingen


Met Azure Active Directory (Azure AD), kunt u eenvoudig inschakelen samenwerking over de grenzen van de organisatie met behulp van de [Azure AD B2B-functie](../b2b/what-is-b2b.md). Gastgebruikers van andere tenants kunnen worden [uitgenodigd door beheerders](../b2b/add-users-administrator.md) of door [andere gebruikers](../b2b/what-is-b2b.md). Deze mogelijkheid geldt ook voor sociale identiteiten, zoals Microsoft-accounts.

U kunt eenvoudig Zorg er ook voor dat gastgebruikers ook kunnen de juiste toegang hebben. U kunt de gasten stellen zelf of een besluitvormer deel te nemen aan een toegangsbeoordeling en opnieuw certificeren (of bevestigen) voor toegang tot de gasten. De beoordelaars kunnen op basis van suggesties uit Azure AD hun mening geven over de mate waarin een gebruiker per se toegang moet krijgen. Wanneer een toegangsbeoordeling is voltooid, kunt u vervolgens wijzigingen aanbrengen en verwijderen van toegang voor gasten die deze niet meer nodig hebben.

> [!NOTE]
> Dit document is gericht op het controleren van toegang van gastgebruikers. Als u wilt bekijken van alle gebruikers toegang, niet alleen gasten, Zie [gebruikerstoegang beheren met toegangsbeoordelingen](manage-user-access-with-access-reviews.md). Als u wilt bekijken, lidmaatschap van een gebruiker in een beheerderrol, zoals globale beheerder, raadpleegt u [een toegangsbeoordeling starten in Azure AD Privileged Identity Management](../privileged-identity-management/pim-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Vereisten 


Toegangsbeoordelingen zijn beschikbaar met de Premium P2-editie van Azure AD, die deel uitmaakt van Microsoft Enterprise Mobility + Security, E5. Zie [Azure Active Directory-edities](../fundamentals/active-directory-whatis.md) voor meer informatie. Elke gebruiker die deze functie gebruikt (waaronder het maken van een beoordeling, het invullen van een beoordeling of het bevestigen van de toegang), heeft een licentie nodig. 

U kunt ook vragen gastgebruikers ook kunnen op hun eigen toegang beoordelen. Voor elke betaalde Azure AD Premium P2-licentie die u aan een van de gebruikers van uw eigen organisatie toewijst, kunt u maximaal vijf gastgebruiker onder de aftrek van de externe gebruiker uitnodigen B2B. Deze gastgebruikers kunnen functies van Azure AD Premium P2 ook gebruiken. Zie voor meer informatie, [licentieverlening van Azure AD B2B-samenwerking](../b2b/licensing-guidance.md).

## <a name="create-and-perform-an-access-review-for-guests"></a>Maken en een toegangscontrole uitvoeren voor gasten

Eerst wordt als een globale beheerder of beheerder van gebruikersaccounts, gaat u naar de [pagina toegangsbeoordelingen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) om ervoor te zorgen dat toegangsbeoordelingen gereed is voor uw organisatie. 

Azure AD kunt verschillende scenario's voor het controleren van gastgebruikers.

U kunt een bekijken:

 - Een groep in Azure AD en waaraan een of meer gasten als leden.
 - Een toepassing verbonden met Azure AD en waaraan een of meer gastgebruikers die zijn toegewezen. 

U kunt vervolgens beslissen of de gast op hun eigen toegang beoordelen of op een of meer gebruikers om te controleren van elke gast toegang vragen stellen.

 Deze scenario's worden behandeld in de volgende secties.

### <a name="ask-guests-to-review-their-own-membership-in-a-group"></a>Gasten om te controleren van hun eigen lidmaatschap in een groep vragen

U kunt met toegangsbeoordelingen gebruiken om ervoor te zorgen dat gebruikers die zijn uitgenodigd en toegevoegd aan een groep blijven toegang nodig hebben. U kunt eenvoudig gasten om te controleren van hun eigen lidmaatschap van die groep vragen.

1. Voor het starten van een toegangscontrole voor de groep, selecteert u de controle van gastgebruikers alleen en leden lees zelf op te nemen. Raadpleeg voor meer informatie [Create an access review](create-access-review.md) (Een toegangscontrole maken).

2. Vraag de Gast om te controleren van hun eigen lidmaatschap. Standaard ontvangt elke gast die een uitnodiging geaccepteerd een e-mailbericht van Azure AD met een koppeling naar de toegangsbeoordeling. Azure AD bevat instructies voor gasten voor het [hun toegang beoordelen](perform-access-review.md).

3. Wanneer alle beoordelaars feedback hebben gegeven, kunt u de toegangsbeoordeling stoppen en de wijzigingen toepassen. Raadpleeg voor meer informatie [Complete an access review](complete-access-review.md) (Een toegangscontrole voltooien).

4. Naast de gebruikers die hun eigen geweigerd voor blijvende toegang nodig, kunt u ook gebruikers die reageerde niet verwijderen. Gebruikers niet reageert ontvangt mogelijk niet langer e-mail.

5. Als de groep wordt niet voor beheer van toegang gebruikt, kunt u ook gebruikers die niet zijn geselecteerd voor deelname aan onderzoek omdat ze hun uitnodiging niet accepteren verwijderen. Accepteert geen kan erop wijzen dat e-mailadres van de uitgenodigde gebruiker al een typefout gemaakt. Als een groep wordt gebruikt als een distributielijst, niet zijn wellicht sommige gastgebruikers ook kunnen geselecteerd om deel te nemen omdat ze contact op met objecten.

### <a name="ask-a-sponsor-to-review-a-guests-membership-in-a-group"></a>Vraag een sponsor om te controleren van een gast-lidmaatschap in een groep

U kunt een sponsor, zoals de eigenaar van een groep te bekijken van een gast nodig voor voortdurende lidmaatschap in een groep vragen.

1. Voor het starten van een toegangscontrole voor de groep, selecteert u de controle om op te nemen alleen gastgebruikers. Geef vervolgens een of meer revisoren. Raadpleeg voor meer informatie [Create an access review](create-access-review.md) (Een toegangscontrole maken).

2. Vraag de beoordelaars feedback te geven. Standaard ontvangen ze allemaal een e-mail van Azure AD met een koppeling naar het toegangsdeelvenster waar ze [hun toegangsbeoordeling kunnen uitvoeren](perform-access-review.md).

3. Wanneer alle beoordelaars feedback hebben gegeven, kunt u de toegangsbeoordeling stoppen en de wijzigingen toepassen. Raadpleeg voor meer informatie [Complete an access review](complete-access-review.md) (Een toegangscontrole voltooien).

### <a name="ask-guests-to-review-their-own-access-to-an-application"></a>Vraag van gasten om te controleren van hun eigen toegang tot een toepassing

U kunt met toegangsbeoordelingen gebruiken om ervoor te zorgen dat gebruikers die zijn uitgenodigd voor een bepaalde toepassing blijven toegang nodig hebben. U kunt eenvoudig vragen de gasten zelf om te controleren van hun eigen nodig om toegang te krijgen.

1. Voor het starten van een toegangscontrole voor de toepassing, selecteert u de controle om op te nemen alleen gasten en dat gebruikers hun eigen toegang beoordelen. Raadpleeg voor meer informatie [Create an access review](create-access-review.md) (Een toegangscontrole maken).

2. Vraag de Gast om te controleren van hun eigen toegang tot de toepassing. Standaard wordt met elke gast die een uitnodiging geaccepteerd een e-mailbericht ontvangt van Azure AD. Dat e-mailbericht een koppeling naar de toegangsbeoordeling in het toegangsvenster van uw organisatie heeft. Azure AD bevat instructies voor gasten voor het [hun toegang beoordelen](perform-access-review.md).

3. Wanneer alle beoordelaars feedback hebben gegeven, kunt u de toegangsbeoordeling stoppen en de wijzigingen toepassen. Raadpleeg voor meer informatie [Complete an access review](complete-access-review.md) (Een toegangscontrole voltooien).

4. Naast de gebruikers die hun eigen geweigerd voor blijvende toegang nodig, kunt u ook gastgebruikers die reageerde niet verwijderen. Gebruikers niet reageert ontvangt mogelijk niet langer e-mail. Ook kunt u gastgebruikers die niet zijn ingeschakeld om deel te nemen, met name als ze zijn niet recent uitgenodigd verwijderen. Deze gebruikers accepteren niet de uitnodiging en zijn er dus geen toegang tot de toepassing. 

### <a name="ask-a-sponsor-to-review-a-guests-access-to-an-application"></a>Vraag een sponsor om te controleren van een gast-toegang tot een toepassing

U kunt een sponsor, zoals de eigenaar van een toepassing, om te controleren van Gast nodig voor blijvende toegang tot de toepassing op te vragen.

1. Voor het starten van een toegangscontrole voor de toepassing, selecteert u de controle om op te nemen alleen gasten. Vervolgens geeft u een of meer gebruikers als beoordelaar. Raadpleeg voor meer informatie [Create an access review](create-access-review.md) (Een toegangscontrole maken).

2. Vraag de beoordelaars feedback te geven. Standaard ontvangen ze allemaal een e-mail van Azure AD met een koppeling naar het toegangsdeelvenster waar ze [hun toegangsbeoordeling kunnen uitvoeren](perform-access-review.md).

3. Wanneer alle beoordelaars feedback hebben gegeven, kunt u de toegangsbeoordeling stoppen en de wijzigingen toepassen. Raadpleeg voor meer informatie [Complete an access review](complete-access-review.md) (Een toegangscontrole voltooien).

### <a name="ask-guests-to-review-their-need-for-access-in-general"></a>Gasten in het algemeen bekijken van de behoefte om toegang te krijgen, vragen

In sommige organisaties gasten mogelijk niet op de hoogte van de groepslidmaatschappen ervan.

> [!NOTE]
> Eerdere versies van de Azure-portal niet toestaan dat beheerderstoegang door gebruikers met de UserType Gast. In sommige gevallen kan een beheerder in uw directory is mogelijk gewijzigd van een gast UserType-waarde in lid met behulp van PowerShell. Als deze wijziging is eerder is opgetreden in uw directory, kan niet alle gastgebruikers die in het verleden had administratieve toegangsrechten bevatten in de vorige query. In dit geval moet u handmatig de Gast opnemen in het lidmaatschap van de of wijzigen van de Gast UserType.

1. Als een geschikte groep nog niet bestaat, moet u een security group maken in Azure AD met de gasten als leden. U kunt bijvoorbeeld een groep maken met een handmatig beheerd lidmaatschap van de gasten. Of u kunt een dynamische groep maken met een naam, bijvoorbeeld 'Gasten van Contoso' voor gebruikers in de Contoso-tenant met de waarde van het UserType-kenmerk van de Gast.  Voor efficiëntie en zorg ervoor dat de groep is voornamelijk gasten - Selecteer een groep met lidgebruikers die, niet als lidgebruikers hoeft te worden gecontroleerd.  Houd er ook rekening mee dat een gastgebruiker lid van de groep is die de andere leden van de groep kan zien.

2. Selecteer eerst een toegangscontrole voor de groep die de beoordelaars feedback te worden van de leden zelf. Raadpleeg voor meer informatie [Create an access review](create-access-review.md) (Een toegangscontrole maken).

3. Vraag de Gast om te controleren van hun eigen lidmaatschap. Standaard ontvangt elke gast die een uitnodiging geaccepteerd een e-mailbericht van Azure AD met een koppeling naar de toegangsbeoordeling in het toegangsvenster van uw organisatie. Azure AD bevat instructies voor gasten voor het [hun toegang beoordelen](perform-access-review.md).  Deze gasten die hun uitnodiging niet geaccepteerd wordt weergegeven in de resultaten bekijken als 'Niet op de hoogte gesteld'.

4. Nadat de revisoren invoer geven, moet u de toegangsbeoordeling stoppen. Raadpleeg voor meer informatie [Complete an access review](complete-access-review.md) (Een toegangscontrole voltooien).

5. Toegang voor gasten voor gasten die zijn geweigerd, de controle is niet voltooid of is niet eerder de uitnodiging accepteren verwijderen. Als u enkele van de gasten zijn contactpersonen die zijn geselecteerd voor deelname aan het controleren of ze niet eerder een uitnodiging accepteren, kunt u hun account uitschakelen met behulp van de Azure portal of PowerShell. Als de Gast niet langer toegang nodig heeft en is niet van een contactpersoon, kunt u hun gebruikersobject uit uw directory verwijderen met behulp van de Azure portal of PowerShell om te verwijderen van het gebruikersobject Gast.

## <a name="next-steps"></a>Volgende stappen

[Een toegangsbeoordeling maken voor leden van een groep of toegang tot een toepassing](create-access-review.md)







