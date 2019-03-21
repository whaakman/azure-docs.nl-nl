---
title: Controleactiviteitenrapporten in Azure Active Directory Portal | Microsoft Docs
description: Ontdek de controleactiviteitenrapporten in de Azure Active Directory Portal
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
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
ms.openlocfilehash: cf069e053068eaee3f36153c3888e6c711724267
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182163"
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Controleactiviteitenrapporten in Azure Active Directory Portal 

Met Azure Active Directory (Azure AD)-rapporten krijgt u informatie die u nodig hebt om te bepalen hoe het gaat met uw omgeving.

De rapportstructuur bestaat uit de volgende onderdelen:

- **Activiteit** 
    - **Aanmeldingen** : de [aanmeldingenrapport](concept-sign-ins.md) vindt u informatie over het gebruik van beheerde toepassingen en de gebruiker aanmeldactiviteiten.
    - **Auditlogboeken**: traceerbaarheid via logboeken voor alle door diverse functies binnen Azure AD uitgevoerde wijzigingen. Voorbeelden van auditlogboekgegevens zijn wijzigingen aangebracht in resources binnen Azure AD, zoals het toevoegen of verwijderen van gebruikers, apps, groepen, rollen en beleid.
- **Beveiliging** 
    - **Riskante aanmeldingen** : een [riskante aanmelding](concept-risky-sign-ins.md) is een indicator van een aanmeldingspoging die mogelijk zijn uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount. 
    - **Gebruikers die zijn gemarkeerd voor risico's** : een [riskante gebruiker](concept-user-at-risk.md) is een indicator van een gebruikersaccount dat mogelijk is aangetast.

In dit artikel biedt een overzicht van het controlerapport.
 
## <a name="who-can-access-the-data"></a>Wie heeft er toegang tot de gegevens?

* Gebruikers in de **Security beheerder**, **Beveiligingslezer**, **rapportlezer** of **hoofdbeheerder** rollen
* Bovendien kunnen hun eigen auditactiviteiten zien in alle gebruikers (niet-beheerders)

## <a name="audit-logs"></a>Controlelogboeken

De Azure AD-audit-logboeken bieden records van systeemactiviteiten voor naleving. Voor toegang tot het controlerapport, selecteer **auditlogboeken** in de **activiteit** sectie van **Azure Active Directory**. Houd er rekening mee dat auditlogboeken met een latentie van maximaal een uur, hebben kunnen zodat deze activiteit controlegegevens worden weergegeven in de portal nadat u de taak hebt voltooid die lang kan duren.

![Controlelogboeken](./media/concept-audit-logs/61.png "Controlelogboeken")

Een controlelogboek heeft een standaardlijstweergave die het volgende laat zien:

- de datum en tijd van de gebeurtenis
- de service die de gebeurtenis vastgelegd
- de categorie en de naam van de activiteit (*wat*) 
- de status van de activiteit (slagen of mislukken)
- het doel
- De initiator / actor (wie) van een activiteit

![Controlelogboeken](./media/concept-audit-logs/listview.png "Controlelogboeken")

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Controlelogboeken](./media/concept-audit-logs/columns.png "Controlelogboeken")

Hiermee kunt u extra velden weergeven of velden verwijderen die al worden weergegeven.

![Controlelogboeken](./media/concept-audit-logs/columnselect.png "Controlelogboeken")

Selecteer een item in de lijstweergave voor meer gedetailleerde informatie.

![Controlelogboeken](./media/concept-audit-logs/details.png "Controlelogboeken")


## <a name="filtering-audit-logs"></a>Controlelogboeken filteren

U kunt de controlegegevens op de volgende velden filteren:

- Service
- Categorie
- Activiteit
- Status
- Doel
- Gestart door (actor)
- Datumbereik

![Controlelogboeken](./media/concept-audit-logs/filter.png "Controlelogboeken")

De **Service** filter kunt u selecteren in een vervolgkeuzelijst van de volgende services:

- Alle
- Toegangsbeoordelingen
- Account inrichten 
- Toepassing eenmalige aanmelding
- Verificatiemethoden
- B2C
- Voorwaardelijke toegang
- Hoofddirectory
- Rechtenbeheer
- Identiteitsbeveiliging
- Uitgenodigde gebruikers
- PIM
- Self-service voor groepsbeheer
- Self-servicegebruikers Passord Management
- Gebruiksvoorwaarden

De **categorie** filter kunt u een van de volgende filters selecteren:

