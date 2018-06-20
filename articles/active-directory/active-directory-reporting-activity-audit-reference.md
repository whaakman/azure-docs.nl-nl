---
title: Referentie voor auditactiviteiten van Azure Active Directory (Azure AD) | Microsoft Docs
description: Overzicht van de auditactiviteiten die kunnen worden geregistreerd in de auditlogboeken van Azure Active Directory (Azure AD).
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 04/19/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: e8959d93d6cc6085e1bb740ab6d5ea07c2ef57a2
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36224518"
---
# <a name="azure-ad-audit-activity-reference"></a>Referentie voor auditactiviteiten van Azure AD

Met rapporten in Azure Active Directory ontvangt u alle informatie die nodig is om te bepalen hoe het gaat met uw omgeving.

De rapportstructuur in Azure AD bestaat uit de volgende onderdelen:

- **Activiteit** 
    - **Aanmeldactiviteiten**: informatie over het gebruik van beheerde toepassingen en aanmeldactiviteiten van gebruikers
    - **Auditlogboeken**: traceerbaarheid via logboeken voor alle door diverse functies binnen Azure AD uitgevoerde wijzigingen. Voorbeelden van auditlogboekgegevens zijn wijzigingen die worden aangebracht in resources binnen Azure AD, zoals gebruikers, apps, groepen, rollen, beleid, verificaties, enzovoort.
- **Beveiliging** 
    - **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. Zie Riskante aanmeldingen voor meer informatie.
    - **Gebruikers van wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. Zie Gebruikers van wie wordt aangegeven dat ze risico lopen voor meer informatie.

In dit artikel worden de auditactiviteiten vermeld die in uw auditlogboeken kunnen worden geregistreerd.
 


## <a name="access-reviews"></a>Toegangsbeoordelingen

|Controlecategorie|Activiteit|
|---|---|
|Mapbeheer|Administratieve eenheid toevoegen|
|Mapbeheer|Lid aan administratieve eenheid toevoegen|
|Mapbeheer|Administratieve eenheid verwijderen|
|Mapbeheer|Lid uit administratieve eenheid verwijderen|
|Mapbeheer|Administratieve eenheid bijwerken|
|Mapbeheer|Beheer|
|Gebruikersbeheer|Directory-bewerking|
|Gebruikersbeheer|Exporteren|
|Gebruikersbeheer|Importeren|
|Gebruikersbeheer|Overige|
|Gebruikersbeheer|Proces-escrow|
|Gebruikersbeheer|Synchronisatieregelactie|
|Gebruikersbeheer|Toepassing toevoegen|
|Gebruikersbeheer|Toepassing verwijderen|
|Gebruikersbeheer|Toepassing bijwerken|
|Gebruikersbeheer|Modus voor eenmalige aanmelding voor toepassingen bijwerken|
|Gebruikersbeheer|Automatische wachtwoordoverschakeling|
|Gebruikersbeheer|V2-toepassingsmachtigingen toevoegen|
|Gebruikersbeheer|V1-toepassing maken|
|Gebruikersbeheer|V2-toepassing maken|
|Gebruikersbeheer|V1-toepassing verwijderen|
|Gebruikersbeheer|V2-toepassing verwijderen|
|Gebruikersbeheer|V2-toepassingsmachtiging verwijderen|
|Gebruikersbeheer|V1- en V2-toepassingen ophalen|
|Gebruikersbeheer|V1-toepassing ophalen|
|Gebruikersbeheer|V1-toepassingen ophalen|
|Gebruikersbeheer|V2-toepassing ophalen|
|Gebruikersbeheer|V2-toepassingen ophalen|




## <a name="account-provisioning"></a>Account inrichten

|Controlecategorie|Activiteit|
|---|---|
|Beheer van toepassingen|V2-toepassingsmachtigingen ophalen|
|Beheer van toepassingen|V2-toepassingsservice-principals in de huidige tenant ophalen|
|Beheer van toepassingen|V1-toepassing bijwerken|
|Beheer van toepassingen|V2-toepassing bijwerken|
|Beheer van toepassingen|V2-toepassingsmachtiging bijwerken|
|Beheer van toepassingen|OAuth2PermissionGrant toevoegen|
|Beheer van toepassingen|App-roltoewijzing aan service-principal toevoegen|

## <a name="application-proxy"></a>Toepassingsproxy

