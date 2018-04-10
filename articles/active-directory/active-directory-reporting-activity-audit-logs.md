---
title: Controleactiviteitenrapporten in Azure Active Directory Portal | Microsoft Docs
description: Ontdek de controleactiviteitenrapporten in de Azure Active Directory Portal
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/31/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 97ea32a1e0f8815accff6201251771ab8c088859
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
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

U kunt de lijst met alle controleactiviteiten opvragen met behulp van de Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, waarbij u $tenantdomain vervangt door de naam van uw domein. U kunt ook het artikel [Controlerapportgebeurtenissen](active-directory-reporting-audit-events.md) raadplegen.


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



## <a name="azure-ad-audit-activity-list"></a>Lijst met Azure AD-controleactiviteiten

Dit gedeelte biedt u een lijst met alle activiteiten die kunnen worden geregistreerd. 


|Servicenaam|Controlecategorie|Type activiteitsresource|Activiteit|
|---|---|---|---|
|Account inrichten|Beheer van toepassingen|Toepassing|Beheer|
|Account inrichten|Beheer van toepassingen|Toepassing|Directory-bewerking|
|Account inrichten|Beheer van toepassingen|Toepassing|Exporteren|
|Account inrichten|Beheer van toepassingen|Toepassing|Importeren|
|Account inrichten|Beheer van toepassingen|Toepassing|Overige|
|Account inrichten|Beheer van toepassingen|Toepassing|Proces-escrow|
|Account inrichten|Beheer van toepassingen|Toepassing|Synchronisatieregelactie|
|Toepassingsproxy|Beheer van toepassingen|Toepassing|Toepassing toevoegen|
|Toepassingsproxy|Resource|Resource|SSL-certificaat voor de toepassing toevoegen|
|Toepassingsproxy|Resource|Resource|SSL-binding verwijderen|
|Toepassingsproxy|Beheer van toepassingen|Toepassing|Toepassing verwijderen|
|Toepassingsproxy|Mapbeheer|Directory|Desktop-SSO uitschakelen|
|Toepassingsproxy|Mapbeheer|Directory|Desktop-SSO uitschakelen voor een specifiek domein|
|Toepassingsproxy|Mapbeheer|Directory|Toepassingsproxy uitschakelen|
|Toepassingsproxy|Mapbeheer|Directory|Passthrough-verificatie uitschakelen|
|Toepassingsproxy|Mapbeheer|Directory|Desktop-SSO inschakelen|
|Toepassingsproxy|Mapbeheer|Directory|Desktop-SSO inschakelen voor een specifiek domein|
|Toepassingsproxy|Mapbeheer|Directory|Toepassingsproxy inschakelen|
|Toepassingsproxy|Mapbeheer|Directory|Passthrough-verificatie inschakelen|
|Toepassingsproxy|Resource|Resource|Connector registreren|
|Toepassingsproxy|Beheer van toepassingen|Toepassing|Toepassing bijwerken|
|Toepassingsproxy|Beheer van toepassingen|Toepassing|Modus voor eenmalige aanmelding voor toepassingen bijwerken|
|Automatische wachtwoordoverschakeling|Beheer van toepassingen|Toepassing|Automatische wachtwoordoverschakeling|
|B2C|Beheer van toepassingen|Toepassing|V2-toepassingsmachtigingen toevoegen|
|B2C|Autorisatie|Autorisatie|V2-toepassingsmachtigingen toevoegen|
|B2C|Sleutel|Sleutel|Een sleutel op basis van een ASCII-geheim aan een CPIM-sleutelcontainer toevoegen|
|B2C|Autorisatie|Autorisatie|Een sleutel op basis van een ASCII-geheim aan een CPIM-sleutelcontainer toevoegen|
|B2C|Sleutel|Sleutel|Een sleutel aan een CPIM-sleutelcontainer toevoegen|
|B2C|Autorisatie|Autorisatie|Een sleutel aan een CPIM-sleutelcontainer toevoegen|
|B2C|Resource|Resource|AdminPolicyDatas-RemoveResources|
|B2C|Autorisatie|Autorisatie|AdminPolicyDatas-SetResources|
|B2C|Resource|Resource|AdminPolicyDatas-SetResources|
|B2C|Resource|Resource|AdminUserJourneys-GetResources|
|B2C|Autorisatie|Autorisatie|AdminUserJourneys-GetResources|
|B2C|Autorisatie|Autorisatie|AdminUserJourneys-RemoveResources|
|B2C|Resource|Resource|AdminUserJourneys-RemoveResources|
|B2C|Resource|Resource|AdminUserJourneys-SetResources|
|B2C|Autorisatie|Autorisatie|AdminUserJourneys-SetResources|
|B2C|Autorisatie|Autorisatie|IdentityProvider maken|
|B2C|Resource|Resource|IdentityProvider maken|
|B2C|Autorisatie|Autorisatie|V1-toepassing maken|
|B2C|Beheer van toepassingen|Toepassing|V1-toepassing maken|
|B2C|Beheer van toepassingen|Toepassing|V2-toepassing maken|
|B2C|Autorisatie|Autorisatie|V2-toepassing maken|
|B2C|Mapbeheer|Directory|Aangepaste domeinen in de tenant maken|
|B2C|Autorisatie|Autorisatie|Aangepaste domeinen in de tenant maken|
|B2C|Autorisatie|Autorisatie|Een nieuwe AdminUserJourney maken|
|B2C|Resource|Resource|Een nieuwe AdminUserJourney maken|
|B2C|Resource|Resource|Gelokaliseerde resource-json maken|
|B2C|Autorisatie|Autorisatie|Gelokaliseerde resource-json maken|
|B2C|Autorisatie|Autorisatie|Nieuwe aangepaste IDP maken|
|B2C|Resource|Resource|Nieuwe aangepaste IDP maken|
|B2C|Resource|Resource|Nieuwe IDP maken|
|B2C|Autorisatie|Autorisatie|Nieuwe IDP maken|
|B2C|Autorisatie|Autorisatie|Een B2C-mapresource maken of bijwerken|
|B2C|Resource|Resource|Een B2C-mapresource maken of bijwerken|
|B2C|Resource|Resource|Beleid maken|
|B2C|Autorisatie|Autorisatie|Beleid maken|
|B2C|Autorisatie|Autorisatie|TrustFramework-beleid maken|
|B2C|Resource|Resource|TrustFramework-beleid maken|
|B2C|Autorisatie|Autorisatie|TrustFramework-beleid maken met configureerbaar voorvoegsel|
|B2C|Resource|Resource|TrustFramework-beleid maken met configureerbaar voorvoegsel|
|B2C|Resource|Resource|Gebruikerskenmerk maken|
|B2C|Autorisatie|Autorisatie|Gebruikerskenmerk maken|
|B2C|Autorisatie|Autorisatie|CreateTrustFrameworkPolicy|
|B2C|Resource|Resource|CreateTrustFrameworkPolicy|
|B2C|Autorisatie|Autorisatie|Een nieuwe AdminUserJourney maken of bijwerken|
|B2C|Resource|Resource|IDP verwijderen|
|B2C|Autorisatie|Autorisatie|IDP verwijderen|
|B2C|Resource|Resource|IdentityProvider verwijderen|
|B2C|Autorisatie|Autorisatie|IdentityProvider verwijderen|
|B2C|Beheer van toepassingen|Toepassing|V1-toepassing verwijderen|
|B2C|Autorisatie|Autorisatie|V1-toepassing verwijderen|
|B2C|Beheer van toepassingen|Toepassing|V2-toepassing verwijderen|
|B2C|Autorisatie|Autorisatie|V2-toepassing verwijderen|
|B2C|Autorisatie|Autorisatie|V2-toepassingsmachtiging verwijderen|
|B2C|Beheer van toepassingen|Toepassing|V2-toepassingsmachtiging verwijderen|
|B2C|Resource|Resource|Een B2C-mapresource verwijderen|
|B2C|Autorisatie|Autorisatie|Een B2C-mapresource verwijderen|
|B2C|Sleutel|Sleutel|Een CPIM-sleutelcontainer verwijderen|
|B2C|Autorisatie|Autorisatie|Een CPIM-sleutelcontainer verwijderen|
|B2C|Sleutel|Sleutel|Een sleutelcontainer verwijderen|
|B2C|Resource|Resource|TrustFramework-beleid verwijderen|
|B2C|Autorisatie|Autorisatie|TrustFramework-beleid verwijderen|
|B2C|Resource|Resource|Gebruikerskenmerk verwijderen|
|B2C|Autorisatie|Autorisatie|Gebruikerskenmerk verwijderen|
|B2C|Autorisatie|Autorisatie|B2C-functie inschakelen|
|B2C|Mapbeheer|Directory|B2C-functie inschakelen|
|B2C|Resource|Resource|B2C-mapresources in een resourcegroep ophalen|
|B2C|Resource|Resource|B2C-mapresources in een abonnement ophalen|
|B2C|Autorisatie|Autorisatie|B2C-mapresources in een abonnement ophalen|
|B2C|Autorisatie|Autorisatie|Aangepaste IDP ophalen|
|B2C|Resource|Resource|Aangepaste IDP ophalen|
|B2C|Resource|Resource|IDP ophalen|
|B2C|Autorisatie|Autorisatie|IDP ophalen|
|B2C|Autorisatie|Autorisatie|V1- en V2-toepassingen ophalen|
|B2C|Beheer van toepassingen|Toepassing|V1- en V2-toepassingen ophalen|
|B2C|Autorisatie|Autorisatie|V1-toepassing ophalen|
|B2C|Beheer van toepassingen|Toepassing|V1-toepassing ophalen|
|B2C|Autorisatie|Autorisatie|V1-toepassingen ophalen|
|B2C|Beheer van toepassingen|Toepassing|V1-toepassingen ophalen|
|B2C|Beheer van toepassingen|Toepassing|V2-toepassing ophalen|
|B2C|Autorisatie|Autorisatie|V2-toepassing ophalen|
|B2C|Beheer van toepassingen|Toepassing|V2-toepassingen ophalen|
|B2C|Autorisatie|Autorisatie|V2-toepassingen ophalen|
|B2C|Resource|Resource|Een B2C-mapresource ophalen|
|B2C|Autorisatie|Autorisatie|Een B2C-mapresource ophalen|
|B2C|Autorisatie|Autorisatie|Een lijst met aangepaste domeinen in de tenant ophalen|
|B2C|Mapbeheer|Directory|Een lijst met aangepaste domeinen in de tenant ophalen|
|B2C|Autorisatie|Autorisatie|Een gebruikersbeleving ophalen|
|B2C|Resource|Resource|Een gebruikersbeleving ophalen|
|B2C|Resource|Resource|Toegestane toepassingsclaims voor gebruikersbeleving ophalen|
|B2C|Autorisatie|Autorisatie|Toegestane toepassingsclaims voor gebruikersbeleving ophalen|
|B2C|Resource|Resource|Toegestane zelf-gecontroleerde claims voor gebruikersbeleving ophalen|
|B2C|Autorisatie|Autorisatie|Toegestane zelf-gecontroleerde claims voor gebruikersbeleving ophalen|
|B2C|Resource|Resource|Toegestane zelf-gecontroleerde claims voor beleid ophalen|
|B2C|Autorisatie|Autorisatie|Toegestane zelf-gecontroleerde claims voor beleid ophalen|
|B2C|Resource|Resource|Lijst met beschikbare uitvoerclaims ophalen|
|B2C|Autorisatie|Autorisatie|Lijst met beschikbare uitvoerclaims ophalen|
|B2C|Resource|Resource|Inhouddefinities voor gebruikersbeleving ophalen|
|B2C|Autorisatie|Autorisatie|Inhouddefinities voor gebruikersbeleving ophalen|
|B2C|Autorisatie|Autorisatie|IDP's voor een specifieke beheerstroom ophalen|
|B2C|Resource|Resource|IDP's voor een specifieke beheerstroom ophalen|
|B2C|Sleutel|Sleutel|Actieve sleutelmetagegevens voor sleutelcontainer in JWK ophalen|
|B2C|Autorisatie|Autorisatie|Actieve sleutelmetagegevens voor sleutelcontainer in JWK ophalen|
|B2C|Sleutel|Sleutel|Metagegevens voor sleutelcontainer ophalen|
|B2C|Resource|Resource|Lijst van alle beheerstromen ophalen|
|B2C|Autorisatie|Autorisatie|Lijst van alle beheerstromen ophalen|
|B2C|Autorisatie|Autorisatie|Lijst met labels voor alle beheerstromen voor alle gebruikers ophalen|
|B2C|Resource|Resource|Lijst met labels voor alle beheerstromen voor alle gebruikers ophalen|
|B2C|Resource|Resource|Lijst met tenants voor een gebruiker ophalen|
|B2C|Autorisatie|Autorisatie|Lijst met tenants voor een gebruiker ophalen|
|B2C|Resource|Resource|Zelf-gecontroleerde claims voor lokale accounts ophalen|
|B2C|Autorisatie|Autorisatie|Zelf-gecontroleerde claims voor lokale accounts ophalen|
|B2C|Resource|Resource|Gelokaliseerde resource-json ophalen|
|B2C|Autorisatie|Autorisatie|Gelokaliseerde resource-json ophalen|
|B2C|Autorisatie|Autorisatie|Bewerkingen van de resourceprovider Microsoft.AzureActiveDirectory ophalen|
|B2C|Resource|Resource|Bewerkingen van de resourceprovider Microsoft.AzureActiveDirectory ophalen|
|B2C|Resource|Resource|Beleidsregels ophalen|
|B2C|Autorisatie|Autorisatie|Beleidsregels ophalen|
|B2C|Resource|Resource|Beleid ophalen|
|B2C|Autorisatie|Autorisatie|Beleid ophalen|
|B2C|Mapbeheer|Directory|Resource-eigenschappen van een tenant ophalen|
|B2C|Autorisatie|Autorisatie|Resource-eigenschappen van een tenant ophalen|
|B2C|Resource|Resource|Lijst met ondersteunde IDP's ophalen|
|B2C|Autorisatie|Autorisatie|Lijst met ondersteunde IDP's ophalen|
|B2C|Resource|Resource|Lijst met ondersteunde IDP's van de gebruikersbeleving ophalen|
|B2C|Autorisatie|Autorisatie|Lijst met ondersteunde IDP's van de gebruikersbeleving ophalen|
|B2C|Mapbeheer|Directory|Tenantgegevens ophalen|
|B2C|Autorisatie|Autorisatie|Tenantgegevens ophalen|
|B2C|Mapbeheer|Directory|Voor tenant toegestane functies ophalen|
|B2C|Autorisatie|Autorisatie|Voor tenant toegestane functies ophalen|
|B2C|Autorisatie|Autorisatie|Voor tenant gedefinieerde, aangepaste IDP-lijst ophalen|
|B2C|Resource|Resource|Voor tenant gedefinieerde, aangepaste IDP-lijst ophalen|
|B2C|Resource|Resource|Voor tenant gedefinieerde IDP-lijst ophalen|
|B2C|Autorisatie|Autorisatie|Voor tenant gedefinieerde IDP-lijst ophalen|
|B2C|Resource|Resource|Voor tenant gedefinieerde, lokale IDP-lijst ophalen|
|B2C|Autorisatie|Autorisatie|Voor tenant gedefinieerde, lokale IDP-lijst ophalen|
|B2C|Resource|Resource|Tenantgegevens voor een gebruiker ophalen voor het maken van resources|
|B2C|Autorisatie|Autorisatie|Tenantgegevens voor een gebruiker ophalen voor het maken van resources|
|B2C|Autorisatie|Autorisatie|Tenantlijst ophalen|
|B2C|Autorisatie|Autorisatie|TenantDomains ophalen|
|B2C|Mapbeheer|Directory|TenantDomains ophalen|
|B2C|Resource|Resource|De standaard ondersteunde cultuur voor CPIM ophalen|
|B2C|Autorisatie|Autorisatie|De standaard ondersteunde cultuur voor CPIM ophalen|
|B2C|Resource|Resource|De gegevens van een beheerstroom ophalen|
|B2C|Autorisatie|Autorisatie|De gegevens van een beheerstroom ophalen|
|B2C|Autorisatie|Autorisatie|De lijst met UserJourneys voor deze tenant ophalen|
|B2C|Resource|Resource|De lijst met UserJourneys voor deze tenant ophalen|
|B2C|Autorisatie|Autorisatie|De set beschikbare ondersteunde culturen voor CPIM ophalen|
|B2C|Resource|Resource|De set beschikbare ondersteunde culturen voor CPIM ophalen|
|B2C|Autorisatie|Autorisatie|TrustFramework-beleid ophalen|
|B2C|Resource|Resource|TrustFramework-beleid ophalen|
|B2C|Autorisatie|Autorisatie|TrustFramework-beleid als xml ophalen|
|B2C|Resource|Resource|TrustFramework-beleid als xml ophalen|
|B2C|Resource|Resource|Gebruikerskenmerk ophalen|
|B2C|Autorisatie|Autorisatie|Gebruikerskenmerk ophalen|
|B2C|Autorisatie|Autorisatie|Gebruikerskenmerken ophalen|
|B2C|Resource|Resource|Gebruikerskenmerken ophalen|
|B2C|Autorisatie|Autorisatie|Lijst met gebruikersbelevingen ophalen|
|B2C|Resource|Resource|Lijst met gebruikersbelevingen ophalen|
|B2C|Autorisatie|Autorisatie|GetIEFPolicies|
|B2C|Resource|Resource|GetIEFPolicies|
|B2C|Autorisatie|Autorisatie|GetIdentityProviders|
|B2C|Resource|Resource|GetIdentityProviders|
|B2C|Resource|Resource|GetTrustFrameworkPolicy|
|B2C|Autorisatie|Autorisatie|GetTrustFrameworkPolicy|
|B2C|Sleutel|Sleutel|Hiermee haalt u een CPIM-sleutelcontainer in jwk-indeling op|
|B2C|Autorisatie|Autorisatie|Hiermee haalt u een CPIM-sleutelcontainer in jwk-indeling op|
|B2C|Sleutel|Sleutel|Hiermee haalt u een lijst met sleutelcontainers in de tenant op|
|B2C|Autorisatie|Autorisatie|Hiermee haalt u een lijst met sleutelcontainers in de tenant op|
|B2C|Autorisatie|Autorisatie|Hiermee wordt het type tenant opgehaald|
|B2C|Mapbeheer|Directory|Hiermee wordt het type tenant opgehaald|
|B2C|Verificatie|Verificatie|Een toegangstoken voor de toepassing uitgeven|
|B2C|Verificatie|Verificatie|Een autorisatiecode aan de toepassing uitgeven|
|B2C|Overige|Overige|Een autorisatiecode aan de toepassing uitgeven|
|B2C|Verificatie|Verificatie|Een id_token voor de toepassing uitgeven|
|B2C|Overige|Overige|Een id_token voor de toepassing uitgeven|
|B2C|Autorisatie|Autorisatie|MigrateTenantMetadata|
|B2C|Resource|Resource|MigrateTenantMetadata|
|B2C|Resource|Resource|Resources verplaatsen|
|B2C|Autorisatie|Autorisatie|IdentityProvider patchen|
|B2C|Resource|Resource|IdentityProvider patchen|
|B2C|Resource|Resource|PutTrustFrameworkPolicy|
|B2C|Autorisatie|Autorisatie|PutTrustFrameworkPolicy|
|B2C|Autorisatie|Autorisatie|PutTrustFrameworkpolicy|
|B2C|Resource|Resource|PutTrustFrameworkpolicy|
|B2C|Resource|Resource|Een gebruikersbeleving verwijderen|
|B2C|Autorisatie|Autorisatie|Een gebruikersbeleving verwijderen|
|B2C|Autorisatie|Autorisatie|Een back-up van een CPIM-sleutelcontainer herstellen|
|B2C|Sleutel|Sleutel|Een back-up van een CPIM-sleutelcontainer herstellen|
|B2C|Beheer van toepassingen|Toepassing|V2-toepassingsmachtigingen ophalen|
|B2C|Autorisatie|Autorisatie|V2-toepassingsmachtigingen ophalen|
|B2C|Beheer van toepassingen|Toepassing|V2-toepassingsservice-principals in de huidige tenant ophalen|
|B2C|Autorisatie|Autorisatie|V2-toepassingsservice-principals in de huidige tenant ophalen|
|B2C|Sleutel|Sleutel|Sleutelcontainer opslaan|
|B2C|Autorisatie|Autorisatie|Aangepaste IDP bijwerken|
|B2C|Resource|Resource|Aangepaste IDP bijwerken|
|B2C|Resource|Resource|IDP bijwerken|
|B2C|Autorisatie|Autorisatie|IDP bijwerken|
|B2C|Resource|Resource|Lokale IDP bijwerken|
|B2C|Autorisatie|Autorisatie|Lokale IDP bijwerken|
|B2C|Beheer van toepassingen|Toepassing|V1-toepassing bijwerken|
|B2C|Autorisatie|Autorisatie|V1-toepassing bijwerken|
|B2C|Beheer van toepassingen|Toepassing|V2-toepassing bijwerken|
|B2C|Autorisatie|Autorisatie|V2-toepassing bijwerken|
|B2C|Beheer van toepassingen|Toepassing|V2-toepassingsmachtiging bijwerken|
|B2C|Autorisatie|Autorisatie|V2-toepassingsmachtiging bijwerken|
|B2C|Resource|Resource|Een B2C-mapresource bijwerken|
|B2C|Resource|Resource|Beleid bijwerken|
|B2C|Autorisatie|Autorisatie|Beleid bijwerken|
|B2C|Resource|Resource|Abonnementstatus bijwerken|
|B2C|Resource|Resource|Gebruikerskenmerk bijwerken|
|B2C|Autorisatie|Autorisatie|Gebruikerskenmerk bijwerken|
|B2C|Sleutel|Sleutel|Een versleutelde CPIM-sleutel uploaden|
|B2C|Autorisatie|Autorisatie|Een versleutelde CPIM-sleutel uploaden|
|B2C|Autorisatie|Autorisatie|Gebruikersautorisatie: API is uitgeschakeld voor de tenantfunctieset|
|B2C|Autorisatie|Autorisatie|Gebruikersautorisatie: Gebruiker toegang verleend als 'tenantbeheerder'|
|B2C|Autorisatie|Autorisatie|Gebruikersautorisatie: De gebruiker heeft toegangsrechten voor 'Geverifieerde gebruikers' gekregen|
|B2C|Verificatie|Verificatie|De referenties voor lokale accounts valideren|
|B2C|Resource|Resource|Resources verplaatsen valideren|
|B2C|Verificatie|Verificatie|Verificatie van de gebruiker valideren|
|B2C|Mapbeheer|Directory|Controleren of de B2C-functie is ingeschakeld|
|B2C|Autorisatie|Autorisatie|Controleren of de B2C-functie is ingeschakeld|
|B2C|Autorisatie|Autorisatie|Controleren of de functie is ingeschakeld|
|B2C|Mapbeheer|Directory|Controleren of de functie is ingeschakeld|
|Hoofddirectory|Beheer van toepassingen|Toepassing|OAuth2PermissionGrant toevoegen|
|Hoofddirectory|Administratieve eenheden beheren|AdministrativeUnit|Administratieve eenheid toevoegen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|App-roltoewijzing aan gebruiker toevoegen|
|Hoofddirectory|Groepsbeheer|Groep|App-roltoewijzing aan groep toevoegen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|App-roltoewijzing aan service-principal toevoegen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Toepassing toevoegen|
|Hoofddirectory|Resource|Resource|Apparaat toevoegen|
|Hoofddirectory|Resource|Resource|Apparaatconfiguratie toevoegen|
|Hoofddirectory|Rolbeheer|Rol|In aanmerking komend lid aan rol toevoegen|
|Hoofddirectory|Groepsbeheer|Groep|Groep toevoegen|
|Hoofddirectory|Administratieve eenheden beheren|AdministrativeUnit|Lid aan administratieve eenheid toevoegen|
|Hoofddirectory|Groepsbeheer|Groep|Lid aan groep toevoegen|
|Hoofddirectory|Rolbeheer|Rol|Lid aan rol toevoegen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Eigenaar aan toepassing toevoegen|
|Hoofddirectory|Groepsbeheer|Groep|Eigenaar aan groep toevoegen|
|Hoofddirectory|Beleidsbeheer|Beleid|Eigenaar aan beleid toevoegen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Eigenaar aan service-principal toevoegen|
|Hoofddirectory|Mapbeheer|Directory|Partner aan bedrijf toevoegen|
|Hoofddirectory|Beleidsbeheer|Beleid|Beleid toevoegen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Beleid aan service-principal toevoegen|
|Hoofddirectory|Resource|Resource|Geregistreerde eigenaar aan apparaat toevoegen|
|Hoofddirectory|Resource|Resource|Geregistreerde gebruikers aan apparaat toevoegen|
|Hoofddirectory|Rolbeheer|Rol|Roltoewijzing aan roldefinitie toevoegen|
|Hoofddirectory|Rolbeheer|Rol|Rol vanuit sjabloon toevoegen|
|Hoofddirectory|Rolbeheer|Rol|Scoped lid aan rol toevoegen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Service-principal toevoegen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Referenties voor service-principal toevoegen|
|Hoofddirectory|Mapbeheer|Directory|Niet-geverifieerd domein toevoegen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gebruiker toevoegen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gegevens voor telefoon-app voor sterke verificatie van gebruikers toevoegen|
|Hoofddirectory|Mapbeheer|Directory|Geverifieerd domein toevoegen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gebruikerslicentie wijzigen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gebruikerswachtwoord wijzigen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Instemmen met toepassing|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Federatieve gebruiker naar beheerd converteren|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Toepassingswachtwoord voor gebruiker maken|
|Hoofddirectory|Mapbeheer|Directory|Bedrijf maken|
|Hoofddirectory|Mapbeheer|Directory|Bedrijfsinstellingen maken|
|Hoofddirectory|Groepsbeheer|Groep|Groepsinstellingen maken|
|Hoofddirectory|Administratieve eenheden beheren|AdministrativeUnit|Administratieve eenheid verwijderen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Toepassing verwijderen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Toepassingswachtwoord voor gebruiker verwijderen|
|Hoofddirectory|Mapbeheer|Directory|Bedrijfsinstellingen verwijderen|
|Hoofddirectory|Resource|Resource|Apparaat verwijderen|
|Hoofddirectory|Resource|Resource|Apparaatconfiguratie verwijderen|
|Hoofddirectory|Groepsbeheer|Groep|Groep verwijderen|
|Hoofddirectory|Groepsbeheer|Groep|Groepsinstellingen verwijderen|
|Hoofddirectory|Beleidsbeheer|Beleid|Beleid verwijderen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gebruiker verwijderen|
|Hoofddirectory|Mapbeheer|Directory|Niveau van partner verlagen|
|Hoofddirectory|Resource|Resource|Apparaat niet meer conform|
|Hoofddirectory|Resource|Resource|Apparaat wordt niet meer beheerd|
|Hoofddirectory|Mapbeheer|Directory|Map verwijderd|
|Hoofddirectory|Mapbeheer|Directory|Map permanent verwijderd|
|Hoofddirectory|Mapbeheer|Directory|Map staat gepland voor verwijdering|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Account uitschakelen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Sterke verificatie inschakelen|
|Hoofddirectory|Groepsbeheer|Groep|Het toepassen van een groepslicentie op gebruikers voltooien|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Toepassing definitief verwijderen|
|Hoofddirectory|Groepsbeheer|Groep|Groep definitief verwijderen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gebruiker definitief verwijderen|
|Hoofddirectory|Mapbeheer|Directory|Niveau van bedrijf verhogen tot partner|
|Hoofddirectory|Mapbeheer|Directory|Rights Management-eigenschappen leegmaken|
|Hoofddirectory|Beheer van toepassingen|Toepassing|OAuth2PermissionGrant verwijderen|
|Hoofddirectory|Groepsbeheer|Groep|App-roltoewijzing uit groep verwijderen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|App-roltoewijzing uit service-principal verwijderen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|App-roltoewijzing uit gebruiker verwijderen|
|Hoofddirectory|Rolbeheer|Rol|In aanmerking komen lid uit rol verwijderen|
|Hoofddirectory|Administratieve eenheden beheren|AdministrativeUnit|Lid uit administratieve eenheid verwijderen|
|Hoofddirectory|Groepsbeheer|Groep|Lid uit groep verwijderen|
|Hoofddirectory|Rolbeheer|Rol|Lid uit rol verwijderen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Eigenaar uit toepassing verwijderen|
|Hoofddirectory|Groepsbeheer|Groep|Eigenaar uit groep verwijderen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Eigenaar uit service-principal verwijderen|
|Hoofddirectory|Mapbeheer|Directory|Partner uit bedrijf verwijderen|
|Hoofddirectory|Beleidsbeheer|Beleid|Beleidsreferenties verwijderen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Beleid uit service-principal verwijderen|
|Hoofddirectory|Resource|Resource|Geregistreerde eigenaar van apparaat verwijderen|
|Hoofddirectory|Resource|Resource|Geregistreerde gebruikers van apparaat verwijderen|
|Hoofddirectory|Rolbeheer|Rol|Roltoewijzing uit roldefinitie verwijderen|
|Hoofddirectory|Rolbeheer|Rol|Scoped lid uit rol verwijderen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Service-principal verwijderen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Referenties voor service-principal verwijderen|
|Hoofddirectory|Mapbeheer|Directory|Niet-geverifieerd domein verwijderen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gegevens voor telefoon-app voor sterke verificatie van gebruikers verwijderen|
|Hoofddirectory|Mapbeheer|Directory|Geverifieerd domein verwijderen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gebruikerswachtwoord opnieuw instellen|
|Hoofddirectory|Groepsbeheer|Groep|Groep herstellen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Toepassing herstellen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gebruiker herstellen|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Toestemming intrekken|
|Hoofddirectory|Mapbeheer|Directory|Bedrijfsgegevens instellen|
|Hoofddirectory|Mapbeheer|Directory|DirSync-functie instellen|
|Hoofddirectory|Mapbeheer|Directory|DirSyncEnabled-vlag instellen|
|Hoofddirectory|Mapbeheer|Directory|Partnerschap instellen|
|Hoofddirectory|Mapbeheer|Directory|Drempelwaarde voor onopzettelijk verwijderen instellen|
|Hoofddirectory|Mapbeheer|Directory|Door het bedrijf toegestane gegevenslocatie instellen|
|Hoofddirectory|Mapbeheer|Directory|Multinationale functie inschakelen|
|Hoofddirectory|Mapbeheer|Directory|Mapfunctie voor tenant instellen|
|Hoofddirectory|Mapbeheer|Directory|Domeinverificatie instellen|
|Hoofddirectory|Mapbeheer|Directory|Federatie-instellingen voor domein instellen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Geforceerd wijzigen van gebruikerswachtwoord instellen|
|Hoofddirectory|Groepsbeheer|Groep|Groepslicentie instellen|
|Hoofddirectory|Groepsbeheer|Groep|Groep instellen op beheer door gebruiker|
|Hoofddirectory|Mapbeheer|Directory|Wachtwoordbeleid instellen|
|Hoofddirectory|Mapbeheer|Directory|Rights Management-eigenschappen instellen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gebruikersbeheerder instellen|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Instellen dat oath-token-metagegevens voor gebruikers zijn ingeschakeld|
|Hoofddirectory|Groepsbeheer|Groep|Starten met het toepassen van een groepslicentie op gebruikers|
|Hoofddirectory|Groepsbeheer|Groep|Herberekening van groepslicentie activeren|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Tijdstempel StsRefreshTokenValidFrom bijwerken|
|Hoofddirectory|Administratieve eenheden beheren|AdministrativeUnit|Administratieve eenheid bijwerken|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Toepassing bijwerken|
|Hoofddirectory|Mapbeheer|Directory|Bedrijf bijwerken|
|Hoofddirectory|Mapbeheer|Directory|Bedrijfsinstellingen bijwerken|
|Hoofddirectory|Resource|Resource|Apparaat bijwerken|
|Hoofddirectory|Resource|Resource|Apparaatconfiguratie bijwerken|
|Hoofddirectory|Mapbeheer|Directory|Domein bijwerken|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Externe geheimen bijwerken|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Externe geheimen bijwerken|
|Hoofddirectory|Groepsbeheer|Groep|Groep bijwerken|
|Hoofddirectory|Groepsbeheer|Groep|Groepsinstellingen bijwerken|
|Hoofddirectory|Beleidsbeheer|Beleid|Beleid bijwerken|
|Hoofddirectory|Rolbeheer|Rol|Rol bijwerken|
|Hoofddirectory|Beheer van toepassingen|Toepassing|Service-principal bijwerken|
|Hoofddirectory|Gebruikersbeheer|Gebruiker|Gebruiker bijwerken|
|Hoofddirectory|Mapbeheer|Directory|Domein verifiëren|
|Hoofddirectory|Mapbeheer|Directory|Door e-mail geverifieerd domein verifiëren|
|Identiteitsbeveiliging|Gebruikersbeheer|Gebruiker|Beheerder genereert een tijdelijk wachtwoord|
|Identiteitsbeveiliging|Gebruikersbeheer|Gebruiker|Beheerders vereisen dat de gebruiker het wachtwoord opnieuw instelt|
|Identiteitsbeveiliging|Overige|Overige|Eén type risicogebeurtenis downloaden|
|Identiteitsbeveiliging|Overige|Overige|Beheerders en status van aanmelding voor wekelijkse samenvatting downloaden|
|Identiteitsbeveiliging|Overige|Overige|Alle risicogebeurtenistypen downloaden|
|Identiteitsbeveiliging|Overige|Overige|Risicogebeurtenissen voor gratis gebruikers downloaden|
|Identiteitsbeveiliging|Overige|Overige|Gebruikers voor wie wordt aangegeven dat ze risico lopen downloaden|
|Identiteitsbeveiliging|Mapbeheer|Directory|Onboarding|
|Identiteitsbeveiliging|Beleidsbeheer|Beleid|Beleid voor MFA-registratie instellen|
|Identiteitsbeveiliging|Beleidsbeheer|Beleid|Beleid voor aanmeldingsrisico's instellen|
|Identiteitsbeveiliging|Beleidsbeheer|Beleid|Beleid voor gebruikersrisico's instellen|
|Identiteitsbeveiliging|Mapbeheer|Directory|Waarschuwingsinstellingen bijwerken|
|Identiteitsbeveiliging|Mapbeheer|Directory|Instellingen voor wekelijkse samenvatting bijwerken|
|Uitgenodigde gebruikers|Gebruikersbeheer|Gebruiker|Externe gebruiker aan toepassing toewijzen|
|Uitgenodigde gebruikers|Overige|Overige|Verwerkte batchuitnodigingen|
|Uitgenodigde gebruikers|Overige|Overige|Geüploade batchuitnodigingen|
|Uitgenodigde gebruikers|Gebruikersbeheer|Gebruiker|E-mail niet verzonden, gebruiker heeft zich afgemeld|
|Uitgenodigde gebruikers|Gebruikersbeheer|Gebruiker|Externe gebruiker uitnodigen|
|Uitgenodigde gebruikers|Gebruikersbeheer|Gebruiker|Uitnodiging externe gebruiker inwisselen|
|Uitgenodigde gebruikers|Gebruikersbeheer|Gebruiker|Virale tenant maken|
|Uitgenodigde gebruikers|Gebruikersbeheer|Gebruiker|Virale gebruiker maken|
|Microsoft Identity Manager (MIM)|Groepsbeheer|Groep|Lid toevoegen|
|Microsoft Identity Manager (MIM)|Groepsbeheer|Groep|Groep maken|
|Microsoft Identity Manager (MIM)|Groepsbeheer|Groep|Groep verwijderen|
|Microsoft Identity Manager (MIM)|Groepsbeheer|Groep|Lid verwijderen|
|Microsoft Identity Manager (MIM)|Groepsbeheer|Groep|Groep bijwerken|
|Microsoft Identity Manager (MIM)|Gebruikersbeheer|Gebruiker|Gebruikerswachtwoordregistratie|
|Microsoft Identity Manager (MIM)|Gebruikersbeheer|Gebruiker|Gebruikerswachtwoordherstel|
|Privileged Identity Management|Rolbeheer|Rol|AccessReview_Review|
|Privileged Identity Management|Rolbeheer|Rol|AccessReview_Update|
|Privileged Identity Management|Rolbeheer|Rol|ActivationAborted|
|Privileged Identity Management|Rolbeheer|Rol|ActivationApproved|
|Privileged Identity Management|Rolbeheer|Rol|ActivationCanceled|
|Privileged Identity Management|Rolbeheer|Rol|ActivationRequested|
|Privileged Identity Management|Rolbeheer|Rol|Toegevoegd|
|Privileged Identity Management|Rolbeheer|Rol|Toewijzen|
|Privileged Identity Management|Rolbeheer|Rol|Verhogen|
|Privileged Identity Management|Rolbeheer|Rol|Verwijderd|
|Privileged Identity Management|Rolbeheer|Rol|Wijzigingen van de rolinstelling|
|Privileged Identity Management|Rolbeheer|Rol|ScanAlertsNow|
|Privileged Identity Management|Rolbeheer|Rol|Registreren|
|Privileged Identity Management|Rolbeheer|Rol|Bevoegdheden verlagen|
|Privileged Identity Management|Rolbeheer|Rol|UpdateAlertSettings|
|Privileged Identity Management|Rolbeheer|Rol|UpdateCurrentState|
|Self-service voor groepsbeheer|Groepsbeheer|Groep|Aanvraag in behandeling voor deelname aan een groep goedkeuren|
|Self-service voor groepsbeheer|Groepsbeheer|Groep|Aanvraag in behandeling voor deelname aan een groep annuleren|
|Self-service voor groepsbeheer|Groepsbeheer|Groep|Levenscyclusbeheerbeleid maken|
|Self-service voor groepsbeheer|Groepsbeheer|Groep|Aanvraag in behandeling voor deelname aan een groep verwijderen|
|Self-service voor groepsbeheer|Groepsbeheer|Groep|Aanvraag in behandeling voor deelname aan een groep weigeren|
|Self-service voor groepsbeheer|Groepsbeheer|Groep|Groep vernieuwen|
|Self-service voor groepsbeheer|Groepsbeheer|Groep|Deelname aan een groep aanvragen|
|Self-service voor groepsbeheer|Groepsbeheer|Groep|Eigenschappen voor dynamische groep instellen|
|Self-service voor groepsbeheer|Groepsbeheer|Groep|Levenscyclusbeheerbeleid bijwerken|
|Selfservice voor wachtwoordbeheer|Gebruikersbeheer|Gebruiker|Selfservice voor wachtwoord opnieuw instellen geblokkeerd|
|Selfservice voor wachtwoordbeheer|Gebruikersbeheer|Gebruiker|Wachtwoord wijzigen (selfservice)|
|Selfservice voor wachtwoordbeheer|Mapbeheer|Directory|Write-back van wachtwoord voor map uitschakelen|
|Selfservice voor wachtwoordbeheer|Mapbeheer|Directory|Write-back van wachtwoord voor map inschakelen|
|Selfservice voor wachtwoordbeheer|Gebruikersbeheer|Gebruiker|Wachtwoord opnieuw instellen (door beheerder)|
|Selfservice voor wachtwoordbeheer|Gebruikersbeheer|Gebruiker|Wachtwoord opnieuw instellen (selfservice)|
|Selfservice voor wachtwoordbeheer|Gebruikersbeheer|Gebruiker|Self-service voor stroomactiviteitvoortgang bij het opnieuw instellen van een wachtwoord|
|Selfservice voor wachtwoordbeheer|Gebruikersbeheer|Gebruiker|Self-service voor stroomactiviteitvoortgang bij het opnieuw instellen van een wachtwoord|
|Selfservice voor wachtwoordbeheer|Gebruikersbeheer|Gebruiker|Gebruikersaccount ontgrendelen (selfservice)|
|Selfservice voor wachtwoordbeheer|Gebruikersbeheer|Gebruiker|Door gebruiker geregistreerd voor selfservice voor het opnieuw instellen van een wachtwoord|
|Gebruiksvoorwaarden|Beleidsbeheer|Beleid|Gebruiksvoorwaarden accepteren|
|Gebruiksvoorwaarden|Beleidsbeheer|Beleid|Gebruiksvoorwaarden maken|
|Gebruiksvoorwaarden|Beleidsbeheer|Beleid|Gebruiksvoorwaarden afwijzen|
|Gebruiksvoorwaarden|Beleidsbeheer|Beleid|Gebruiksvoorwaarden verwijderen|
|Gebruiksvoorwaarden|Beleidsbeheer|Beleid|Gebruiksvoorwaarden bewerken|
|Gebruiksvoorwaarden|Beleidsbeheer|Beleid|Gebruiksvoorwaarden publiceren|
|Gebruiksvoorwaarden|Beleidsbeheer|Beleid|Publicatie van gebruiksvoorwaarden ongedaan maken|




## <a name="next-steps"></a>Volgende stappen

Zie [Azure Active Directory-rapportage](active-directory-reporting-azure-portal.md) voor een overzicht van de rapportage.

