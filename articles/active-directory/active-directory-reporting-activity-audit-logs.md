---
title: Controleactiviteitenrapporten in Azure Active Directory Portal | Microsoft Docs
description: Ontdek de controleactiviteitenrapporten in de Azure Active Directory Portal
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f2d0332d815c82d7d47625e020de2e9c5099deeb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Controleactiviteitenrapporten in Azure Active Directory Portal 

Met rapporten in Azure Active Directory ontvangt u alle informatie die nodig is om te bepalen hoe het gaat met uw omgeving.

De rapportstructuur in Azure AD bestaat uit de volgende onderdelen:

- **Activiteit** 
    - **Aanmeldactiviteiten**: informatie over het gebruik van beheerde toepassingen en aanmeldactiviteiten van gebruikers
    - **Controlelogboeken**: informatie over systeemactiviteit van gebruikers, groepsbeheer, uw beheerde toepassingen en directory-activiteiten.
- **Beveiliging** 
    - **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. Zie Riskante aanmeldingen voor meer informatie.
    - **Gebruikers van wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. Zie Gebruikers van wie wordt aangegeven dat ze risico lopen voor meer informatie.

In dit onderwerp vindt u meer informatie over de controleactiviteiten.
 
## <a name="who-can-access-the-data"></a>Wie heeft er toegang tot de gegevens?
* Gebruikers met de rol Beveiligingsbeheerder of Beveiligingslezer
* Globale beheerders
* Afzonderlijke gebruikers (niet-beheerders) kunnen hun eigen activiteiten zien


## <a name="audit-logs"></a>Controlelogboeken

In de controlelogboeken in Azure Active Directory staan records van systeemactiviteiten voor naleving.  
Uw eerste beginpunt voor alle controlegegevens is **Controlelogboeken** in het gedeelte **Activiteit** van **Azure Active Directory**.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/61.png "Controlelogboeken")

Een controlelogboek heeft een standaardlijstweergave die het volgende laat zien:

- de datum en tijd van de gebeurtenis
- de initiator/actor (*wie*) van een activiteit 
- de activiteit (*wat*) 
- het doel

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/18.png "Controlelogboeken")

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/19.png "Controlelogboeken")

Hiermee kunt u extra velden weergeven of velden verwijderen die al worden weergegeven.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/21.png "Controlelogboeken")


Wanneer u op een item in de lijstweergave klikt, krijgt u er alle beschikbare informatie over te zien.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/22.png "Controlelogboeken")


## <a name="filtering-audit-logs"></a>Controlelogboeken filteren

Als u de gerapporteerde gegevens wilt beperken tot een niveau dat geschikt is voor u, kunt u de controlegegevens filteren met de volgende velden:

- Datumbereik
- Gestart door (actor)
- Category
- Resourcetype van activiteit
- Activiteit

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/23.png "Controlelogboeken")


Met het filter **datumbereik** kunt u een tijdsbestek opgeven voor de geretourneerde gegevens.  
Mogelijke waarden zijn:

- 1 maand
- 7 dagen
- 24 uur
- Aangepast telefoonnummer

Wanneer u een aangepast tijdsbestek selecteert, kunt u een begintijd en eindtijd configureren.

Met het filter **gestart door** kunt u de naam of UPN (Universal Principal Name) van een actor definiëren.

Met het filter **aanmeldingsstatus** kunt u een van de volgende filters selecteren:

- Alle
- Hoofdcategorie
- Hoofddirectory
- Wachtwoordbeheer via selfservice
- Groepsbeheer via selfservice
- Account inrichten - Automatische wachtwoordoverschakeling
- Uitgenodigde gebruikers
- MIM-service
- Identiteitsbeveiliging
- B2C

Met het filter **type activiteitsresource** kunt u een van de volgende filters selecteren:

- Alle 
- Groep
- Directory
- Gebruiker
- Toepassing
- Beleid
- Apparaat
- Overige

Wanneer u **Groep** selecteert als **type activiteitsresource**, krijgt u een extra filtercategorie waarmee u ook een **Bron** kunt opgeven:

- Azure AD
- O365


Het filter **activiteit** is gebaseerd op de selectie die u maakt voor de categorie en het type activiteitsresource. U kunt een specifieke activiteit of alle activiteiten selecteren. 

U kunt de lijst met alle controle-activiteiten opvragen met behulp van de Graph API https://graph.windows.net/$tenantdomein/activities/auditActivityTypes?api-version=beta, waarbij u $tenantdomain vervangt door de naam van uw domein, of raadpleeg het artikel [Azure Active Directory audit report events](active-directory-reporting-audit-events.md) (Gebeurtenissen in Azure Active Directory-controlerapporten).


## <a name="audit-logs-shortcuts"></a>Snelkoppelingen naar controlelogboeken

Naast **Azure Active Directory** biedt de Azure Portal twee extra beginpunten voor gegevenscontrole:

- Gebruikers en groepen
- Bedrijfstoepassingen

### <a name="users-and-groups-audit-logs"></a>Controlelogboeken voor gebruikers en groepen

Met de controlerapporten op basis van gebruikers en groepen krijgt u antwoord op vragen zoals:

- Welke soorten updates zijn toegepast op de gebruikers?

- Hoeveel gebruikers zijn gewijzigd?

- Hoeveel wachtwoorden zijn gewijzigd?

- Wat heeft een beheerder in een directory gedaan?

- Welke groepen zijn toegevoegd?

- Zijn er groepen met wijzigingen in het lidmaatschap?

- Zijn de eigenaren van een groep gewijzigd?

- Welke licenties zijn toegewezen aan een groep of een gebruiker?

Als u alleen controlegegevens wilt bekijken die aan gebruikers en groepen zijn gerelateerd, kunt u een gefilterde weergave openen via **Controlelogboeken** in het gedeelte **Activiteit** van **Gebruikers en groepen**. Dit beginpunt heeft **Gebruikers en groepen** als vooraf geselecteerd **Type activiteitsresource**.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/93.png "Controlelogboeken")

### <a name="enterprise-applications-audit-logs"></a>Controlelogboeken voor bedrijfstoepassingen

Met de controlerapporten op basis van toepassingen krijgt u antwoord op vragen zoals:

* Welke toepassingen zijn toegevoegd of bijgewerkt?
* Welke toepassingen zijn verwijderd?
* Is de service-principal van een toepassing gewijzigd?
* Zijn de namen van toepassingen gewijzigd?
* Wie heeft toestemming gegeven voor een toepassing?

Als u alleen controlegegevens wilt bekijken die aan uw toepassingen zijn gerelateerd, kunt u een gefilterde weergave openen via **Controlelogboeken** in het gedeelte **Activiteit** van de blade **Bedrijfstoepassingen**. Dit beginpunt heeft **Bedrijfstoepassingen** als vooraf geselecteerd **Type activiteitsresource**.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/134.png "Controlelogboeken")

U kunt deze weergave verder filteren naar alleen **Groepen** of alleen **Gebruikers**.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/25.png "Controlelogboeken")


## <a name="next-steps"></a>Volgende stappen

Zie [Azure Active Directory-rapportage](active-directory-reporting-azure-portal.md) voor een overzicht van de rapportage.