|Controlecategorie|Activiteit|
|---|---|
|Beheer van toepassingen|Toepassing toevoegen|
|Beheer van toepassingen|Eigenaar aan toepassing toevoegen|
|Beheer van toepassingen|Eigenaar aan service-principal toevoegen|
|Beheer van toepassingen|Beleid aan service-principal toevoegen|
|Mapbeheer|Service-principal toevoegen|
|Mapbeheer|Referenties voor service-principal toevoegen|
|Mapbeheer|Instemmen met toepassing|
|Mapbeheer|Toepassing verwijderen|
|Mapbeheer|Toepassing definitief verwijderen|
|Mapbeheer|OAuth2PermissionGrant verwijderen|
|Mapbeheer|App-roltoewijzing uit service-principal verwijderen|
|Mapbeheer|Eigenaar uit toepassing verwijderen|
|Resource|Eigenaar uit service-principal verwijderen|
|Resource|Beleid uit service-principal verwijderen|
|Resource|Service-principal verwijderen|


## <a name="automated-password-rollover"></a>Automatische wachtwoordoverschakeling

|Controlecategorie|Activiteit|
|---|---|
|Beheer van toepassingen|Referenties voor service-principal verwijderen|


## <a name="b2c"></a>B2C

|Controlecategorie|Activiteit|
|---|---|
|Beheer van toepassingen|Toepassing herstellen|
|Beheer van toepassingen|Toestemming intrekken|
|Beheer van toepassingen|Toepassing bijwerken|
|Beheer van toepassingen|Externe geheimen bijwerken|
|Beheer van toepassingen|Service-principal bijwerken|
|Beheer van toepassingen|Een toegangstoken voor de toepassing uitgeven|
|Beheer van toepassingen|Een autorisatiecode aan de toepassing uitgeven|
|Beheer van toepassingen|Een id_token voor de toepassing uitgeven|
|Beheer van toepassingen|De referenties voor lokale accounts valideren|
|Beheer van toepassingen|Verificatie van de gebruiker valideren|
|Beheer van toepassingen|V2-toepassingsmachtigingen toevoegen|
|Beheer van toepassingen|Een sleutel op basis van een ASCII-geheim aan een CPIM-sleutelcontainer toevoegen|
|Beheer van toepassingen|Een sleutel aan een CPIM-sleutelcontainer toevoegen|
|Beheer van toepassingen|AdminPolicyDatas-SetResources|
|Beheer van toepassingen|AdminUserJourneys-GetResources|
|Beheer van toepassingen|AdminUserJourneys-RemoveResources|
|Verificatie|AdminUserJourneys-SetResources|
|Verificatie|IdentityProvider maken|
|Verificatie|V1-toepassing maken|
|Verificatie|V2-toepassing maken|
|Verificatie|Aangepaste domeinen in de tenant maken|
|Autorisatie|Een nieuwe AdminUserJourney maken|
|Autorisatie|Gelokaliseerde resource-json maken|
|Autorisatie|Nieuwe aangepaste IDP maken|
|Autorisatie|Nieuwe IDP maken|
|Autorisatie|Een B2C-mapresource maken of bijwerken|
|Autorisatie|Beleid maken|
|Autorisatie|TrustFramework-beleid maken|
|Autorisatie|TrustFramework-beleid maken met configureerbaar voorvoegsel|
|Autorisatie|Gebruikerskenmerk maken|
|Autorisatie|CreateTrustFrameworkPolicy|
|Autorisatie|Een nieuwe AdminUserJourney maken of bijwerken|
|Autorisatie|IDP verwijderen|
|Autorisatie|IdentityProvider verwijderen|
|Autorisatie|V1-toepassing verwijderen|
|Autorisatie|V2-toepassing verwijderen|
|Autorisatie|V2-toepassingsmachtiging verwijderen|
|Autorisatie|Een B2C-mapresource verwijderen|
|Autorisatie|Een CPIM-sleutelcontainer verwijderen|
|Autorisatie|TrustFramework-beleid verwijderen|
|Autorisatie|Gebruikerskenmerk verwijderen|
|Autorisatie|B2C-functie inschakelen|
|Autorisatie|B2C-mapresources in een abonnement ophalen|
|Autorisatie|Aangepaste IDP ophalen|
|Autorisatie|IDP ophalen|
|Autorisatie|V1- en V2-toepassingen ophalen|
|Autorisatie|V1-toepassing ophalen|
|Autorisatie|V1-toepassingen ophalen|
|Autorisatie|V2-toepassing ophalen|
|Autorisatie|V2-toepassingen ophalen|
|Autorisatie|Een B2C-mapresource ophalen|
|Autorisatie|Een lijst met aangepaste domeinen in de tenant ophalen|
|Autorisatie|Een gebruikersbeleving ophalen|
|Autorisatie|Toegestane toepassingsclaims voor gebruikersbeleving ophalen|
|Autorisatie|Toegestane zelf-gecontroleerde claims voor gebruikersbeleving ophalen|
|Autorisatie|Toegestane zelf-gecontroleerde claims voor beleid ophalen|
|Autorisatie|Lijst met beschikbare uitvoerclaims ophalen|
|Autorisatie|Inhouddefinities voor gebruikersbeleving ophalen|
|Autorisatie|IDP's voor een specifieke beheerstroom ophalen|
|Autorisatie|Actieve sleutelmetagegevens voor sleutelcontainer in JWK ophalen|
|Autorisatie|Lijst van alle beheerstromen ophalen|
|Autorisatie|Lijst met labels voor alle beheerstromen voor alle gebruikers ophalen|
|Autorisatie|Lijst met tenants voor een gebruiker ophalen|
|Autorisatie|Zelf-gecontroleerde claims voor lokale accounts ophalen|
|Autorisatie|Gelokaliseerde resource-json ophalen|
|Autorisatie|Bewerkingen van de resourceprovider Microsoft.AzureActiveDirectory ophalen|
|Autorisatie|Beleidsregels ophalen|
|Autorisatie|Beleid ophalen|
|Autorisatie|Resource-eigenschappen van een tenant ophalen|
|Autorisatie|Lijst met ondersteunde IDP's ophalen|
|Autorisatie|Lijst met ondersteunde IDP's van de gebruikersbeleving ophalen|
|Autorisatie|Tenantgegevens ophalen|
|Autorisatie|Voor tenant toegestane functies ophalen|
|Autorisatie|Voor tenant gedefinieerde, aangepaste IDP-lijst ophalen|
|Autorisatie|Voor tenant gedefinieerde IDP-lijst ophalen|
|Autorisatie|Voor tenant gedefinieerde, lokale IDP-lijst ophalen|
|Autorisatie|Tenantgegevens voor een gebruiker ophalen voor het maken van resources|
|Autorisatie|Tenantlijst ophalen|
|Autorisatie|TenantDomains ophalen|
|Autorisatie|De standaard ondersteunde cultuur voor CPIM ophalen|
|Autorisatie|De gegevens van een beheerstroom ophalen|
|Autorisatie|De lijst met UserJourneys voor deze tenant ophalen|
|Autorisatie|De set beschikbare ondersteunde culturen voor CPIM ophalen|
|Autorisatie|TrustFramework-beleid ophalen|
|Autorisatie|TrustFramework-beleid als xml ophalen|
|Autorisatie|Gebruikerskenmerk ophalen|
|Autorisatie|Gebruikerskenmerken ophalen|
|Autorisatie|Lijst met gebruikersbelevingen ophalen|
|Autorisatie|GetIEFPolicies|
|Autorisatie|GetIdentityProviders|
|Autorisatie|GetTrustFrameworkPolicy|
|Autorisatie|Hiermee haalt u een CPIM-sleutelcontainer in jwk-indeling op|
|Autorisatie|Hiermee haalt u een lijst met sleutelcontainers in de tenant op|
|Autorisatie|Hiermee wordt het type tenant opgehaald|
|Autorisatie|MigrateTenantMetadata|
|Autorisatie|IdentityProvider patchen|
|Autorisatie|PutTrustFrameworkPolicy|
|Autorisatie|PutTrustFrameworkpolicy|
|Autorisatie|Een gebruikersbeleving verwijderen|
|Autorisatie|Een back-up van een CPIM-sleutelcontainer herstellen|
|Autorisatie|V2-toepassingsmachtigingen ophalen|
|Autorisatie|V2-toepassingsservice-principals in de huidige tenant ophalen|
|Autorisatie|Aangepaste IDP bijwerken|
|Autorisatie|IDP bijwerken|
|Autorisatie|Lokale IDP bijwerken|
|Autorisatie|V1-toepassing bijwerken|
|Autorisatie|V2-toepassing bijwerken|
|Autorisatie|V2-toepassingsmachtiging bijwerken|
|Autorisatie|Beleid bijwerken|
|Autorisatie|Gebruikerskenmerk bijwerken|
|Autorisatie|Een versleutelde CPIM-sleutel uploaden|
|Autorisatie|Gebruikersautorisatie: API is uitgeschakeld voor de tenantfunctieset|
|Autorisatie|Gebruikersautorisatie: Gebruiker toegang verleend als 'tenantbeheerder'|
|Autorisatie|Gebruikersautorisatie: De gebruiker heeft toegangsrechten voor 'Geverifieerde gebruikers' gekregen|
|Autorisatie|Controleren of de B2C-functie is ingeschakeld|
|Autorisatie|Controleren of de functie is ingeschakeld|
|Autorisatie|Programma maken|
|Autorisatie|Programma verwijderen|
|Autorisatie|Programmabeheer koppelen|
|Autorisatie|Onboarding bij Azure AD-toegangsbeoordelingen|
|Autorisatie|Programmabeheer ontkoppelen|
|Autorisatie|Programma bijwerken|
|Autorisatie|Desktop-SSO uitschakelen|
|Autorisatie|Desktop-SSO uitschakelen voor een specifiek domein|
|Autorisatie|Toepassingsproxy uitschakelen|
|Autorisatie|Passthrough-verificatie uitschakelen|
|Autorisatie|Desktop-SSO inschakelen|
|Mapbeheer|Desktop-SSO inschakelen voor een specifiek domein|
|Mapbeheer|Toepassingsproxy inschakelen|
|Mapbeheer|Passthrough-verificatie inschakelen|
|Mapbeheer|Aangepaste domeinen in de tenant maken|
|Mapbeheer|B2C-functie inschakelen|
|Mapbeheer|Een lijst met aangepaste domeinen in de tenant ophalen|
|Mapbeheer|Resource-eigenschappen van een tenant ophalen|
|Mapbeheer|Tenantgegevens ophalen|
|Mapbeheer|Voor tenant toegestane functies ophalen|
|Mapbeheer|TenantDomains ophalen|
|Sleutel|Hiermee wordt het type tenant opgehaald|
|Sleutel|Controleren of de B2C-functie is ingeschakeld|
|Sleutel|Controleren of de functie is ingeschakeld|
|Sleutel|Partner aan bedrijf toevoegen|
|Sleutel|Niet-geverifieerd domein toevoegen|
|Sleutel|Geverifieerd domein toevoegen|
|Sleutel|Bedrijf maken|
|Sleutel|Bedrijfsinstellingen maken|
|Sleutel|Bedrijfsinstellingen verwijderen|
|Sleutel|Niveau van partner verlagen|
|Sleutel|Map verwijderd|
|Overige|Map permanent verwijderd|
|Overige|Map staat gepland voor verwijdering|
|Resource|Niveau van bedrijf verhogen tot partner|
|Resource|Rights Management-eigenschappen leegmaken|
|Resource|Partner uit bedrijf verwijderen|
|Resource|Niet-geverifieerd domein verwijderen|
|Resource|Geverifieerd domein verwijderen|
|Resource|Bedrijfsgegevens instellen|
|Resource|DirSync-functie instellen|
|Resource|DirSyncEnabled-vlag instellen|
|Resource|Partnerschap instellen|
|Resource|Drempelwaarde voor onopzettelijk verwijderen instellen|
|Resource|Door het bedrijf toegestane gegevenslocatie instellen|
|Resource|Multinationale functie inschakelen|
|Resource|Mapfunctie voor tenant instellen|
|Resource|Domeinverificatie instellen|
|Resource|Federatie-instellingen voor domein instellen|
|Resource|Wachtwoordbeleid instellen|
|Resource|Rights Management-eigenschappen instellen|
|Resource|Bedrijf bijwerken|
|Resource|Bedrijfsinstellingen bijwerken|
|Resource|Domein bijwerken|
|Resource|Domein verifiëren|
|Resource|Door e-mail geverifieerd domein verifiëren|
|Resource|Onboarding|
|Resource|Waarschuwingsinstellingen bijwerken|
|Resource|Instellingen voor wekelijkse samenvatting bijwerken|
|Resource|Write-back van wachtwoord voor map uitschakelen|
|Resource|Write-back van wachtwoord voor map inschakelen|
|Resource|App-roltoewijzing aan groep toevoegen|
|Resource|Groep toevoegen|
|Resource|Lid aan groep toevoegen|
|Resource|Eigenaar aan groep toevoegen|
|Resource|Groepsinstellingen maken|
|Resource|Groep verwijderen|
|Resource|Groepsinstellingen verwijderen|
|Resource|Het toepassen van een groepslicentie op gebruikers voltooien|
|Resource|Groep definitief verwijderen|
|Resource|App-roltoewijzing uit groep verwijderen|
|Resource|Lid uit groep verwijderen|
|Resource|Eigenaar uit groep verwijderen|
|Resource|Groep herstellen|
|Resource|Groepslicentie instellen|
|Resource|Groep instellen op beheer door gebruiker|
|Resource|Starten met het toepassen van een groepslicentie op gebruikers|
|Resource|Herberekening van groepslicentie activeren|
|Resource|Groep bijwerken|
|Resource|Groepsinstellingen bijwerken|
|Resource|Lid toevoegen|
|Resource|Groep maken|
|Resource|Groep verwijderen|
|Resource|Lid verwijderen|
|Resource|Groep bijwerken|
|Resource|Aanvraag in behandeling voor deelname aan een groep goedkeuren|
|Resource|Aanvraag in behandeling voor deelname aan een groep annuleren|
|Resource|Levenscyclusbeheerbeleid maken|
|Resource|Aanvraag in behandeling voor deelname aan een groep verwijderen|
|Resource|Aanvraag in behandeling voor deelname aan een groep weigeren|
|Resource|Groep vernieuwen|
|Resource|Deelname aan een groep aanvragen|
|Resource|Eigenschappen voor dynamische groep instellen|
|Resource|Levenscyclusbeheerbeleid bijwerken|
|Resource|Een sleutel op basis van een ASCII-geheim aan een CPIM-sleutelcontainer toevoegen|
|Resource|Een sleutel aan een CPIM-sleutelcontainer toevoegen|
|Resource|Een CPIM-sleutelcontainer verwijderen|
|Resource|Een sleutelcontainer verwijderen|
|Resource|Actieve sleutelmetagegevens voor sleutelcontainer in JWK ophalen|
|Resource|Metagegevens voor sleutelcontainer ophalen|
|Resource|Hiermee haalt u een CPIM-sleutelcontainer in jwk-indeling op|
|Resource|Hiermee haalt u een lijst met sleutelcontainers in de tenant op|
|Resource|Een back-up van een CPIM-sleutelcontainer herstellen|
|Resource|Sleutelcontainer opslaan|
|Resource|Een versleutelde CPIM-sleutel uploaden|
|Resource|Een autorisatiecode aan de toepassing uitgeven|
|Resource|Een id_token voor de toepassing uitgeven|


