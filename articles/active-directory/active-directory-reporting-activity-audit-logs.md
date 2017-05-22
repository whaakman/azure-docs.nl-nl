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
ms.date: 05/04/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 4065682658bdd99066266b8b4e5e4c4605ff3db9
ms.contentlocale: nl-nl
ms.lasthandoff: 05/08/2017


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
- Account inrichten
- Automatische wachtwoordoverschakeling
- uitgenodigde gebruikers
- MIM-service

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

| Activiteitscategorie| Type activiteitsresource| Activiteit |
| :-- | :-: | :-- |
| Hoofddirectory| Groep| Groepsinstellingen verwijderen|
| Hoofddirectory| Directory| Domein bijwerken|
| Hoofddirectory| Directory| Partner uit bedrijf verwijderen|
| Hoofddirectory| Gebruiker| Rol bijwerken|
| Hoofddirectory| Gebruiker| Rol toevoegen vanuit sjabloon|
| Hoofddirectory| Groep| App-roltoewijzing toevoegen aan groep|
| Hoofddirectory| Groep| Starten met het toepassen van een groepslicentie op gebruikers|
| Hoofddirectory| Toepassing| Service-principal toevoegen|
| Hoofddirectory| Beleid| Beleid bijwerken|
| Hoofddirectory| Beleid| Beleid toevoegen aan service-principal|
| Hoofddirectory| Apparaat| Geregistreerde eigenaar toevoegen aan apparaat|
| Hoofddirectory| Apparaat| Geregistreerde gebruikers toevoegen aan apparaat|
| Hoofddirectory| Apparaat| Apparaatconfiguratie bijwerken|
| Selfservice voor wachtwoordbeheer| Gebruiker| Wachtwoord opnieuw instellen (selfservice)|
| Selfservice voor wachtwoordbeheer| Gebruiker| Gebruikersaccount ontgrendelen (selfservice)|
| Selfservice voor wachtwoordbeheer| Gebruiker| Wachtwoord opnieuw instellen (door beheerder)|
| Self-service voor groepsbeheer| Groep| Aanvraag in behandeling voor deelname aan een groep verwijderen|
| Account inrichten| Toepassing| Proces-escrow|
| Automatische wachtwoordoverschakeling| Toepassing| Automatische wachtwoordoverschakeling|
| Uitgenodigde gebruikers| Overige| Verwerkte batchuitnodigingen|
| Hoofddirectory| Directory| Geverifieerd domein verwijderen|
| Hoofddirectory| Directory| Niet-geverifieerd domein toevoegen|
| Hoofddirectory| Directory| Geverifieerd domein toevoegen|
| Hoofddirectory| Directory| Directory-functie voor tenant instellen|
| Hoofddirectory| Directory| De vlag Dirsyncenabled instellen|
| Hoofddirectory| Directory| Bedrijfsinstellingen maken|
| Hoofddirectory| Directory| Bedrijfsinstellingen bijwerken|
| Hoofddirectory| Directory| Bedrijfsinstellingen verwijderen|
| Hoofddirectory| Directory| Door het bedrijf toegestane gegevenslocatie instellen|
| Hoofddirectory| Directory| Multinationale functie inschakelen|
| Hoofddirectory| Gebruiker| Gebruiker bijwerken|
| Hoofddirectory| Gebruiker| Gebruiker verwijderen|
| Hoofddirectory| Groep| Lid verwijderen uit groep|
| Hoofddirectory| Groep| Groepslicentie instellen|
| Hoofddirectory| Groep| Groepsinstellingen maken|
| Hoofddirectory| Toepassing| Service-principal bijwerken|
| Hoofddirectory| Toepassing| Toepassing verwijderen|
| Hoofddirectory| Toepassing| Toepassing bijwerken|
| Hoofddirectory| Toepassing| Service-principal verwijderen|
| Hoofddirectory| Toepassing| Referenties voor service-principal toevoegen|
| Hoofddirectory| Toepassing| App-roltoewijzing verwijderen uit service-principal|
| Hoofddirectory| Toepassing| Eigenaar uit toepassing verwijderen|
| Hoofddirectory| Apparaat| Geregistreerde eigenaar uit apparaat verwijderen|
| Self-service voor wachtwoordbeheer| Gebruiker| Self-service voor stroomactiviteitvoortgang bij het opnieuw instellen van een wachtwoord|
| Account inrichten| Toepassing| Beheer|
| Account inrichten| Toepassing| Directory-bewerking|
| MIM-service| Groep| Lid verwijderen|
| Hoofddirectory| Beleid| Beleid verwijderen|
| Uitgenodigde gebruikers| Gebruiker| Virale tenant maken|
| Hoofddirectory| Directory| Externe geheimen bijwerken|
| Hoofddirectory| Directory| Rights Management-eigenschappen instellen|
| Hoofddirectory| Directory| Bedrijf bijwerken|
| Hoofddirectory| Gebruiker| Gebruiker toevoegen|
| Hoofddirectory| Gebruiker| Federatieve gebruiker converteren naar beheerd|
| Hoofddirectory| Gebruiker| Toepassingswachtwoord voor gebruiker maken|
| Hoofddirectory| Groep| Lid toevoegen aan groep|
| Hoofddirectory| Groep| Groep toevoegen|
| Hoofddirectory| Toepassing| Instemmen met toepassing|
| Hoofddirectory| Toepassing| Toepassing toevoegen|
| Hoofddirectory| Toepassing| Eigenaar toevoegen aan service-principal|
| Hoofddirectory| Toepassing| Oauth2Permissiongrant verwijderen|
| Hoofddirectory| Beleid| Beleidsreferenties verwijderen|
| Hoofddirectory| Apparaat| Apparaatconfiguratie verwijderen|
| Self-service voor groepsbeheer| Groep| Eigenschappen voor dynamische groep instellen|
| Self-service voor groepsbeheer| Groep| Levenscyclusbeheerbeleid bijwerken|
| Account inrichten| Toepassing| Synchronisatieregelactie|
| Uitgenodigde gebruikers| Overige| Batchuitnodigingen geüpload|
| MIM-service| Groep| Lid toevoegen|
| Hoofddirectory| Gebruiker| Licentie-eigenschappen instellen|
| Hoofddirectory| Gebruiker| Gebruiker herstellen|
| Hoofddirectory| Gebruiker| Lid verwijderen uit rol|
| Hoofddirectory| Gebruiker| App-roltoewijzing verwijderen uit gebruiker|
| Hoofddirectory| Gebruiker| Scoped lid verwijderen uit rol|
| Hoofddirectory| Groep| Groep bijwerken|
| Hoofddirectory| Groep| Eigenaar toevoegen aan groep|
| Hoofddirectory| Groep| Het toepassen van een groepslicentie op gebruikers voltooien|
| Hoofddirectory| Groep| App-roltoewijzing verwijderen uit groep|
| Hoofddirectory| Groep| Groep instellen op beheer door gebruiker|
| Hoofddirectory| Toepassing| Oauth2Permissiongrant toevoegen|
| Hoofddirectory| Toepassing| App-roltoewijzing toevoegen aan service-principal|
| Hoofddirectory| Toepassing| Referenties voor service-principal verwijderen|
| Hoofddirectory| Beleid| Beleid verwijderen uit service-principal|
| Hoofddirectory| Apparaat| Apparaat bijwerken|
| Hoofddirectory| Apparaat| Apparaat toevoegen|
| Hoofddirectory| Apparaat| Apparaatconfiguratie toevoegen|
| Self-service voor wachtwoordbeheer| Gebruiker| Wachtwoord wijzigen (selfservice)|
| Self-service voor wachtwoordbeheer| Gebruiker| Door gebruiker geregistreerd voor selfservice voor het opnieuw instellen van een wachtwoord|
| Self-service voor groepsbeheer| Groep| Aanvraag in behandeling voor deelname aan een groep goedkeuren|
| Hoofddirectory| Directory| Niet-geverifieerd domein verwijderen|
| Hoofddirectory| Directory| Domein verifiëren|
| Hoofddirectory| Directory| Domeinverificatie instellen|
| Hoofddirectory| Directory| Wachtwoordbeleid instellen|
| Hoofddirectory| Directory| Partner aan bedrijf toevoegen|
| Hoofddirectory| Directory| Niveau van bedrijf verhogen tot partner|
| Hoofddirectory| Directory| Partnerschap instellen|
| Hoofddirectory| Directory| Drempelwaarde voor onopzettelijk verwijderen instellen|
| Hoofddirectory| Directory| Niveau van partner verlagen|
| Uitgenodigde gebruikers| Gebruiker| Externe gebruiker uitnodigen|
| Account inrichten| Toepassing| Importeren|
| Hoofddirectory| Toepassing| Eigenaar verwijderen uit service-principal|
| Hoofddirectory| Apparaat| Geregistreerde gebruikers uit apparaat verwijderen|
| Hoofddirectory| Directory| Bedrijfsgegevens instellen|
| Hoofddirectory| Directory| Federatie-instellingen voor domein instellen|
| Hoofddirectory| Directory| Bedrijf maken|
| Hoofddirectory| Directory| Rights Management-eigenschappen leegmaken|
| Hoofddirectory| Directory| Dirsync-functie instellen|
| Hoofddirectory| Directory| Door e-mail geverifieerd domein verifiëren|
| Hoofddirectory| Gebruiker| Gebruikerslicentie wijzigen|
| Hoofddirectory| Gebruiker| Gebruikerswachtwoord wijzigen|
| Hoofddirectory| Gebruiker| Gebruikerswachtwoord opnieuw instellen|
| Hoofddirectory| Gebruiker| App-roltoewijzing toekennen aan gebruiker toevoegen|
| Hoofddirectory| Gebruiker| Lid toevoegen aan rol|
| Hoofddirectory| Gebruiker| Toepassingswachtwoord voor gebruiker verwijderen|
| Hoofddirectory| Gebruiker| Gebruikersreferenties bijwerken|
| Hoofddirectory| Gebruiker| Gebruikersbeheer instellen|
| Hoofddirectory| Gebruiker| Scoped lid toevoegen aan rol|
| Hoofddirectory| Groep| Groep verwijderen|
| Hoofddirectory| Groep| Eigenaar verwijderen uit groep|
| Hoofddirectory| Groep| Groepsinstellingen bijwerken|
| Hoofddirectory| Toepassing| Eigenaar toevoegen aan toepassing|
| Hoofddirectory| Toepassing| Toestemming intrekken|
| Hoofddirectory| Beleid| Beleid toevoegen|
| Hoofddirectory| Apparaat| Apparaat verwijderen|
| Self-service voor wachtwoordbeheer| Gebruiker| Selfservice voor het opnieuw instellen van een wachtwoord geblokkeerd|
| Self-service voor groepsbeheer| Groep| Deelname aan een groep aanvragen|
| Self-service voor groepsbeheer| Groep| Levenscyclusbeheerbeleid maken|
| Self-service voor groepsbeheer| Groep| Aanvraag in behandeling voor deelname aan een groep weigeren|
| Self-service voor groepsbeheer| Groep| Aanvraag in behandeling voor deelname aan een groep annuleren|
| Self-service voor groepsbeheer| Groep| Groep vernieuwen|
| Account inrichten| Toepassing| Exporteren|
| Account inrichten| Toepassing| Overige|
| Uitgenodigde gebruikers| Gebruiker| Uitnodiging externe gebruiker inwisselen|
| Uitgenodigde gebruikers| Gebruiker| Virale gebruiker maken|
| Uitgenodigde gebruikers| Gebruiker| Externe gebruiker toewijzen aan toepassing|




## <a name="audit-logs-shortcuts"></a>Snelkoppelingen naar controlelogboeken

Naast **Azure Active Directory** biedt de Azure Portal twee extra beginpunten voor gegevenscontrole:

- Gebruikers en groepen
- Bedrijfstoepassingen

Zie de [lijst met rapporten over controlegebeurtenissen](active-directory-reporting-audit-events.md#list-of-audit-report-events) voor een volledig overzicht van alle rapporten over controleactiviteiten.


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
Zie de [Azure Active Directory-rapportagegids](active-directory-reporting-guide.md).


