---
title: Aanmeldactiviteitenrapporten in Azure Active Directory Portal | Microsoft Docs
description: Ontdek de aanmeldactiviteitenrapporten in de Azure Active Directory Portal
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b958a8d7aa8566c62d5fdaf6b5b51064e4e5123
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58183744"
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Aanmeldactiviteitenrapporten in Azure Active Directory Portal

De rapportstructuur in Azure Active Directory (Azure AD) bestaat uit de volgende onderdelen:

- **Activiteit** 
    - **Aanmeldingen** : informatie over het gebruik van beheerde toepassingen en aanmeldactiviteiten van gebruikers.
    - **Auditlogboeken** - [auditlogboeken](concept-audit-logs.md) systeeminformatie over systeemactiviteit van gebruikers en beheer van groepen, beheerde toepassingen en directory-activiteiten bevatten.
- **Beveiliging** 
    - **Riskante aanmeldingen** : een [riskante aanmelding](concept-risky-sign-ins.md) is een indicator van een aanmeldingspoging die mogelijk zijn uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount.
    - **Gebruikers die zijn gemarkeerd voor risico's** : een [riskante gebruiker](concept-user-at-risk.md) is een indicator van een gebruikersaccount dat mogelijk is aangetast.

In dit onderwerp vindt u een overzicht van het rapport-aanmeldingen.

## <a name="prerequisites"></a>Vereisten

### <a name="who-can-access-the-data"></a>Wie heeft er toegang tot de gegevens?
* Gebruikers in de rollen Beveiligingsbeheerder, Beveiligingslezer en rapportlezer
* Globale beheerders
* Bovendien een gebruiker (niet-beheerders) toegang krijgen tot hun eigen aanmeldingen 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Welke Azure AD-licentie heb ik nodig voor toegang tot aanmeldingsactiviteiten?
* Uw tenant moet beschikken over een Azure AD Premium-licentie die is gekoppeld aan om te zien van het activiteitenrapport-alles op aanmelden. Zie [Aan de slag met Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) om uw versie van Azure Active Directory te upgraden. Als er vóór de upgrade nog geen activiteitgegevens waren, duurt het na het upgraden naar een premium-licentie enkele dagen voordat er gegevens worden weergegeven in de rapporten.

## <a name="sign-ins-report"></a>Aanmeldingenrapport

De gebruiker aanmeldingen rapport vindt u antwoorden op de volgende vragen:

* Wat is het aanmeldingspatroon van een gebruiker?
* Hoeveel gebruikers hebben zich gedurende een week aangemeld?
* Wat is de status van deze aanmeldingen?

U kunt toegang tot het rapport-aanmeldingen door te selecteren **aanmeldingen** in de **activiteit** sectie van de **Azure Active Directory** -blade in de [vanAzureportal](https://portal.azure.com). Houd er rekening mee dat het duurt maximaal twee uur voor aanmelding bij records worden weergegeven in de portal.

![Aanmeldingsactiviteit](./media/concept-sign-ins/61.png "Aanmeldingsactiviteit")

> [!IMPORTANT]
> Rapport wordt weergegeven de aanmeldingen de **interactieve** aanmeldingen, die is, waarbij een gebruiker handmatig zich aanmeldt met hun gebruikersnaam en wachtwoord voor aanmeldingen. Niet-interactieve aanmeldingen, zoals verificatie van service-naar-service, worden niet weergegeven in het rapport-aanmeldingen. 

Een aanmeldingslogboek heeft een standaardlijstweergave die het volgende laat zien:

- De aanmeldingsdatum
- De gerelateerde gebruiker
- De toepassing waarbij de gebruiker zich heeft aangemeld
- De aanmeldingsstatus
- De status van de risicodetectie
- De status van de vereiste voor meervoudige verificatie (MFA)

![Aanmeldingsactiviteit](./media/concept-sign-ins/01.png "Aanmeldingsactiviteit")

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Aanmeldingsactiviteit](./media/concept-sign-ins/19.png "Aanmeldingsactiviteit")

Hiermee kunt u extra velden weergeven of velden verwijderen die al worden weergegeven.

![Aanmeldingsactiviteit](./media/concept-sign-ins/02.png "Aanmeldingsactiviteit")

Selecteer een item in de lijstweergave voor meer gedetailleerde informatie.

![Aanmeldingsactiviteit](./media/concept-sign-ins/03.png "Aanmeldingsactiviteit")