## <a name="core-directory"></a>Hoofddirectory

|Controlecategorie|Activiteit|
|---|---|
|Administratieve eenheden beheren|Eén type risicogebeurtenis downloaden|
|Administratieve eenheden beheren|Beheerders en status van aanmelding voor wekelijkse samenvatting downloaden|
|Administratieve eenheden beheren|Alle risicogebeurtenistypen downloaden|
|Administratieve eenheden beheren|Risicogebeurtenissen voor gratis gebruikers downloaden|
|Administratieve eenheden beheren|Gebruikers voor wie wordt aangegeven dat ze risico lopen downloaden|
|Beheer van toepassingen|Verwerkte batchuitnodigingen|
|Beheer van toepassingen|Geüploade batchuitnodigingen|
|Beheer van toepassingen|Eigenaar aan beleid toevoegen|
|Beheer van toepassingen|Beleid toevoegen|
|Beheer van toepassingen|Beleid verwijderen|
|Beheer van toepassingen|Beleidsreferenties verwijderen|
|Beheer van toepassingen|Beleid bijwerken|
|Beheer van toepassingen|Beleid voor MFA-registratie instellen|
|Beheer van toepassingen|Beleid voor aanmeldingsrisico's instellen|
|Beheer van toepassingen|Beleid voor gebruikersrisico's instellen|
|Beheer van toepassingen|Gebruiksvoorwaarden accepteren|
|Beheer van toepassingen|Gebruiksvoorwaarden maken|
|Beheer van toepassingen|Gebruiksvoorwaarden afwijzen|
|Beheer van toepassingen|Gebruiksvoorwaarden verwijderen|
|Beheer van toepassingen|Gebruiksvoorwaarden bewerken|
|Beheer van toepassingen|Gebruiksvoorwaarden publiceren|
|Beheer van toepassingen|Publicatie van gebruiksvoorwaarden ongedaan maken|
|Beheer van toepassingen|SSL-certificaat voor de toepassing toevoegen|
|Beheer van toepassingen|SSL-binding verwijderen|
|Beheer van toepassingen|Connector registreren|
|Beheer van toepassingen|AdminPolicyDatas-RemoveResources|
|Beheer van toepassingen|AdminPolicyDatas-SetResources|
|Beheer van toepassingen|AdminUserJourneys-GetResources|
|Mapbeheer|AdminUserJourneys-RemoveResources|
|Mapbeheer|AdminUserJourneys-SetResources|
|Mapbeheer|IdentityProvider maken|
|Mapbeheer|Een nieuwe AdminUserJourney maken|
|Mapbeheer|Gelokaliseerde resource-json maken|
|Mapbeheer|Nieuwe aangepaste IDP maken|
|Mapbeheer|Nieuwe IDP maken|
|Mapbeheer|Een B2C-mapresource maken of bijwerken|
|Mapbeheer|Beleid maken|
|Mapbeheer|TrustFramework-beleid maken|
|Mapbeheer|TrustFramework-beleid maken met configureerbaar voorvoegsel|
|Mapbeheer|Gebruikerskenmerk maken|
|Mapbeheer|CreateTrustFrameworkPolicy|
|Mapbeheer|IDP verwijderen|
|Mapbeheer|IdentityProvider verwijderen|
|Mapbeheer|Een B2C-mapresource verwijderen|
|Mapbeheer|TrustFramework-beleid verwijderen|
|Mapbeheer|Gebruikerskenmerk verwijderen|
|Mapbeheer|B2C-mapresources in een resourcegroep ophalen|
|Mapbeheer|B2C-mapresources in een abonnement ophalen|
|Mapbeheer|Aangepaste IDP ophalen|
|Mapbeheer|IDP ophalen|
|Mapbeheer|Een B2C-mapresource ophalen|
|Mapbeheer|Een gebruikersbeleving ophalen|
|Mapbeheer|Toegestane toepassingsclaims voor gebruikersbeleving ophalen|
|Mapbeheer|Toegestane zelf-gecontroleerde claims voor gebruikersbeleving ophalen|
|Mapbeheer|Toegestane zelf-gecontroleerde claims voor beleid ophalen|
|Mapbeheer|Lijst met beschikbare uitvoerclaims ophalen|
|Mapbeheer|Inhouddefinities voor gebruikersbeleving ophalen|
|Mapbeheer|IDP's voor een specifieke beheerstroom ophalen|
|Mapbeheer|Lijst van alle beheerstromen ophalen|
|Mapbeheer|Lijst met labels voor alle beheerstromen voor alle gebruikers ophalen|
|Groepsbeheer|Lijst met tenants voor een gebruiker ophalen|
|Groepsbeheer|Zelf-gecontroleerde claims voor lokale accounts ophalen|
|Groepsbeheer|Gelokaliseerde resource-json ophalen|
|Groepsbeheer|Bewerkingen van de resourceprovider Microsoft.AzureActiveDirectory ophalen|
|Groepsbeheer|Beleidsregels ophalen|
|Groepsbeheer|Beleid ophalen|
|Groepsbeheer|Lijst met ondersteunde IDP's ophalen|
|Groepsbeheer|Lijst met ondersteunde IDP's van de gebruikersbeleving ophalen|
|Groepsbeheer|Voor tenant gedefinieerde, aangepaste IDP-lijst ophalen|
|Groepsbeheer|Voor tenant gedefinieerde IDP-lijst ophalen|
|Groepsbeheer|Voor tenant gedefinieerde, lokale IDP-lijst ophalen|
|Groepsbeheer|Tenantgegevens voor een gebruiker ophalen voor het maken van resources|
|Groepsbeheer|De standaard ondersteunde cultuur voor CPIM ophalen|
|Groepsbeheer|De gegevens van een beheerstroom ophalen|
|Groepsbeheer|De lijst met UserJourneys voor deze tenant ophalen|
|Groepsbeheer|De set beschikbare ondersteunde culturen voor CPIM ophalen|
|Groepsbeheer|TrustFramework-beleid ophalen|
|Groepsbeheer|TrustFramework-beleid als xml ophalen|
|Groepsbeheer|Gebruikerskenmerk ophalen|
|Beleidsbeheer|Gebruikerskenmerken ophalen|
|Beleidsbeheer|Lijst met gebruikersbelevingen ophalen|
|Beleidsbeheer|GetIEFPolicies|
|Beleidsbeheer|GetIdentityProviders|
|Beleidsbeheer|GetTrustFrameworkPolicy|
|Resource|MigrateTenantMetadata|
|Resource|Resources verplaatsen|
|Resource|IdentityProvider patchen|
|Resource|PutTrustFrameworkPolicy|
|Resource|PutTrustFrameworkpolicy|
|Resource|Een gebruikersbeleving verwijderen|
|Resource|Aangepaste IDP bijwerken|
|Resource|IDP bijwerken|
|Resource|Lokale IDP bijwerken|
|Resource|Een B2C-mapresource bijwerken|
|Resource|Beleid bijwerken|
|Resource|Abonnementstatus bijwerken|
|Rolbeheer|Gebruikerskenmerk bijwerken|
|Rolbeheer|Resources verplaatsen valideren|
|Rolbeheer|Apparaat toevoegen|
|Rolbeheer|Apparaatconfiguratie toevoegen|
|Rolbeheer|Geregistreerde eigenaar aan apparaat toevoegen|
|Rolbeheer|Geregistreerde gebruikers aan apparaat toevoegen|
|Rolbeheer|Apparaat verwijderen|
|Rolbeheer|Apparaatconfiguratie verwijderen|
|Rolbeheer|Apparaat niet meer conform|
|Rolbeheer|Apparaat wordt niet meer beheerd|
|Gebruikersbeheer|Geregistreerde eigenaar van apparaat verwijderen|
|Gebruikersbeheer|Geregistreerde gebruikers van apparaat verwijderen|
|Gebruikersbeheer|Apparaat bijwerken|
|Gebruikersbeheer|Apparaatconfiguratie bijwerken|
|Gebruikersbeheer|In aanmerking komend lid aan rol toevoegen|
|Gebruikersbeheer|Lid aan rol toevoegen|
|Gebruikersbeheer|Roltoewijzing aan roldefinitie toevoegen|
|Gebruikersbeheer|Rol vanuit sjabloon toevoegen|
|Gebruikersbeheer|Scoped lid aan rol toevoegen|
|Gebruikersbeheer|In aanmerking komen lid uit rol verwijderen|
|Gebruikersbeheer|Lid uit rol verwijderen|
|Gebruikersbeheer|Roltoewijzing uit roldefinitie verwijderen|
|Gebruikersbeheer|Scoped lid uit rol verwijderen|
|Gebruikersbeheer|Rol bijwerken|
|Gebruikersbeheer|AccessReview_Review|
|Gebruikersbeheer|AccessReview_Update|
|Gebruikersbeheer|ActivationAborted|
|Gebruikersbeheer|ActivationApproved|
|Gebruikersbeheer|ActivationCanceled|
|Gebruikersbeheer|ActivationRequested|
|Gebruikersbeheer|Toegevoegd|
|Gebruikersbeheer|Toewijzen|


