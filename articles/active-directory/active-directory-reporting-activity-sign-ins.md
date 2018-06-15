---
title: Aanmeldactiviteitenrapporten in Azure Active Directory Portal | Microsoft Docs
description: Ontdek de aanmeldactiviteitenrapporten in de Azure Active Directory Portal
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/17/2018
ms.author: rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: 6414be82e57741bf2af1de3650a3d9a28c4d6427
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34588604"
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Aanmeldactiviteitenrapporten in Azure Active Directory Portal

Met Azure AD-rapporten (Azure Active Directory) in [Azure Portal](https://portal.azure.com) ontvangt u alle informatie die nodig is om te bepalen hoe het gaat met uw omgeving.

De rapportstructuur in Azure Active Directory bestaat uit de volgende onderdelen:

- **Activiteit** 
    - **Aanmeldactiviteiten**: informatie over het gebruik van beheerde toepassingen en aanmeldactiviteiten van gebruikers
    - **Controlelogboeken**: informatie over systeemactiviteit van gebruikers, groepsbeheer, uw beheerde toepassingen en directory-activiteiten.
- **Beveiliging** 
    - **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. Zie Riskante aanmeldingen voor meer informatie.
    - **Gebruikers van wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. Zie Gebruikers van wie wordt aangegeven dat ze risico lopen voor meer informatie.

In dit onderwerp vindt u meer informatie over de aanmeldactiviteiten.

## <a name="prerequisites"></a>Vereisten

### <a name="who-can-access-the-data"></a>Wie heeft er toegang tot de gegevens?
* Gebruikers met de rol Beveiligingsbeheerder, Beveiligingslezer of Rapportlezer
* Globale beheerders
* Alle gebruiker (niet-beheerders) hebben toegang tot hun eigen aanmeldingen 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Welke Azure AD-licentie heb ik nodig voor toegang tot aanmeldingsactiviteiten?
* Uw tenant moet beschikken over een Azure AD Premium-licentie om het rapport met alle aanmeldingsactiviteiten te kunnen raadplegen


## <a name="sign-in-activities"></a>Aanmeldactiviteiten

In de informatie die wordt aangeboden in het rapport over aanmeldingsactiviteiten van gebruikers, vindt u antwoord op vragen zoals:

* Wat is het aanmeldingspatroon van een gebruiker?
* Hoeveel gebruikers hebben zich gedurende een week aangemeld?
* Wat is de status van deze aanmeldingen?

Uw eerste ingangspunt voor alle aanmeldingsactiviteitgegevens is **Aanmeldingen** in het gedeelte activiteit van **Azure Active**.


![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/61.png "Aanmeldingsactiviteit")


Een aanmeldingslogboek heeft een standaardlijstweergave die het volgende laat zien:

- De aanmeldingsdatum
- De gerelateerde gebruiker
- De toepassing waarbij de gebruiker zich heeft aangemeld
- De aanmeldingsstatus
- De status van de risicodetectie
- De status van de vereiste voor meervoudige verificatie (MFA) 

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/01.png "Aanmeldingsactiviteit")

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/19.png "Aanmeldingsactiviteit")

Hiermee kunt u extra velden weergeven of velden verwijderen die al worden weergegeven.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/02.png "Aanmeldingsactiviteit")

Wanneer u op een item in de lijstweergave klikt, worden in een horizontale weergave alle beschikbare gegevens voor het item vermeld.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/03.png "Aanmeldingsactiviteit")


## <a name="filter-sign-in-activities"></a>Aanmeldactiviteiten filteren

Als u de gerapporteerde gegevens wilt beperken tot een bepaald niveau, kunt u de aanmeldingsgegevens filteren met de volgende standaardvelden:

- Gebruiker
- Toepassing
- Aanmeldingsstatus
- Status van de risicodetectie
- Date


![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/04.png "Aanmeldingsactiviteit")

Met het filter **Gebruiker** kunt u de naam of de UPN (User Principal Name) van de gewenste gebruiker opgeven. 

Met het filter **Toepassing** kunt u de naam van de gewenste toepassing opgeven.  

Met het filter **Aanmeldingsstatus** kunt u selecteren:

- Alle 
- Geslaagd
- Fout

Met het filter **Risico gedetecteerd** kunt u selecteren:

- Alle
- Ja
- Nee 


Met het filter **Datum** kunt u een tijdsbestek opgeven voor de geretourneerde gegevens.  
Mogelijke waarden zijn:

- 1 maand
- 7 dagen
- 24 uur
- Aangepast tijdsinterval

Wanneer u een aangepast tijdsbestek selecteert, kunt u een begintijd en eindtijd configureren.

Als u extra velden toevoegt aan uw aanmeldingsweergave, worden deze velden automatisch toegevoegd aan de lijst met filters. Als u bijvoorbeeld het veld **Client-app** aan uw lijst toevoegt, krijgt u ook een andere filteroptie waarmee u de volgende filters kunt instellen:

- Browser      
- Exchange ActiveSync (ondersteund)               
- Exchange ActiveSync (niet ondersteund)
- Andere clients               
    - IMAP
    - MAPI
    - Oudere Office-clients
    - POP
    - SMTP


