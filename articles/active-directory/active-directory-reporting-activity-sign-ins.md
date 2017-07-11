---
title: Aanmeldactiviteitenrapporten in Azure Active Directory Portal | Microsoft Docs
description: Ontdek de aanmeldactiviteitenrapporten in de Azure Active Directory Portal
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 0e8ae05b5f8644d4dfd0e6205224cabfb0919531
ms.contentlocale: nl-nl
ms.lasthandoff: 05/26/2017


---
<a id="sign-in-activity-reports-in-the-azure-active-directory-portal" class="xliff"></a>

# Aanmeldactiviteitenrapporten in Azure Active Directory Portal

Met Azure AD-rapporten (Azure Active Directory) in [Azure Portal](https://portal.azure.com) ontvangt u alle informatie die nodig is om te bepalen hoe het gaat met uw omgeving.

De rapportstructuur in Azure Active Directory bestaat uit de volgende onderdelen:

- **Activiteit** 
    - **Aanmeldactiviteiten**: informatie over het gebruik van beheerde toepassingen en aanmeldactiviteiten van gebruikers
    - **Controlelogboeken**: informatie over systeemactiviteit van gebruikers, groepsbeheer, uw beheerde toepassingen en directory-activiteiten.
- **Beveiliging** 
    - **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. Zie Riskante aanmeldingen voor meer informatie.
    - **Gebruikers van wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. Zie Gebruikers van wie wordt aangegeven dat ze risico lopen voor meer informatie.

In dit onderwerp vindt u meer informatie over de aanmeldactiviteiten.

<a id="pre-requisite" class="xliff"></a>

## Vereiste

<a id="who-can-access-the-data" class="xliff"></a>

### Wie heeft er toegang tot de gegevens?
* Gebruikers met de rol Beveiligingsbeheerder of Beveiligingslezer
* Globale beheerders
* Alle gebruiker (niet-beheerders) hebben toegang tot hun eigen aanmeldingen 

<a id="what-azure-ad-license-do-you-need-to-access-sign-in-activity" class="xliff"></a>

### Welke Azure AD-licentie heb ik nodig voor toegang tot aanmeldingsactiviteiten?
* Uw tenant moet beschikken over een Azure AD Premium-licentie om het rapport met alle aanmeldingsactiviteiten te kunnen raadplegen


<a id="signs-in-activities" class="xliff"></a>

## Aanmeldactiviteiten

In de informatie die wordt aangeboden in het rapport over aanmeldactiviteiten van gebruikers, vindt u antwoord op vragen zoals:

* Wat is het aanmeldingspatroon van een gebruiker?
* Hoeveel keer hebben gebruikers zich aangemeld gedurende een week?
* Wat is de status van deze aanmeldingen?

Uw eerste ingangspunt voor alle aanmeldingsactiviteitgegevens is **Aanmeldingen** in het gedeelte activiteit van **Azure Active**.


![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/61.png "Aanmeldingsactiviteit")


Een controlelogboek heeft een standaardlijstweergave die het volgende laat zien:

- de gerelateerde gebruiker
- de toepassing waarbij de gebruiker is aangemeld
- de aanmeldingsstatus
- de aanmeldingstijd

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/41.png "Aanmeldingsactiviteit")

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/19.png "Aanmeldingsactiviteit")

Hiermee kunt u extra velden weergeven of velden verwijderen die al worden weergegeven.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/42.png "Aanmeldingsactiviteit")

Wanneer u op een item in de lijstweergave klikt, krijgt u er alle beschikbare informatie over te zien.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/43.png "Aanmeldingsactiviteit")


<a id="filtering-sign-in-activities" class="xliff"></a>

## Aanmeldingsactiviteiten filteren

Als u de gerapporteerde gegevens wilt beperken tot een bepaald niveau, kunt u de aanmeldingsgegevens filteren met de volgende velden:

- Tijdsinterval
- Gebruiker
- Toepassing
- Client
- Aanmeldingsstatus

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/44.png "Aanmeldingsactiviteit")


Met het filter **tijdsinterval** kunt u een tijdsbestek opgeven voor de geretourneerde gegevens.  
Mogelijke waarden zijn:

- 1 maand
- 7 dagen
- 24 uur
- Aangepast telefoonnummer