## <a name="identity-protection"></a>Identiteitsbeveiliging

|Controlecategorie|Activiteit|
|---|---|
|Mapbeheer|Verhogen|
|Mapbeheer|Verwijderd|
|Mapbeheer|Wijzigingen van de rolinstelling|
|Overige|ScanAlertsNow|
|Overige|Registreren|
|Overige|Bevoegdheden verlagen|
|Overige|UpdateAlertSettings|
|Overige|UpdateCurrentState|
|Beleidsbeheer|Toegangsbeoordeling beëindigd|
|Beleidsbeheer|Goedkeurder toevoegen om goedkeuring te vragen|
|Beleidsbeheer|Revisor toevoegen voor toegang tot revisie|
|Gebruikersbeheer|Toegangsbeoordeling toepassen|
|Gebruikersbeheer|Toegangsbeoordeling maken|


## <a name="invited-users"></a>Uitgenodigde gebruikers

|Controlecategorie|Activiteit|
|---|---|
|Overige|Aanvraaggoedkeuring maken|
|Overige|Toegangsbeoordeling verwijderen|
|Gebruikersbeheer|Revisor verwijderen uit toegangsbeoordeling|
|Gebruikersbeheer|Toepassen van beoordelingsresultaat aanvragen|
|Gebruikersbeheer|Stoppen beoordeling aanvragen|
|Gebruikersbeheer|App-toewijzing beoordelen|
|Gebruikersbeheer|Groepslidmaatschap beoordelen|
|Gebruikersbeheer|RBAC-rollidmaatschap beoordelen|