![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/12.png "Aanmeldingsactiviteit")


> [!TIP] 
> Naast de standaardfilters worden alle aanvullende velden die u toevoegt aan de aanmeldingsweergave filtervelden.


## <a name="download-sign-in-activities"></a>Aanmeldactiviteiten downloaden

U kunt de gegevens van de aanmeldingsactiviteiten downloaden als u buiten Azure Portal met de gegevens wilt werken. Naast een downloadknop bevat Azure Portal ook een optie voor het genereren van een script voor het downloaden van uw gegevens.  

![Downloaden](./media/active-directory-reporting-activity-sign-ins/71.png "Downloaden")

Als u op **Downloaden** klikt, wordt er een CSV-bestand gemaakt met de vijfduizend meest recente records. Als u meer flexibiliteit nodig hebt, kunt u kiezen voor de scriptoplossing. Klik op **Script** om een script te maken dat alle filters bevat die u hebt ingesteld. Naast de technische implementatie, wordt het aantal records dat u kunt downloaden ook beperkt door het [retentiebeleid voor Azure Active Directory-rapporten](active-directory-reporting-retention.md).  



## <a name="sign-in-activities-shortcuts"></a>Snelkoppelingen voor aanmeldingsactiviteiten

Naast Azure Active Directory biedt de Azure Portal twee extra toegangspunten voor aanmeldingsactiviteitgegevens:

- Overzicht van identiteitbeveiliging en -bescherming
- Gebruikers
- Groepen
- Bedrijfstoepassingen


### <a name="users-sign-ins-activities"></a>Aanmeldingsactiviteiten van gebruikers

In de informatie die wordt aangeboden in het rapport over aanmeldingsactiviteiten van gebruikers, vindt u antwoord op vragen zoals:

- Wat is het aanmeldingspatroon van een gebruiker?
- Hoeveel keer hebben gebruikers zich aangemeld gedurende een week?
- Wat is de status van deze aanmeldingen?



Uw beginpunt voor deze gegevens is de aanmeldingsgrafiek van gebruikers op de overzichtspagina **Identiteitbeveiliging en -bescherming**. In de aanmeldingsgrafiek van gebruikers ziet u alle aanmeldingen van alle gebruikers gedurende een bepaalde periode. De standaard ingestelde periode is 30 dagen.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/06.png "Aanmeldingsactiviteit")

Als u in de aanmeldingsgrafiek op een dag klikt, ziet u een overzicht van de aanmeldingsactiviteiten voor die dag.


Elke rij in de lijst met aanmeldingsactiviteiten geeft het volgende weer:

* Wie heeft zich aangemeld?
* Welke toepassing was het aanmeldingsdoel?
* Wat is de status van de aanmelding?
* Wat is de MFA-status van de aanmelding?

Door op een item te klikken, krijgt u meer informatie over de aanmelding:

- Gebruikers-ID
- Gebruiker
- Gebruikersnaam
- Toepassings-id
- Toepassing
- Client
- Locatie
- IP-adres
- Date
- MFA vereist
- Aanmeldingsstatus

 
Op de pagina **Gebruikers** krijgt u een volledig overzicht van alle aanmeldingen van gebruikers door in de sectie **Activiteit** op **Aanmelden** te klikken.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/08.png "Aanmeldingsactiviteit")




## <a name="usage-of-managed-applications"></a>Het gebruik van beheerde toepassingen

Met een toepassingsgerichte weergave van uw aanmeldingsgegevens kunt u antwoord vinden op vragen zoals:

* Wie gebruikt mijn toepassingen?
* Wat zijn de drie meest gebruikte toepassingen in uw organisatie?
* Ik heb onlangs een toepassing geïmplementeerd. Hoe gaat het ermee?

Uw beginpunt voor deze gegevens is het overzicht van de *drie populairste toepassingen in uw organisatie volgens het rapport van de laatste 30 dagen*. Het overzicht vindt u in de sectie **Overzicht** van de pagina **Bedrijfstoepassingen**.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/10.png "Aanmeldingsactiviteit")

In de grafiek over appgebruik staat een wekelijks overzicht van alle aanmeldingen bij de drie populairste toepassingen gedurende een bepaalde periode. De standaard ingestelde periode is 30 dagen.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/47.png "Aanmeldingsactiviteit")

Als u wilt, kunt u de focus instellen op een specifieke toepassing.


![Rapportage](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Rapportage")

Als u op een dag in de appgebruikgrafiek klikt, ziet u een gedetailleerd overzicht van de aanmeldactiviteiten.




Met de optie **Aanmeldingen** krijgt u een volledig overzicht van alle aanmeldingsgebeurtenissen voor uw toepassingen.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins/11.png "Aanmeldingsactiviteit")



## <a name="next-steps"></a>Volgende stappen

Als u meer wilt weten over foutcodes voor aanmeldingsactiviteiten, raadpleegt u [Foutcodes voor aanmeldactiviteitenrapporten in Azure Active Directory Portal](active-directory-reporting-activity-sign-ins-errors.md).