Wanneer u een aangepast tijdsbestek selecteert, kunt u een begintijd en eindtijd configureren.

Met het filter **gebruiker** kunt u de naam of de UPN (User Principal Name) van de gewenste gebruiker opgeven.

Met het filter **toepassing** kunt u de naam van de gewenste toepassing opgeven.

Met het filter **client** kunt u informatie over het gewenste apparaat opgeven.

Met het filter **aanmeldingsstatus** kunt u een van de volgende filters selecteren:

- Alle
- Geslaagd
- Fout


<a id="sign-in-activities-shortcuts" class="xliff"></a>

## Snelkoppelingen voor aanmeldingsactiviteiten

Naast Azure Active Directory biedt de Azure Portal twee extra toegangspunten voor aanmeldingsactiviteitgegevens:

- Gebruikers en groepen
- Bedrijfstoepassingen


<a id="users-and-groups-sign-ins-activities" class="xliff"></a>

### Aanmeldingsactiviteiten van gebruikers en groepen

In de informatie die wordt aangeboden in het rapport over aanmeldingsactiviteiten van gebruikers, vindt u antwoord op vragen zoals:

- Wat is het aanmeldingspatroon van een gebruiker?
- Hoeveel keer hebben gebruikers zich aangemeld gedurende een week?
- Wat is de status van deze aanmeldingen?



Uw beginpunt voor deze gegevens is de aanmeldingsgrafiek van gebruikers in het gedeelte **Overzicht** onder **Gebruikers en groepen**.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/45.png "Aanmeldingsactiviteit")

In de aanmeldingsgrafiek van gebruikers ziet u alle aanmeldingen van alle gebruikers gedurende een bepaalde periode. De standaard ingestelde periode is 30 dagen.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/46.png "Aanmeldingsactiviteit")

Als u in de aanmeldingsgrafiek op een dag klikt, ziet u een gedetailleerd overzicht van de aanmeldingsactiviteiten voor die dag.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/41.png "Aanmeldingsactiviteit")

Elke rij in de lijst met aanmeldingsactiviteiten bevat gedetailleerde informatie over de geselecteerde aanmelding, zoals:

* Wie heeft zich aangemeld?
* Wat was de gerelateerde UPN?
* Welke toepassing was het aanmeldingsdoel?
* Wat is het IP-adres van de persoon die zich heeft aangemeld?
* Wat is de status van de aanmelding?

Met de optie **Aanmeldingen** krijgt u een volledig overzicht van alle gebruikersaanmeldingen.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/51.png "Aanmeldingsactiviteit")



<a id="usage-of-managed-applications" class="xliff"></a>

## Het gebruik van beheerde toepassingen

Met een toepassingsgerichte weergave van uw aanmeldingsgegevens kunt u antwoord vinden op vragen zoals:

* Wie gebruikt mijn toepassingen?
* Wat zijn de drie meest gebruikte toepassingen in uw organisatie?
* Ik heb onlangs een toepassing geïmplementeerd. Hoe gaat het ermee?

Uw beginpunt voor deze gegevens is het overzicht van de drie populairste toepassingen in uw organisatie volgens het rapport van de laatste 30 dagen. Het overzicht vindt u in het gedeelte **Overzicht** onder **Bedrijfstoepassingen**.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/64.png "Aanmeldingsactiviteit")

In de grafiek over appgebruik staat een wekelijks overzicht van alle aanmeldingen bij de drie populairste toepassingen gedurende een bepaalde periode. De standaard ingestelde periode is 30 dagen.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/47.png "Aanmeldingsactiviteit")

Als u wilt, kunt u de focus instellen op een specifieke toepassing.


![Rapportage](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Rapportage")

Als u op een dag in de appgebruikgrafiek klikt, ziet u een gedetailleerd overzicht van de aanmeldactiviteiten.


![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/48.png "Aanmeldingsactiviteit")


Met de optie **Aanmeldingen** krijgt u een volledig overzicht van alle aanmeldingsgebeurtenissen voor uw toepassingen.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/49.png "Aanmeldingsactiviteit")



<a id="next-steps" class="xliff"></a>

## Volgende stappen
Zie de [Azure Active Directory-rapportagegids](active-directory-reporting-guide.md).