## <a name="microsoft-identity-manager-mim"></a>Microsoft Identity Manager (MIM)

|Controlecategorie|Activiteit|
|---|---|
|Groepsbeheer|Aanvraag voor goedkeuring aanvraag beoordelen|
|Groepsbeheer|Toegangsbeoordeling bijwerken|
|Groepsbeheer|Instellingen voor de e-mailmelding van de toegangsbeoordeling bijwerken|
|Groepsbeheer|Instelling voor aantal herhalingen toegangsbeoordeling bijwerken|
|Groepsbeheer|Instelling voor duur herhalingen toegangsbeoordeling bijwerken|
|Gebruikersbeheer|Instelling voor eindtype herhalingen toegangsbeoordeling bijwerken|
|Gebruikersbeheer|Instelling voor type herhalingen toegangsbeoordeling bijwerken|



## <a name="privileged-identity-management"></a>Privileged Identity Management

|Controlecategorie|Activiteit|
|---|---|
|Rolbeheer|Herinneringsinstellingen toegangsbeoordeling bijwerken|
|Rolbeheer|Aanvraaggoedkeuring bijwerken|
|Rolbeheer|App-roltoewijzing aan gebruiker toevoegen|
|Rolbeheer|Gebruiker toevoegen|
|Rolbeheer|Gegevens voor telefoon-app voor sterke verificatie van gebruikers toevoegen|
|Rolbeheer|Gebruikerslicentie wijzigen|
|Rolbeheer|Gebruikerswachtwoord wijzigen|
|Rolbeheer|Federatieve gebruiker naar beheerd converteren|
|Rolbeheer|Toepassingswachtwoord voor gebruiker maken|
|Rolbeheer|Toepassingswachtwoord voor gebruiker verwijderen|
|Rolbeheer|Gebruiker verwijderen|
|Rolbeheer|Account uitschakelen|
|Rolbeheer|Sterke verificatie inschakelen|
|Rolbeheer|Gebruiker definitief verwijderen|
|Rolbeheer|App-roltoewijzing uit gebruiker verwijderen|
|Rolbeheer|Gegevens voor telefoon-app voor sterke verificatie van gebruikers verwijderen|