- Alle
- AdministrativeUnit
- Programmeert
- Authentication
- Autorisatie
- Contactpersoon
- Apparaat
- Apparaatconfiguratie
- DirectoryManagement
- EntitlementManagement
- GroupManagement
- Overige
- Beleid
- ResourceManagement
- RoleManagement
- UserManagement

De **activiteit** filteren is gebaseerd op de categorie en activiteit resource type selectie die u maakt. U kunt een specifieke activiteit of alle activiteiten selecteren. 

U kunt de lijst met alle controleactiviteiten opvragen met behulp van de Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, waarbij u $tenantdomain vervangt door de naam van uw domein. U kunt ook het artikel [Controlerapportgebeurtenissen](reference-audit-activities.md) raadplegen.

De **Status** filter kunt u filteren op basis van de status van een audit-bewerking. De status kan zijn dat een van de volgende:

- Alle
- Geslaagd
- Fout

De **doel** filter kunt u zoeken naar een bepaald doel met de naam of user principal name (UPN). De naam van de doel- en UPN zijn hoofdlettergevoelig. 

De **gestart door** filter kunt u voor het definiëren van een actor-naam of een universal principal name (UPN). De naam en het UPN-zijn hoofdlettergevoelig.

De **datumbereik** filter kunt u een tijdsbestek opgeven voor de geretourneerde gegevens.  
Mogelijke waarden zijn:

- 1 maand
- 7 dagen
- 24 uur
- Aangepast telefoonnummer

Wanneer u een aangepast tijdsbestek selecteert, kunt u een begintijd en eindtijd configureren.

U kunt ook kiezen voor het downloaden van de gefilterde gegevens, maximaal 250.000 records, door het selecteren van de **downloaden** knop. U kunt kiezen om de logboeken in CSV of JSON-indeling te downloaden. Het aantal records die u kunt downloaden wordt beperkt door de [bewaarbeleid Azure Active Directory-rapporten](reference-reports-data-retention.md).

![Controlelogboeken](./media/concept-audit-logs/download.png "Controlelogboeken")

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

Als u alleen bekijken van controlegegevens die is gerelateerd aan gebruikers wilt, vindt u een gefilterde weergave openen via **auditlogboeken** in de **activiteit** sectie van de **gebruikers** tabblad. Dit beginpunt heeft **UserManagement** als vooraf geselecteerde categorie.

![Controlelogboeken](./media/concept-audit-logs/users.png "Controlelogboeken")

Als u alleen bekijken van controlegegevens met betrekking tot groepen wilt, vindt u een gefilterde weergave openen via **auditlogboeken** in de **activiteit** sectie van de **groepen** tabblad. Dit beginpunt heeft **GroupManagement** als vooraf geselecteerde categorie.

![Controlelogboeken](./media/concept-audit-logs/groups.png "Controlelogboeken")

### <a name="enterprise-applications-audit-logs"></a>Controlelogboeken voor bedrijfstoepassingen

Met de controlerapporten op basis van toepassingen krijgt u antwoord op vragen zoals:

* Welke toepassingen zijn toegevoegd of bijgewerkt?
* Welke toepassingen zijn verwijderd?
* Is een service-principal voor een toepassing gewijzigd?
* Zijn de namen van toepassingen gewijzigd?
* Wie heeft toestemming gegeven voor een toepassing?

Als u bekijken van controlegegevens die betrekking hebben op uw toepassingen wilt, vindt u een gefilterde weergave openen via **auditlogboeken** in de **activiteit** sectie van de **bedrijfstoepassingen** de blade. Dit beginpunt heeft **bedrijfstoepassingen** vooraf zijn geselecteerd als de **toepassingstype**.

![Controlelogboeken](./media/concept-audit-logs/enterpriseapplications.png "Controlelogboeken")

## <a name="office-365-activity-logs"></a>Office 365-activiteitenlogboeken

Vindt u Office 365-activiteitenlogboeken van de [Microsoft 365-beheercentrum](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center). Hoewel Office 365-activiteit en Azure AD-activiteit logboeken delen veel directoryresources, alleen de Microsoft 365-beheercentrum biedt een volledig overzicht van de Office 365-activiteitenlogboeken. 

U kunt ook toegang tot de Office 365-activiteitenlogboeken programmatisch met behulp van de [Management API's van Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview).

## <a name="next-steps"></a>Volgende stappen

- [Azure AD referentie voor auditactiviteiten van](reference-audit-activities.md)
- [Azure AD-rapporten retentie-verwijzing](reference-reports-data-retention.md)
- [Azure AD log latenties verwijzen naar](reference-reports-latencies.md)