> [!NOTE]
> Klanten kunnen nu beleidsregels voor voorwaardelijke toegang met behulp van rapporten voor alle aanmelden oplossen. Door te klikken op de **voorwaardelijke toegang** tabblad voor een record aanmelding bij klanten de status voor voorwaardelijke toegang en informatie over de details van het beleid toegepast op de aanmelding en het resultaat voor elk beleid kunnen bekijken.
> Zie voor meer informatie de [Veelgestelde vragen over gegevens van de Certificeringsinstantie in alle aanmeldingen](reports-faq.md#conditional-access).

![Aanmeldingsactiviteit](./media/concept-sign-ins/ConditionalAccess.png "Aanmeldingsactiviteit")


## <a name="filter-sign-in-activities"></a>Aanmeldactiviteiten filteren

Als u de gerapporteerde gegevens wilt beperken tot een bepaald niveau, kunt u de aanmeldingsgegevens filteren met de volgende standaardvelden:

- Gebruiker
- Toepassing
- Aanmeldingsstatus
- Voorwaardelijke toegang
- Date

![Aanmeldingsactiviteit](./media/concept-sign-ins/04.png "Aanmeldingsactiviteit")

Met het filter **Gebruiker** kunt u de naam of de UPN (User Principal Name) van de gewenste gebruiker opgeven.

Met het filter **Toepassing** kunt u de naam van de gewenste toepassing opgeven.

Met het filter **Aanmeldingsstatus** kunt u selecteren:

- Alle
- Geslaagd
- Fout

De **voorwaardelijke toegang** filter kunt u de status van de CA-beleid voor de aanmelding selecteren:

- Alle
- Niet toegepast
- Geslaagd
- Fout

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


![Aanmeldingsactiviteit](./media/concept-sign-ins/12.png "Aanmeldingsactiviteit")


## <a name="download-sign-in-activities"></a>Aanmeldactiviteiten downloaden

U kunt [downloaden van de aanmeldingsgegevens](quickstart-download-sign-in-report.md) als u wilt werken met deze buiten de Azure-portal. Te klikken op **downloaden** biedt de mogelijkheid om een CSV-bestand of JSCON bestand van de meest recente 250.000 records te maken.  

![Downloaden](./media/concept-sign-ins/71.png "Downloaden")

> [!IMPORTANT]
> Het aantal records die u kunt downloaden wordt beperkt door de [bewaarbeleid Azure Active Directory-rapporten](reference-reports-data-retention.md).  


## <a name="sign-ins-data-shortcuts"></a>Aanmeldingen gegevens snelkoppelingen

Naast Azure AD biedt Azure portal de extra toegangspunten voor aanmeldingsgegevens:

- Het overzicht van de beveiliging van identiteit
- Gebruikers
- Groepen
- Bedrijfstoepassingen

### <a name="users-sign-ins-data-in-identity-security-protection"></a>Gegevens van de gebruikers-aanmeldingen in Identiteitbeveiliging en-bescherming

De gebruiker aanmelden grafiek in de **Identiteitbeveiliging en-bescherming** overzichtspagina aanmeldingsgrafiek van aanmelding bevat modules voor alle gebruikers in een bepaalde periode. De standaard ingestelde periode is 30 dagen.

![Aanmeldingsactiviteit](./media/concept-sign-ins/06.png "Aanmeldingsactiviteit")

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

> [!NOTE]
> IP-adressen worden uitgegeven zodanig dat er is geen definitieve verbinding tussen een IP-adres en waar de computer met dit adres zich fysiek bevindt. Toewijzing van IP-adressen wordt bemoeilijkt door het feit dat mobiele providers en VPN's uitgeven IP-adressen uit de centrale pools die vaak erg zijn ver ligt waar het client-apparaat daadwerkelijk wordt gebruikt. In Azure AD-rapporten is IP-adres converteren naar een fysieke locatie momenteel een best-effort op basis van traceringen, register, omgekeerde zoekopdrachten en andere gegevens.

Op de pagina **Gebruikers** krijgt u een volledig overzicht van alle aanmeldingen van gebruikers door in de sectie **Activiteit** op **Aanmelden** te klikken.

![Aanmeldingsactiviteit](./media/concept-sign-ins/08.png "Aanmeldingsactiviteit")

## <a name="usage-of-managed-applications"></a>Het gebruik van beheerde toepassingen

Met een toepassingsgerichte weergave van uw aanmeldingsgegevens kunt u antwoord vinden op vragen zoals:

* Wie gebruikt mijn toepassingen?
* Wat zijn de drie meest gebruikte toepassingen in uw organisatie?
* Ik heb onlangs een toepassing geïmplementeerd. Hoe gaat het ermee?

Uw beginpunt voor deze gegevens is het overzicht van de drie populairste toepassingen in uw organisatie volgens het rapport van de laatste 30 dagen. Het overzicht vindt u in het gedeelte **Overzicht** onder **Bedrijfstoepassingen**.

![Aanmeldingsactiviteit](./media/concept-sign-ins/10.png "Aanmeldingsactiviteit")

In de grafiek over appgebruik staat een wekelijks overzicht van alle aanmeldingen bij de drie populairste toepassingen gedurende een bepaalde periode. De standaard ingestelde periode is 30 dagen.

![Aanmeldingsactiviteit](./media/concept-sign-ins/47.png "Aanmeldingsactiviteit")

Als u wilt, kunt u de focus instellen op een specifieke toepassing.

![Rapportage](./media/concept-sign-ins/single_spp_usage_graph.png "Rapportage")

Als u op een dag in de appgebruikgrafiek klikt, ziet u een gedetailleerd overzicht van de aanmeldactiviteiten.

Met de optie **Aanmeldingen** krijgt u een volledig overzicht van alle aanmeldingsgebeurtenissen voor uw toepassingen.

![Aanmeldingsactiviteit](./media/concept-sign-ins/11.png "Aanmeldingsactiviteit")

## <a name="office-365-activity-logs"></a>Office 365-activiteitenlogboeken

Vindt u Office 365-activiteitenlogboeken van de [Microsoft 365-beheercentrum](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center). Hoewel Office 365-activiteit en Azure AD-activiteit logboeken delen veel directoryresources, alleen de Microsoft 365-beheercentrum biedt een volledig overzicht van de Office 365-activiteitenlogboeken. 

U kunt ook toegang tot de Office 365-activiteitenlogboeken programmatisch met behulp van de [Management API's van Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview).

## <a name="next-steps"></a>Volgende stappen

* [Foutcodes voor aanmeldingsactiviteiten rapport](reference-sign-ins-error-codes.md)
* [Bewaarbeleid voor gegevens met Azure AD](reference-reports-data-retention.md)
* [Azure AD-rapport latenties](reference-reports-latencies.md)