## <a name="self-service-group-management"></a>Groepsbeheer via selfservice

|Controlecategorie|Activiteit|
|---|---|
|Groepsbeheer|Gebruikerswachtwoord opnieuw instellen|
|Groepsbeheer|Gebruiker herstellen|
|Groepsbeheer|Geforceerd wijzigen van gebruikerswachtwoord instellen|
|Groepsbeheer|Gebruikersbeheerder instellen|
|Groepsbeheer|Instellen dat oath-token-metagegevens voor gebruikers zijn ingeschakeld|
|Groepsbeheer|Tijdstempel StsRefreshTokenValidFrom bijwerken|
|Groepsbeheer|Externe geheimen bijwerken|
|Groepsbeheer|Gebruiker bijwerken|
|Groepsbeheer|Beheerder genereert een tijdelijk wachtwoord|


## <a name="self-service-password-management"></a>Wachtwoordbeheer via selfservice

|Controlecategorie|Activiteit|
|---|---|
|Mapbeheer|Beheerders vereisen dat de gebruiker het wachtwoord opnieuw instelt|
|Mapbeheer|Externe gebruiker aan toepassing toewijzen|
|Gebruikersbeheer|E-mail niet verzonden, gebruiker heeft zich afgemeld|
|Gebruikersbeheer|Externe gebruiker uitnodigen|
|Gebruikersbeheer|Uitnodiging externe gebruiker inwisselen|
|Gebruikersbeheer|Virale tenant maken|
|Gebruikersbeheer|Virale gebruiker maken|
|Gebruikersbeheer|Gebruikerswachtwoordregistratie|
|Gebruikersbeheer|Gebruikerswachtwoordherstel|
|Gebruikersbeheer|Selfservice voor wachtwoord opnieuw instellen geblokkeerd|


## <a name="terms-of-use"></a>Gebruiksvoorwaarden

|Controlecategorie|Activiteit|
|---|---|
|Beleidsbeheer|Wachtwoord wijzigen (selfservice)|
|Beleidsbeheer|Wachtwoord opnieuw instellen (door beheerder)|
|Beleidsbeheer|Wachtwoord opnieuw instellen (selfservice)|
|Beleidsbeheer|Self-service voor stroomactiviteitvoortgang bij het opnieuw instellen van een wachtwoord|
|Beleidsbeheer|Self-service voor stroomactiviteitvoortgang bij het opnieuw instellen van een wachtwoord|
|Beleidsbeheer|Gebruikersaccount ontgrendelen (selfservice)|
|Beleidsbeheer|Door gebruiker geregistreerd voor selfservice voor het opnieuw instellen van een wachtwoord|




## <a name="next-steps"></a>Volgende stappen

Voor een overzicht van:

- Rapportage: zie [Azure Active Directory-rapportage](active-directory-reporting-azure-portal.md).

- Controleactiviteitenrapporten: zie [Controleactiviteitenrapporten in de Azure Active Directory-portal](active-directory-reporting-activity-audit-logs.md). 

