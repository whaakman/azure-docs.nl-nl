---
title: Azure Resource Manager-Provider Operations | Microsoft Docs
description: Details van de bewerkingen die beschikbaar zijn op de Microsoft Azure Resource Manager-resourceproviders
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/06/2018
ms.author: rolyon
ms.openlocfilehash: 80724a24fe7cf2565334a5212a0877589eb1eecf
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Azure Resource Manager Resource Provider-bewerkingen

Dit document vindt u de bewerkingen die beschikbaar zijn voor elke resourceprovider van Microsoft Azure Resource Manager. Deze kunnen worden gebruikt in aangepaste rollen gedetailleerde op rollen gebaseerde toegangsbeheer (RBAC) machtigingen opgeven voor resources in Azure. Houd er rekening mee dat niet een uitgebreide lijst en bewerkingen kunnen worden toegevoegd of verwijderd als elke provider wordt bijgewerkt. Voer de bewerking tekenreeksen de indeling van `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. 

> [!NOTE]
> Gebruik voor een lijst met uitgebreide en huidige `Get-AzureRmProviderOperation` (in PowerShell) of `az provider operation list` (in de Azure CLI-v2) voor de bewerkingen na opvragen van de Azure-resourceproviders.

## <a name="microsoftaad"></a>Microsoft.AAD

| Bewerking | Beschrijving |
|---|---|
|/domainServices/delete|Hiermee verwijdert u Domain Services.|
|/domainServices/Read|Leest Domain Services.|
|domainServices/schrijven|Schrijven van domeinservices|
|/Locations/operationresults/Read|Lees de status van een asynchrone bewerking.|
|Bewerkingen/leestijd|De gelokaliseerde beschrijving voor de bewerking, die moet worden weergegeven voor de gebruiker.|

## <a name="microsoftaadiam"></a>microsoft.aadiam

| Bewerking | Beschrijving |
|---|---|
|/diagnosticsettings/DELETE|Verwijderen van een diagnostische instelling|
|/diagnosticsettings/Read|Lezen van een diagnostische instelling|
|diagnosticsettings/schrijven|Schrijven van een diagnostische instelling|
|/diagnosticsettingscategories/Read|Lezen van een diagnostische instelling categorieën|
|/tenants/providers/Microsoft.Insights/diagnosticSettings/read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/tenants/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/tenants/providers/Microsoft.Insights/logDefinitions/read|De beschikbare logboeken opgehaald voor tenants|

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| Bewerking | Beschrijving |
|---|---|
|/ configuration/actie|Configuratie van de Tenant updates.|
|/Configuration/Read|Leest de Tenantconfiguratie van de.|
|/ configuration/schrijven|De Tenantconfiguratie van een maakt.|
|/ services/actie|Een service-exemplaar in de tenant wordt bijgewerkt.|
|/Services/Alerts/Read|Leest de waarschuwingen voor een service.|
|/Services/Alerts/Read|Leest de waarschuwingen voor een service.|
|/Services/DELETE|Hiermee verwijdert u een service-exemplaar in de tenant.|
|/Services/Read|Leest de service-exemplaren in de tenant.|
|/Services/servicemembers/Action|Maakt een exemplaar van de service lid in de service.|
|/Services/servicemembers/Alerts/Read|Leest de waarschuwingen voor een lid van de service.|
|/Services/servicemembers/DELETE|Hiermee verwijdert u een exemplaar van de service lid in de service.|
|/Services/servicemembers/Read|Het lid service-exemplaar in de service worden gelezen.|
|/ services/schrijven|Maakt een service-exemplaar in de tenant.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| Bewerking | Beschrijving |
|---|---|
|/Configurations/Read|Configuraties ophalen|
|configuraties van/schrijven|Bijgewerkte configuratie|
|/ generateRecommendations/actie|Geeft aanbevelingen|
|/generateRecommendations/Read|Haalt status aanbevelingen genereren|
|/Operations/Read|De bewerkingen voor de Microsoft Advisor opgehaald|
|/Recommendations/Read|Leest aanbevelingen|
|/Recommendations/suppressions/DELETE|Hiermee verwijdert u de onderdrukking|
|/Recommendations/suppressions/Read|Suppressions opgehaald|
|/Recommendations/suppressions/Write|Bijgewerkte suppressions|
|registratie-/ actie|Registreert het abonnement voor Microsoft Advisor|
|/suppressions/DELETE|Hiermee verwijdert u de onderdrukking|
|/suppressions/Read|Suppressions opgehaald|
|suppressions/schrijven|Bijgewerkte suppressions|
|/ unregister/actie|Heft de registratie van het abonnement voor Microsoft Advisor|

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

| Bewerking | Beschrijving |
|---|---|
|/Alerts/Read|Alle waarschuwingen voor de invoer-filters niet ophalen.|
|/Alerts/Resolve/Action|Wijzig de status van de waarschuwing om 'Op te lossen'|
|/alertsSummary/Read|Ophalen van de samenvatting van waarschuwingen|
|Bewerkingen/leestijd|Leest de bewerkingen die zijn opgegeven|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| Bewerking | Beschrijving |
|---|---|
|/Locations/checkNameAvailability/Action|Controleert dat de opgegeven Analysis Server naam geldig is en niet in gebruik.|
|/Locations/operationresults/Read|Haalt de informatie van het resultaat van de opgegeven bewerking.|
|/Locations/operationstatuses/Read|Haalt de informatie van de status van de opgegeven bewerking.|
|/Operations/Read|De informatie van bewerkingen opgehaald|
|registratie-/ actie|Resourceprovider van Analysis Services wordt geregistreerd.|
|/servers/DELETE|Hiermee verwijdert u de Analysis-Server.|
|/servers/listGatewayStatus/action|Lijst van de status van de gateway die is gekoppeld aan de server.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|De diagnostische instelling voor Analysis-Server opgehaald|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Maken of bijwerken van de diagnostische instelling voor Analysis-Server|
|/servers/providers/Microsoft.Insights/logDefinitions/read|De beschikbare logboeken opgehaald voor servers|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Hiermee wordt de beschikbare metrische gegevens voor Analysis-Server|
|/servers/Read|Haalt de informatie van de opgegeven Analysis-Server.|
|/servers/Resume/Action|De Analysis-Server hervat.|
|/servers/skus/Read|Beschikbare SKU-informatie voor de server ophalen|
|/servers/Suspend/Action|Onderbreekt de Analysis-Server.|
|servers/schrijven|Maken of bijwerken van de opgegeven Analysis-Server.|
|/skus/Read|Haalt de informatie van SKU 's|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| Bewerking | Beschrijving |
|---|---|
|/checkNameAvailability/Read|Controleert of de opgegeven servicenaam is beschikbaar|
|/Operations/Read|Leesbewerkingen voor alle API beschikbaar voor Microsoft.ApiManagement resource|
|registratie-/ actie|Abonnement voor Microsoft.ApiManagement resourceprovider registreren|
|/Reports/Read|Rapporten kunnen samenvoegen met perioden, geografische regio, ontwikkelaars, producten, API's, bewerkingen, abonnement en byRequest ophalen.|
|/Service/APIs/DELETE|Verwijder de bestaande API|
|/Service/APIs/Diagnostics/DELETE|Bestaande diagnostische verwijderen|
|/Service/APIs/Diagnostics/loggers/DELETE|Toewijzing van een logboek met een diagnostische instelling verwijderen|
|/Service/APIs/Diagnostics/loggers/Read|Lijst met bestaande diagnostische voorkomen|
|/Service/APIs/Diagnostics/loggers/Write|Berichtenlogboek toewijzen aan een diagnostische instelling|
|/Service/APIs/Diagnostics/Read|Lijst met diagnostische gegevens of Get-details van diagnose|
|/Service/APIs/Diagnostics/Write|Nieuwe diagnostische toevoegen of bijwerken van bestaande diagnostische gegevens|
|/Service/APIs/Operations/DELETE|Verwijder de bestaande API-bewerking|
|/Service/APIs/Operations/Policies/DELETE|Verwijder voor beleidsconfiguratie van beleid voor API-bewerking|
|/Service/APIs/Operations/Policies/Read|Beleid voor API-bewerking of configuratiedetails Get-beleid voor API-bewerking ophalen|
|/Service/APIs/Operations/Policies/Write|Configuratiedetails van beleid voor API-bewerking instellen|
|/Service/APIs/Operations/Policy/DELETE|Voor beleidsconfiguratie van de bewerking verwijderen|
|/Service/APIs/Operations/Policy/Read|Configuratiedetails van beleid voor bewerking ophalen|
|/Service/APIs/Operations/Policy/Write|Configuratiedetails van beleid voor opnieuw instellen|
|/Service/APIs/Operations/Read|Ophalen van lijst met bewerkingen voor bestaande API of details van API-bewerking ophalen|
|/Service/APIs/Operations/tags/DELETE|Koppeling van de bestaande code met bestaande bewerking verwijderen|
|/Service/APIs/Operations/tags/Read|Ophalen van de labels die zijn gekoppeld aan de details van de bewerking of Tag ophalen|
|/Service/APIs/Operations/tags/Write|Bestaande code te koppelen aan bestaande bewerking|
|/Service/APIs/Operations/Write|Nieuwe API-bewerkingen voor het maken of bijwerken van bestaande API-bewerking|
|/Service/APIs/operationsByTags/Read|Lijst met koppelingen bewerking/label|
|/Service/APIs/Policies/DELETE|Configuratie van beleid uit de API-beleid verwijderen|
|/Service/APIs/Policies/Read|Beleidsregels voor configuratiedetails API of Get-beleid voor API niet ophalen|
|/Service/APIs/Policies/Write|Configuratiedetails van beleid voor API instellen|
|/Service/APIs/Policy/DELETE|Verwijder voor beleidsconfiguratie van de API|
|/Service/APIs/Policy/Read|Configuratiedetails van beleid voor API ophalen|
|/Service/APIs/Policy/Write|Configuratiedetails van beleid voor API instellen|
|/Service/APIs/Products/Read|Ophalen van alle producten die de API deel uitmaakt van|
|/Service/APIs/Read|Lijst met alle geregistreerde API's of Get details van de API|
|/Service/APIs/releases/DELETE|Hiermee verwijdert u alle versies van de API of verwijder API-versie|
|/Service/APIs/releases/Read|Get-versies voor een API of Get-details van de API reelase|
|/Service/APIs/releases/Write|Nieuwe API-versie maken of bijwerken van bestaande API-versie|
|/Service/APIs/Revisions/DELETE|Hiermee verwijdert u alle versies van een API|
|/Service/APIs/Revisions/Read|Ophalen van revisies die horen bij een API|
|/Service/APIs/schemas/DELETE|Hiermee verwijdert u het bestaande Schema|
|/Service/APIs/schemas/document/Read|Het document met een beschrijving van het Schema ophalen|
|/Service/APIs/schemas/document/Write|Het document met een beschrijving van het Schema bijwerken|
|/Service/APIs/schemas/Read|De schema's opgehaald voor een bepaalde API of de schema's die worden gebruikt door de API opgehaald|
|/Service/APIs/schemas/Write|Hiermee stelt u de schema's die worden gebruikt door de API|
|/Service/APIs/tagDescriptions/DELETE|Beschrijving van de Tag verwijderen van de API|
|/Service/APIs/tagDescriptions/Read|Beschrijvingen van de labels in het bereik van de API of Tag ophalen beschrijving opvragen in het bereik van de API|
|/Service/APIs/tagDescriptions/Write|De beschrijving van de Tag maken/wijzigen in het bereik van de API|
|/Service/APIs/tags/DELETE|Verwijder bestaande API/Tag-koppeling|
|/Service/APIs/tags/Read|Alle API/Tag-koppeling voor de API of Get-details van de API/Tag koppeling ophalen|
|/Service/APIs/tags/Write|Nieuwe API/Tag koppeling toevoegen|
|/Service/APIs/Write|Nieuwe API maken of bijwerken van bestaande API-details|
|/Service/apisByTags/Read|Lijst met API/Tag koppelingen|
|/Service/API-Version-sets/DELETE|Verwijder de bestaande VersionSet|
|/Service/API-Version-sets/Read|Lijst met versie groepsentiteiten of details van een VersionSet opgehaald|
|/Service/API-Version-sets/Versions/Read|Lijst met versie entiteiten ophalen|
|/Service/API-Version-sets/Write|Nieuwe VersionSet maken of bijwerken van bestaande VersionSet details|
|/Service/applynetworkconfigurationupdates/Action|De Microsoft.ApiManagement resources voor uitvoering in het virtuele netwerk kiest bijgewerkte netwerkinstellingen-updates.|
|/service/authorizationServers/delete|Verwijderen van bestaande autorisatie-server|
|/Service/authorizationServers/Read|Ophalen van lijst met autorisatieservers of details van de autorisatie-server ophalen|
|/Service/authorizationServers/Write|Een nieuwe autorisatie-server of de details van de Update van een bestaande autorisatie-server maken|
|/Service/backends/DELETE|Verwijder de bestaande back-end|
|/Service/backends/Read|Ophalen van lijst met back-ends of details van back-end ophalen|
|/Service/backends/Reconnect/Action|Een aanvraag voor opnieuw verbinden maken|
|/Service/backends/Write|Een nieuwe back-end toevoegen of bijwerken van bestaande back-end-details|
|/Service/Backup/Action|Back-API Management-Service aan de opgegeven container in een gebruiker opgegeven storage-account|
|/Service/Certificates/DELETE|Verwijder bestaande certificaat|
|/Service/Certificates/Read|Ophalen van lijst met certificaten of details van een certificaat ophalen|
|/Service/Certificates/Write|Nieuw certificaat toevoegen|
|/Service/DELETE|Verwijderen van exemplaar van API Management-Service|
|/Service/Diagnostics/DELETE|Bestaande diagnostische verwijderen|
|/Service/Diagnostics/loggers/DELETE|Toewijzing van een logboek met een diagnostische instelling verwijderen|
|/Service/Diagnostics/loggers/Read|Lijst met bestaande diagnostische voorkomen|
|/Service/Diagnostics/loggers/Write|Berichtenlogboek toewijzen aan een diagnostische instelling|
|/Service/Diagnostics/Read|Lijst met diagnostische gegevens of Get-details van diagnose|
|/Service/Diagnostics/Write|Nieuwe diagnostische toevoegen of bijwerken van bestaande diagnostische gegevens|
|/Service/getssotoken/Action|Opgehaald SSO-token dat kan worden gebruikt om aan te melden bij API Management-Service Legacy-portal als beheerder|
|/Service/Groups/DELETE|Verwijder bestaande groep|
|/Service/Groups/Read|Lijst met groepen of informatie opgehaald van een groep|
|/Service/Groups/Users/DELETE|Bestaande gebruiker verwijderen uit bestaande groep|
|/Service/Groups/Users/Read|Lijst van de groepsgebruikers ophalen|
|/Service/Groups/Users/Write|Bestaande gebruiker toevoegen aan bestaande groep|
|/Service/Groups/Write|Nieuwe groep maken of bijwerken van bestaande groepsnaam details|
|/service/identityProviders/delete|Bestaande id-Provider verwijderen|
|/Service/identityProviders/Read|Ophalen van lijst met identiteitsproviders of Get-details van de identiteitsprovider|
|/service/identityProviders/write|Een nieuwe id-Provider of Update details van een bestaande id-Provider maken|
|/Service/Locations/networkstatus/Read|Hiermee haalt de netwerk-toegangsstatus van bronnen waarop de service is afhankelijk in de locatie.|
|/Service/loggers/DELETE|Bestaande Berichtenlogboek verwijderen|
|/Service/loggers/Read|Ophalen van lijst met voorkomen of details van berichtenlogboek ophalen|
|/Service/loggers/Write|Nieuwe berichtenlogboek toevoegen of bestaande berichtenlogboek details bijwerken|
|/Service/managedeployments/Action|SKU/eenheden toevoegen of verwijderen van regionale implementaties van API Management-Service wijzigen|
|/Service/networkstatus/Read|Hiermee haalt u de netwerk-toegangsstatus van bronnen waarop de service is afhankelijk.|
|/Service/Notifications/Action|Wordt een melding verzonden naar een opgegeven gebruiker|
|/Service/Notifications/Read|Alle meldingen van API Management-uitgever of ophalen van API Management publisher opgehaald details van melding|
|/service/notifications/recipientEmails/delete|Hiermee verwijdert u het die bestaande e-mailadres gekoppeld aan een melding|
|/Service/Notifications/recipientEmails/Read|E-mailontvangers die zijn gekoppeld aan API Management Publisher melding ophalen|
|/Service/Notifications/recipientEmails/Write|Maken van nieuwe e-mailontvanger van de kennisgeving|
|/Service/Notifications/recipientUsers/DELETE|Hiermee verwijdert u de gebruiker die is gekoppeld aan de ontvangers van meldingen|
|/Service/Notifications/recipientUsers/Read|Ontvanger gebruikers die zijn gekoppeld aan de melding ophalen|
|/Service/Notifications/recipientUsers/Write|Gebruiker toevoegen aan de ontvangers van meldingen|
|/Service/Notifications/Write|Maken of Update API Management publisher meldingen|
|/service/openidConnectProviders/delete|Bestaande OpenID Connect Provider verwijderen|
|/Service/openidConnectProviders/Read|Lijst met providers OpenID Connect of details van OpenID Connect-Provider opvragen|
|/service/openidConnectProviders/write|Een nieuwe OpenID Connect-Provider of Update details van een bestaande OpenID Connect-Provider maken|
|/Service/operationresults/Read|Huidige status van een langdurige bewerking opgehaald|
|/Service/Policies/DELETE|Configuratie van beleid uit Tenant beleid verwijderen|
|/Service/Policies/Read|Beleidsregels voor Tenant of Get policy-configuratiegegevens voor de Tenant niet ophalen|
|/Service/Policies/Write|Configuratiedetails van beleid instellen voor Tenant|
|/Service/policySnippets/Read|Alle beleid codefragmenten ophalen|
|/Service/portalsettings/Read|Aanmelden bij instellingen voor de Portal of Get-instellingen voor de Portal aanmelden of ophalen van delegatie voor de Portal-instellingen niet ophalen|
|/Service/portalsettings/Write|Instellingen voor aanmelden of Update aanmelden instellingen of Update aanmelden instellingen of Update aanmelden instellingen of Update Overdrachtinstellingen of Update Overdrachtinstellingen bijwerken|
|/Service/Products/APIs/DELETE|Bestaande API verwijderen uit bestaande product|
|/Service/Products/APIs/Read|Lijst met API's toegevoegd aan bestaande product ophalen|
|/Service/Products/APIs/Write|Bestaande API toevoegen aan bestaande product|
|/Service/Products/DELETE|Verwijder bestaande product|
|/Service/Products/Groups/DELETE|Koppeling van bestaande developer-groep met bestaande product verwijderen|
|/Service/Products/Groups/Read|Lijst met groepen van ontwikkelaars die zijn gekoppeld aan het product verkrijgen|
|/Service/Products/Groups/Write|Bestaande developer-groep koppelen aan bestaande product|
|/Service/Products/Policies/DELETE|Configuratie van beleid vanaf de Product-beleid verwijderen|
|/Service/Products/Policies/Read|Beleidsregels voor het Product of Get policy-configuratiegegevens voor Product niet ophalen|
|/Service/Products/Policies/Write|Configuratiedetails van beleid instellen voor het Product|
|/Service/Products/Policy/DELETE|Configuratie van beleid verwijderen uit bestaande product|
|/Service/Products/Policy/Read|Voor beleidsconfiguratie van bestaande product ophalen|
|/Service/Products/Policy/Write|Stel de beleidsconfiguratie van voor bestaande product|
|/Service/Products/Read|Ophalen van lijst met producten of details van product ophalen|
|/Service/Products/Subscriptions/Read|Lijst met abonnementen product ophalen|
|/Service/Products/tags/DELETE|Koppeling van de bestaande code met bestaande Product verwijderen|
|/Service/Products/tags/Read|Ophalen van de labels die zijn gekoppeld aan de details van de Product- of Tag ophalen|
|/Service/Products/tags/Write|Bestaand label koppelen aan bestaande Product|
|/Service/Products/Write|Nieuw product maken of bijwerken van bestaande Productgegevens|
|/Service/Properties/DELETE|Hiermee verwijdert u het bestaande eigenschap|
|/Service/Properties/Read|Lijst van alle eigenschappen opgehaald of details van de opgegeven eigenschap opgehaald|
|/Service/Properties/Write|Een nieuwe eigenschap maken of bijwerken van waarde voor de opgegeven eigenschap|
|/service/providers/Microsoft.Insights/diagnosticSettings/Read|De diagnostische instelling voor API Management-service opgehaald|
|/service/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instelling voor API Management-service|
|/service/providers/Microsoft.Insights/logDefinitions/read|De beschikbare logboeken opgehaald voor API Management-service|
|/service/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor API Management-service|
|/Service/Quotas/Periods/Read|De itemwaarde quotum voor periode ophalen|
|/Service/Quotas/Periods/Write|Huidige waarde van het prestatiemeteritem quotum instellen|
|/Service/Quotas/Read|Ophalen van waarden voor quotum|
|/Service/Quotas/Write|Huidige waarde van het prestatiemeteritem quotum instellen|
|/Service/Read|Lezen van metagegevens voor een exemplaar van API Management-Service|
|/Service/Reports/Read|Rapport samenvoegen met perioden of Get-rapport samenvoegen met geografische regio of Get-rapport geaggregeerd door ontwikkelaars ophalen. of rapport samenvoegen met producten ophalen. of rapport samenvoegen met API's of Get rapport samenvoegen met de bewerkingen of Get-rapport samengevoegd per abonnement ophalen. of rapportagegegevens van aanvragen|
|/Service/Restore/Action|API Management-Service van de opgegeven container in het opgegeven opslagaccount van een gebruiker herstellen|
|/Service/Subscriptions/DELETE|Het abonnement verwijderd. Deze bewerking kan worden gebruikt voor het abonnement verwijderen|
|/Service/Subscriptions/Read|Een lijst met abonnementen product of details van het product abonnement ophalen|
|/Service/Subscriptions/regeneratePrimaryKey/Action|Abonnement primaire sleutel opnieuw genereren|
|/Service/Subscriptions/regenerateSecondaryKey/Action|Abonnement secundaire sleutel opnieuw genereren|
|/Service/Subscriptions/Write|Een bestaande gebruiker aan een bestaand product abonneren of bijwerken van bestaande abonnementsgegevens. Deze bewerking kan worden gebruikt voor het abonnement te verlengen|
|/Service/tagResources/Read|Lijst van labels met gekoppelde Resources ophalen|
|/service/tags/DELETE|Bestaande code verwijderen|
|/service/tags/Read|Lijst met Tags of details van label ophalen|
|/service/tags/Write|Nieuw label toevoegen of bijwerken van bestaande label details|
|/Service/templates/DELETE|Standaard API Management e-mailsjabloon opnieuw instellen|
|/Service/templates/Read|Alle e-mailsjablonen of opgehaald API Management-e-mailbericht Sjabloondetails opgehaald|
|/Service/templates/Write|Maken of bijwerken van API Management-e-mailsjabloon of API Management Updates-e-mailsjabloon|
|/Service/tenant/DELETE|Configuratie van beleid voor de tenant verwijderen|
|/Service/tenant/Deploy/Action|Voert een taak voor implementatie om de wijzigingen van de vertakking opgegeven git toepassen op de configuratie in de database.|
|/Service/tenant/operationResults/Read|Ophalen van lijst met Bewerkingsresultaten of resultaat van een specifieke bewerking ophalen|
|/Service/tenant/Read|Configuratie van beleid voor de of Get-tenant toegang informatie details ophalen|
|/Service/tenant/regeneratePrimaryKey/Action|Primaire sleutel opnieuw genereren|
|/Service/tenant/regenerateSecondaryKey/Action|Secundaire toegangssleutel opnieuw genereren|
|/Service/tenant/Save/Action|Hiermee maakt u doorvoeren met configuratie-momentopname voor de opgegeven vertakking in de opslagplaats|
|/Service/tenant/syncState/Read|Status van laatste git-synchronisatie ophalen|
|/Service/tenant/Validate/Action|Wijzigingen van de vertakking opgegeven git valideert|
|/Service/tenant/Write|Stel de beleidsconfiguratie van voor de tenant of Update tenant toegangsgegevens informatie|
|/Service/updatecertificate/Action|SSL-certificaat uploaden voor een API Management-Service|
|/Service/updatehostname/Action|Setup, bijwerken of verwijderen van aangepaste domeinnamen voor een API Management-Service|
|/Service/Users/Action|Een nieuwe gebruiker registreren|
|/Service/Users/Applications/Attachments/DELETE|Hiermee verwijdert u een bijlage|
|/Service/Users/Applications/Attachments/Read|Toepassing bijlagen of opgehaald bijlage opgehaald|
|/Service/Users/Applications/Attachments/Write|Koppelen aan een toepassing toevoegen|
|/Service/Users/Applications/DELETE|Hiermee verwijdert u de bestaande toepassing|
|/Service/Users/Applications/Read|Lijst van alle gebruikerstoepassingen of toepassingsgegevens opgehaald API Management|
|/Service/Users/Applications/Write|Registreert een toepassing op toepassingsgegevens van API Management of Updates|
|/Service/Users/DELETE|Gebruikersaccount verwijderen|
|/service/users/generateSsoUrl/action|URL SSO genereren. De URL kan worden gebruikt voor toegang tot de beheerportal|
|/Service/Users/Groups/Read|Ophalen van lijst met gebruikersgroepen|
|/Service/Users/Keys/Read|Lijst met gebruikerssleutels|
|/Service/Users/Read|Een lijst met geregistreerde gebruikers of accountdetails van de van een gebruiker ophalen|
|/Service/Users/Subscriptions/Read|Lijst met gebruikersabonnementen ophalen|
|/Service/Users/token/Action|Token toegangstoken ophalen voor een gebruiker|
|/Service/Users/Write|Een nieuwe gebruiker of de details van de Update-account van een bestaande gebruiker registreren|
|/ service/schrijven|Maak een nieuw exemplaar van API Management-Service|
|/ unregister/actie|Abonnement voor Microsoft.ApiManagement resourceprovider van de registratie ongedaan maken|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| Bewerking | Beschrijving |
|---|---|
|/ checkAccess/actie|Hiermee wordt gecontroleerd of de aanroeper is gemachtigd een bepaalde actie uit te voeren|
|/classicAdministrators/DELETE|Hiermee wordt de beheerder verwijderd uit het abonnement.|
|/classicAdministrators/Read|Hiermee worden de beheerders voor het abonnement ingelezen.|
|classicAdministrators/schrijven|Hiermee wordt de beheerder gewijzigd of aan een abonnement toegevoegd.|
|/ elevateAccess/actie|Hiermee wordt oproepende functie de rechten van Administrator voor gebruikerstoegang gegeven voor het tenantbereik|
|/locks/DELETE|De vergrendelingen bij het opgegeven bereik worden verwijderd.|
|/locks/Read|De vergrendelingen bij het opgegeven bereik worden opgehaald.|
|vergrendelingen/schrijven|Hiermee worden vergrendelingen toegevoegd bij het opgegeven bereik.|
|/permissions/Read|Hiermee worden alle machtigingen van de beller voor een bepaald bereik weergegeven in een lijst.|
|/policyAssignments/DELETE|Een beleidstoewijzing verwijderen bij het opgegeven bereik.|
|/policyAssignments/Read|Informatie ophalen over een beleidstoewijzing.|
|policyAssignments/schrijven|Een beleidstoewijzing maken bij het opgegeven bereik.|
|/policyDefinitions/DELETE|Een beleidsdefinitie verwijderen.|
|/policyDefinitions/Read|Informatie ophalen over een beleidsdefinitie.|
|policyDefinitions/schrijven|Een aangepaste beleidsdefinitie maken.|
|/policySetDefinitions/delete|Een beleidssetdefinitie verwijderen.|
|/policySetDefinitions/Read|Informatie ophalen over een beleidssetdefinitie.|
|policySetDefinitions/schrijven|Een aangepaste beleidssetdefinitie maken.|
|/providerOperations/Read|Bewerkingen ophalen voor alle resourceproviders die kunnen worden gebruikt in roldefinities.|
|/roleAssignments/DELETE|Een roltoewijzing bij het opgegeven bereik verwijderen.|
|/roleAssignments/Read|Informatie ophalen over een roltoewijzing.|
|roleAssignments/schrijven|Een roltoewijzing bij het opgegeven bereik maken.|
|/roleDefinitions/DELETE|De opgegeven aangepaste roldefinitie verwijderen.|
|/roleDefinitions/Read|Informatie ophalen over een roldefinitie.|
|roleDefinitions/schrijven|Een aangepaste roldefinitie maken of bijwerken met opgegeven machtigingen of toewijsbare bereiken.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| Bewerking | Beschrijving |
|---|---|
|/automationAccounts/agentRegistrationInformation/Read|Registratie-informatie voor een Azure Automation DSC lezen|
|/automationAccounts/agentRegistrationInformation/regenerateKey/Action|Schrijft een aanvraag voor het genereren van sleutels voor Azure Automation DSC|
|/automationAccounts/certificates/delete|Een Azure Automation-certificaatactivum verwijderen|
|/automationAccounts/Certificates/Read|Een Azure Automation-certificaatasset opgehaald|
|/automationAccounts/Certificates/Write|Maken of bijwerken van een Azure Automation-certificaatactivum|
|/automationAccounts/compilationjobs/Read|Een Azure Automation DSC van compilatie leest|
|/automationAccounts/compilationjobs/Write|Een Azure Automation DSC van compilatie schrijft|
|/automationAccounts/configurations/delete|Hiermee verwijdert u een Azure Automation DSC-inhoud|
|/automationAccounts/configurations/getCount/action|Leest de telling van de inhoud van een Azure Automation DSC|
|/automationAccounts/Configurations/Read|Hiermee wordt de inhoud van een Azure Automation DSC opgehaald|
|/automationAccounts/Configurations/Write|Schrijft een Azure Automation DSC-inhoud|
|/automationAccounts/Connections/DELETE|Hiermee wordt een Azure Automation verbindingstype-asset verwijderd|
|/automationAccounts/Connections/Read|Een Azure Automation-verbindingsasset opgehaald|
|/automationAccounts/Connections/Write|Maken of bijwerken van een Azure Automation-verbindingsasset|
|/automationAccounts/connectionTypes/delete|Hiermee wordt een Azure Automation verbindingstype-asset verwijderd|
|/automationAccounts/connectionTypes/Read|Een Azure Automation verbindingstype-asset opgehaald|
|/automationAccounts/connectionTypes/write|Hiermee maakt u een Azure Automation verbindingstype-asset|
|/automationAccounts/credentials/delete|Hiermee wordt een Azure Automation-referentieasset verwijderd|
|/automationAccounts/credentials/read|Een Azure Automation-referentieasset opgehaald|
|/automationAccounts/credentials/Write|Maken of bijwerken van een Azure Automation-referentieasset|
|/automationAccounts/delete|Hiermee verwijdert u een Azure Automation-account|
|/automationAccounts/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/automationAccounts/diagnosticSettings/Write|Hiermee stelt u de diagnostische instelling voor de resource|
|/automationAccounts/hybridRunbookWorkerGroups/delete|Resources voor de Hybrid Runbook Worker worden verwijderd|
|/automationAccounts/hybridRunbookWorkerGroups/Read|Resources voor de Hybrid Runbook Worker gelezen|
|/automationAccounts/Jobs/output/Action|De uitvoer van een taak opgehaald|
|/automationAccounts/Jobs/output/Action|De uitvoer van een taak opgehaald|
|/automationAccounts/Jobs/Read|Een Azure Automation-taak opgehaald|
|/automationAccounts/Jobs/Read|Een Azure Automation-taak opgehaald|
|/automationAccounts/Jobs/Resume/Action|Een Azure Automation-taak hervatten|
|/automationAccounts/Jobs/Resume/Action|Een Azure Automation-taak hervatten|
|/automationAccounts/jobs/runbookContent/action|De inhoud van het Azure Automation-runbook opgehaald op het moment van de taak wordt uitgevoerd|
|/automationAccounts/jobs/runbookContent/action|De inhoud van het Azure Automation-runbook opgehaald op het moment van de taak wordt uitgevoerd|
|/automationAccounts/Jobs/Stop/Action|Hiermee wordt een Azure Automation-taak gestopt|
|/automationAccounts/Jobs/Stop/Action|Hiermee wordt een Azure Automation-taak gestopt|
|/automationAccounts/Jobs/streams/Read|Een Azure Automation-taakstroom opgehaald|
|/automationAccounts/Jobs/streams/Read|Een Azure Automation-taakstroom opgehaald|
|/automationAccounts/Jobs/Suspend/Action|Een Azure Automation-taak wordt onderbroken|
|/automationAccounts/Jobs/Suspend/Action|Een Azure Automation-taak wordt onderbroken|
|/automationAccounts/jobs/write|Hiermee maakt u een Azure Automation-taak|
|/automationAccounts/jobs/write|Hiermee maakt u een Azure Automation-taak|
|/automationAccounts/jobSchedules/delete|Hiermee wordt een Azure Automation-taakschema verwijderd|
|/automationAccounts/jobSchedules/Read|Een Azure Automation-taakschema opgehaald|
|/automationAccounts/jobSchedules/Write|Hiermee maakt u een Azure Automation-taakschema|
|/automationAccounts/linkedWorkspace/read|De werkruimte die is gekoppeld aan het automation-account opgehaald|
|/automationAccounts/logDefinitions/read|De beschikbare logboeken opgehaald voor het automation-account|
|/automationAccounts/modules/Activities/Read|Azure Automatiseringsactiviteiten opgehaald|
|/automationAccounts/modules/delete|Hiermee verwijdert u een Azure Automation-module|
|/automationAccounts/modules/Read|Een Azure Automation-module opgehaald|
|/automationAccounts/modules/Write|Maken of bijwerken van een Azure Automation-module|
|/automationAccounts/nodeConfigurations/delete|Hiermee verwijdert u een Azure Automation DSC-knooppuntconfiguratie|
|/automationAccounts/nodeConfigurations/read|Hiermee wordt een Azure Automation DSC-knooppuntconfiguratie gelezen|
|/automationAccounts/nodeConfigurations/readContent/action|Leest configuratie-inhoud voor een Azure Automation DSC-knooppunt|
|/automationAccounts/nodeConfigurations/write|Een Azure Automation DSC-knooppuntconfiguratie schrijft|
|/automationAccounts/nodes/delete|Hiermee verwijdert u Azure Automation DSC-knooppunten|
|/automationAccounts/nodes/Read|Azure Automation DSC-knooppunten leest|
|/automationAccounts/nodes/Reports/Read|Azure Automation DSC rapport contentss leest|
|/automationAccounts/nodes/Reports/Read|Leest Azure Automation DSC-rapporten|
|/automationAccounts/objectDataTypes/Fields/Read|Azure Automation TypeFields opgehaald|
|/automationAccounts/providers/Microsoft.Insights/metricDefinitions/read|Automation metrische definities opgehaald|
|/automationAccounts/Read|Hiermee ontvangt u een Azure Automation-account|
|/automationAccounts/runbooks/delete|Hiermee verwijdert u een Azure Automation-runbook|
|/automationAccounts/runbooks/draft/publish/action|Een Azure Automation-runbookconcept gepubliceerd|
|/automationAccounts/runbooks/draft/read|Een Azure Automation-runbookconcept opgehaald|
|/automationAccounts/runbooks/draft/readContent/action|Hiermee wordt de inhoud van een Azure Automation-runbookconcept opgehaald|
|/automationAccounts/runbooks/draft/testJob/read|Een Azure Automation-runbookconcept opgehaald|
|/automationAccounts/runbooks/draft/testJob/resume/action|Een Azure Automation-runbookconcept hervatten|
|/automationAccounts/runbooks/draft/testJob/stop/action|Hiermee wordt een Azure Automation-runbookconcept gestopt|
|/automationAccounts/runbooks/draft/testJob/suspend/action|Wordt een Azure Automation-runbookconcept onderbroken|
|/automationAccounts/runbooks/draft/testJob/write|Hiermee maakt u een Azure Automation-runbookconcept|
|/automationAccounts/runbooks/draft/undoEdit/action|Bewerkingen aan een Azure Automation-runbookconcept ongedaan maken|
|/automationAccounts/runbooks/draft/writeContent/action|De inhoud van een Azure Automation-runbookconcept gemaakt|
|/automationAccounts/runbooks/read|Een Azure Automation-runbook opgehaald|
|/automationAccounts/runbooks/readContent/action|De inhoud van een Azure Automation-runbook opgehaald|
|/automationAccounts/runbooks/Write|Maken of bijwerken van een Azure Automation-runbook|
|/automationAccounts/schedules/delete|Hiermee wordt een Azure Automation-planningsasset verwijderd|
|/automationAccounts/Schedules/Read|Een Azure Automation-planningsasset opgehaald|
|/automationAccounts/Schedules/Write|Maken of bijwerken van een Azure Automation-planningsasset|
|/automationAccounts/statistics/Read|Azure Automation-statistieken opgehaald|
|/automationAccounts/usages/Read|Gebruik Azure Automation opgehaald|
|/automationAccounts/Variables/DELETE|Hiermee wordt een variabel Azure Automation-asset verwijderd|
|/automationAccounts/Variables/Read|Hiermee wordt een variabel Azure Automation-asset gelezen|
|/automationAccounts/Variables/Write|Maken of bijwerken van een Azure Automation-variabelenactivum|
|/automationAccounts/watchers/streams/Read|Een Azure Automation-taakstroom voor watcher opgehaald|
|/automationAccounts/webhooks/delete|Hiermee wordt een Azure Automation-webhook verwijderd |
|/automationAccounts/webhooks/generateUri/Action|Een URI voor een Azure Automation-webhook gegenereerd|
|/automationAccounts/webhooks/Read|Hiermee wordt een Azure Automation-webhook gelezen|
|/automationAccounts/webhooks/Write|Maken of bijwerken van een Azure Automation-webhook|
|automationAccounts/schrijven|Maken of bijwerken van een Azure Automation-account|
|automationAccounts/schrijven|Maken of bijwerken van een Azure Automation-account|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

| Bewerking | Beschrijving |
|---|---|
|/b2cDirectories/DELETE|B2C-mapresource verwijderen|
|/b2cDirectories/Read|B2C-mapresource weergeven|
|b2cDirectories/schrijven|B2C-mapresource maken of bijwerken|
|/Operations/Read|Alle API-bewerkingen lezen die beschikbaar zijn voor de resourceprovider Microsoft.AzureActiveDirectory|
|registratie-/ actie|Abonnement voor de resourceprovider Microsoft.AzureActiveDirectory registreren|

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

| Bewerking | Beschrijving |
|---|---|
|Bewerkingen/leestijd|De eigenschappen van een resource provider-bewerking opgehaald|
|registratie-/ actie|Abonnement registreert bij de resourceprovider Microsoft.AzureStack|
|/Registrations/customerSubscriptions/DELETE|Hiermee verwijdert u een abonnement van de klant Azure Stack|
|/Registrations/customerSubscriptions/Read|De eigenschappen van een Azure-abonnement Stack klant opgehaald|
|/Registrations/customerSubscriptions/Write|Maken of bijwerken van een Azure-Stack klant-abonnement|
|/Registrations/DELETE|Hiermee verwijdert u een Azure-Stack-registratie|
|/Registrations/getActivationKey/Action|De nieuwste Azure-Stack activeringssleutel opgehaald|
|/Registrations/Products/listDetails/Action|Details voor een Azure-Stack Marketplace-product uitgebreid opgehaald|
|/Registrations/Products/Read|Haalt u de eigenschappen van een Azure-Stack Marketplace-product|
|/Registrations/Read|De eigenschappen van een Azure-Stack-registratie opgehaald|
|registraties/schrijven|Maken of bijwerken van een Azure-Stack-registratie|

## <a name="microsoftbatch"></a>Microsoft.Batch

| Bewerking | Beschrijving |
|---|---|
|/batchAccounts/Applications/DELETE|Een toepassing verwijderen|
|/batchAccounts/Applications/Read|Een lijst met toepassingen of haalt u de eigenschappen van een toepassing|
|/batchAccounts/Applications/Versions/Activate/Action|Hiermee activeert u een toepassingspakket|
|/batchAccounts/Applications/Versions/DELETE|Hiermee verwijdert u een toepassingspakket|
|/batchAccounts/Applications/Versions/Read|De eigenschappen van een toepassingspakket opgehaald|
|/batchAccounts/Applications/Versions/Write|Een nieuw pakket maken of bijwerken van een bestaand toepassingspakket|
|/batchAccounts/Applications/Write|Een nieuwe toepassing maken of bijwerken van een bestaande toepassing|
|/batchAccounts/certificateOperationResults/Read|De resultaten van een langdurige certificaatbewerking op een Batch-account opgehaald|
|/batchAccounts/Certificates/cancelDelete/Action|Hiermee annuleert u het verwijderen van een certificaat op een Batch-account is mislukt|
|/batchAccounts/Certificates/DELETE|Hiermee verwijdert u een certificaat van een Batch-account|
|/batchAccounts/Certificates/Read|De eigenschappen van een certificaat opgehaald of ingesteld vindt u certificaten in een Batch-account|
|/batchAccounts/Certificates/Write|Maakt een nieuw certificaat op een Batch-account of een bestaand certificaat updates|
|/batchAccounts/delete|Hiermee verwijdert u een Batch-account|
|/batchAccounts/listkeys/Action|Een lijst met sleutels voor een Batch-account toegang tot|
|/batchAccounts/operationResults/Read|De resultaten van een langdurige bewerking van de Batch-account opgehaald|
|/batchAccounts/poolOperationResults/Read|De resultaten van een langdurige bewerking van de groep van toepassingen in een Batch-account opgehaald|
|/batchAccounts/Pools/DELETE|Een groep verwijdert uit een Batch-account|
|/batchAccounts/Pools/disableAutoscale/Action|Schakelt automatisch schalen voor een Batch-pool-account|
|/batchAccounts/Pools/Read|Een lijst met toepassingen op een Batch-account of haalt u de eigenschappen van een groep|
|/batchAccounts/Pools/stopResize/Action|Stopt een lopende vergroten of verkleinen van de bewerking op een Batch-pool-account|
|/batchAccounts/Pools/upgradeOs/Action|Upgrades van het besturingssysteem van een Batch-pool-account|
|/batchAccounts/Pools/Write|Maakt een nieuwe pool voor een batchaccount of een bestaande toepassingen bijwerken|
|/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/batchAccounts/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/batchAccounts/providers/Microsoft.Insights/logDefinitions/Read|De beschikbare logboeken opgehaald voor de Batch-service|
|/batchAccounts/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor de Batch-service|
|/batchAccounts/Read|Een lijst met Batch-accounts of haalt u de eigenschappen van een Batch-account|
|/batchAccounts/regeneratekeys/Action|Opnieuw toegangssleutels voor een Batch-account|
|/batchAccounts/syncAutoStorageKeys/action|Toegangssleutels voor het opslagaccount automatisch is geconfigureerd voor een Batch-account wordt gesynchroniseerd|
|batchAccounts/schrijven|Een nieuwe Batch-account maken of bijwerken van een bestaande Batch-account|
|/Locations/checkNameAvailability/Action|Controleert of de accountnaam geldig en niet in gebruik is.|
|/Locations/Quotas/Read|Batch-quota van het opgegeven abonnement bij de opgegeven Azure-regio worden opgehaald|
|registratie-/ actie|Het abonnement voor de Batch-Resourceprovider geregistreerd en wordt het maken van de Batch-accounts|
|/ unregister/actie|Heft de registratie van het abonnement voor de Batch-Resourceprovider zo wordt voorkomen dat het maken van de Batch-accounts|

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

| Bewerking | Beschrijving |
|---|---|
|/clusters/DELETE|Hiermee verwijdert u een Batch AI-cluster|
|/clusters/Read|Een lijst met Batch AI clusters of haalt u de eigenschappen van een Batch AI-cluster|
|/clusters/remoteLoginInformation/Action|Een lijst met extern-aanmeldingsgegevens voor een Batch AI-cluster|
|clusters/schrijven|Een nieuw Batch AI-cluster maken of bijwerken van een bestaand Batch AI-cluster|
|/fileservers/DELETE|Hiermee verwijdert u een Batch AI-bestandsserver|
|/fileservers/Read|Een lijst met Batch AI bestandsservers of haalt u de eigenschappen van een Batch AI-bestandsserver|
|/fileservers/Resume/Action|Een Batch AI-bestandsserver wordt hervat|
|/fileservers/Suspend/Action|Een Batch AI-bestandsserver wordt onderbroken|
|bestandsservers/schrijven|Een nieuwe Batch AI-bestandsserver maken of bijwerken van een bestaande Batch AI-bestandsserver|
|/jobs/delete|Hiermee verwijdert u een Batch AI-taak|
|/jobs/Read|Een lijst met taken in Batch AI of haalt u de eigenschappen van een Batch AI-taak|
|/jobs/remoteLoginInformation/Action|Een lijst met extern-aanmeldingsgegevens voor een Batch AI-taak|
|/jobs/Terminate/Action|Een taak met Batch AI beëindigd|
|taken/schrijven|Een nieuwe Batch AI-taak maken of bijwerken van een bestaande Batch AI-taak|
|registratie-/ actie|Het abonnement voor de Batch AI-Resourceprovider geregistreerd en wordt het maken van Batch AI-resources|

## <a name="microsoftbilling"></a>Microsoft.Billing

| Bewerking | Beschrijving |
|---|---|
|/billingPeriods/Read|Een lijst met beschikbare facturering punten|
|/Invoices/Read|Een lijst met beschikbare facturen|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| Bewerking | Beschrijving |
|---|---|
|/mapApis/Delete|Verwijderbewerking|
|/mapApis/listSecrets/action|Lijst met geheimen weergeven|
|/mapApis/listSingleSignOnToken/Action|Verificatietoken voor eenmalige aanmelding voor de resource lezen|
|/mapApis/Read|Leesbewerking|
|/mapApis/regenerateKey/Action|De sleutel opnieuw genereren|
|/ mapApis/schrijven|Schrijfbewerking|
|Bewerkingen/leestijd|Beschrijving van de bewerking.|

## <a name="microsoftcache"></a>Microsoft.Cache

| Bewerking | Beschrijving |
|---|---|
|/ checknameavailability/actie|Hiermee wordt gecontroleerd of een naam beschikbaar is voor gebruik met een nieuwe Redis Cache|
|/Operations/Read|Hier worden de bewerkingen die 'Microsoft.Cache' provider ondersteunt.|
|/redis/DELETE|Volledige Redis-cache verwijderen|
|/redis/export/Action|Redis-gegevens exporteren naar vooraf ingestelde opslag-blobs in een opgegeven indeling|
|/redis/firewallRules/delete|IP-firewallregels voor een Redis Cache verwijderen|
|/redis/firewallRules/Read|De IP-firewallregels voor een Redis Cache ophalen|
|/redis/firewallRules/Write|De IP-firewallregels voor een Redis Cache bewerken|
|/redis/forceReboot/Action|Een cache-instantie geforceerd opnieuw opstarten met mogelijk gegevensverlies.|
|/redis/import/Action|Gegevens met een opgegeven indeling in Redis importeren vanuit meerdere blobs|
|/redis/linkedservers/delete|Gekoppelde server uit een Redis-cache verwijderen|
|/redis/linkedservers/Read|Gekoppelde servers die zijn gekoppeld aan een Redis-cache ophalen.|
|/redis/linkedservers/Write|Gekoppelde server aan een Redis-cache toevoegen|
|/redis/listKeys/Action|De waarde van de toegangssleutels voor Redis-cache weergeven in de beheerportal|
|/redis/listUpgradeNotifications/Read|De nieuwste upgrademeldingen weergeven voor de cachetenant.|
|/redis/Locations/operationresults/Read|Haalt het resultaat van een long waarvoor de koptekst 'Location' eerder werd geretourneerd naar de client opnieuw uitgevoerd|
|/redis/metricDefinitions/Read|Hiermee worden de beschikbare metrische gegevens voor een Redis-cache opgehaald|
|/redis/patchSchedules/DELETE|Het schema voor het toepassen van patches voor een Redis Cache verwijderen|
|/redis/patchSchedules/Read|Hiermee wordt het schema voor het toepassen van patches voor een Redis Cache opgehaald|
|/redis/patchSchedules/Write|Het schema voor het toepassen van patches voor een Redis Cache aanpassen|
|/redis/Read|De instellingen en configuratie van Redis-cache weergeven in de beheerportal|
|/redis/regenerateKey/Action|De waarde van de toegangssleutels voor Redis-cache wijzigen in de beheerportal|
|/redis/start/Action|Start een cache-instantie.|
|/redis/Stop/Action|Stop een cache-instantie.|
|redis/schrijven|De instellingen en configuratie van Redis-cache wijzigen in de beheerportal|
|registratie-/ actie|Hiermee wordt de resourceprovider Microsoft.Cache geregistreerd bij een abonnement|
|/ unregister/actie|Hiermee wordt de registratie van de resourceprovider Microsoft.Cache bij een abonnement ongedaan gemaakt|

## <a name="microsoftcapacity"></a>Microsoft.Capacity

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|De capaciteit resourceprovider geregistreerd en het maken van capaciteit resources maakt.|
|/ reservationorders/actie|Reservering bijwerken|
|/reservationorders/DELETE|Reservering verwijderen|
|/reservationorders/Read|Alle reserveringen lezen|
|/reservationorders/Reservations/Action|Reservering bijwerken|
|/reservationorders/Reservations/DELETE|Reservering verwijderen|
|/reservationorders/Reservations/Read|Alle reserveringen lezen|
|/reservationorders/Reservations/Revisions/Read|Alle reserveringen lezen|
|/reservationorders/Reservations/Write|Reservering maken|
|reservationorders/schrijven|Reservering maken|

## <a name="microsoftcdn"></a>Microsoft.Cdn

| Bewerking | Beschrijving |
|---|---|
|/ CheckNameAvailability/actie||
|/ CheckResourceUsage/actie||
|/edgenodes/DELETE||
|/edgenodes/Read||
|edgenodes/schrijven||
|/operationresults/DELETE||
|/operationresults/profileresults/CheckResourceUsage/Action||
|/operationresults/profileresults/DELETE||
|/operationresults/profileresults/endpointresults/CheckResourceUsage/Action||
|/operationresults/profileresults/endpointresults/customdomainresults/delete||
|/operationresults/profileresults/endpointresults/customdomainresults/ DisableCustomHttps/action||
|/operationresults/profileresults/endpointresults/customdomainresults/ EnableCustomHttps/action||
|/operationresults/profileresults/endpointresults/customdomainresults/read||
|/operationresults/profileresults/endpointresults/customdomainresults/write||
|/operationresults/profileresults/endpointresults/delete||
|/operationresults/profileresults/endpointresults/Load/Action||
|/operationresults/profileresults/endpointresults/originresults/delete||
|/operationresults/profileresults/endpointresults/originresults/Read||
|/operationresults/profileresults/endpointresults/originresults/Write||
|/operationresults/profileresults/endpointresults/Purge/Action||
|/operationresults/profileresults/endpointresults/Read||
|/operationresults/profileresults/endpointresults/start/Action||
|/operationresults/profileresults/endpointresults/Stop/Action||
|/operationresults/profileresults/endpointresults/ValidateCustomDomain/action||
|/operationresults/profileresults/endpointresults/Write||
|/operationresults/profileresults/GenerateSsoUri/action||
|/operationresults/profileresults/GetSupportedOptimizationTypes/action||
|/operationresults/profileresults/Read||
|/operationresults/profileresults/Write||
|/operationresults/Read||
|operationresults/schrijven||
|/Operations/Read||
|/Profiles/CheckResourceUsage/Action||
|/Profiles/DELETE||
|/Profiles/endpoints/CheckResourceUsage/Action||
|/profiles/endpoints/customdomains/delete||
|/profiles/endpoints/customdomains/DisableCustomHttps/action||
|/profiles/endpoints/customdomains/EnableCustomHttps/action||
|/Profiles/endpoints/customdomains/Read||
|/Profiles/endpoints/customdomains/Write||
|/Profiles/endpoints/DELETE||
|/Profiles/endpoints/Load/Action||
|/Profiles/endpoints/origins/DELETE||
|/Profiles/endpoints/origins/Read||
|/Profiles/endpoints/origins/Write||
|/Profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/Read|De diagnostische instellingen voor de resource opgehaald|
|/Profiles/endpoints/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instellingen voor de bron|
|/profiles/endpoints/providers/Microsoft.Insights/logDefinitions/read|De beschikbare logboeken opgehaald voor Microsoft.Cdn|
|/Profiles/endpoints/Purge/Action||
|/Profiles/endpoints/Read||
|/Profiles/endpoints/start/Action||
|/Profiles/endpoints/Stop/Action||
|/profiles/endpoints/ValidateCustomDomain/action||
|/Profiles/endpoints/Write||
|/Profiles/GenerateSsoUri/Action||
|/Profiles/GetSupportedOptimizationTypes/Action||
|/Profiles/Read||
|profielen/schrijven||
|registratie-/ actie|Hiermee wordt het abonnement voor de CDN-resourceprovider geregistreerd en wordt het maken van CDN-profielen mogelijk.|
|/ ValidateProbe/actie||

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| Bewerking | Beschrijving |
|---|---|
|/certificateOrders/Certificates/DELETE|Een bestaand certificaat verwijderen|
|/certificateOrders/Certificates/Read|De lijst met certificaten ophalen|
|/certificateOrders/Certificates/Write|Toevoegen van een nieuw certificaat of een bestaande bijgewerkt|
|/certificateOrders/Delete|Een bestaande AppServiceCertificate verwijderen|
|/certificateOrders/Operations/Read|Lijst van alle bewerkingen van het app service-certificaat is geregistreerd|
|/ certificateOrders/lezen|De lijst met CertificateOrders ophalen|
|/certificateOrders/reissue/Action|Geef een bestaande certificateorder|
|/certificateOrders/renew/Action|Een bestaande certificateorder vernieuwen|
|/certificateOrders/resendEmail/Action|Certificaat-e-mail opnieuw verzenden|
|/certificateOrders/resendRequestEmails/Action|Aanvraag e-mailberichten naar een ander e-mailadres verzenden|
|/certificateOrders/resendRequestEmails/Action|Site zegel voor een uitgegeven certificaat van de App-Service ophalen|
|/certificateOrders/retrieveCertificateActions/Action|De lijst met acties certificaat ophalen|
|/certificateOrders/retrieveEmailHistory/Action|Geschiedenis van de e-certificaat ophalen|
|/certificateOrders/verifyDomainOwnership/Action|Domeineigendom controleren|
|/ certificateOrders/schrijven|Een nieuwe certificateOrder toevoegen of een bestaande bijgewerkt|
|/provisionGlobalAppServicePrincipalInUserTenant/Action|Service-principal voor service-principal voor app inrichten|
|registratie-/ actie|Registreer de provider van Microsoft-Certificates resource voor het abonnement|
|/ validateCertificateRegistrationInformation/actie|Aankoop certificaatobject valideren zonder deze te verzenden|

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

| Bewerking | Beschrijving |
|---|---|
|/Capabilities/Read|Hiermee worden de mogelijkheden weergegeven|
|/ checkDomainNameAvailability/actie|Hiermee wordt gecontroleerd of een bepaald domeinnaam beschikbaar is.|
|/domainNames/Active/Write|Hiermee wordt de naam ingesteld van het actieve domein.|
|/domainNames/availabilitySets/Read|De beschikbaarheidsset voor de resource wordt weergegeven.|
|/domainNames/Capabilities/Read|Geeft de mogelijkheden voor domeinnaamgeving weer|
|/domainNames/delete|De domeinnamen voor resources verwijderen.|
|/domainNames/Extensions/DELETE|De domeinnaamextensies verwijderen.|
|/domainNames/Extensions/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de domeinnaamextensies gelezen.|
|/domainNames/Extensions/Read|Hiermee kunt u de domeinnaamextensies retourneren.|
|/domainNames/Extensions/Write|De domeinnaamextensies toevoegen.|
|/domainNames/internalLoadBalancers/delete|Een nieuwe interne taakverdeling verwijderen.|
|/domainNames/internalLoadBalancers/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de interne load balancers van domeinnamen gelezen.|
|/domainNames/internalLoadBalancers/read|De interne load balancers worden opgehaald.|
|/domainNames/internalLoadBalancers/write|Hiermee wordt een nieuwe interne taakverdeling gemaakt.|
|/domainNames/loadBalancedEndpointSets/operationStatuses/read|Hiermee wordt de bewerkingsstatus van de eindpuntsets voor de taakverdeling van de domeinnamen gelezen.|
|/domainNames/loadBalancedEndpointSets/read|Hiermee worden de eindpuntsets van de taakverdeling weergegeven|
|/domainNames/Read|De domeinnamen voor resources retourneren.|
|/domainNames/serviceCertificates/delete|De gebruikte servicecertificaten verwijderen.|
|/domainNames/serviceCertificates/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de servicecertificaten van domeinnamen gelezen.|
|/domainNames/serviceCertificates/Read|Hiermee worden de gebruikte servicecertificaten geretourneerd.|
|/domainNames/serviceCertificates/write|Hiermee worden de gebruikte servicecertificaten toegevoegd of gewijzigd.|
|/domainNames/slots/DELETE|Hiermee wordt een bepaald implementatiesleuf verwijderd.|
|/domainNames/slots/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de domeinnaamsleuven gelezen.|
|/domainNames/slots/Read|Hiermee worden de implementatiesleuven weergegeven.|
|/domainNames/slots/roles/extensionReferences/DELETE|De extensieverwijzing voor de implementatiesite verwijderen.|
|/domainNames/slots/roles/extensionReferences/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de extensieverwijzing van de rollen voor domeinnaamsleuven gelezen.|
|/domainNames/slots/roles/extensionReferences/Read|Hiermee kunt u de extensieverwijzing voor de implementatiesiterol retourneren.|
|/domainNames/slots/roles/extensionReferences/Write|Hiermee kunt u de extensieverwijzing voor de implementatiesiterol toevoegen of aanpassen.|
|/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee worden de diagnostische instellingen opgehaald.|
|/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee worden diagnostische instellingen toegevoegd of gewijzigd.|
|/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee worden de metrische definities opgehaald.|
|/domainNames/slots/roles/Read|Hiermee wordt de rol van de implementatiesleuf opgehaald.|
|/domainNames/slots/roles/roleInstances/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van rolinstanties van rollen voor domeinnaamsleuven gelezen.|
|/domainNames/slots/roles/roleInstances/Read|Hiermee wordt het rolexemplaar opgehaald.|
|/domainNames/slots/roles/roleInstances/rebuild/Action|Hiermee wordt de rolinstantie herbouwd.|
|/domainNames/slots/roles/roleInstances/reimage/Action|Hiermee wordt de installatiekopie van het rolexemplaar opnieuw toegepast.|
|/domainNames/slots/roles/roleInstances/restart/Action|Hiermee worden rolinstanties opnieuw gestart.|
|/domainNames/slots/start/Action|Hiermee wordt een implementatiesleuf gestart.|
|/domainNames/slots/State/start/Write|Hiermee wordt de status van de implementatiesleuf gewijzigd naar Gestopt.|
|/domainNames/slots/State/Stop/Write|Hiermee wordt de status van de implementatiesleuf gewijzigd naar Gestart.|
|/domainNames/slots/Stop/Action|Hiermee wordt de implementatiesleuf tijdelijk stopgezet.|
|/domainNames/slots/upgradeDomain/Write|Hiermee wordt stap voor stap een domeinupgrade uitgevoerd.|
|/domainNames/slots/Write|Hiermee wordt de implementatie gemaakt of bijgewerkt.|
|/domainNames/swap/Action|Hiermee wordt de staging-site gewisseld naar de productiesite.|
|domainNames/schrijven|Hiermee worden de domeinnamen voor resources toegevoegd of gewijzigd.|
|/ moveSubscriptionResources/actie|Verplaats alle klassieke resources naar een ander abonnement.|
|/operatingSystemFamilies/Read|Hiermee wordt een lijst met de gastbesturingssysteemfamilies gemaakt die beschikbaar zijn in Microsoft Azure. Ook worden de besturingssysteemversies weergegeven die beschikbaar zijn voor elke serie.|
|/operatingSystems/Read|Hiermee wordt een lijst van de versies van gastbesturingssystemen gemaakt die momenteel beschikbaar zijn in Microsoft Azure.|
|/Quotas/Read|Hiermee worden de quota voor het abonnement opgehaald.|
|registratie-/ actie|Registreren bij klassieke berekening|
|/resourceTypes/skus/Read|Haalt de SKU-lijst op voor ondersteunde resourcetypen.|
|/validateSubscriptionMoveAvailability/action|De beschikbaarheid van het abonnement voor de klassieke verplaatsingsbewerking valideren.|
|/virtualMachines/associatedNetworkSecurityGroups/delete|Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan de virtuele machine is gekoppeld.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read|Hiermee wordt de bewerkingsstatus van virtuele machines die zijn gekoppeld aan netwerkbeveiligingsgroepen gelezen.|
|/virtualMachines/associatedNetworkSecurityGroups/read|Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan de virtuele machine is gekoppeld.|
|/virtualMachines/associatedNetworkSecurityGroups/write|Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan de virtuele machine is gekoppeld.|
|/virtualMachines/asyncOperations/Read|Hiermee worden de mogelijk asynchrone bewerkingen opgehaald|
|/virtualMachines/attachDisk/Action|Hiermee wordt een gegevensschijf gekoppeld aan een virtuele machine.|
|/virtualMachines/delete|Hiermee worden virtuele machines verwijderd.|
|/virtualMachines/detachDisk/Action|Hiermee wordt een gegevensschijf losgekoppeld van een virtuele machine.|
|/virtualMachines/Disks/Read|Hiermee wordt een lijst met gegevensschijven opgehaald|
|/virtualMachines/downloadRemoteDesktopConnectionFile/action|Hiermee wordt het RDP-bestand voor de virtuele machine gedownload.|
|/virtualMachines/extensions/operationStatuses/read|Hiermee wordt de bewerkingsstatus van extensies van virtuele machines gelezen.|
|/virtualMachines/Extensions/Read|Hiermee wordt de extensie van de virtuele machine opgehaald.|
|/virtualMachines/Extensions/Write|Hiermee wordt de extensie voor de virtuele machine ingesteld.|
|/virtualMachines/metrics/Read|Hiermee worden de metrische gegevens opgehaald.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete|Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan de netwerkinterface is gekoppeld.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/ operationStatuses/read|Hiermee wordt de bewerkingsstatus van virtuele machines die zijn gekoppeld aan netwerkbeveiligingsgroepen gelezen.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read|Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan de netwerkinterface is gekoppeld.|
|/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write|Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan de netwerkinterface is gekoppeld.|
|/virtualMachines/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van virtuele machines gelezen.|
|/virtualMachines/performMaintenance/Action|Hiermee wordt onderhoud van de virtuele machine uitgevoerd.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read|Hiermee worden de diagnostische instellingen opgehaald.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write|Hiermee worden diagnostische instellingen toegevoegd of gewijzigd.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read|Hiermee worden de metrische definities opgehaald.|
|/virtualMachines/Read|Hiermee wordt een lijst met virtuele machines opgehaald.|
|/virtualMachines/redeploy/Action|Hiermee wordt de virtuele machine opnieuw geïmplementeerd.|
|/virtualMachines/restart/Action|Hiermee worden virtuele machines opnieuw gestart.|
|/virtualMachines/Shutdown/Action|Hiermee wordt de virtuele machine afgesloten.|
|/virtualMachines/start/Action|Hiermee wordt de virtuele machine gestart.|
|/virtualMachines/Stop/Action|Hiermee wordt de virtuele machine gestopt.|
|virtuele machines/schrijven|Hiermee worden virtuele machines toegevoegd of gewijzigd.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| Bewerking | Beschrijving |
|---|---|
|/gatewaySupportedDevices/Read|De lijst met ondersteunde apparaten wordt opgehaald.|
|/networkSecurityGroups/delete|Hiermee wordt de netwerkbeveiligingsgroep verwijderd.|
|/networkSecurityGroups/operationStatuses/read|Hiermee wordt de bewerkingsstatus van de netwerkbeveiligingsgroep gelezen.|
|/networkSecurityGroups/read|Hiermee wordt de netwerkbeveiligingsgroep opgehaald.|
|/networkSecurityGroups/securityRules/delete|Hiermee wordt de beveiligingsregel verwijderd.|
|/networkSecurityGroups/securityRules/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de beveiligingsregels voor de netwerkbeveiligingsgroep gelezen.|
|/networkSecurityGroups/securityRules/Read|Hiermee wordt de beveiligingsregel opgehaald.|
|/networkSecurityGroups/securityRules/write|Hiermee wordt een beveiligingsregel toegevoegd of bijgewerkt.|
|/networkSecurityGroups/write|Hiermee wordt een nieuwe netwerkbeveiligingsgroep toegevoegd.|
|/Quotas/Read|Hiermee worden de quota voor het abonnement opgehaald.|
|registratie-/ actie|Registreren bij klassiek netwerk|
|/reservedIps/delete|Hiermee wordt een gereserveerd IP verwijderd.|
|/reservedIps/join/Action|Deelnemen aan een gereserveerde IP|
|/reservedIps/link/action|Een gereserveerde IP koppelen|
|/reservedIps/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de gereserveerde IP's gelezen.|
|/reservedIps/read|Hiermee worden de gereserveerde IP's opgehaald.|
|reservedip/schrijven|Een nieuw gereserveerd IP toevoegen|
|/virtualNetworks/capabilities/read|Hiermee worden de mogelijkheden weergegeven|
|/virtualNetworks/checkIPAddressAvailability/Action|Hiermee wordt de beschikbaarheid van een bepaald IP-adres in een virtueel netwerk gecontroleerd.|
|/virtualNetworks/delete|Hiermee wordt het virtuele netwerk verwijderd.|
|/virtualNetworks/gateways/clientRevokedCertificates/delete|Hiermee wordt de intrekking van een clientcertificaat ongedaan gemaakt.|
|/virtualNetworks/gateways/clientRevokedCertificates/read|De ingetrokken clientcertificaten lezen.|
|/virtualNetworks/gateways/clientRevokedCertificates/write|Hiermee wordt een clientcertificaat ingetrokken.|
|/virtualNetworks/gateways/clientRootCertificates/delete|Hiermee wordt het clientcertificaat voor de gateway van het virtuele netwerk verwijderd.|
|/virtualNetworks/gateways/clientRootCertificates/download/action|Hiermee wordt een certificaat gedownload op basis van miniatuur.|
|/virtualNetworks/gateways/clientRootCertificates/listPackage/action|Het certificaatpakket van de virtuele netwerkgateway wordt weergegeven.|
|/virtualNetworks/gateways/clientRootCertificates/read|Clienthoofdcertificaten zoeken.|
|/virtualNetworks/gateways/clientRootCertificates/write|Hiermee wordt een nieuw clienthoofdcertificaat geüpload.|
|/virtualNetworks/gateways/connections/connect/action|Hiermee wordt een gatewayverbinding tussen sites ingesteld.|
|/virtualNetworks/gateways/connections/disconnect/action|Hiermee wordt een gatewayverbinding tussen sites verbroken.|
|/virtualNetworks/gateways/Connections/Read|Hiermee wordt de lijst met verbindingen opgehaald.|
|/virtualNetworks/gateways/Connections/test/Action|Hiermee wordt een gatewayverbinding tussen sites getest.|
|/virtualNetworks/gateways/DELETE|De gateway van het virtuele netwerk wordt verwijderd.|
|/virtualNetworks/gateways/downloadDeviceConfigurationScript/action|Het configuratiescript voor het apparaat wordt gedownload.|
|/virtualNetworks/gateways/downloadDiagnostics/action|Hiermee worden de diagnotische gegevens van de gateway gedownload.|
|/virtualNetworks/gateways/listCircuitServiceKey/action|Hiermee wordt de circuitservicesleutel opgehaald.|
|/virtualNetworks/gateways/listPackage/Action|Het gatewaypakket voor het virtuele netwerk wordt weergegeven.|
|/virtualNetworks/gateways/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de virtuele netwerkgateways gelezen.|
|/virtualNetworks/gateways/Packages/Read|Het gatewaypakket voor het virtuele netwerk wordt opgehaald.|
|/virtualNetworks/gateways/Read|De gateways voor het virtuele netwerk worden opgehaald.|
|/virtualNetworks/gateways/startDiagnostics/action|Hiermee wordt de diagnostische test voor de gateway van het virtuele netwerk gestart.|
|/virtualNetworks/gateways/stopDiagnostics/action|Hiermee wordt de diagnostische test voor de gateway van het virtuele netwerk gestopt.|
|/virtualNetworks/gateways/Write|Een gateway voor het virtuele netwerk wordt toegevoegd.|
|/virtualNetworks/join/Action|Hiermee wordt de deelname aan het virtuele netwerk uitgevoerd.|
|/virtualNetworks/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de virtuele netwerken gelezen.|
|/virtualNetworks/peer/Action|Hiermee wordt een virtueel netwerk via peercommunicatie gebruikt met een ander virtueel netwerk.|
|/virtualNetworks/read|Het virtuele netwerk wordt opgehaald.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete|Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan het subnet is gekoppeld.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read|Hiermee wordt de bewerkingsstatus van de netwerkbeveiligingsgroep die aan het subnet van het virtueel netwerk is gekoppeld gelezen.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/read|Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan het subnet is gekoppeld.|
|/virtualNetworks/subnets/associatedNetworkSecurityGroups/write|Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan het subnet is gekoppeld.|
|virtualNetworks/schrijven|Een nieuw virtueel netwerk wordt toegevoegd.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| Bewerking | Beschrijving |
|---|---|
|/Capabilities/Read|Hiermee worden de mogelijkheden weergegeven|
|/ checkStorageAccountAvailability/actie|Hiermee wordt gecontroleerd of een opslagaccount beschikbaar is.|
|/Disks/Read|Hiermee wordt de opslagaccountschijf geretourneerd.|
|/images/Read|Hiermee wordt de afbeelding geretourneerd.|
|/osImages/Read|De installatiekopie van het besturingssysteem herstellen.|
|/publicImages/Read|Hiermee wordt de openbare installatiekopie voor de virtuele machine opgehaald.|
|/Quotas/Read|Hiermee worden de quota voor het abonnement opgehaald.|
|registratie-/ actie|Registreren bij klassieke opslag|
|/storageAccounts/delete|Hiermee wordt het opslagaccount verwijderd.|
|/storageAccounts/disks/delete|Hiermee wordt een opgegeven opslagaccountschijf verwijderd.|
|/storageAccounts/Disks/operationStatuses/Read|Hiermee wordt de bewerkingsstatus voor de resource ingelezen.|
|/storageAccounts/disks/read|Hiermee wordt de opslagaccountschijf geretourneerd.|
|/storageAccounts/disks/write|Hiermee wordt een opslagaccountschijf toegevoegd.|
|/storageAccounts/images/delete|Hiermee verwijdert u een installatiekopie van het opgegeven opslagaccount.|
|/storageAccounts/images/Read|Retourneert de installatiekopie van het opslagaccount.|
|/storageAccounts/listKeys/Action|Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven.|
|/storageAccounts/operationStatuses/Read|Hiermee wordt de bewerkingsstatus voor de resource ingelezen.|
|/storageAccounts/osImages/DELETE|Hiermee wordt de betreffende installatiekopie van het besturingssysteem van een opslagaccount verwijderd.|
|/storageAccounts/osImages/Read|Hiermee wordt de installatiekopie van het besturingssysteem van een opslagaccount hersteld.|
|/storageAccounts/Read|Opslagaccount met het opgegeven account wordt geretourneerd.|
|/storageAccounts/regenerateKey/Action|De bestaande toegangssleutels voor het opslagaccount worden geregenereerd.|
|/storageAccounts/Services/diagnosticSettings/Read|Hiermee worden de diagnostische instellingen opgehaald.|
|/storageAccounts/Services/diagnosticSettings/Write|Hiermee worden diagnostische instellingen toegevoegd of gewijzigd.|
|/storageAccounts/Services/metricDefinitions/Read|Hiermee worden de metrische definities opgehaald.|
|/storageAccounts/Services/metrics/Read|Hiermee worden de metrische gegevens opgehaald.|
|/storageAccounts/Services/Read|De beschikbare services ophalen.|
|/storageAccounts/write|Hiermee wordt een nieuw opslagaccount toegevoegd.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| Bewerking | Beschrijving |
|---|---|
|/accounts/DELETE|API-accounts verwijderen|
|/accounts/listKeys/Action|Een lijst met sleutels maken|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee worden de beschikbare metrische gegevens opgehaald voor Cognitive Services.|
|/Accounts/read|Hiermee worden API-accounts gelezen.|
|/accounts/regenerateKey/Action|Sleutel opnieuw genereren|
|/accounts/skus/Read|Hiermee worden beschikbare SKU's voor een bestaande resource gelezen.|
|/accounts/usages/Read|Het quotumgebruik voor een bestaande resource ophalen.|
|accounts/schrijven|Hiermee worden API-accounts geschreven.|
|/Locations/checkSkuAvailability/Action|Hiermee worden beschikbare SKU's voor een abonnement gelezen.|
|Bewerkingen/leestijd|Lijst van alle beschikbare bewerkingen|
|registratie-/ actie|Abonnement voor cognitieve Services registreren|
|/skus/Read|Beschikbare SKU's leest voor Services cognitieve.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| Bewerking | Beschrijving |
|---|---|
|RateCard/leestijd|Retourneert bieden gegevens, metagegevens van de resource/meter en tarieven voor het opgegeven abonnement.|
|UsageAggregates/leestijd|Haalt de Microsoft Azure verbruik door een abonnement. Het resultaat bevat statistische gegevens over het gebruik, abonnement en de resource verwante gegevens, op een bepaalde periode.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| Bewerking | Beschrijving |
|---|---|
|/availabilitySets/DELETE|Hiermee wordt de beschikbaarheidsset verwijderd|
|/availabilitySets/Read|Hiermee worden de eigenschappen van een beschikbaarheidsset opgehaald|
|/availabilitySets/vmSizes/Read|Lijst met beschikbare grootten voor het maken of bijwerken van een virtuele machine in de beschikbaarheidsset weergeven|
|/ availabilitySets/schrijven|Hiermee wordt een nieuwe beschikbaarheidsset gemaakt of een bestaande bijgewerkt|
|/disks/beginGetAccess/action|Hiermee wordt de SAS-URI van de schijf opgehaald voor blob-toegang|
|/Disks/DELETE|Hiermee wordt de schijf verwijderd|
|/disks/endGetAccess/action|Hiermee wordt de SAS-URI van de schijf ingetrokken|
|/Disks/Read|Hiermee worden de eigenschappen van een schijf opgehaald|
|schijven/schrijven|Hiermee wordt een nieuwe schijf gemaakt of een bestaande bijgewerkt|
|/images/DELETE|Hiermee wordt de installatiekopie verwijderd|
|/images/Read|Ga naar de eigenschappen van de installatiekopie|
|installatiekopieën van/schrijven|Hiermee wordt een nieuwe installatiekopie gemaakt of een bestaande bijgewerkt|
|/Locations/capsOperations/Read|De status van een asynchrone bewerking van Caps opgehaald|
|/Locations/diskOperations/Read|Hiermee wordt de status van een asynchrone schijfbewerking opgehaald|
|/Locations/Operations/Read|Hiermee wordt de status van een asynchrone bewerking opgehaald|
|/Locations/Publishers/artifacttypes/Offers/Read|De eigenschappen van een Platform installatiekopie bieden opgehaald|
|/locations/publishers/artifacttypes/offers/skus/read|De eigenschappen van een installatiekopie Platform Sku ophalen|
|/Locations/Publishers/artifacttypes/Offers/skus/Versions/Read|De eigenschappen van een versie van de installatiekopie Platform opgehaald|
|/Locations/Publishers/artifacttypes/types/Read|De eigenschappen van een VMExtension Type opgehaald|
|/Locations/Publishers/artifacttypes/types/Versions/Read|De eigenschappen van een versie VMExtension opgehaald|
|/Locations/Publishers/Read|De eigenschappen van een uitgever opgehaald|
|/Locations/runCommands/Read|Hiermee wordt een lijst gemaakt van de beschikbare opdrachten op de locatie|
|/Locations/usages/Read|Hiermee worden servicelimieten en huidige gebruikshoeveelheden opgehaald voor de rekenresources van het abonnement op een locatie|
|/Locations/vmSizes/Read|Hiermee wordt een lijst weergegeven met de beschikbare grootten van virtuele machines op een locatie|
|/Operations/Read|Hiermee wordt een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.Compute-resourceprovider|
|registratie-/ actie|Hiermee wordt het abonnement bij de Microsoft.Compute-resourceprovider geregistreerd|
|/restorePointCollections/delete|Hiermee worden de herstelpuntverzameling en de hierin opgenomen herstelpunten verwijderd|
|/restorePointCollections/Read|Hiermee worden de eigenschappen van een herstelpuntverzameling opgehaald|
|/restorePointCollections/restorePoints/DELETE|Hiermee wordt het herstelpunt verwijderd|
|/restorePointCollections/restorePoints/Read|Hiermee worden de eigenschappen van een herstelpunt opgehaald|
|/restorePointCollections/restorePoints/retrieveSasUris/action|Hiermee worden de eigenschappen van een herstelpunt en de URI's van de SAS-blob opgehaald|
|/restorePointCollections/restorePoints/Write|Hiermee wordt een nieuw herstelpunt gemaakt|
|restorePointCollections/schrijven|Hiermee wordt een nieuwe herstelpuntverzameling gemaakt of een bestaande bijgewerkt|
|/sharedVMImages/DELETE|Hiermee verwijdert u de SharedVMImage|
|/sharedVMImages/Read|De eigenschappen van een SharedVMImage opgehaald|
|/sharedVMImages/Versions/DELETE|Een SharedVMImageVersion verwijderen|
|/sharedVMImages/Versions/Read|De eigenschappen van een SharedVMImageVersion opgehaald|
|/sharedVMImages/Versions/Replicate/Action|Een SharedVMImageVersion repliceren naar doelregio|
|/sharedVMImages/Versions/Write|Een nieuwe SharedVMImageVersion maken of bijwerken van een bestaande|
|sharedVMImages/schrijven|Hiermee maakt u een nieuwe SharedVMImage of een bestaande bijgewerkt|
|/skus/Read|Hiermee haalt u de lijst van de Microsoft.Compute-SKU's beschikbaar voor uw abonnement|
|/Snapshots/beginGetAccess/Action|De SAS-URI van de momentopname ophalen voor blob-toegang|
|/Snapshots/DELETE|Hiermee wordt een momentopname verwijderd|
|/snapshots/endGetAccess/action|De SAS-URI van de momentopname intrekken|
|/Snapshots/Read|Hiermee worden de eigenschappen van een momentopname opgehaald|
|momentopnamen van/schrijven|Hiermee wordt een nieuwe momentopname gemaakt of een bestaande bijgewerkt|
|/virtualMachines/Capture/Action|Hiermee wordt de virtuele machine vastgelegd via het kopiëren van virtuele harde schijven en wordt een sjabloon gegenereerd waarmee soortgelijke virtuele machines kunnen worden gemaakt|
|/virtualMachines/convertToManagedDisks/action|Hiermee worden de op blobs gebaseerde schijven van de virtuele machine geconverteerd naar beheerde schijven|
|/virtualMachines/deallocate/Action|Hiermee wordt de virtuele machine uitgeschakeld en worden de rekenresources vrijgegeven|
|/virtualMachines/delete|Hiermee wordt de virtuele machine verwijderd|
|/virtualMachines/extensions/delete|Hiermee wordt de extensie van de virtuele machine verwijderd|
|/virtualMachines/Extensions/Read|Hiermee worden de eigenschappen van de extensie van een virtuele machine opgehaald|
|/virtualMachines/Extensions/Write|Hiermee wordt een nieuwe extensie van een virtuele machine gemaakt of een bestaande extensie bijgewerkt|
|/virtualMachines/generalize/Action|Hiermee wordt de status van de virtuele machine ingesteld op Gegeneraliseerd en wordt de virtuele machine voorbereid voor vastleggen|
|/virtualMachines/instanceView/Read|Hiermee wordt de gedetailleerde runtimestatus van de virtuele machine en bijbehorende resources opgehaald|
|/virtualMachines/performMaintenance/Action|Hiermee wordt de onderhoudsbewerking uitgevoerd op de VM.|
|/virtualMachines/powerOff/Action|Hiermee wordt de virtuele machine uitgeschakeld. Houd er rekening mee dat de virtuele machine wordt nog wel gefactureerd.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read|Hiermee worden de metrische definities van de virtuele machine gelezen|
|/virtualMachines/Read|Hiermee worden de eigenschappen van een virtuele machine opgehaald|
|/virtualMachines/redeploy/Action|Hiermee wordt de virtuele machine opnieuw geïmplementeerd|
|/virtualMachines/reimage/Action|Hiermee wordt een installatiekopie teruggezet van de virtuele machine die een differentiërende schijf gebruikt.|
|/virtualMachines/restart/Action|Hiermee wordt de virtuele machine opnieuw gestart|
|/virtualMachines/runCommand/action|Hiermee wordt een vooraf gedefinieerd script uitgevoerd op de virtuele machine|
|/virtualMachines/start/Action|Hiermee wordt de virtuele machine gestart|
|/virtualMachines/vmSizes/Read|Hiermee wordt een lijst weergegeven met de beschikbare grootten waarnaar de virtuele machine kan worden bijgewerkt|
|virtuele machines/schrijven|Hiermee wordt een nieuwe virtuele machine gemaakt of een bestaande bijgewerkt|
|/virtualMachineScaleSets/deallocate/action|Hiermee worden de rekenresources voor de instanties van de virtuele-machineschaalset uitgeschakeld en vrijgegeven |
|/virtualMachineScaleSets/delete|Hiermee wordt de virtuele-machineschaalset verwijderd|
|/virtualMachineScaleSets/delete/action|Hiermee worden de instanties van de virtuele-machineschaalset verwijderd|
|/virtualMachineScaleSets/extensions/delete|Hiermee wordt de extensie van de virtuele-machineschaalset verwijderd|
|/virtualMachineScaleSets/extensions/read|Hiermee worden de eigenschappen voor de extensie van een virtuele-machineschaalset opgehaald|
|/virtualMachineScaleSets/extensions/write|Hiermee wordt een nieuwe extensie van de virtuele-machineschaalset gemaakt of een bestaande extensie bijgewerkt|
|/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action|Helpt u handmatig de domeinen van de update platform van een service fabric virtuele-Machineschaalset te voltooien van een update in behandeling is vastgelopen|
|/virtualMachineScaleSets/instanceView/read|Hiermee wordt de instantieweergave van de virtuele-machineschaalset opgehaald|
|/virtualMachineScaleSets/manualUpgrade/action|Hiermee worden instanties handmatig bijgewerkt naar het laatste model van de virtuele-machineschaalset|
|/virtualMachineScaleSets/networkInterfaces/read|Eigenschappen van alle netwerkinterfaces van een virtuele-Machineschaalset opgehaald|
|/virtualMachineScaleSets/osUpgradeHistory/Read|De geschiedenis van upgrades voor het besturingssysteem voor een virtuele-Machineschaalset opgehaald|
|/virtualMachineScaleSets/performMaintenance/action|Gepland onderhoud uitgevoerd op de exemplaren van de virtuele-Machineschaalset|
|/virtualMachineScaleSets/powerOff/action|Hiermee worden de instanties van de virtuele-machineschaalset uitgeschakeld|
|/virtualMachineScaleSets/providers/Microsoft.Insights/metricDefinitions/read|Hiermee worden de metrische definities van de virtuele-machineschaalset gelezen|
|/virtualMachineScaleSets/publicIPAddresses/read|Eigenschappen van alle openbare IP-adressen van een virtuele-Machineschaalset opgehaald|
|/virtualMachineScaleSets/read|Hiermee worden de eigenschappen van de virtuele-machineschaalset opgehaald|
|/virtualMachineScaleSets/redeploy/action|De exemplaren van de virtuele-Machineschaalset opnieuw implementeren|
|/virtualMachineScaleSets/reimage/action|Hiermee worden installatiekopieën voor de instanties van de virtuele-machineschaalset teruggezet|
|/virtualMachineScaleSets/restart/action|Hiermee worden de instanties van de virtuele-machineschaalset opnieuw gestart|
|/virtualMachineScaleSets/rollingUpgrades/cancel/action|Hiermee wordt de roulerende upgrade van een virtuele-machineschaalset geannuleerd|
|/virtualMachineScaleSets/rollingUpgrades/read|De meest recente status van de roulerende upgrade voor een virtuele-machineschaalset ophalen|
|/virtualMachineScaleSets/Scale/Action|Controleer of een bestaande virtuele-machineschaalset kan in-/uitschalen naar een opgegeven aantal instanties|
|/virtualMachineScaleSets/skus/Read|Hiermee wordt een lijst weergegeven van de geldige SKU's voor een bestaande virtuele-machineschaalset|
|/virtualMachineScaleSets/start/Action|Hiermee worden de instanties van de virtuele-machineschaalset gestart|
|/virtualMachineScaleSets/virtualMachines/deallocate/action|Hiermee worden de rekenresources voor een virtuele machine in een VM-schaalset uitgeschakeld en vrijgegeven|
|/virtualMachineScaleSets/virtualMachines/delete|Een specifieke virtuele machine uit een VM-schaalset verwijderen.|
|/virtualMachineScaleSets/virtualMachines/instanceView/Read|Hiermee wordt de instantieweergave van een virtuele machine in een VM-schaalset opgehaald.|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/ ipConfigurations/publicIPAddresses/read|Eigenschappen van openbare IP-adres die zijn gemaakt met behulp van virtuele-Machineschaalset opgehaald. Virtuele-Machineschaalset kunt mag maximaal één openbare IP per ipconfiguration (privé-IP) maken|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read|Eigenschappen van een of alle IP-configuraties van een netwerkinterface die is gemaakt met behulp van virtuele-Machineschaalset opgehaald. IP-configuraties vertegenwoordigen persoonlijke IP-adressen|
|/virtualMachineScaleSets/virtualMachines/networkInterfaces/read|Eigenschappen van een of alle netwerkinterfaces van een virtuele machine gemaakt met behulp van virtuele-Machineschaalset opgehaald|
|/virtualMachineScaleSets/virtualMachines/performMaintenance/action|Gepland onderhoud uitgevoerd op een exemplaar van de virtuele Machine in een virtuele-Machineschaalset|
|/virtualMachineScaleSets/virtualMachines/powerOff/action|Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset uitgeschakeld.|
|/virtualMachineScaleSets/virtualMachines/providers/ Microsoft.Insights/metricDefinitions/read|Hiermee worden de metrische definities in de virtuele-machineschaalset gelezen|
|/virtualMachineScaleSets/virtualMachines/read|Hiermee worden de eigenschappen van een virtuele machine in een VM-schaalset opgehaald|
|/virtualMachineScaleSets/virtualMachines/redeploy/action|De instantie van een virtuele Machine in een virtuele-Machineschaalset redeploys|
|/virtualMachineScaleSets/virtualMachines/reimage/action|Hiermee wordt de installatiekopie van een instantie van een virtuele machine in een virtuele-machineschaalset teruggezet.|
|/virtualMachineScaleSets/virtualMachines/restart/action|Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset opnieuw gestart.|
|/virtualMachineScaleSets/virtualMachines/start/Action|Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset gestart.|
|/virtualMachineScaleSets/virtualMachines/write|Hiermee worden de eigenschappen van een virtuele machine in een VM-schaalset bijgewerkt|
|/virtualMachineScaleSets/write|Hiermee wordt een nieuwe virtuele-machineschaalset gemaakt of een bestaande bijgewerkt|

## <a name="microsoftconsumption"></a>Microsoft.Consumption

| Bewerking | Beschrijving |
|---|---|
|/balances/Read|Lijst van het gebruik van de samenvatting voor een factureringsperiode voor een beheergroep.|
|/budgets/Read|Lijst van de budgetten voor een abonnement of een beheergroep.|
|budgetten/schrijven|Maakt, bijwerken en verwijderen van de budgetten voor een abonnement of een beheergroep.|
|/marketplaces/Read|Lijst van de gebruiksdetails van de marketplace-resource voor een scope voor abonnementen EA en WebDirect.|
|/Operations/Read|Lijst van alle ondersteunde bewerkingen door Microsoft.Consumption resourceprovider.|
|/pricesheets/Read|Een lijst met Pricesheets gegevens voor een abonnement of een beheergroep.|
|/reservationDetails/read|Lijst van de details van tapegebruik voor gereserveerde exemplaren door reservering volgorde of beheer van groepen. De gegevens details zijn per exemplaar per dag niveau.|
|/reservationSummaries/Read|Lijst van het gebruik van de samenvatting voor gereserveerde exemplaren door reservering volgorde of beheer van groepen. De gegevens van de samenvatting bevindt zich op niveau van de maand of per dag.|
|/reservationTransactions/Read|Lijst van de transactiegeschiedenis voor gereserveerde exemplaren door beheergroepen.|
|/terms/Read|Lijst van de voorwaarden voor een abonnement of een beheergroep.|
|/usageDetails/Read|Lijst van de gebruiksgegevens voor een scope voor abonnementen EA en WebDirect.|

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

| Bewerking | Beschrijving |
|---|---|
|/containerGroups/containers/Logs/Read|Logbestanden voor een specifieke container ophalen.|
|/containerGroups/delete|De specifieke containergroep verwijderen.|
|/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read|Hiermee haalt u de diagnostische instelling voor de containergroep.|
|/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write|Maken of bijwerken van de diagnostische instelling voor de containergroep.|
|/containerGroups/providers/Microsoft.Insights/metricDefinitions/read|Hiermee haalt u de beschikbare metrische gegevens voor containergroep.|
|/containerGroups/Read|Alle containergroepen ophalen.|
|containerGroups/schrijven|Een specifieke containergroep maken of bijwerken.|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| Bewerking | Beschrijving |
|---|---|
|/checkNameAvailability/Read|Controleert of de containernaam van het register beschikbaar voor gebruik is.|
|/Locations/operationResults/Read|Een asynchrone bewerkingsresultaat opgehaald|
|/Operations/Read|Een lijst met alle beschikbare Azure Container register REST-API-bewerkingen|
|registratie-/ actie|Het abonnement voor de container register-resourceprovider geregistreerd en het maken van de container registers maakt.|
|/registries/DELETE|Hiermee verwijdert u een container-register.|
|/registries/eventGridFilters/delete|Een raster gebeurtenisfilter verwijdert uit een container-register.|
|/registries/eventGridFilters/Read|De eigenschappen van de opgegeven gebeurtenis raster filter opgehaald of een lijst met alle gebeurtenis raster filters voor het register opgegeven container.|
|/registries/eventGridFilters/Write|Maken of bijwerken van een raster gebeurtenisfilter voor een container-registersleutel met de opgegeven parameters.|
|/registries/listCredentials/Action|Hier worden de aanmeldingsreferenties voor het register opgegeven container.|
|/registries/listUsages/Read|Bevat de quota voor gebruik voor het register opgegeven container.|
|/registries/operationStatuses/Read|De status van een register asynchrone bewerking opgehaald|
|/registries/Read|De eigenschappen van het register opgegeven container opgehaald of een lijst met alle container registers onder de opgegeven resourcegroep of abonnement.|
|/registries/regenerateCredential/Action|Genereert een van de aanmeldingsreferenties voor het register opgegeven container.|
|/registries/replications/DELETE|Hiermee verwijdert u een replicatie uit een container-register.|
|/registries/replications/operationStatuses/Read|De status van een replicatie asynchrone bewerking opgehaald|
|/registries/replications/Read|De eigenschappen van de opgegeven replicatie opgehaald of een lijst met alle replicaties voor het register opgegeven container.|
|/registries/replications/Write|Maken of bijwerken van een replicatie voor een container-registersleutel met de opgegeven parameters.|
|/registries/webhooks/DELETE|Hiermee wordt een webhook verwijderd uit het register van een container.|
|/registries/webhooks/getCallbackConfig/Action|Hiermee wordt de configuratie van de service-URI en aangepaste headers voor de webhook.|
|/registries/webhooks/listEvents/Action|Een lijst met recente gebeurtenissen voor de opgegeven webhook.|
|/registries/webhooks/operationStatuses/Read|De status van een webhook asynchrone bewerking opgehaald|
|/registries/webhooks/ping/Action|Een ping gebeurtenis moet worden verzonden naar de webhook.|
|/registries/webhooks/Read|De eigenschappen van de opgegeven webhook opgehaald of een lijst met alle webhooks voor het register opgegeven container.|
|/registries/webhooks/Write|Maken of bijwerken van een webhook voor een container-registersleutel met de opgegeven parameters.|
|registers/schrijven|Maken of bijwerken van het register van een container met de opgegeven parameters.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| Bewerking | Beschrijving |
|---|---|
|/containerServices/delete|De opgegeven Containerservice verwijderen|
|/containerServices/Read|De opgegeven Containerservice opgehaald|
|/containerServices/write|Instellen of de opgegeven Containerservice bijwerken|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| Bewerking | Beschrijving |
|---|---|
|/Applications/DELETE|Verwijderbewerking|
|/Applications/listSecrets/Action|Geheimen vermelden|
|/Applications/listSingleSignOnToken/Action|Eenmalige aanmelding Tokens lezen|
|/Applications/Read|Leesbewerking|
|/ applications/schrijven|Schrijfbewerking|
|/ applications/schrijven|Schrijfbewerking|
|/ listCommunicationPreference/actie|Lijst communicatievoorkeur|
|/Operations/Read|leesbewerkingen|
|/updateCommunicationPreference/action|Communicatievoorkeur bijwerken|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| Bewerking | Beschrijving |
|---|---|
|/hubs/adobemetadata/Action|Maken of bijwerken van de klant Azure Insights Adobe metagegevens|
|/hubs/adobemetadata/Read|Azure klant Insights Adobe metagegevens lezen|
|/hubs/authorizationPolicies/delete|Een Azure klant Insights Shared Access Signature-beleid verwijderen|
|/hubs/authorizationPolicies/Read|Geen inzichten Azure klant gedeeld toegangsbeleid handtekening lezen|
|/hubs/authorizationPolicies/regeneratePrimaryKey/Action|Azure klant Insights handtekening beleid voor gedeelde toegang primaire sleutel opnieuw genereren|
|/hubs/authorizationPolicies/regenerateSecondaryKey/Action|Azure klant Insights handtekening beleid voor gedeelde toegang secundaire sleutel opnieuw genereren|
|/hubs/authorizationPolicies/Write|Een Azure klant Insights Shared Access Signature beleid maken of bijwerken|
|/hubs/connectors/Activate/Action|Activeren van een Azure klant Insights-Connector|
|/hubs/connectors/Activate/Action|Activeren van een Azure klant Insights-Connector|
|/hubs/connectors/delete|Elke klant Azure Insights-Connector verwijderen|
|/hubs/connectors/getruntimestatus/Action|De status van elk Azure klant Insights-Connector runtime ophalen|
|/hubs/connectors/mappings/Activate/Action|Activeren van een toewijzing van Azure klant Insights-Connector|
|/hubs/connectors/mappings/DELETE|Een toewijzing van Azure klant Insights-Connector verwijderen|
|/hubs/connectors/mappings/Operations/Read|Het resultaat van een toewijzing van Azure klant Insights-Connector bewerking lezen|
|/hubs/connectors/mappings/Read|Lezen van een toewijzing van Azure klant Insights-Connector|
|/hubs/connectors/mappings/Write|Maken of bijwerken van een toewijzing van Azure klant Insights-Connector|
|/hubs/connectors/Operations/Read|Het resultaat van een klant Insights-Connector van Azure-bewerking lezen|
|/hubs/connectors/Read|Lezen van een Azure klant Insights-Connector|
|/hubs/connectors/saveauthinfo/Action|Maken of bijwerken van een Azure klant Insights-Connector verificatie informatie over de gatewayverbinding|
|/hubs/connectors/update/Action|Bijwerken van een Azure klant Insights-Connector|
|/hubs/connectors/Write|Maken of bijwerken van een Azure klant Insights-Connector|
|/hubs/crmmetadata/Action|Maken of bijwerken van alle Azure klant Insights Crm-metagegevens|
|/hubs/crmmetadata/Read|Alle Azure klant Insights Crm-metagegevens lezen|
|/hubs/delete|Verwijderen van een Azure klant Insights Hub|
|/hubs/gdpr/DELETE|Een Azure klant Insights Gdpr verwijderen|
|/hubs/gdpr/Read|Een Azure klant Insights Gdpr lezen|
|/hubs/gdpr/Write|Maken of bijwerken van een Azure klant Insights Gdpr|
|/hubs/getbillingcredits/Read|Azure klant Insights Hub facturering tegoed ophalen|
|/hubs/getbillinghistory/Read|Azure klant Insights Hub facturering geschiedenis ophalen|
|/hubs/images/delete|De installatiekopie van een Azure klant Insights verwijderen|
|/hubs/images/Read|Een Azure klant Insights afbeelding lezen|
|/hubs/images/Write|Maken of bijwerken van een Azure klant Insights-afbeelding|
|/hubs/interactions/DELETE|Azure klant Insights interacties verwijderen|
|/hubs/interactions/Operations/Read|Het resultaat van een Azure klant Insights interactie bewerking lezen|
|/hubs/interactions/Read|Elke klant Azure Insights interactie lezen|
|/hubs/interactions/suggestrelationshiplinks/action|Relatie koppelingen aanbevelen voor Azure klant Insights interacties|
|/hubs/interactions/Write|Maken of bijwerken van elke klant Azure Insights interactie|
|/hubs/kpi/delete|Een Azure klant Insights Key Performance Indicator verwijderen|
|/hubs/KPI/Operations/Read|Het resultaat van een Azure klant Insights Key Performance Indicators bewerking lezen|
|/hubs/KPI/Read|Een Azure klant Insights Key Performance Indicator lezen|
|/hubs/KPI/Reprocess/Action|Een Azure klant Insights prestatie-indicatoren opnieuw verwerken|
|/hubs/KPI/Write|Maken of bijwerken van een Azure klant Insights Key Performance Indicator|
|/hubs/links/delete|Een Azure klant Insights koppelingen verwijderen|
|/hubs/links/Operations/Read|Het resultaat van een Azure klant Insights koppelingen bewerking lezen|
|/hubs/links/Read|Azure klant Insights koppelingen lezen|
|/hubs/links/Write|Maken of bijwerken van de klant Azure koppelingen|
|/hubs/msemetadata/Action|Maken of bijwerken van alle metagegevens van de klant Azure Insights muis|
|/hubs/msemetadata/Read|Azure klant Insights muis metagegevens lezen|
|/hubs/operationresults/Read|Resultaat van de klant Azure Insights Hub-bewerking ophalen|
|/hubs/predictions/DELETE|Een Azure klant Insights voorspellingen verwijderen|
|/hubs/predictions/Operations/Read|Het resultaat van een Azure klant Insights voorspellingen bewerking lezen|
|/hubs/predictions/Read|Een Azure klant Insights voorspellingen lezen|
|/hubs/predictions/Write|Maken of bijwerken van een Azure klant voorspellingen|
|/hubs/predictivematchpolicies/delete|Een Azure klant Insights voorspellende overeen beleid verwijderen|
|/hubs/predictivematchpolicies/Operations/Read|Het resultaat van een bewerking Azure klant Insights voorspellende overeen beleid lezen|
|/hubs/predictivematchpolicies/Read|Azure klant Insights voorspellende overeen beleid lezen|
|/hubs/predictivematchpolicies/Write|Een Azure klant Insights voorspellende overeen beleid maken of bijwerken|
|/hubs/profiles/delete|Een Azure klant Insights-profiel verwijderen|
|/hubs/Profiles/Operations/Read|Het resultaat van een Azure klant Insights profiel bewerking lezen|
|/hubs/Profiles/Read|Een Azure klant Insights profiel lezen|
|/hubs/Profiles/Write|Schrijven van een Azure klant Insights-profiel|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/hubs/providers/Microsoft.Insights/logDefinitions/read|De beschikbare logboeken opgehaald voor resource|
|/hubs/providers/Microsoft.Insights/metricDefinitions/read|Hiermee worden de beschikbare metrische gegevens voor de resource opgehaald|
|/hubs/Read|Een Azure klant Insights Hub lezen|
|/hubs/relationshiplinks/DELETE|Een Azure klant Insights relatie koppelingen verwijderen|
|/hubs/relationshiplinks/Operations/Read|Het resultaat van een Azure klant Insights relatie koppelingen bewerking lezen|
|/hubs/relationshiplinks/Read|Alle koppelingen van Azure klant Insights relatie lezen|
|/hubs/relationshiplinks/Write|Maken of bijwerken van de klant Azure Insights relatie koppelingen|
|/hubs/Relationships/DELETE|Verwijder alle Azure klant Insights relaties|
|/hubs/Relationships/Operations/Read|Het resultaat van een Azure Insights relaties bewerking lezen|
|/hubs/Relationships/Read|De relaties die Azure klant Insights lezen|
|/hubs/Relationships/Write|Maken of bijwerken van de klant Azure Insights relaties|
|/hubs/roleAssignments/DELETE|Verwijderen van een Azure klant Insights Rbac-toewijzing|
|/hubs/roleAssignments/Operations/Read|Het resultaat van een Azure klant Insights Rbac toewijzing bewerking lezen|
|/hubs/roleAssignments/Read|Lezen van een Azure klant Insights Rbac-toewijzing|
|/hubs/roleAssignments/Write|Maken of bijwerken van een Azure klant Insights Rbac-toewijzing|
|/hubs/roles/Read|Azure klant Insights Rbac rollen lezen|
|/hubs/salesforcemetadata/Action|Maken of bijwerken van de klant Azure Insights SalesForce metagegevens|
|/hubs/salesforcemetadata/Read|Alle metagegevens van de klant Azure Insights SalesForce lezen|
|/hubs/Segments/DELETE|Een Azure klantsegmenten Insights verwijderen|
|/hubs/Segments/Dynamic/Action|Beheer alle Azure klant inzicht dynamische segmenten|
|/hubs/Segments/Read|Een Azure klantsegmenten Insights lezen|
|/hubs/Segments/static/Action|Beheer alle Azure klant inzicht statische segmenten|
|/hubs/Segments/Write|Maken of bijwerken van een Azure klantsegmenten Insights|
|/hubs/sqlconnectionstrings/DELETE|Een Azure klant Insights SqlConnectionStrings verwijderen|
|/hubs/sqlconnectionstrings/Read|Een Azure klant Insights SqlConnectionStrings lezen|
|/hubs/sqlconnectionstrings/Write|Maken of bijwerken van een Azure klant Insights SqlConnectionStrings|
|/hubs/suggesttypeschema/Action|Type Schema voorstellen genereren van voorbeeldgegevens|
|/hubs/tenantmanagement/Read|Een Azure klant Insights hub-instellingen beheren|
|/hubs/Views/DELETE|Elke klant Azure Insights App weergave verwijderen|
|/hubs/Views/Read|Elke weergave Azure klant Insights-App lezen|
|/hubs/Views/Write|Maken of bijwerken van een Azure klant Insights App weergeven|
|/hubs/widgettypes/Read|Geen typen Azure klant Insights App Widget lezen|
|hubs/schrijven|Maken of bijwerken van een Azure klant Insights Hub|
|/Operations/Read|Azure klant Insights Api Metadatas lezen|
|registratie-/ actie|Het abonnement voor de klant Insights-resourceprovider geregistreerd en wordt het maken van de klant Insights-resources|
|/ unregister/actie|Heft de registratie van het abonnement voor de klant Insights-resourceprovider|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| Bewerking | Beschrijving |
|---|---|
|/Catalogs/DELETE|Hiermee verwijdert u de catalogus.|
|/Catalogs/Read|Eigenschappen voor catalogi onder het abonnement of resourcegroep worden opgehaald.|
|catalogussen/schrijven|Catalogus maken of bijwerken van de labels en eigenschappen voor de catalogus.|
|/ checkNameAvailability/actie|Controleert de beschikbaarheid van de catalogus-naam voor de tenant.|
|/Operations/Read|Een lijst met bewerkingen weergegeven die beschikbaar zijn op Microsoft.DataCatalog resourceprovider.|
|registratie-/ actie|Abonnement registreert met Microsoft.DataCatalog resourceprovider.|
|/ unregister/actie|Registratie opheffen Microsoft.DataCatalog resourceprovider-abonnement.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| Bewerking | Beschrijving |
|---|---|
|/datafactories/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/datafactories/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/datafactories/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor datafactories|
|/factories/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/factories/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/factories/providers/Microsoft.Insights/logDefinitions/Read|De beschikbare logboeken opgehaald voor fabrieken|
|/factories/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor fabrieken|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| Bewerking | Beschrijving |
|---|---|
|/accounts/computePolicies/delete|Een compute-beleid verwijderen.|
|/accounts/computePolicies/Read|Informatie ophalen over een compute-beleid.|
|/accounts/computePolicies/Write|Maken of bijwerken van een compute-beleid.|
|/accounts/dataLakeStoreAccounts/delete|Ontkoppelen van een account DataLakeStore van een DataLakeAnalytics-account.|
|/accounts/dataLakeStoreAccounts/Read|Informatie ophalen over een gekoppelde DataLakeStore-account van een DataLakeAnalytics-account.|
|/accounts/dataLakeStoreAccounts/write|Maken of bijwerken van een gekoppelde DataLakeStore-account van een DataLakeAnalytics-account.|
|/accounts/DELETE|Een DataLakeAnalytics-account verwijderen.|
|/accounts/firewallRules/DELETE|Een firewallregel verwijderen.|
|/accounts/firewallRules/Read|Informatie ophalen over een firewallregel.|
|/accounts/firewallRules/Write|Maken of bijwerken van een firewallregel.|
|/accounts/operationResults/Read|Resultaat van een bewerking van de account DataLakeAnalytics ophalen.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/Read|De diagnostische instellingen voor het account DataLakeAnalytics ophalen.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instellingen voor het account DataLakeAnalytics.|
|/accounts/providers/Microsoft.Insights/logDefinitions/read|De beschikbare logboeken voor het account DataLakeAnalytics niet ophalen.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/Read|De beschikbare metrische gegevens voor het account DataLakeAnalytics niet ophalen.|
|/Accounts/read|Informatie ophalen over een bestaande DataLakeAnalytics-account.|
|/accounts/storageAccounts/Containers/listSasTokens/action|Lijst met SAS-tokens voor storage-containers van een gekoppelde Storage-account van een DataLakeAnalytics-account.|
|/accounts/storageAccounts/containers/Read|Containers van een gekoppelde Storage-account van een account DataLakeAnalytics ophalen.|
|/accounts/storageAccounts/delete|Een opslagaccount van een account DataLakeAnalytics ontkoppelen.|
|/accounts/storageAccounts/Read|Informatie ophalen over een gekoppelde Storage-account van een DataLakeAnalytics-account.|
|/accounts/storageAccounts/write|Maken of bijwerken van een gekoppelde Storage-account van een DataLakeAnalytics-account.|
|/accounts/TakeOwnerShip/Action|Machtigingen verlenen voor het annuleren van de taken die worden verzonden door andere gebruikers.|
|accounts/schrijven|Maken of bijwerken van een DataLakeAnalytics-account.|
|/Locations/capability/Read|Ophalen van informatie over de functionaliteit van een abonnement met betrekking tot DataLakeAnalytics gebruiken.|
|/Locations/checkNameAvailability/Action|Controleer de beschikbaarheid van een accountnaam DataLakeAnalytics.|
|/Locations/operationResults/Read|Resultaat van een bewerking van de account DataLakeAnalytics ophalen.|
|/Operations/Read|Beschikbare bewerkingen van DataLakeAnalytics ophalen.|
|registratie-/ actie|Abonnement op DataLakeAnalytics registreren.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| Bewerking | Beschrijving |
|---|---|
|/accounts/DELETE|Een DataLakeStore-account verwijderen.|
|/accounts/enableKeyVault/action|KeyVault inschakelen voor een account DataLakeStore.|
|/accounts/firewallRules/DELETE|Een firewallregel verwijderen.|
|/accounts/firewallRules/Read|Informatie ophalen over een firewallregel.|
|/accounts/firewallRules/Write|Maken of bijwerken van een firewallregel.|
|/accounts/operationResults/Read|Resultaat van een bewerking van de account DataLakeStore ophalen.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/Read|De diagnostische instellingen voor het account DataLakeStore ophalen.|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instellingen voor het account DataLakeStore.|
|/accounts/providers/Microsoft.Insights/logDefinitions/read|De beschikbare logboeken voor het account DataLakeStore niet ophalen.|
|/accounts/providers/Microsoft.Insights/metricDefinitions/Read|De beschikbare metrische gegevens voor het account DataLakeStore niet ophalen.|
|/Accounts/read|Informatie ophalen over een bestaande DataLakeStore-account.|
|/accounts/SuperUser/Action|Supergebruiker op Data Lake Store als verleend met Microsoft.Authorization/roleAssignments/write verlenen.|
|/accounts/trustedIdProviders/delete|Verwijderen van een vertrouwde id-provider.|
|/accounts/trustedIdProviders/read|Informatie ophalen over een vertrouwde id-provider.|
|/accounts/trustedIdProviders/Write|Maken of bijwerken van een vertrouwde id-provider.|
|accounts/schrijven|Maken of bijwerken van een DataLakeStore-account.|
|/Locations/capability/Read|Ophalen van informatie over de functionaliteit van een abonnement met betrekking tot DataLakeStore gebruiken.|
|/Locations/checkNameAvailability/Action|Controleer de beschikbaarheid van een accountnaam DataLakeStore.|
|/Locations/operationResults/Read|Resultaat van een bewerking van de account DataLakeStore ophalen.|
|/Operations/Read|Beschikbare bewerkingen van DataLakeStore ophalen.|
|registratie-/ actie|Abonnement op DataLakeStore registreren.|

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

| Bewerking | Beschrijving |
|---|---|
|/Locations/performanceTiers/Read|Retourneert de lijst met beschikbare Prestatielagen.|
|/performanceTiers/Read|Retourneert de lijst met beschikbare Prestatielagen.|
|/servers/DELETE|Hiermee verwijdert u een bestaande server.|
|/servers/firewallRules/DELETE|Hiermee wordt een bestaande firewallregel verwijderd.|
|/servers/firewallRules/Read|De lijst met firewall regels voor een server of haalt u de eigenschappen voor de opgegeven firewallregel retourneren.|
|/servers/firewallRules/Write|Hiermee maakt een firewallregel met de opgegeven parameters of update een bestaande regel.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|De disagnostic instelling voor de resource opgehaald|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Retourtypen van metrische gegevens die beschikbaar voor databases zijn|
|/servers/Read|Retourneert de lijst met servers of de eigenschappen voor de opgegeven server opgehaald.|
|/servers/recoverableServers/Read|Herstelbare gegevens van de MySQL-Server te retourneren|
|/servers/virtualNetworkRules/delete|Hiermee verwijdert u een bestaande regel voor het virtuele netwerk|
|/servers/virtualNetworkRules/Read|De lijst van het virtuele netwerk regels of haalt u de eigenschappen voor de opgegeven virtuele netwerk regel retourneren.|
|/servers/virtualNetworkRules/Write|Een virtueel netwerk maken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de regel voor de opgegeven virtuele netwerk.|
|servers/schrijven|Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server.|

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

| Bewerking | Beschrijving |
|---|---|
|/Locations/performanceTiers/Read|Retourneert de lijst met beschikbare Prestatielagen.|
|/performanceTiers/Read|Retourneert de lijst met beschikbare Prestatielagen.|
|/servers/DELETE|Hiermee verwijdert u een bestaande server.|
|/servers/firewallRules/DELETE|Hiermee wordt een bestaande firewallregel verwijderd.|
|/servers/firewallRules/Read|De lijst met firewall regels voor een server of haalt u de eigenschappen voor de opgegeven firewallregel retourneren.|
|/servers/firewallRules/Write|Hiermee maakt een firewallregel met de opgegeven parameters of update een bestaande regel.|
|/servers/providers/Microsoft.Insights/diagnosticSettings/read|De disagnostic instelling voor de resource opgehaald|
|/servers/providers/Microsoft.Insights/diagnosticSettings/write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/servers/providers/Microsoft.Insights/logDefinitions/read|Retourtypen van logboeken die beschikbaar voor databases zijn|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Retourtypen van metrische gegevens die beschikbaar voor databases zijn|
|/servers/Read|Retourneert de lijst met servers of de eigenschappen voor de opgegeven server opgehaald.|
|/servers/recoverableServers/Read|De herstelbare PostgreSQL-serverinformatie retourneren|
|/servers/virtualNetworkRules/delete|Hiermee verwijdert u een bestaande regel voor het virtuele netwerk|
|/servers/virtualNetworkRules/Read|De lijst van het virtuele netwerk regels of haalt u de eigenschappen voor de opgegeven virtuele netwerk regel retourneren.|
|/servers/virtualNetworkRules/Write|Een virtueel netwerk maken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de regel voor de opgegeven virtuele netwerk.|
|servers/schrijven|Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Controleer als IotHub naam beschikbaar is|
|/checkProvisioningServiceNameAvailability/Action|Controleer als IotHub naam beschikbaar is|
|ElasticPools/diagnosticSettings/leestijd|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/ ElasticPools/diagnosticSettings/schrijven|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/elasticPools/iotHubTenants/Delete|Verwijder de IotHub-tenant-bron|
|ElasticPools/IotHubTenants/diagnosticSettings/leestijd|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/ ElasticPools/IotHubTenants/diagnosticSettings/schrijven|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete|EventHub Consumer-groep verwijderen|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read|EventHub Consumer groep(en) ophalen|
|/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write|EventHub Consumergroep maken|
|/elasticPools/iotHubTenants/exportDevices/Action|Exporteren van apparaten|
|/elasticPools/iotHubTenants/getStats/Read|De IotHub-tenant statistieken resource opgehaald|
|/elasticPools/iotHubTenants/importDevices/Action|Apparaten importeren|
|/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action|De IotHub-tenantsleutel opgehaald|
|/elasticPools/iotHubTenants/jobs/Read|Details van ingediend op gegeven IotHub taken ophalen|
|/elasticPools/iotHubTenants/listKeys/Action|IotHub tenantsleutels opgehaald|
|/ElasticPools/IotHubTenants/logDefinitions/read|De beschikbare logboekdefinities voor de IotHub-Service opgehaald|
|/ElasticPools/IotHubTenants/metricDefinitions/read|Hiermee wordt de beschikbare metrische gegevens voor de IotHub-service|
|/elasticPools/iotHubTenants/quotaMetrics/Read|Quotum metrische gegevens ophalen|
|/elasticPools/iotHubTenants/Read|De bron van de tenant IotHub opgehaald|
|/elasticPools/iotHubTenants/routing/routes/$testall/Action|Een bericht met alle bestaande Routes testen|
|/elasticPools/iotHubTenants/routing/routes/$testnew/Action|Een bericht met een opgegeven test Route testen|
|/elasticPools/iotHubTenants/routingEndpointsHealth/Read|Hiermee wordt de status van alle routering eindpunten voor een IotHub|
|/elasticPools/iotHubTenants/Write|Maken of bijwerken van de bron van de tenant IotHub|
|ElasticPools/metricDefinitions/leestijd|Hiermee wordt de beschikbare metrische gegevens voor de IotHub-service|
|/iotHubs/certificates/generateVerificationCode/Action|Verificatiecode genereren|
|/iotHubs/Certificates/Verify/Action|Controleer de resource-certificaat|
|/iotHubs/Delete|IotHub-bron verwijderen|
|IotHubs/diagnosticSettings/leestijd|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/ IotHubs/diagnosticSettings/schrijven|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/iotHubs/eventGridFilters/Delete|Hiermee verwijdert u het filter gebeurtenis raster|
|/iotHubs/eventGridFilters/Read|Het filter gebeurtenis raster opgehaald|
|/iotHubs/eventGridFilters/Write|Maken van nieuwe of bestaande gebeurtenis raster filter bijwerken|
|/iotHubs/eventHubEndpoints/consumerGroups/Delete|EventHub Consumer-groep verwijderen|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|EventHub Consumer groep(en) ophalen|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|EventHub Consumergroep maken|
|/iotHubs/exportDevices/Action|Exporteren van apparaten|
|/iotHubs/importDevices/Action|Apparaten importeren|
|/iotHubs/iotHubKeys/listkeys/Action|Ophalen van IotHub Key voor de opgegeven naam|
|/iotHubs/iotHubStats/Read|IotHub-statistieken opvragen|
|/iotHubs/jobs/Read|Details van ingediend op gegeven IotHub taken ophalen|
|/iotHubs/listkeys/Action|Alle IotHub-sleutels ophalen|
|IotHubs/logDefinitions/leestijd|De beschikbare logboekdefinities voor de IotHub-Service opgehaald|
|IotHubs/metricDefinitions/leestijd|Hiermee wordt de beschikbare metrische gegevens voor de IotHub-service|
|/iotHubs/quotaMetrics/Read|Quotum metrische gegevens ophalen|
|/iotHubs/Read|De IotHub-resources opgehaald|
|/iotHubs/routing/$testall/Action|Een bericht met alle bestaande Routes testen|
|/iotHubs/routing/$testnew/Action|Een bericht met een opgegeven test Route testen|
|/iotHubs/routingEndpointsHealth/Read|Hiermee wordt de status van alle routering eindpunten voor een IotHub|
|/iotHubs/skus/Read|Geldige IotHub Skus ophalen|
|/iotHubs/Write|Maken of bijwerken van de IotHub-bron|
|/ operations/lezen|Alle bewerkingen van de ResourceProvider ophalen|
|/provisioningServices/Certificates/generateVerificationCode/Action|Verificatiecode genereren|
|/provisioningServices/Certificates/Verify/Action|Controleer de resource-certificaat|
|/ provisioningServices/verwijderen|IotDps bron verwijderen|
|/provisioningServices/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/provisioningServices/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/provisioningServices/listkeys/Action|Alle IotDps-sleutels ophalen|
|/provisioningServices/logDefinitions/Read|De beschikbare logboekdefinities voor de inrichting Service opgehaald|
|/provisioningServices/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor de inrichting-service|
|/provisioningServices/ProvisioningServiceKeys/listkeys/Action|IotDps sleutels voor de naam van de sleutel ophalen|
|/ provisioningServices/lezen|Ophalen van IotDps|
|/provisioningServices/skus/Read|Geldige IotDps Skus ophalen|
|/ provisioningServices/schrijven|IotDps resource maken|
|registratie-/ actie|Het abonnement voor de IotHub-resourceprovider registreren en wordt het maken van resources met IotHub|
|registratie-/ actie|Het abonnement voor de IotHub-resourceprovider registreren en wordt het maken van resources met IotHub|
|Gebruik/lezen|Neem abonnement informatie over het gebruik voor deze provider.|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| Bewerking | Beschrijving |
|---|---|
|/labCenters/DELETE|Lab centers verwijderen.|
|/labCenters/Read|Lab centers lezen.|
|labCenters/schrijven|Toevoegen of wijzigen van de testomgeving centers.|
|/Labs/artifactSources/armTemplates/Read|Lezen van azure resource manager-sjablonen.|
|/Labs/artifactSources/artifacts/GenerateArmTemplate/Action|Genereert een ARM-sjabloon voor het opgegeven artefact, uploadt de vereiste bestanden naar een opslagaccount en valideert het gegenereerde artefact.|
|/Labs/artifactSources/artifacts/Read|Lezen van artefacten.|
|/labs/artifactSources/delete|Verwijderen van artefacten gegevensbronnen.|
|/Labs/artifactSources/Read|Artefacten bronnen lezen.|
|/Labs/artifactSources/Write|Toevoegen of wijzigen van artefacten bronnen.|
|/labs/ClaimAnyVm/action|Een willekeurige claimable virtuele machine in de testomgeving claimen.|
|/Labs/costs/Read|Kosten worden gelezen.|
|/Labs/costs/Write|Toevoegen of wijzigen van de kosten.|
|/Labs/CreateEnvironment/Action|Virtuele machines maken in een testomgeving.|
|/labs/customImages/delete|Verwijderen van aangepaste installatiekopieën.|
|/labs/customImages/read|Lezen van aangepaste installatiekopieën.|
|/labs/customImages/write|Toevoegen of wijzigen van aangepaste installatiekopieën.|
|/Labs/DELETE|Labs verwijderen.|
|/Labs/ExportResourceUsage/Action|Het brongebruik lab exporteert naar een opslagaccount|
|/Labs/Formulas/DELETE|Verwijder de formules.|
|/Labs/Formulas/Read|Lees de formules.|
|/Labs/Formulas/Write|Toevoegen of wijzigen van formules.|
|/Labs/galleryImages/Read|Afbeeldingen worden gelezen.|
|/Labs/GenerateUploadUri/Action|Een URI genereren voor het aangepaste schijfkopieën uploaden naar een testomgeving.|
|/Labs/ImportVirtualMachine/Action|Een virtuele machine in een andere testomgeving importeren.|
|/labs/ListVhds/action|Lijst van installatiekopieën van de schijven beschikbaar zijn voor het maken van aangepaste installatiekopie.|
|/labs/notificationChannels/delete|Notificationchannels verwijderen.|
|/Labs/notificationChannels/Notify/Action|Melding verzonden naar opgegeven kanaal.|
|/Labs/notificationChannels/Read|Notificationchannels lezen.|
|/Labs/notificationChannels/Write|Toevoegen of wijzigen van notificationchannels.|
|/labs/policySets/EvaluatePolicies/action|Lab beleid evalueert.|
|/Labs/policySets/Policies/DELETE|Beleid verwijderen.|
|/Labs/policySets/Policies/Read|Lezen van beleid.|
|/Labs/policySets/Policies/Write|Toevoegen of wijzigen van beleid.|
|/Labs/Read|Labs lezen.|
|/Labs/Schedules/DELETE|Verwijderen van schema's.|
|/Labs/Schedules/Execute/Action|Een schema worden uitgevoerd.|
|/Labs/Schedules/ListApplicable/Action|Geeft een lijst van alle toepasselijke schema 's|
|/Labs/Schedules/Read|Schema's worden gelezen.|
|/Labs/Schedules/Write|Toevoegen of wijzigen van schema's.|
|/Labs/serviceRunners/DELETE|Service uitlopers verwijderen.|
|/Labs/serviceRunners/Read|Service uitlopers lezen.|
|/labs/serviceRunners/write|Toevoegen of wijzigen van de service uitlopers.|
|/Labs/Users/DELETE|Gebruikersprofielen verwijderen.|
|/Labs/Users/Disks/Attach/Action|Koppelen en maken van de lease van de schijf voor de virtuele machine.|
|/Labs/Users/Disks/DELETE|Schijven worden verwijderd.|
|/Labs/Users/Disks/detach/Action|Ontkoppel en de lease van de schijf die is gekoppeld aan de virtuele machine wordt verbroken.|
|/Labs/Users/Disks/Read|Schijven worden gelezen.|
|/Labs/Users/Disks/Write|Toevoegen of wijzigen van de schijven.|
|/Labs/Users/Environments/DELETE|Verwijder omgevingen.|
|/Labs/Users/Environments/Read|Lezen omgevingen.|
|/Labs/Users/Environments/Write|Toevoegen of wijzigen van omgevingen.|
|/Labs/Users/Read|Gebruikersprofielen worden gelezen.|
|/Labs/Users/secrets/DELETE|Geheimen verwijderen.|
|/Labs/Users/secrets/Read|Lezen van geheimen.|
|/Labs/Users/secrets/Write|Toevoegen of wijzigen van geheimen.|
|/Labs/Users/serviceFabrics/DELETE|Verwijder de service fabrics.|
|/Labs/Users/serviceFabrics/ListApplicableSchedules/Action|Geeft een lijst van alle toepasselijke schema 's|
|/Labs/Users/serviceFabrics/Read|Service-fabrics lezen.|
|/Labs/Users/serviceFabrics/Schedules/DELETE|Verwijderen van schema's.|
|/Labs/Users/serviceFabrics/Schedules/Execute/Action|Een schema worden uitgevoerd.|
|/Labs/Users/serviceFabrics/Schedules/Read|Schema's worden gelezen.|
|/Labs/Users/serviceFabrics/Schedules/Write|Toevoegen of wijzigen van schema's.|
|/Labs/Users/serviceFabrics/start/Action|Start een service fabric.|
|/Labs/Users/serviceFabrics/Stop/Action|Een service fabric stoppen|
|/Labs/Users/serviceFabrics/Write|Toevoegen of wijzigen van de service-fabrics.|
|/Labs/Users/Write|Toevoegen of wijzigen van gebruikersprofielen.|
|/Labs/virtualMachines/AddDataDisk/Action|Een nieuwe of bestaande gegevensschijf koppelen aan de virtuele machine.|
|/Labs/virtualMachines/ApplyArtifacts/Action|Artefacten van toepassing op virtuele machine.|
|/Labs/virtualMachines/claim/Action|Eigenaar worden van een bestaande virtuele machine|
|/Labs/virtualMachines/DELETE|Verwijder de virtuele machines.|
|/Labs/virtualMachines/DetachDataDisk/Action|Loskoppelen van de opgegeven schijf van de virtuele machine.|
|/Labs/virtualMachines/ListApplicableSchedules/Action|Geeft een lijst van alle toepasselijke schema 's|
|/Labs/virtualMachines/Read|Virtuele machines gelezen.|
|/Labs/virtualMachines/restart/Action|Start opnieuw op een virtuele machine.|
|/Labs/virtualMachines/Schedules/DELETE|Verwijderen van schema's.|
|/Labs/virtualMachines/Schedules/Execute/Action|Een schema worden uitgevoerd.|
|/Labs/virtualMachines/Schedules/Read|Schema's worden gelezen.|
|/Labs/virtualMachines/Schedules/Write|Toevoegen of wijzigen van schema's.|
|/Labs/virtualMachines/start/Action|Een virtuele machine start.|
|/Labs/virtualMachines/Stop/Action|Een virtuele machine stoppen|
|/Labs/virtualMachines/TransferDisks/Action|Het eigendom overdraagt van gegevensschijven van de virtuele machine aan uzelf|
|/labs/virtualMachines/UnClaim/action|Release-eigendom van een bestaande virtuele machine|
|/Labs/virtualMachines/Write|Hiermee worden virtuele machines toegevoegd of gewijzigd.|
|/labs/virtualNetworks/delete|Verwijder de virtuele netwerken.|
|/Labs/virtualNetworks/Read|Virtuele netwerken gelezen.|
|/Labs/virtualNetworks/Write|Toevoegen of wijzigen van virtuele netwerken.|
|labs/schrijven|Toevoegen of wijzigen van labs.|
|/Locations/Operations/Read|Lees-en schrijfopdrachten.|
|registratie-/ actie|Registreert het abonnement|
|/Schedules/DELETE|Verwijderen van schema's.|
|/Schedules/Execute/Action|Een schema worden uitgevoerd.|
|/Schedules/Read|Schema's worden gelezen.|
|/Schedules/Retarget/Action|Updates van een planning doelresource-id.|
|schema's / schrijven|Toevoegen of wijzigen van schema's.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| Bewerking | Beschrijving |
|---|---|
|/databaseAccountNames/read|Controleert of de naam van beschikbaarheid.|
|/databaseAccounts/changeResourceGroup/action|Resourcegroep van een databaseaccount wijzigen|
|/databaseAccounts/databases/Collections/metricDefinitions/Read|De verzameling leest metrische definities.|
|/databaseAccounts/databases/Collections/metrics/Read|Hiermee wordt de verzameling metrische gegevens gelezen.|
|/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read|Database-account partitie belangrijkste niveau metrische gegevens lezen|
|/databaseAccounts/databases/Collections/Partitions/metrics/Read|Database-account partitie niveau metrische gegevens lezen|
|/databaseAccounts/databases/Collections/Partitions/usages/Read|Databaseaccount partitie niveau gebruiksregels lezen|
|/databaseAccounts/databases/Collections/usages/Read|Leest het gebruik van de verzameling.|
|/databaseAccounts/databases/metricDefinitions/Read|Metrische definities van de database gelezen|
|/databaseAccounts/databases/metrics/Read|Hiermee wordt de database metrische gegevens gelezen.|
|/databaseAccounts/databases/usages/Read|Hiermee wordt het gebruik van de database gelezen.|
|/databaseAccounts/delete|Hiermee verwijdert u de database-accounts.|
|/databaseAccounts/failoverPriorityChange/Action|Failover prioriteiten van regio's van een databaseaccount wijzigen. Dit wordt gebruikt om handmatige failover uit te voeren|
|/databaseAccounts/listConnectionStrings/action|De verbindingsreeksen niet ophalen voor een databaseaccount|
|/databaseAccounts/listKeys/Action|Lijst met sleutels van een databaseaccount|
|/databaseAccounts/metricDefinitions/Read|Leest de database account metrische definities.|
|/databaseAccounts/metrics/Read|Hiermee wordt de database-account metrische gegevens gelezen.|
|/databaseAccounts/operationResults/read|Status van de asynchrone bewerking lezen|
|/databaseAccounts/Percentile/metrics/Read|Latentie metrische gegevens lezen|
|/databaseAccounts/Percentile/sourceRegion/targetRegion/metrics/Read|Latentie metrische gegevens voor een specifieke bron en doel regio lezen|
|/databaseAccounts/Percentile/targetRegion/metrics/Read|Latentie metrische gegevens voor een specifieke regio lezen|
|/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/databaseAccounts/providers/Microsoft.Insights/diagnosticSettings/write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/databaseAccounts/providers/Microsoft.Insights/logDefinitions/read|Hiermee wordt de beschikbare logboek catageries voor databaseaccount|
|/databaseAccounts/providers/Microsoft.Insights/metricDefinitions/read|Hiermee wordt de beschikbare metrische gegevens voor de database-Account|
|/databaseAccounts/Read|Een databaseaccount leest.|
|/databaseAccounts/readonlykeys/Action|Leest de database alleen-lezen sleutels.|
|/databaseAccounts/readonlykeys/Read|Leest de database alleen-lezen sleutels.|
|/databaseAccounts/regenerateKey/Action|Sleutels van een databaseaccount draaien|
|/databaseAccounts/Region/databases/Collections/metrics/Read|Hiermee wordt de regionale verzameling metrische gegevens gelezen.|
|/databaseAccounts/Region/databases/Collections/partitionKeyRangeId/metrics/Read|Lezen van regionale database account partitie belangrijkste niveau metrische gegevens|
|/databaseAccounts/Region/databases/Collections/Partitions/metrics/Read|Lezen van regionale database account partitie niveau metrische gegevens|
|/databaseAccounts/Region/databases/Collections/Partitions/Read|Database-account partities in een verzameling lezen|
|/databaseAccounts/Region/metrics/Read|Hiermee wordt de regio en database account metrische gegevens gelezen.|
|/databaseAccounts/usages/Read|Het gebruik van de account van de database gelezen.|
|/databaseAccounts/write|Bijwerken van een database-accounts.|
|/locations/deleteVirtualNetworkOrSubnets/action|Microsoft.DocumentDB waarschuwt dat VirtualNetwork of Subnet wordt verwijderd|
|/operationResults/Read|Status van de asynchrone bewerking lezen|
|/Operations/Read|Leesbewerkingen die beschikbaar zijn voor de Microsoft-DocumentDB |
|registratie-/ actie| Registreer de provider van de resource Microsoft DocumentDB voor het abonnement|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| Bewerking | Beschrijving |
|---|---|
|/ checkDomainAvailability/actie|Controleer of een domein aanschaffen is|
|/ domeinen/verwijderen|Een bestaand domein verwijdert.|
|/Domains/domainownershipidentifiers/DELETE|Eigenaar-id verwijderen|
|/Domains/domainownershipidentifiers/Read|Lijst met eigenaar-id 's|
|/Domains/domainownershipidentifiers/Read|Eigenaar-id ophalen|
|/Domains/domainownershipidentifiers/Write|Maken of bijwerken van id|
|/Domains/operationresults/Read|De bewerking van een domein ophalen|
|/Domains/Operations/Read|Lijst van alle bewerkingen van het app service-domeinregistratie|
|/ domeinen/lezen|De lijst met domeinen ophalen|
|/ domeinen/lezen|Domein ophalen|
|/Domains/renew/Action|Vernieuwen van een bestaand domein.|
|/ domeinen/schrijven|Een nieuw domein toevoegen of een bestaande bijgewerkt|
|/generateSsoRequest/Action|Genereren van een aanvraag voor het aanmelden bij domein control center.|
|/ listDomainRecommendations/actie|Ophalen van de lijst domein aanbevelingen op basis van trefwoorden|
|registratie-/ actie|Registreer de provider van Microsoft Domains resource voor het abonnement|
|/topLevelDomains/listAgreements/Action|Actie van de overeenkomst|
|/topLevelDomains/Read|Toplevel domeinen ophalen|
|/topLevelDomains/Read|Toplevel domein ophalen|
|/ validateDomainRegistrationInformation/actie|Aankoop domeinobject valideren zonder deze te verzenden|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| Bewerking | Beschrijving |
|---|---|
|/lcsprojects/clouddeployments/Read|Microsoft Dynamics AX 2012 R3 Evaluation-implementaties in een Microsoft Dynamics Lifecycle Services-project die deel uitmaken van een gebruiker weergeven|
|/lcsprojects/clouddeployments/Write|Microsoft Dynamics AX 2012 R3 evaluatie-implementatie in een project Microsoft Dynamics levenscyclus van Services die deel uitmaken van een gebruiker maken. Implementaties kunnen worden beheerd vanaf de Azure-beheerportal|
|/lcsprojects/connectors/Read|Lezen van connectors die deel uitmaken van een project Microsoft Dynamics levenscyclus van Services|
|/lcsprojects/connectors/Write|Maken en bijwerken van connectors die deel uitmaken van een project Microsoft Dynamics levenscyclus van Services|
|/lcsprojects/DELETE|Microsoft Dynamics Lifecycle Services-projecten die deel uitmaken van de gebruiker verwijderen|
|/lcsprojects/Read|Microsoft Dynamics Lifecycle Services-projecten die deel uitmaken van een gebruiker weergeven|
|lcsprojects/schrijven|Maken en bijwerken van de levenscyclus van Services van Microsoft Dynamics-projecten die deel uitmaken van de gebruiker. Alleen de naam en beschrijving van eigenschappen kunnen worden bijgewerkt. Het abonnement en de locatie die is gekoppeld aan het project kunnen niet worden bijgewerkt nadat het maken|

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

| Bewerking | Beschrijving |
|---|---|
|/eventSubscriptions/DELETE|Een eventSubscription verwijderen|
|/eventSubscriptions/getFullUrl/action|Volledige url in voor het gebeurtenisabonnement ophalen|
|/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read|De diagnostische instelling voor abonnementen opgehaald|
|/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instelling voor gebeurtenisabonnementen|
|/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read|Hiermee wordt de beschikbare metrische gegevens voor eventSubscriptions|
|/eventSubscriptions/Read|Een eventSubscription lezen|
|eventSubscriptions/schrijven|Maken of bijwerken van een eventSubscription|
|/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/Read|De diagnostische instelling voor onderwerpen opgehaald|
|/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instelling voor onderwerpen|
|/extensionTopics/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor onderwerpen|
|registratie-/ actie|Registreert de eventSubscription voor de resourceprovider EventGrid en wordt het maken van abonnementen gebeurtenis raster.|
|/Topics/DELETE|Een onderwerp verwijderen|
|/Topics/listKeys/Action|Lijst met sleutels voor het onderwerp|
|/Topics/providers/Microsoft.Insights/diagnosticSettings/Read|De diagnostische instelling voor onderwerpen opgehaald|
|/Topics/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instelling voor onderwerpen|
|/Topics/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor onderwerpen|
|/Topics/Read|Een onderwerp lezen|
|/Topics/regenerateKey/Action|Opnieuw genereren van de sleutel voor het onderwerp|
|onderwerpen over/schrijven|Maken of bijwerken van een onderwerp|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd.|
|/ checkNamespaceAvailability/actie|Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. Deze API is afgeschaft. Gebruik in plaats hiervan CheckNameAvailabiltiy.|
|/Namespaces/authorizationRules/Action|Updates Namespace-autorisatieregel. Deze API is depricated. Gebruik een PUT-aanroep in plaats daarvan de autorisatieregel Namespace bijwerken... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API.|
|/Namespaces/authorizationRules/DELETE|Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd. |
|/Namespaces/authorizationRules/listkeys/Action|De verbindingstekenreeks naar de naamruimte ophalen|
|/Namespaces/authorizationRules/Read|De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen.|
|/Namespaces/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/authorizationRules/Write|Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/ naamruimten/verwijderen|De resource van een naamruimte verwijderen|
|/Namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/Action|De autorisatie regels sleutels voor de primaire naamruimte voor herstel na noodgevallen opgehaald|
|/Namespaces/disasterRecoveryConfigs/authorizationRules/Read|Disaster Recovery primaire Namespace van autorisatieregels ophalen|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Schakelt Disaster Recovery uit en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten.|
|/Namespaces/disasterrecoveryconfigs/checkNameAvailability/Action|Beschikbaarheid van de controles van naamruimtealias onder het betreffende abonnement.|
|/namespaces/disasterRecoveryConfigs/delete|Hiermee verwijdert u de configuratie van het herstel na noodgevallen die zijn gekoppeld aan de naamruimte. Deze bewerking kan alleen worden aangeroepen via de primaire naamruimte.|
|/Namespaces/disasterRecoveryConfigs/failover/Action|Hiermee wordt een GEO DR-failover aangeroepen en wordt het naamruimtealias zo geconfigureerd dat verwezen wordt naar de secundaire naamruimte.|
|/namespaces/disasterRecoveryConfigs/read|Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort opgehaald.|
|/namespaces/disasterRecoveryConfigs/write|Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort gemaakt of bijgewerkt.|
|/Namespaces/eventhubs/authorizationRules/Action|Bewerking voor het bijwerken van de EventHub. Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. Autorisatieregels. Gebruik een PUT-aanroep autorisatieregel bijwerken.|
|/Namespaces/eventhubs/authorizationRules/DELETE|Bewerking voor het verwijderen van EventHub-autorisatieregels|
|/Namespaces/eventhubs/authorizationRules/listkeys/Action|De verbindingsreeks naar EventHub ophalen|
|/Namespaces/eventhubs/authorizationRules/Read| Haal de lijst van EventHub-autorisatieregels|
|/Namespaces/eventhubs/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/eventhubs/authorizationRules/Write|Autorisatieregels EventHub maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels kunnen worden bijgewerkt.|
|/Namespaces/eventHubs/consumergroups/DELETE|Bewerking ConsumerGroup bron verwijderen|
|/Namespaces/eventHubs/consumergroups/Read|Lijst met beschrijvingen van de Resource ConsumerGroup ophalen|
|/Namespaces/eventHubs/consumergroups/Write|Maken of bijwerken ConsumerGroup eigenschappen.|
|/Namespaces/eventhubs/DELETE|Bewerking EventHub-bron verwijderen|
|/Namespaces/eventhubs/Read|Lijst met beschrijvingen van EventHub-Resource|
|/Namespaces/eventhubs/Write|Maken of bijwerken EventHub-eigenschappen.|
|/Namespaces/messagingPlan/Read|Hiermee wordt de Messaging plannen voor een naamruimte. Deze API is afgeschaft. Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API.|
|/Namespaces/messagingPlan/Write|De Messaging plannen voor een naamruimte-updates. Deze API is afgeschaft. Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API.|
|/Namespaces/operationresults/Read|De status van de naamruimtebewerking ophalen|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/read|Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen|
|/Namespaces/providers/Microsoft.Insights/diagnosticSettings/Write|Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen|
|/namespaces/providers/Microsoft.Insights/logDefinitions/read|Lijst met Namespace logboeken Resource beschrijvingen|
|/Namespaces/providers/Microsoft.Insights/metricDefinitions/Read|Lijst met Namespace metrische Resource beschrijvingen|
|/Namespaces/Read|De lijst met beschrijvingen van resources van naamruimten ophalen|
|naamruimten/schrijven|Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Labels en capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt.|
|/Operations/Read|Bewerkingen ophalen|
|registratie-/ actie|Hiermee wordt het abonnement voor de EventHub-resourceprovider geregistreerd en wordt het maken van EventHub-resources mogelijk|
|/SKU/Read|Lijst met beschrijvingen van de Resource-Sku ophalen|
|/SKU/Regions/Read|Lijst met beschrijvingen van de Resource SkuRegions ophalen|
|/ unregister/actie|Hiermee wordt de EventHub-resourceprovider geregistreerd|

## <a name="microsoftfeatures"></a>Microsoft.Features

| Bewerking | Beschrijving |
|---|---|
|/Features/Read|Hiermee kunt u de functies van een abonnement ophalen.|
|/providers/Features/Read|Hiermee kunt u de functie van een abonnement voor een opgegeven resourceprovider ophalen.|
|/providers/Features/register/Action|Hiermee kunt u de functie van een abonnement voor een opgegeven resourceprovider registreren.|
|/providers/Features/unregister/Action|Hiermee kunt u de registratie van de functie van een abonnement voor een opgegeven resourceprovider ongedaan maken.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| Bewerking | Beschrijving |
|---|---|
|/clusters/changerdpsetting/Action|RDP-instelling voor HDInsight-Cluster wijzigen|
|/clusters/Configurations/Action|Werk de configuratie van de HDInsight-Cluster|
|/clusters/Configurations/Read|HDInsight-Cluster configuraties ophalen|
|/clusters/DELETE|Een HDInsight-Cluster verwijderen|
|/clusters/providers/Microsoft.Insights/diagnosticSettings/Read|De diagnostische instelling voor de HDInsight-Cluster van de resource opgehaald|
|/clusters/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instelling voor de resource HDInsight-Cluster|
|/clusters/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor HDInsight-Cluster|
|/clusters/Read|Meer informatie over de HDInsight-Cluster|
|/clusters/roles/Resize/Action|Een HDInsight-Cluster vergroten of verkleinen|
|clusters/schrijven|Maken of bijwerken van HDInsight-Cluster|
|/Locations/Capabilities/Read|Ophalen van abonnement mogelijkheden|
|/Locations/checkNameAvailability/Read|Beschikbaarheid van de naam controleren|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| Bewerking | Beschrijving |
|---|---|
|/jobs/delete|Hiermee verwijdert u een bestaande taak.|
|/jobs/listBitLockerKeys/action|Hiermee haalt u de BitLocker-sleutels voor de gespecificeerde taak.|
|/jobs/Read|Hiermee wordt de eigenschappen voor de opgegeven taak of de lijst met taken geretourneerd.|
|taken/schrijven|Maakt een taak met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de gespecificeerde taak.|
|/Locations/Read|Haalt u de eigenschappen voor de opgegeven locatie of de lijst met locaties geretourneerd.|
|registratie-/ actie|Registreert het abonnement voor de resourceprovider voor importeren/exporteren en wordt het maken van de taken voor importeren/exporteren.|

## <a name="microsoftinsights"></a>Microsoft.Insights

| Bewerking | Beschrijving |
|---|---|
|/ ActionGroups/verwijderen|Een actiegroep verwijderen|
|ActionGroups/leestijd|Een actiegroep lezen|
|/ ActionGroups/schrijven|Een actiegroep schrijven|
|/ ActivityLogAlerts/geactiveerd/actie|Waarschuwing voor een activiteitenlogboek geactiveerd|
|/ ActivityLogAlerts/verwijderen|Een waarschuwing voor een activiteitenlogboek verwijderen|
|ActivityLogAlerts/leestijd|Een waarschuwing voor een activiteitenlogboek lezen|
|/ ActivityLogAlerts/schrijven|Een waarschuwing voor een activiteitenlogboek lezen|
|/ AlertRules/geactiveerd/actie|Waarschuwingsregel geactiveerd|
|/ AlertRules/verwijderen|Configuratie van een waarschuwingsregel verwijderen|
|AlertRules/incidenten/leestijd|Incidentconfiguratie van een waarschuwingsregel lezen|
|AlertRules/leestijd|Configuratie van een waarschuwingsregel lezen|
|/ AlertRules/opgelost/actie|Waarschuwingsregel opgelost|
|/ AlertRules/beperkt/actie|De waarschuwingsregel wordt beperkt|
|/ AlertRules/schrijven|Naar de configuratie van een waarschuwingsregel schrijven|
|/ AutoscaleSettings/verwijderen|Configuratie-instelling voor automatisch schalen verwijderen|
|/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read|Metrische definities lezen|
|AutoscaleSettings/leestijd|Configuratie-instelling voor automatisch schalen lezen|
|/ AutoscaleSettings/Scaledown/actie|Bewerking Omlaag schalen via automatisch schalen|
|/ AutoscaleSettings/Scaleup/actie|Bewerking Omhoog schalen via automatisch schalen|
|/ AutoscaleSettings/schrijven|Naar een configuratie-instelling voor automatisch schalen schrijven|
|Onderdelen, AnalyticsItems/verwijderen|Een Application Insights analytics-item verwijderen|
|Onderdelen/AnalyticsItems/leestijd|Lezen van een Application Insights analytics-item|
|Onderdelen/AnalyticsItems/schrijftijd|Schrijven van een Application Insights analytics-item|
|/ Components/AnalyticsTables/actie|Actie voor Application Insights analytics tabel|
|Onderdelen, AnalyticsTables/verwijderen|Verwijderen van een Application Insights tabel analytics schema|
|Onderdelen/AnalyticsTables/leestijd|Een Application Insights lezen tabel analytics schema|
|Onderdelen/AnalyticsTables/schrijftijd|Schrijven van een Application Insights tabel analytics schema|
|/ Components/aantekeningen/verwijderen|Verwijderen van een aantekening Application Insights|
|Onderdelen/aantekeningen/leestijd|Lezen van een aantekening Application Insights|
|Onderdelen/aantekeningen/schrijftijd|Schrijven van een aantekening Application Insights|
|Onderdelen/Api/leestijd|Lezen van gegevens van Application Insights-onderdeel API|
|/ Components/ApiKeys/actie|Een Application Insights-API-sleutel te genereren|
|Onderdelen, ApiKeys/verwijderen|Verwijderen van een Application Insights-API-sleutel|
|Onderdelen/ApiKeys/leestijd|Lezen van een Application Insights-API-sleutel|
|Onderdelen/BillingPlanForComponent/leestijd|Lezen van een abonnement voor Application Insights-onderdeel|
|Onderdelen/CurrentBillingFeatures/leestijd|Lezen van de huidige facturering functies voor Application Insights-onderdeel|
|Onderdelen/CurrentBillingFeatures/schrijftijd|Schrijven van huidige facturering functies voor Application Insights-onderdeel|
|/Components/DefaultWorkItemConfig/Read|Een Application Insights ALM integratie standaardconfiguratie lezen|
|Onderdelen, verwijderen|De configuratie van een Application Insights-onderdeel verwijderen|
|/ Components/ExportConfiguration/actie|Application Insights exporteren actie instellingen|
|Onderdelen, ExportConfiguration/verwijderen|Verwijderen van Application Insights exporteren-instellingen|
|Onderdelen/ExportConfiguration/leestijd|Lezen van Application Insights exporteren-instellingen|
|Onderdelen/ExportConfiguration/schrijftijd|Schrijven Application Insights exporteren-instellingen|
|Onderdelen/ExtendQueries/leestijd|Lezen van Application Insights onderdeel uitgebreide queryresultaten|
|Onderdelen, Favorieten/verwijderen|Een Application Insights-favoriet verwijderen|
|Onderdelen/Favorieten/leestijd|Lezen van een Application Insights-favoriet|
|Onderdelen/Favorieten/schrijftijd|Schrijven van een Application Insights-favoriet|
|Onderdelen/FeatureCapabilities/leestijd|Functiemogelijkheden lezen Application Insights-onderdeel|
|Onderdelen/GetAvailableBillingFeatures/leestijd|Lezen van Application Insights onderdeel beschikbare facturering functies|
|/Components/GetToken/Read|Lezen van een token Application Insights-onderdeel|
|/ Components/ListMigrationDate/actie|Get back abonnement migratiedatum|
|Onderdelen/ListMigrationDate/leestijd|Get back abonnement migratiedatum|
|Onderdelen/MetricDefinitions/leestijd|Application Insights onderdeel metrische definities lezen|
|Onderdelen/metrische gegevens/leestijd|Metrische gegevens lezen Application Insights-onderdeel|
|/Components/MigrateToNewpricingModel/Action|Abonnement migreren naar nieuw prijsmodel|
|/ Components / / actie verplaatsen|Een Application Insights-Component verplaatsen naar een andere resourcegroep of abonnement|
|/Components/MyAnalyticsItems/Delete|Een Application Insights persoonlijke analytics-item verwijderen|
|Onderdelen/MyAnalyticsItems/leestijd|Lezen van een Application Insights persoonlijke analytics-item|
|Onderdelen/MyAnalyticsItems/schrijftijd|Schrijven van een Application Insights persoonlijke analytics-item|
|Onderdelen/mijn Favorieten/leestijd|Lezen van een persoonlijke Application Insights-favoriet|
|Onderdelen/PricingPlans/leestijd|Lezen van een plan prijzen Application Insights-onderdeel|
|Onderdelen/PricingPlans/schrijftijd|Een Application Insights-onderdeel prijzen plan schrijven|
|/Components/ProactiveDetectionConfigs/Read|Configuratie van de proactieve detectie lezen Application Insights|
|/Components/ProactiveDetectionConfigs/Write|Configuratie van Application Insights proactieve detectie schrijven|
|/Components/providers/Microsoft.Insights/MetricDefinitions/Read|Metrische definities lezen|
|Onderdelen/QuotaStatus/leestijd|Lezen van de quota van Application Insights Onderdeelstatus|
|Onderdelen/leestijd|De configuratie van een Application Insights-onderdeel lezen|
|/ Components/RollbackToLegacyPricingModel/actie|Terugdraaien abonnement op oudere model prijzen|
|/Components/SyntheticMonitorLocations/Read|Lezen van Application Insights webtest locaties|
|Onderdelen, WorkItemConfigs/verwijderen|Verwijderen van de configuratie van een Application Insights ALM-integratie|
|/Components/WorkItemConfigs/Read|Lezen van de configuratie van een Application Insights ALM-integratie|
|Onderdelen/WorkItemConfigs/schrijftijd|Schrijven van een Application Insights ALM integratie-configuratie|
|Onderdelen/schrijftijd|Schrijven naar de configuratie van een Application Insights-onderdeel|
|/ DiagnosticSettings/verwijderen|Configuratie voor de diagnostische gegevens verwijderen|
|DiagnosticSettings/leestijd|Een configuratie voor de diagnostische instellingen lezen|
|/ DiagnosticSettings/schrijven|Schrijven naar de configuratie voor de diagnostische instellingen|
|EventCategories/leestijd|Een gebeurteniscategorie lezen|
|/EventTypes/digestevents/Read|Samenvatting beheergebeurtenistype lezen|
|/EventTypes/Values/Read|Waarden beheergebeurtenistype lezen|
|/ ExtendedDiagnosticSettings/verwijderen|Configuratie voor de uitgebreide diagnostische instellingen verwijderen|
|ExtendedDiagnosticSettings/leestijd|Configuratie voor de uitgebreide diagnostische instellingen lezen|
|/ ExtendedDiagnosticSettings/schrijven|Configuratie voor de uitgebreide diagnostische instellingen schrijven|
|LogDefinitions/leestijd|Logboekdefinities lezen|
|/ LogProfiles/verwijderen|Configuratie voor logboekprofielen verwijderen|
|LogProfiles/leestijd|Logboekprofielen lezen|
|/ LogProfiles/schrijven|Schrijven naar een configuratie voor een logboekprofiel|
|/ MetricAlerts/verwijderen|Een waarschuwing voor metrische gegevens verwijderen|
|MetricAlerts/leestijd|Een waarschuwing voor metrische gegevens lezen|
|/ MetricAlerts/schrijven|Een waarschuwing voor metrische gegevens schrijven|
|/MetricDefinitions/Microsoft.Insights/Read|Metrische definities lezen|
|/MetricDefinitions/providers/Microsoft.Insights/Read|Metrische definities lezen|
|MetricDefinitions/leestijd|Metrische definities lezen|
|Metrische gegevens/providers/metrische gegevens/leestijd|De metrische gegevens lezen|
|Metrische gegevens/leestijd|De metrische gegevens lezen|
|Metrische gegevens/schrijftijd|Metrische gegevens schrijven|
|Bewerkingen/leestijd|De bewerkingen lezen|
|/ Registreren/actie|De Microsoft Insights-provider registeren|
|/ Tenants/registreren/actie|Hiermee wordt de Microsoft Insights-provider geïnitialiseerd|
|/ Unregister/actie|De Microsoft Insights-provider registeren|
|/ Webtests/verwijderen|Een configuratie voor een webtest verwijderen|
|/Webtests/GetToken/Read|Lezen van het token van een webtest|
|Webtests/MetricDefinitions/leestijd|Een webtest metrische definities lezen|
|Webtests/metrische gegevens/leestijd|Lezen van een webtest metrische gegevens|
|Webtests/leestijd|Een configuratie voor een webtest lezen|
|/ Webtests/schrijven|Schrijven naar een configuratie voor een webtest|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| Bewerking | Beschrijving |
|---|---|
|/checkNameAvailability/Read|Hiermee wordt gecontroleerd of de opgegeven sleutelkluisnaam geldig is en niet wordt gebruikt|
|/deletedVaults/read|De eigenschappen van voorlopig verwijderde sleutelkluizen weergeven|
|/hsmPools/delete|Een HSM-groep verwijderen|
|/hsmPools/joinVault/action|Deelnemen aan een sleutelkluis voor een HSM-groep|
|/hsmPools/Read|De eigenschappen van een HSM-groep weergeven|
|hsmPools/schrijven|Een nieuwe HSM-groep maken of de eigenschappen van een bestaande HSM-groep bijwerken|
|/Locations/deletedVaults/Purge/Action|Een voorlopig verwijderde sluitelkluis leegmaken|
|/Locations/deletedVaults/Read|De eigenschappen van voorlopig verwijderde sleutelkluis weergeven|
|/locations/deleteVirtualNetworkOrSubnets/action|Hiermee wordt aan Microsoft.KeyVault doorgegeven dat een virtueel netwerk of subnet wordt verwijderd|
|/Locations/operationResults/Read|Het resultaat van een langdurige uitvoeringsbewerking controleren|
|/Operations/Read|Hiermee wordt een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.KeyVault-resourceprovider|
|registratie-/ actie|Hiermee wordt een abonnement geregistreerd|
|/ unregister/actie|Hiermee wordt de registratie van een abonnement opgeheven|
|/vaults/accessPolicies/Write|Een bestaand toegangsbeleid bijwerken door samenvoegen of vervangen, of een nieuw toegangsbeleid toevoegen aan een kluis.|
|/vaults/DELETE|Een sleutelkluis verwijderen|
|/vaults/Deploy/Action|Hiermee wordt toegang tot de geheimen in een sleutelkluis ingeschakeld tijdens het implementeren van Azure-resources|
|/vaults/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/vaults/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/vaults/providers/Microsoft.Insights/logDefinitions/read|Hiermee worden de beschikbare logboeken voor een sleutelkluis opgehaald|
|/vaults/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee worden de beschikbare metrische gegevens voor een sleutelkluis opgehaald|
|/vaults/Read|De eigenschappen van een sleutelkluis weergeven|
|/vaults/secrets/Read|De eigenschappen van een geheim weergeven, maar niet de waarde|
|/vaults/secrets/Write|Een nieuw geheim maken of de waarde van een bestaand geheim bijwerken|
|kluizen/schrijven|Een nieuwe sleutelkluis maken of de eigenschappen van een bestaande sleutelkluis bijwerken|

## <a name="microsoftlabservices"></a>Microsoft.LabServices

| Bewerking | Beschrijving |
|---|---|
|/labAccounts/CreateLab/action|Een lab maken in een lab-account.|
|/labAccounts/delete|Lab-accounts te verwijderen.|
|/labAccounts/labs/delete|Labs verwijderen.|
|/labAccounts/labs/environmentSettings/delete|Omgevingsinstelling verwijderen.|
|/labAccounts/labs/environmentSettings/environments/delete|Verwijder omgevingen.|
|/labAccounts/Labs/environmentSettings/Environments/Read|Lezen omgevingen.|
|/labAccounts/labs/environmentSettings/environments/write|Toevoegen of wijzigen van omgevingen.|
|/labAccounts/labs/environmentSettings/Publish/action|Bepalingen/deprovisions vereiste resources voor een omgeving instellen op basis van de huidige status van de testomgeving /-instelling.|
|/labAccounts/Labs/environmentSettings/Read|Omgevingsinstelling gelezen.|
|/labAccounts/labs/environmentSettings/write|Toevoegen of wijzigen van de omgeving.|
|/labAccounts/Labs/Read|Labs lezen.|
|/labAccounts/labs/users/delete|Gebruikers verwijderen.|
|/labAccounts/Labs/Users/Read|Gebruikers worden gelezen.|
|/labAccounts/Labs/Users/Write|Toevoegen of wijzigen van gebruikers.|
|/labAccounts/labs/write|Toevoegen of wijzigen van labs.|
|/labAccounts/Read|Lab accounts lezen.|
|labAccounts/schrijven|Toevoegen of wijzigen van accounts lab.|
|/Locations/Operations/Read|Lees-en schrijfopdrachten.|
|registratie-/ actie|Registreert het abonnement|

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

| Bewerking | Beschrijving |
|---|---|
|/accounts/DELETE|Verwijderen van een locatie op basis van Services-Account.|
|/accounts/listKeys/Action|Lijst met Services-Account van locatie op basis van sleutels|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/accounts/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/accounts/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor de locatie op basis van serviceaccounts|
|/Accounts/read|Ophalen van een locatie op basis van Services-Account.|
|/accounts/regenerateKey/Action|Nieuwe locatie op basis van Services-Account primaire of secundaire sleutel genereren|
|accounts/schrijven|Maken of bijwerken van een locatie op basis van Services-Account.|
|registratie-/ actie|Registreer de provider|

## <a name="microsoftlogic"></a>Microsoft.Logic

| Bewerking | Beschrijving |
|---|---|
|/integrationAccounts/agreements/delete|Hiermee verwijdert u de overeenkomst in integratie-account.|
|/integrationAccounts/agreements/listContentCallbackUrl/Action|Hiermee wordt de URL van de callback voor inhoud van de overeenkomst in integratie-account.|
|/integrationAccounts/agreements/Read|Leest de overeenkomst in integratie-account.|
|/integrationAccounts/agreements/Write|Maken of bijwerken van de overeenkomst in integratie-account.|
|/integrationAccounts/assemblies/delete|Hiermee verwijdert u de assembly in integratie-account.|
|/integrationAccounts/Assemblies/listContentCallbackUrl/Action|Hiermee wordt de callback-URL voor assembly-inhoud in integratie-account.|
|/integrationAccounts/Assemblies/Read|Leest de assembly in integratie-account.|
|/integrationAccounts/Assemblies/Write|Maken of bijwerken van de assembly in integratie-account.|
|/integrationAccounts/batchConfigurations/delete|Hiermee verwijdert u de configuratie van de batch in integratie-account.|
|/integrationAccounts/batchConfigurations/Read|Hiermee wordt de configuratie van de batch in integratie account gelezen.|
|/integrationAccounts/batchConfigurations/Write|Maken of bijwerken van de configuratie van de batch in integratie-account.|
|/integrationAccounts/certificates/delete|Hiermee verwijdert u het certificaat in integratie-account.|
|/integrationAccounts/Certificates/Read|Het certificaat in het account van de integratie worden gelezen.|
|/integrationAccounts/Certificates/Write|Maken of bijwerken van het certificaat in integratie-account.|
|/integrationAccounts/delete|Hiermee verwijdert u de integratie-account.|
|/integrationAccounts/listCallbackUrl/Action|Hiermee wordt de URL van de callback voor integratie-account.|
|/integrationAccounts/listKeyVaultKeys/action|Hiermee haalt de sleutels in de sleutelkluis.|
|/integrationAccounts/logTrackingEvents/action|Hiermee wordt de gebeurtenissen bijhouden in de integratie-account.|
|/integrationAccounts/maps/delete|Hiermee verwijdert u de kaart in integratie-account.|
|/integrationAccounts/Maps/listContentCallbackUrl/Action|Hiermee wordt de URL van de callback voor kaartinhoud in integratie-account.|
|/integrationAccounts/Maps/Read|Leest de kaart in integratie-account.|
|/integrationAccounts/Maps/Write|Maken of bijwerken van de kaart in integratie-account.|
|/integrationAccounts/partners/delete|Hiermee verwijdert u de partner in integratie-account.|
|/integrationAccounts/partners/listContentCallbackUrl/Action|Hiermee wordt de URL van de callback voor Partnerinformatie in integratie-account.|
|/integrationAccounts/partners/read|Leest de vennoot in integratie-account.|
|/integrationAccounts/partners/Write|Maken of bijwerken van de partner in integratie-account.|
|/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read|Hiermee worden de logboekdefinities van het integratieaccount gelezen.|
|/integrationAccounts/Read|Leest de integratie-account.|
|/integrationAccounts/regenerateAccessKey/Action|Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd.|
|/integrationAccounts/schemas/delete|Hiermee verwijdert u het schema in integratie-account.|
|/integrationAccounts/schemas/listContentCallbackUrl/Action|Hiermee wordt de callback-URL voor schema-inhoud in integratie-account.|
|/integrationAccounts/schemas/Read|Leest het schema in integratie-account.|
|/integrationAccounts/schemas/Write|Maken of bijwerken van het schema in integratie-account.|
|/integrationAccounts/Sessions/DELETE|Hiermee verwijdert u de sessie in integratie-account.|
|/integrationAccounts/Sessions/Read|Hiermee wordt de configuratie van de batch in integratie account gelezen.|
|/integrationAccounts/Sessions/Write|Maken of bijwerken van de sessie in integratie-account.|
|integrationAccounts/schrijven|Maken of bijwerken van de integratie-account.|
|/Locations/workflows/Validate/Action|Hiermee wordt de werkstroom gevalideerd.|
|/Operations/Read|Hiermee haalt u de bewerking.|
|registratie-/ actie|Registreert de Microsoft.Logic resourceprovider voor een bepaald abonnement.|
|/workflows/accessKeys/DELETE|Hiermee wordt de toegangssleutel verwijderd.|
|/workflows/accessKeys/List/Action|Hiermee wordt een lijst met toegangssleutelgeheimen gemaakt.|
|/workflows/accessKeys/Read|Hiermee wordt de toegangssleutel gelezen.|
|/workflows/accessKeys/Regenerate/Action|Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd.|
|/workflows/accessKeys/Write|Hiermee wordt de toegangssleutel opnieuw gemaakt of bijgewerkt.|
|/workflows/DELETE|Hiermee wordt de werkstroom verwijderd.|
|/workflows/Disable/Action|Hiermee wordt de werkstroom uitgeschakeld.|
|/workflows/Enable/Action|Hiermee wordt de werkstroom ingeschakeld.|
|/workflows/listCallbackUrl/Action|Hiermee wordt de URL voor terugbellen van de werkstroom opgehaald.|
|/workflows/listSwagger/Action|Hiermee worden de swaggerdefinities van de werkstroom opgehaald.|
|/workflows/Move/Action|Hiermee wordt de werkstroom verplaatst van de bestaande abonnements-id, resourcegroep en/of naam naar een andere abonnements-id, resourcegroep en/of naam.|
|/workflows/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee worden de diagnostische instellingen van de werkstroom gelezen.|
|/workflows/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee worden de diagnostische instelling van de werkstroom gemaakt of bijgewerkt.|
|/workflows/providers/Microsoft.Insights/logDefinitions/Read|Hiermee worden de logboekdefinities van de werkstroom gelezen.|
|/workflows/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee worden de metrische definities van de werkstroom gelezen.|
|/workflows/Read|Hiermee wordt de werkstroom gelezen.|
|/workflows/regenerateAccessKey/Action|Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd.|
|/workflows/Run/Action|Hiermee wordt de werkstroomuitvoering gestart.|
|/workflows/runs/Actions/listExpressionTraces/Action|Hiermee worden de expressietraceringen voor de uitvoeringsactie van de werkstroom opgehaald.|
|/workflows/runs/Actions/Read|Hiermee wordt de uitvoeringsactie van de werkstroom gelezen.|
|/workflows/runs/Actions/repetitions/listExpressionTraces/Action|Hiermee haalt u de werkstroom actie herhaling expressie traceringen worden uitgevoerd.|
|/workflows/runs/Actions/repetitions/Read|Hiermee wordt de werkstroom actie herhaling gelezen.|
|/workflows/runs/Actions/scoperepetitions/Read|Hiermee wordt de werkstroom actie bereik herhaling gelezen.|
|/workflows/runs/Cancel/Action|Hiermee wordt de werkstroomuitvoering geannuleerd.|
|/workflows/runs/Operations/Read|Hiermee wordt de bewerkingsstatus van de werkstroomuitvoering gelezen.|
|werkstromen/wordt uitgevoerd/lezen|Hiermee wordt de werkstroomuitvoering gelezen.|
|/workflows/Suspend/Action|Onderbreekt de werkstroom.|
|/workflows/triggers/histories/Read|Hiermee wordt de geschiedenis van de triggers gelezen.|
|/workflows/triggers/histories/resubmit/Action|Hiermee wordt de werkstroomtrigger opnieuw verzonden.|
|/workflows/triggers/listCallbackUrl/Action|Hiermee wordt de URL voor terugbellen van de trigger opgehaald.|
|/workflows/triggers/Read|Hiermee wordt de trigger gelezen.|
|/workflows/triggers/reset/Action|Hiermee stelt u de trigger.|
|/workflows/triggers/Run/Action|Hiermee wordt de trigger uitgevoerd.|
|/workflows/triggers/setState/Action|Hiermee stelt u de status van de trigger.|
|/workflows/Validate/Action|Hiermee wordt de werkstroom gevalideerd.|
|/workflows/Versions/Read|Hiermee wordt de werkstroomversie gelezen.|
|/workflows/Versions/triggers/listCallbackUrl/Action|Hiermee wordt de URL voor terugbellen van de trigger opgehaald.|
|werkstromen/schrijven|Hiermee wordt de werkstroom gemaakt of bijgewerkt.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| Bewerking | Beschrijving |
|---|---|
|/commitmentPlans/commitmentAssociations/Move/Action|Verplaatsen van een Machine Learning-streven Plan koppeling|
|/commitmentPlans/commitmentAssociations/Read|Lezen van een Machine Learning-streven Plan koppeling|
|/commitmentPlans/DELETE|Een Machine streven trainingsplan verwijderen|
|/commitmentPlans/join/Action|Lid worden van een Machine streven trainingsplan|
|/commitmentPlans/Read|Een Machine streven trainingsplan lezen|
|commitmentPlans/schrijven|Maken of bijwerken van een Machine Learning streven plannen|
|/Locations/operationresults/Read|Resultaat van een Machine Learning-bewerking ophalen|
|/Locations/operationsstatus/Read|Status van een actieve Machine Learning-bewerking ophalen|
|/Operations/Read|Machine Learning-bewerkingen ophalen|
|registratie-/ actie|Het abonnement voor de machine learning web service resourceprovider registreert en wordt het maken van webservices.|
|/skus/Read|Machine Learning-streven Plan SKU's ophalen|
|webServices/actie|Regionale Web Service-eigenschappen voor ondersteunde regio's maken|
|/webServices/delete|Verwijderen van een Machine Learning-webservice|
|/webServices/Read|Lezen van een Machine Learning-webservice|
|/webServices/write|Maken of bijwerken van een Machine Learning-webservice|
|-Werkruimten, verwijderen|Verwijderen van een Machine Learning-werkruimte|
|/ Werkruimten/listworkspacekeys/actie|Lijst met sleutels voor een Machine Learning-werkruimte|
|Werkruimten/leestijd|Lezen van een Machine Learning-werkruimte|
|/ Werkruimten/resyncstoragekeys/actie|Sleutels van opslagaccount geconfigureerd voor een Machine Learning-werkruimte synchroniseren|
|/ Werkruimten/schrijven|Een Machine Learning-werkruimte gemaakt of bijgewerkt|

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

| Bewerking | Beschrijving |
|---|---|
|/operationalizationClusters/checkUpdate/Action|Controleer of er updates beschikbaar voor systeemservices voor het cluster uitoefening zijn|
|/operationalizationClusters/DELETE|Hosting-account verwijderen|
|/operationalizationClusters/listKeys/Action|Lijst met sleutels die zijn gekoppeld aan uitoefening cluster|
|/operationalizationClusters/Read|Een hosting-account lezen|
|/operationalizationClusters/updateSystem/action|De systeemservices in een cluster uitoefening bijwerken|
|operationalizationClusters/schrijven|Hosting-account maken of bijwerken|
|registratie-/ actie|Registreert de abonnements-ID voor de resourceprovider en wordt het maken van een machine learning rekenresources|

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

| Bewerking | Beschrijving |
|---|---|
|/accounts/DELETE|Hosting-account verwijderen|
|/Accounts/read|Een hosting-account lezen|
|accounts/schrijven|Hosting-account maken of bijwerken|
|registratie-/ actie|Registreert de abonnements-ID voor de resourceprovider en wordt het maken van een hosting-account|

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

| Bewerking | Beschrijving |
|---|---|
|/userAssignedIdentities/Assign/Action|RBAC-actie voor het toewijzen van een bestaande gebruiker identiteit toegewezen aan een resource|
|/userAssignedIdentities/delete|Hiermee verwijdert u een bestaande gebruiker toegewezen identiteit|
|/userAssignedIdentities/read|Een bestaande gebruiker toegewezen identiteit opgehaald|
|userAssignedIdentities/schrijven|Een nieuwe gebruiker toegewezen identiteit maken of bijwerken van de labels van een bestaande gebruiker toegewezen identiteit|

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

| Bewerking | Beschrijving |
|---|---|
|/labAccounts/CreateLab/action|Een lab maken in een lab-account.|
|/labAccounts/delete|Lab-accounts te verwijderen.|
|/labAccounts/labs/delete|Labs verwijderen.|
|/labAccounts/labs/environmentSettings/delete|Omgevingsinstelling verwijderen.|
|/labAccounts/labs/environmentSettings/environments/delete|Verwijder omgevingen.|
|/labAccounts/Labs/environmentSettings/Environments/Read|Lezen omgevingen.|
|/labAccounts/labs/environmentSettings/environments/write|Toevoegen of wijzigen van omgevingen.|
|/labAccounts/Labs/environmentSettings/Read|Omgevingsinstelling gelezen.|
|/labAccounts/labs/environmentSettings/write|Toevoegen of wijzigen van de omgeving.|
|/labAccounts/labs/labVms/delete|Verwijder de virtuele machines.|
|/labAccounts/labs/labVms/read|De virtuele machines gelezen.|
|/labAccounts/labs/labVms/write|Toevoegen of wijzigen van de virtuele machines.|
|/labAccounts/Labs/Read|Labs lezen.|
|/labAccounts/labs/write|Toevoegen of wijzigen van labs.|
|/labAccounts/Read|Lab accounts lezen.|
|labAccounts/schrijven|Toevoegen of wijzigen van accounts lab.|
|/Locations/Operations/Read|Lees-en schrijfopdrachten.|
|registratie-/ actie|Registreert het abonnement|

## <a name="microsoftmanagement"></a>Microsoft.Management

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Controleert of de naam van de opgegeven beheergroep geldig en uniek is.|
|/ getEntities/actie|Lijst van alle entiteiten (beheergroepen, abonnementen, enz.) voor de geverifieerde gebruiker.|
|/managementGroups/DELETE|Verwijder de beheergroep.|
|/managementGroups/Read|Lijst met beheergroepen voor de geverifieerde gebruiker.|
|/managementGroups/Subscriptions/DELETE|Abonnement van de beheergroep koppelt ongedaan.|
|/managementGroups/Subscriptions/Write|Gekoppeld bestaande abonnement met de beheergroep.|
|managementGroups/schrijven|Maken of bijwerken van een beheergroep.|

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

| Bewerking | Beschrijving |
|---|---|
|/ ClassicDevServices/verwijderen|Biedt een DELETE-bewerking op de bron van een klassieke dev-service.|
|/ClassicDevServices/listSecrets/action|Hiermee haalt u een klassieke dev sleutels voor resource management service.|
|/ClassicDevServices/listSingleSignOnToken/action|Hiermee wordt de eenmalige aanmelding op URL voor een klassieke dev-service.|
|ClassicDevServices/leestijd|Biedt een GET-bewerking voor een klassieke dev-service.|
|/ ClassicDevServices/regenerateKey/actie|Genereert een klassieke dev service resource management-sleutels.|
|Bewerkingen/leestijd|Leesbewerkingen voor alle resourcetypen.|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| Bewerking | Beschrijving |
|---|---|
|/agreements/Offers/plans/Cancel/Action|Annuleren van een overeenkomst voor een bepaalde marketplace-item|
|/agreements/Offers/plans/Read|Retourneren van een overeenkomst voor een bepaalde marketplace-item|
|/agreements/Offers/plans/Sign/Action|Meld u aan een overeenkomst voor een bepaalde marketplace-item|
|/agreements/Read|Retourneren van alle overeenkomsten onder opgegeven abonnement|
|/offertypes/Publishers/Offers/plans/agreements/Read|Een overeenkomst voor een bepaalde marketplace-item voor virtuele machine ophalen|
|/offertypes/Publishers/Offers/plans/agreements/Write|Meld u aan of een overeenkomst voor een item van de virtuele machine opgegeven marketplace annuleren|

## <a name="microsoftmedia"></a>Microsoft.Media

| Bewerking | Beschrijving |
|---|---|
|/ checknameavailability/actie|Controleert of de naam van een Media Services-account beschikbaar is|
|/mediaservices/DELETE|Een Media Services-Account verwijderen|
|/mediaservices/listKeys/Action|Lijst van de ACS-sleutels voor de Media Services-account|
|/mediaservices/Read|Een Media Services-Account lezen|
|/mediaservices/regenerateKey/Action|Een Media Services ACS-sleutel opnieuw genereren|
|/mediaservices/syncStorageKeys/Action|Synchroniseren van de sleutels van de opslag voor een gekoppelde Azure Storage-account|
|mediaservices/schrijven|Een Media Services-Account maken of bijwerken|
|/Operations/Read|Een Media Services-Account lezen|
|registratie-/ actie|Het abonnement voor de Media Services-resourceprovider geregistreerd en wordt het maken van Media Services-accounts|

## <a name="microsoftmigrate"></a>Microsoft.Migrate

| Bewerking | Beschrijving |
|---|---|
|Bewerkingen/leestijd|Leest de beschikbare bewerkingen|

## <a name="microsoftnetwork"></a>Microsoft.Network

| Bewerking | Beschrijving |
|---|---|
|/applicationGatewayAvailableSslOptions/predefinedPolicies/read|Application Gateway Ssl vooraf gedefinieerd beleid|
|/applicationGatewayAvailableSslOptions/Read|Application Gateway beschikbare Ssl-opties|
|/applicationGatewayAvailableWafRuleSets/Read|Toepassingsgateway beschikbaar Waf regelsets opgehaald|
|/applicationGateways/backendAddressPools/join/Action|Een application gateway back-end-adresgroep toevoegen|
|/applicationGateways/backendhealth/Action|Een back-end toepassingsstatus van de gateway opgehaald|
|/applicationGateways/DELETE|Een toepassingsgateway verwijderen|
|/applicationGateways/effectiveNetworkSecurityGroups/action|Routetabel geconfigureerd op de toepassingsgateway ophalen|
|/applicationGateways/effectiveRouteTable/Action|Routetabel geconfigureerd op de toepassingsgateway ophalen|
|/applicationGateways/providers/Microsoft.Insights/logDefinitions/Read|Hiermee worden de gebeurtenissen voor Application Gateway opgehaald|
|/applicationGateways/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee worden de beschikbare metrische gegevens voor Application Gateway opgehaald|
|/applicationGateways/Read|Een application gateway opgehaald|
|/applicationGateways/setSecurityCenterConfiguration/action|Sets Application Gateway Security Center-configuratie|
|/applicationGateways/start/Action|Een application gateway wordt gestart|
|/applicationGateways/Stop/Action|Een application gateway wordt gestopt|
|applicationGateways/schrijven|Een toepassingsgateway maken of bijwerken van een application gateway|
|/applicationSecurityGroups/delete|Hiermee verwijdert u de beveiligingsgroep van een toepassing|
|/applicationSecurityGroups/joinIpConfiguration/Action|Een IP-configuratie koppelt aan beveiligingsgroepen toepassing.|
|/applicationSecurityGroups/joinNetworkSecurityRule/Action|Een beveiligingsregel koppelt aan beveiligingsgroepen van toepassing.|
|/applicationSecurityGroups/Read|Opgehaald van een toepassing beveiligingsgroep-ID.|
|applicationSecurityGroups/schrijven|Maakt u de beveiligingsgroep van een toepassing of updates van een bestaande beveiligingsgroep voor de toepassing.|
|/bgpServiceCommunities/Read|Bgp-Service-community's ophalen|
|/checkTrafficManagerNameAvailability/action|Controleert de beschikbaarheid van een Traffic Manager ten opzichte van DNS-naam.|
|/Connections/DELETE|Deletes VirtualNetworkGatewayConnection|
|/Connections/Read|Gets VirtualNetworkGatewayConnection|
|/Connections/sharedkey/Action|VirtualNetworkGatewayConnection SharedKey ophalen|
|/Connections/sharedKey/Read|VirtualNetworkGatewayConnection SharedKey opgehaald|
|/Connections/sharedKey/Write|Maken of bijwerken van een bestaande VirtualNetworkGatewayConnection SharedKey|
|/Connections/vpndeviceconfigurationscript/Read|VPN-apparaatconfiguratie van VirtualNetworkGatewayConnection opgehaald|
|verbindingen/schrijven|Maken of bijwerken van een bestaande VirtualNetworkGatewayConnection|
|/ddosProtectionPlans/ddosProtectionPlanProxies/delete|Hiermee verwijdert u een Proxy DDoS-beveiliging plannen|
|/ddosProtectionPlans/ddosProtectionPlanProxies/read|De definitie van een DDoS-beveiliging plannen Proxy ophalen|
|/ddosProtectionPlans/ddosProtectionPlanProxies/Write|Maakt een DDoS-Proxy voor het plannen van beveiliging of updates en bestaande DDoS-beveiliging plannen Proxy|
|/ddosProtectionPlans/delete|Hiermee wordt een beveiligingsplan DDoS verwijderd|
|/ddosProtectionPlans/join/Action|Lid wordt een beveiligingsplan DDoS|
|/ddosProtectionPlans/read|Een beveiligingsplan DDoS opgehaald|
|ddosProtectionPlans/schrijven|Maakt een DDoS-beveiliging plannen of updates van een DDoS-beveiliging plannen |
|/dnsoperationresults/Read|Resultaten van een DNS-bewerking opgehaald|
|/dnsoperationstatuses/Read|Hiermee wordt de status van een DNS-bewerking opgehaald |
|/dnszones/A/DELETE|Verwijder de recordset met een bepaalde naam en typt u "A" uit een DNS-zone.|
|/dnszones/A/Read|De recordset van het type "A", in JSON-indeling verkrijgen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/A/Write|Maken of bijwerken van een recordset van het type "A" binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/AAAA/delete|Verwijder de recordset met een bepaalde naam en typ 'AAAA' uit een DNS-zone.|
|/dnszones/AAAA/read|De recordset van het type 'AAAA', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/AAAA/write|Maken of bijwerken van een recordset van het type 'AAAA' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/All/Read|DNS-recordsets in verschillende typen opgehaald|
|/dnszones/CAA/delete|Verwijder de recordset met een bepaalde naam en typ 'CAA' uit een DNS-zone.|
|/dnszones/CAA/read|De recordset van het type 'CAA', in JSON-indeling ophalen. De recordset omvat de TTL, labels en etag.|
|/dnszones/CAA/Write|Maken of bijwerken van een recordset van het type 'CAA' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/CNAME/delete|Verwijder de recordset met een bepaalde naam en typ 'CNAME' uit een DNS-zone.|
|/dnszones/CNAME/read|De recordset van het type 'CNAME', in JSON-indeling ophalen. De recordset omvat de TTL, labels en etag.|
|/dnszones/CNAME/write|Maken of bijwerken van een recordset van het type 'CNAME' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/DELETE|Verwijder de DNS-zone, in JSON-indeling. De zone-eigenschappen zijn labels, etag, numberOfRecordSets en maxNumberOfRecordSets.|
|/dnszones/MX/DELETE|Verwijder de recordset met een bepaalde naam en typ 'MX' uit een DNS-zone.|
|/dnszones/MX/Read|De recordset van het type 'MX', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/MX/Write|Maken of bijwerken van een recordset van het type 'MX' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/NS/DELETE|Hiermee verwijdert u de DNS-recordset van het type NS|
|/dnszones/NS/Read|DNS-recordset van het type NS opgehaald|
|/dnszones/NS/Write|Maken of bijwerken van DNS-recordset van het type NS|
|/dnszones/providers/Microsoft.Insights/diagnosticSettings/read|Diagnostische instellingen voor de DNS-zone opgehaald|
|/dnszones/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instellingen voor DNS-zone|
|/dnszones/providers/Microsoft.Insights/metricDefinitions/read|De DNS-zone metrische definities opgehaald|
|/dnszones/PTR/delete|Verwijder de recordset met een bepaalde naam en typ 'PTR' uit een DNS-zone.|
|/dnszones/PTR/read|De recordset van het type 'PTR', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/PTR/write|Maken of bijwerken van een recordset van het type 'PTR' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/Read|Ophalen van de DNS-zone, in JSON-indeling. De zone-eigenschappen zijn labels, etag, numberOfRecordSets en maxNumberOfRecordSets. Houd er rekening mee dat deze opdracht niet de recordsets in de zone ophaalt.|
|/dnszones/recordsets/Read|DNS-recordsets in verschillende typen opgehaald|
|/dnszones/SOA/Read|DNS-recordset van het type SOA opgehaald|
|/dnszones/SOA/Write|Maken of bijwerken van DNS-recordset van het type SOA|
|/dnszones/SRV/delete|Verwijder de recordset met een bepaalde naam en typ 'SRV' uit een DNS-zone.|
|/dnszones/SRV/Read|De recordset van het type 'SRV', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/SRV/write|Maken of bijwerken van de recordset van het type SRV|
|/dnszones/TXT/delete|Verwijder de recordset met een bepaalde naam en typ 'TXT' uit een DNS-zone.|
|/dnszones/txt/Read|De recordset van het type 'TXT', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/txt/Write|Maken of bijwerken van een recordset van het type 'TXT' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|dnszones/schrijven|Maken of bijwerken van een DNS-zone binnen een resourcegroep.  Gebruikt voor het bijwerken van de labels van een DNS-zone-resource. Houd er rekening mee dat deze opdracht kan niet worden gebruikt om te maken of bijwerken van recordsets binnen de zone.|
|/expressRouteCircuits/authorizations/delete|Hiermee verwijdert u een ExpressRouteCircuit-autorisatie|
|/expressRouteCircuits/authorizations/Read|Een vergunning ExpressRouteCircuit opgehaald|
|/expressRouteCircuits/authorizations/Write|Maken of bijwerken van een bestaande ExpressRouteCircuit-autorisatie|
|/expressRouteCircuits/delete|Een ExpressRouteCircuit verwijderen|
|/expressRouteCircuits/peerings/arpTables/Action|Een ExpressRouteCircuit Peering ArpTable opgehaald|
|/expressRouteCircuits/peerings/Connections/DELETE|Hiermee verwijdert u een verbinding ExpressRouteCircuit|
|/expressRouteCircuits/peerings/Connections/Read|Een verbinding ExpressRouteCircuit opgehaald|
|/expressRouteCircuits/peerings/Connections/Write|Maken of bijwerken van een bestaande ExpressRouteCircuit verbinding Resource|
|/expressRouteCircuits/peerings/DELETE|Hiermee verwijdert u een ExpressRouteCircuit Peering|
|/expressRouteCircuits/peerings/Read|Een ExpressRouteCircuit Peering opgehaald|
|/expressRouteCircuits/peerings/routeTables/Action|Een ExpressRouteCircuit Peering migratiestatus opgehaald|
|/expressRouteCircuits/peerings/routeTablesSummary/Action|Een overzicht van ExpressRouteCircuit Peering migratiestatus opgehaald|
|/expressRouteCircuits/peerings/stats/Read|Een ExpressRouteCircuit Peering statistieken opgehaald|
|/expressRouteCircuits/peerings/Write|Maken of bijwerken van een bestaande Peering ExpressRouteCircuit|
|/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/read|Diagnostische instellingen voor ExpressRoute-Circuits opgehaald|
|/expressRouteCircuits/providers/Microsoft.Insights/diagnosticSettings/write|Maken of bijwerken van diagnostische instellingen voor ExpressRoute-Circuits|
|/expressRouteCircuits/providers/Microsoft.Insights/logDefinitions/read|Ophalen van de gebeurtenissen voor ExpressRoute-Circuits|
|/expressRouteCircuits/providers/Microsoft.Insights/metricDefinitions/read|De metrische definities voor ExpressRoute-Circuits opgehaald|
|/expressRouteCircuits/Read|Een ExpressRouteCircuit ophalen|
|/expressRouteCircuits/stats/Read|Een ExpressRouteCircuit Stat opgehaald|
|expressRouteCircuits/schrijven|Maken of bijwerken van een bestaande ExpressRouteCircuit|
|/expressRouteCrossConnections/DELETE|Verwijderen van Express Route Cross-verbinding|
|/expressRouteCrossConnections/join/Action|Cross join een Express Route-verbinding|
|/expressRouteCrossConnections/peerings/arpTables/Action|Een Express Route kruislings verbinding Peering Arp-tabel opgehaald|
|/expressRouteCrossConnections/peerings/DELETE|Hiermee verwijdert u een Express Route kruislings verbinding Peering|
|/expressRouteCrossConnections/peerings/Read|Een Route snelle Cross-verbinding Peering opgehaald|
|/expressRouteCrossConnections/peerings/routeTables/Action|Een Express Route kruislings verbinding Peering routetabel opgehaald|
|/expressRouteCrossConnections/peerings/routeTableSummary/Action|Een Express Route kruislings samenvattingstabel verbinding Peering Route opgehaald|
|/expressRouteCrossConnections/peerings/stats/Read|Een Route snelle Cross-verbinding Peering statistieken opgehaald|
|/expressRouteCrossConnections/peerings/Write|Maakt een Cross-verbinding Peering voor Express Route of een bestaande Cross verbinding Peering voor Express Route-Updates|
|/expressRouteCrossConnections/Read|Ophalen van Express Route Cross-verbinding|
|expressRouteCrossConnections/schrijven|Maken of bijwerken Cross Express Route-verbinding|
|/expressRouteServiceProviders/read|Express Route-serviceproviders opgehaald|
|/loadBalancers/backendAddressPools/join/Action|Lid wordt van een back-endadresgroep voor load balancer|
|/loadBalancers/backendAddressPools/Read|Een definitie van het groep back-end-adres van load balancer ophalen|
|/loadBalancers/DELETE|Hiermee verwijdert u een load balancer|
|/loadBalancers/frontendIPConfigurations/Read|Een definitie van load balancer frontend IP-configuratie ophalen|
|/loadBalancers/inboundNatPools/join/action|Lid wordt van een load balancer binnenkomende nat-pool|
|/loadBalancers/inboundNatPools/read|Een load balancer ophalen inkomende nat-pooldefinitie|
|/loadBalancers/inboundNatRules/delete|Een binnenkomende nat-regel van load balancer verwijderen|
|/loadBalancers/inboundNatRules/join/Action|Lid wordt van een load balancer binnenkomende nat-regel|
|/loadBalancers/inboundNatRules/Read|Een load balancer ophalen inkomende nat-regel definiëren|
|/loadBalancers/inboundNatRules/Write|Een binnenkomende nat-regel van load balancer maken of bijwerken van een bestaande load balancer binnenkomende nat-regel|
|/loadBalancers/loadBalancingRules/Read|Een load balancer load taakverdeling ophalen|
|/loadBalancers/networkInterfaces/read|Verwijzingen naar alle netwerkinterfaces onder een load balancer ophalen|
|/loadBalancers/outboundNatRules/Read|Een definitie voor uitgaande nat-regel van load balancer opgehaald|
|/loadBalancers/probes/join/Action|U kunt met behulp van de tests van een load balancer. Bijvoorbeeld: met deze machtiging healthProbe-eigenschap van VM-scale set kan verwijzen naar de test.|
|/loadBalancers/probes/Read|Een load balancer-test opgehaald|
|/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/Read|De diagnostische instellingen van de Load Balancer opgehaald|
|/loadBalancers/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instellingen van de Load Balancer|
|/loadBalancers/providers/Microsoft.Insights/logDefinitions/Read|Hiermee worden de gebeurtenissen voor de Load Balancer opgehaald|
|/loadBalancers/providers/Microsoft.Insights/metricDefinitions/Read|De beschikbare metrische gegevens voor de Load Balancer opgehaald|
|/loadBalancers/Read|De definitie van een load balancer ophalen|
|/loadBalancers/virtualMachines/Read|Verwijzingen naar alle virtuele machines onder een load balancer ophalen|
|loadBalancers/schrijven|Een load balancer maken of bijwerken van een bestaande load balancer|
|/localnetworkgateways/delete|Deletes LocalNetworkGateway|
|/localnetworkgateways/read|Gets LocalNetworkGateway|
|localnetworkgateways/schrijven|Maken of bijwerken van een bestaande LocalNetworkGateway|
|/Locations/checkDnsNameAvailability/Read|Controleert of de DNS-label beschikbaar op de opgegeven locatie is|
|/Locations/operationResults/Read|Het bewerkingsresultaat van een asynchrone post- of DELETE-bewerking opgehaald|
|/Locations/Operations/Read|Bewerkingsresource ophalen die de status van een asynchrone bewerking vertegenwoordigt|
|/Locations/usages/Read|Meetgegevens voor softwaregebruik voor resources ophalen|
|/locations/virtualNetworkAvailableEndpointServices/read|Een lijst met beschikbare virtuele netwerk eindpunt Services ophalen|
|/networkInterfaces/delete|Hiermee verwijdert u een netwerkinterface|
|/networkInterfaces/diagnosticIdentity/read|Diagnostische identiteit van de Resource opgehaald|
|/networkInterfaces/effectiveNetworkSecurityGroups/action|Netwerkbeveiligingsgroepen geconfigureerde op Network Interface van de Vm ophalen|
|/networkInterfaces/effectiveRouteTable/action|De routetabel is geconfigureerd op de netwerkinterface van de virtuele machine ophalen|
|/networkInterfaces/ipconfigurations/read|Hiermee haalt u de definitie van de IP-configuratie voor een netwerk-interface. |
|/networkInterfaces/join/Action|Een virtuele Machine koppelt aan een netwerkinterface|
|/networkInterfaces/loadBalancers/read|Alle load balancers die de netwerkinterface deel van uitmaakt opgehaald|
|/networkInterfaces/providers/Microsoft.Insights/metricDefinitions/read|Hiermee worden beschikbare metrische gegevens voor de netwerkinterface opgehaald|
|/networkInterfaces/read|Haalt de definitie van een netwerkinterface. |
|networkInterfaces/schrijven|Een netwerkinterface maken of bijwerken van een bestaande netwerkinterface. |
|/networkSecurityGroups/defaultSecurityRules/read|Een standaardbeveiligingsregeldefinitie ophalen opgehaald|
|/networkSecurityGroups/delete|Hiermee wordt een netwerkbeveiligingsgroep verwijderd|
|/networkSecurityGroups/join/Action|Lid wordt van een netwerkbeveiligingsgroep|
|/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/Read|De diagnostische instellingen voor Netwerkbeveiligingsgroepen opgehaald|
|/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instellingen voor Netwerkbeveiligingsgroepen deze bewerking wordt geleverd door de Insights-resourceprovider.|
|/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read|De gebeurtenissen voor de netwerkbeveiligingsgroep opgehaald|
|/networkSecurityGroups/read|Een definitie van een netwerk opgehaald|
|/networkSecurityGroups/securityRules/delete|Hiermee verwijdert u een beveiligingsregel|
|/networkSecurityGroups/securityRules/Read|Een beveiligingsregeldefinitie ophalen opgehaald|
|/networkSecurityGroups/securityRules/write|Een beveiligingsregel maken of een bestaande beveiligingsregel bijwerken|
|/networkSecurityGroups/write|Een netwerkbeveiligingsgroep maken of bijwerken van een bestaande netwerkbeveiligingsgroep|
|/networkWatchers/availableProvidersList/action|Retourneert alle beschikbare internet serviceproviders voor een opgegeven Azure-regio.|
|/networkWatchers/azureReachabilityReport/action|Retourneert de score relatieve latentie voor internet serviceproviders van een opgegeven locatie en Azure-regio's.|
|/networkWatchers/configureFlowLog/action|Hiermee configureert u stroom logboekregistratie voor een doelbron.|
|/networkWatchers/connectionMonitors/delete|Hiermee verwijdert u een Monitor verbinding|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ diagnosticSettings leestijd|De diagnostische instellingen van de Monitor-verbinding ophalen|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ diagnosticSettings/schrijven|Maken of bijwerken van diagnostische instellingen voor de Monitor|
|/networkWatchers/connectionMonitors/providers/Microsoft.Insights/ metricDefinitions leestijd|Hiermee wordt de beschikbare metrische gegevens voor verbinding Monitor|
|/networkWatchers/connectionMonitors/query/action|Query-verbindingen tussen de opgegeven eindpunten controleren|
|/networkWatchers/connectionMonitors/read|Details van de Monitor-verbinding ophalen|
|/networkWatchers/connectionMonitors/start/action|Starten van de controle van de verbinding tussen de opgegeven eindpunten|
|/networkWatchers/connectionMonitors/stop/action|Connectiviteit tussen de opgegeven eindpunten bewaking tijdelijk onderbreken en stoppen|
|/networkWatchers/connectionMonitors/write|Hiermee maakt u een Monitor verbinding|
|/networkWatchers/connectivityCheck/Action|Controleert of de mogelijkheid tot stand brengen van een directe TCP-verbinding van een virtuele machine naar een opgegeven eindpunt, met inbegrip van een andere virtuele machine of een willekeurige externe server.|
|/networkWatchers/delete|Hiermee verwijdert u een netwerk-watcher|
|/networkWatchers/ipFlowVerify/action|Retourneert of het pakket wordt toegestaan of geweigerd of naar een specifieke bestemming.|
|/networkWatchers/lenses/delete|Hiermee verwijdert u een Lens|
|/networkWatchers/lenses/query/action|Bewaking van netwerkverkeer op een opgegeven eindpunt query|
|/networkWatchers/lenses/read|Details van de Lens ophalen|
|/networkWatchers/lenses/start/action|Controle van netwerkverkeer op een opgegeven eindpunt starten|
|/networkWatchers/lenses/stop/action|Bewaking van netwerkverkeer op een opgegeven eindpunt tijdelijk onderbreken en stoppen|
|/networkWatchers/lenses/write|Maakt een Lens|
|/networkWatchers/nextHop/action|Het volgende hoptype retourneren en vervolgens hopen dat IP-adres voor een opgegeven doel- en IP-doeladres.|
|/networkWatchers/packetCaptures/delete|Hiermee verwijdert u een pakketopname|
|/networkWatchers/packetCaptures/queryStatus/Action|Hiermee haalt u informatie over eigenschappen en de status van een pakket vastleggen resource.|
|/networkWatchers/packetCaptures/Read|De definitie voor het vastleggen van pakket ophalen|
|/networkWatchers/packetCaptures/Stop/Action|Stop de actieve opnamesessie van het pakket.|
|/networkWatchers/packetCaptures/Write|Maakt een pakketopname|
|/networkWatchers/queryFlowLogStatus/action|Hiermee haalt u de status van logboekregistratie voor een bron-stroom.|
|/networkWatchers/queryTroubleshootResult/action|Het probleemoplossing resultaat van de eerder uitgevoerde of momenteel haalt voor probleemoplossing bewerking uitgevoerd.|
|/networkWatchers/read|Ophalen van de definitie van het netwerk-watcher|
|/networkWatchers/securityGroupView/action|De geconfigureerde en effectieve netwerkbeveiligingsgroepen toegepast op een virtuele machine weergeven.|
|/networkWatchers/Topology/Action|Hiermee haalt de weergave van resources en hun relaties in een netwerk-niveau in een resourcegroep.|
|/networkWatchers/troubleshoot/action|Start het oplossen van een resource netwerken in Azure.|
|/networkWatchers/write|Een netwerk-watcher maken of bijwerken van een bestaande netwerk-watcher|
|/Operations/Read|Beschikbare bewerkingen ophalen|
|/publicIPAddresses/delete|Hiermee verwijdert u een openbaar IP-adres.|
|/publicIPAddresses/join/Action|Koppelt een openbare IP-adres|
|/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/read|Diagnostische instellingen voor het openbare IP-adres ophalen|
|/publicIPAddresses/providers/Microsoft.Insights/diagnosticSettings/write|De diagnostische instellingen van het openbare IP-adres maken of bijwerken|
|/publicIPAddresses/providers/Microsoft.Insights/logDefinitions/read|De logboekdefinities van openbare IP-adres ophalen|
|/publicIPAddresses/providers/Microsoft.Insights/metricDefinitions/read|De metrische definities van openbare IP-adres ophalen|
|/publicIPAddresses/Read|De definitie van een openbaar IP-adres ophalen.|
|publicIPAddresses/schrijven|Een openbaar IP-adres maken of bijwerken van een bestaand openbaar IP-adres. |
|registratie-/ actie|Registreert het abonnement|
|/routeFilters/delete|Hiermee verwijdert u een route filterdefinitie|
|/routeFilters/join/Action|Lid wordt van een routefilter|
|/routeFilters/Read|De definitie van een route filter ophalen|
|/routeFilters/routeFilterRules/delete|Hiermee verwijdert u een route regel filterdefinitie|
|/routeFilters/routeFilterRules/Read|De definitie van een route filter regel ophalen|
|/routeFilters/routeFilterRules/Write|Een filterregel route maken of bijwerken van een bestaande route filterregel|
|routeFilters/schrijven|Een routefilter maken of bijwerken van een bestaand rotue-filter|
|/routeTables/DELETE|Een routetabeldefinitie verwijderen|
|/routeTables/join/Action|Een routetabel joins|
|/routeTables/Read|Een routetabeldefinitie opgehaald|
|/routeTables/routes/DELETE|Hiermee verwijdert u de definitie van een route|
|/routeTables/routes/Read|De definitie van een route ophalen|
|/routeTables/routes/Write|Een route maken of een bestaande route bijwerken|
|routeTables/schrijven|Een routetabel maken of een bestaande routetabel bijwerken|
|/securegateways/applicationRuleCollections/DELETE|Hiermee verwijdert u een toepassing regelverzameling voor een veilige Gateway|
|/securegateways/applicationRuleCollections/Read|Een toepassing regelverzameling ophalen voor een opgegeven Secure Gateway|
|/securegateways/applicationRuleCollections/Write|Maken of bijwerken van een toepassing regelverzameling voor een Gateway beveiligen|
|/securegateways/DELETE|Beveiligde Gateway verwijderen|
|/securegateways/networkRuleCollections/DELETE|Hiermee verwijdert u de verzameling van een regel voor een veilige Gateway|
|/securegateways/networkRuleCollections/Read|De verzameling van een regel voor een opgegeven Secure Gateway ophalen|
|/securegateways/networkRuleCollections/Write|Maken of bijwerken van de verzameling van een regel voor een Gateway beveiligen|
|/securegateways/Read|Beveiligde Gateway verkrijgen|
|securegateways/schrijven|Maken of bijwerken van een Gateway beveiligen|
|/serviceEndpointPolicies/delete|Hiermee verwijdert u een Service-eindpunt-beleid|
|/serviceEndpointPolicies/join/Action|Lid wordt van een Service-eindpunt-beleid|
|/serviceEndpointPolicies/joinSubnet/action|Een Subnet koppelt aan beleidsregels voor Service-eindpunt|
|/serviceEndpointPolicies/Read|Haalt een Beleidsbeschrijving voor Service-eindpunt|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete|Hiermee verwijdert u de beleidsdefinitie van een Service-eindpunt|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/Read|Een Service-eindpunt beleid definitie decodering opgehaald|
|/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write|Een beleidsdefinitie voor Service-eindpunt maakt of een bestaande Service-eindpunt beleidsdefinitie-updates|
|/serviceEndpointPolicies/write|Een Service-eindpunt-beleid maken of bijwerken van een bestaand beleid van de Service-eindpunt|
|/trafficManagerGeographicHierarchies/Read|Het Traffic Manager geografisch hiërarchie met regio's die kunnen worden gebruikt met de geografische verkeersrouteringsmethode opgehaald|
|/trafficManagerProfiles/azureEndpoints/delete|Hiermee verwijdert u een Azure-eindpunt van een bestaand Traffic Manager-profiel. Traffic Manager stopt routeren van verkeer naar de verwijderde Azure-eindpunt.|
|/trafficManagerProfiles/azureEndpoints/read|Hiermee haalt u een Azure-eindpunt dat tot een Traffic Manager-profiel behoort, inclusief de eigenschappen van dat Azure-eindpunt.|
|/trafficManagerProfiles/azureEndpoints/write|Een nieuw Azure-eindpunt in een bestaand Traffic Manager-profiel toevoegen of bijwerken van de eigenschappen van een bestaande Azure-eindpunt in dat Traffic Manager-profiel.|
|/trafficManagerProfiles/delete|Het Traffic Manager-profiel verwijdert. Alle instellingen die zijn gekoppeld aan het Traffic Manager-profiel niet verloren en het profiel kan niet meer worden gebruikt om gegevensverkeer te routeren.|
|/trafficManagerProfiles/externalEndpoints/delete|Hiermee verwijdert u een extern eindpunt van een bestaand Traffic Manager-profiel. Traffic Manager stopt routeren van verkeer naar de verwijderde externe eindpunt.|
|/trafficManagerProfiles/externalEndpoints/read|Hiermee haalt u een extern eindpunt dat tot een Traffic Manager-profiel behoort, inclusief de eigenschappen van dat externe eindpunt.|
|/trafficManagerProfiles/externalEndpoints/write|Een nieuwe Extern eindpunt in een bestaand Traffic Manager-profiel toevoegen of bijwerken van de eigenschappen van een bestaande Extern eindpunt in dat Traffic Manager-profiel.|
|/trafficManagerProfiles/heatMaps/Read|Hiermee wordt de Traffic Manager-Heatmap voor bepaalde Traffic Manager-profiel waarin de aantallen en de latentie gegevens opvragen per locatie en de bron-IP.|
|/trafficManagerProfiles/nestedEndpoints/delete|Een eindpunt genest verwijdert uit een bestaand Traffic Manager-profiel. Traffic Manager stopt met het verwijderde genest eindpunt routeren van verkeer.|
|/trafficManagerProfiles/nestedEndpoints/read|Hiermee haalt u een genest eindpunt dat tot een Traffic Manager-profiel behoort, inclusief de eigenschappen van dat eindpunt genest.|
|/trafficManagerProfiles/nestedEndpoints/write|Een nieuw eindpunt voor genest in een bestaand Traffic Manager-profiel toevoegen of bijwerken van de eigenschappen van een bestaand eindpunt dat kan worden genest in dat Traffic Manager-profiel.|
|/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/Read|De diagnose van Traffic Manager-instellingen opgehaald|
|/trafficManagerProfiles/providers/Microsoft.Insights/diagnosticSettings/Write|Maken of bijwerken van de diagnostische instellingen voor het Traffic Manager deze bewerking wordt geleverd door de insights-resourceprovider.|
|/trafficManagerProfiles/providers/Microsoft.Insights/logDefinitions/read|Hiermee haalt u de gebeurtenissen voor Traffic Manager|
|/trafficManagerProfiles/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee haalt u de beschikbare metrische gegevens voor Traffic Manager.|
|/trafficManagerProfiles/Read|De profielconfiguratie Traffic Manager-ophalen. Dit omvat DNS-instellingen, instellingen voor verkeersroutering, instellingen voor eindpuntcontrole en de lijst met eindpunten die worden gerouteerd door dit Traffic Manager-profiel.|
|trafficManagerProfiles/schrijven|Een Traffic Manager-profiel maken, of wijzig de configuratie van een bestaand Traffic Manager-profiel. Dit omvat inschakelen of uitschakelen van een profiel en DNS-instellingen, instellingen voor verkeersroutering of instellingen voor eindpuntcontrole wijzigen. Eindpunten die worden gerouteerd door het Traffic Manager-profiel kunnen worden toegevoegd, verwijderd, ingeschakeld of uitgeschakeld.|
|/trafficManagerUserMetricsKeys/delete|Hiermee verwijdert u de abonnement sleutel gebruikt voor de verzameling van Realtime metrische gegevens over gebruikers.|
|/trafficManagerUserMetricsKeys/read|Hiermee haalt u de abonnement sleutel gebruikt voor de verzameling van Realtime metrische gegevens over gebruikers.|
|/trafficManagerUserMetricsKeys/write|Maakt een nieuw abonnement sleutel moet worden gebruikt voor de verzameling van Realtime metrische gegevens over gebruikers.|
|/ unregister/actie|Heft de registratie van het abonnement|
|/virtualHubs/delete|Hiermee verwijdert u een virtuele-Hub|
|/virtualHubs/hubVirtualNetworkConnections/delete|Hiermee verwijdert u een HubVirtualNetworkConnection|
|/virtualHubs/hubVirtualNetworkConnections/read|Een HubVirtualNetworkConnection ophalen|
|/virtualHubs/hubVirtualNetworkConnections/write|Maken of bijwerken van een HubVirtualNetworkConnection|
|/virtualHubs/Read|Ophalen van een virtuele-Hub|
|virtualHubs/schrijven|Maken of bijwerken van een virtuele-Hub|
|/virtualnetworkgateways/connections/read|VirtualNetworkGatewayConnection ophalen|
|/virtualNetworkGateways/delete|Hiermee verwijdert u een virtualNetworkGateway|
|/virtualnetworkgateways/generatevpnclientpackage/Action|Een pakket voor virtualNetworkGateway genereren|
|/virtualnetworkgateways/generatevpnprofile/action|VpnProfile-pakket voor VirtualNetworkGateway genereren|
|/virtualnetworkgateways/getadvertisedroutes/action|Opgehaald virtualNetworkGateway aangekondigd routes|
|/virtualnetworkgateways/getbgppeerstatus/action|Haalt virtualNetworkGateway bgp-peer-status|
|/virtualnetworkgateways/getlearnedroutes/action|Virtualnetworkgateway geleerde routes opgehaald|
|/virtualnetworkgateways/getvpnclientipsecparameters/action|Het ophalen van Vpnclient Ipsec parameters voor VirtualNetworkGateway P2S-client.|
|/virtualnetworkgateways/getvpnprofilepackageurl/action|De URL van het hulpprogramma voor het profiel van een vooraf gegenereerde vpn-clientpakket opgehaald|
|/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/read|De diagnostische instellingen van het virtuele netwerk Gateway opgehaald|
|/virtualNetworkGateways/providers/Microsoft.Insights/diagnosticSettings/write|Maken of bijwerken van de diagnostische instellingen voor virtuele netwerkgateway deze bewerking wordt geleverd door de insights-resourceprovider.|
|/virtualNetworkGateways/providers/Microsoft.Insights/logDefinitions/read|Hiermee haalt u de gebeurtenissen voor de virtuele netwerkgateway|
|/virtualNetworkGateways/providers/Microsoft.Insights/metricDefinitions/read|Hiermee wordt de beschikbare metrische gegevens voor de virtuele netwerkgateway|
|/virtualNetworkGateways/read|Een VirtualNetworkGateway opgehaald|
|/virtualnetworkgateways/reset/Action|Hiermee stelt u een virtualNetworkGateway|
|/virtualnetworkgateways/setvpnclientipsecparameters/action|Stel Vpnclient Ipsec-parameters voor VirtualNetworkGateway P2S-client.|
|/virtualnetworkgateways/supportedvpndevices/action|Een lijst met ondersteunde VPN-apparaten|
|/virtualNetworkGateways/write|Maken of bijwerken van een VirtualNetworkGateway|
|/virtualNetworks/checkIpAddressAvailability/read|Controleer of IP-adres beschikbaar op de opgegeven virtuele netwerk is|
|/virtualNetworks/customViews/get/action|Een virtueel netwerk aangepaste weergave-inhoud ophalen|
|/virtualNetworks/customViews/read|Definitie van een aangepaste weergave van het virtuele netwerk ophalen|
|/virtualNetworks/delete|Hiermee verwijdert u een virtueel netwerk|
|/virtualNetworks/peer/Action|Een virtueel netwerk met een ander virtueel netwerk samenwerkt|
|/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/read|De diagnostische instellingen van het virtuele netwerk ophalen|
|/virtualNetworks/providers/Microsoft.Insights/diagnosticSettings/write|De diagnostische instellingen van het virtuele netwerk maken of bijwerken|
|/virtualNetworks/providers/Microsoft.Insights/logDefinitions/read|De definities van de logboekbestanden van het virtuele netwerk ophalen|
|/virtualNetworks/providers/Microsoft.Insights/metricDefinitions/read|De metrische definities van het virtuele netwerk ophalen|
|/virtualNetworks/read|De definitie van het virtuele netwerk ophalen|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete|Hiermee verwijdert u een virtueel netwerk peering proxy|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/read|Een virtueel netwerk peering proxydefinitie zijn opgehaald|
|/virtualNetworks/remoteVirtualNetworkPeeringProxies/write|Een virtueel netwerk peering proxy maken of bijwerken van een bestaande virtuele netwerk peering-proxy|
|/virtualNetworks/subnets/delete|Hiermee verwijdert u een virtueel netwerksubnet|
|/virtualNetworks/subnets/join/action|Lid wordt van een virtueel netwerk|
|/virtualNetworks/subnets/joinViaServiceEndpoint/action|Resource zoals opslagaccount of de SQL-database koppelt aan een subnet.|
|/virtualNetworks/subnets/Read|De subnet-definitie van een virtueel netwerk ophalen|
|/virtualNetworks/subnets/resourceNavigationLinks/delete|Hiermee verwijdert u een Resource navigatiekoppeling|
|/virtualNetworks/subnets/resourceNavigationLinks/Read|De definitie van de navigatiekoppeling Resource ophalen|
|/virtualNetworks/subnets/resourceNavigationLinks/write|Een navigatie Resourcekoppeling maken of bijwerken van een bestaande Resource navigatie-koppeling|
|/virtualNetworks/subnets/virtualMachines/Read|Verwijzingen naar alle virtuele machines ophalen in een virtueel netwerksubnet|
|/virtualNetworks/subnets/Write|Een virtueel netwerksubnet maken of bijwerken van een bestaand virtueel netwerksubnet|
|/virtualNetworks/taggedTrafficConsumers/delete|Hiermee verwijdert u een label gegevensverkeer Consumer|
|/virtualNetworks/taggedTrafficConsumers/read|De definitie gelabeld verkeer Consumer ophalen|
|/virtualNetworks/taggedTrafficConsumers/Validate/Action|Valideert een label gegevensverkeer Consumer|
|/virtualNetworks/taggedTrafficConsumers/write|Een Consumer verkeer gelabeld maken of bijwerken van een bestaande label gegevensverkeer Consumer|
|/virtualNetworks/usages/Read|Het gebruik van de IP voor elk subnet van het virtuele netwerk ophalen|
|/virtualNetworks/virtualMachines/Read|Verwijzingen naar alle virtuele machines ophalen in een virtueel netwerk|
|/virtualNetworks/virtualNetworkPeerings/delete|Hiermee verwijdert u een virtueel netwerk peering|
|/virtualNetworks/virtualNetworkPeerings/read|De definitie van een virtueel netwerk peering ophalen|
|/virtualNetworks/virtualNetworkPeerings/write|Een virtueel netwerk peering of een bestaande virtuele netwerk-peering bijwerken|
|virtualNetworks/schrijven|Een virtueel netwerk maken of bijwerken van een bestaand virtueel netwerk|
|/virtualNetworkTaps/delete|Tik op virtueel netwerk verwijderen|
|/virtualNetworkTaps/join/Action|Een virtueel netwerk-tap joins|
|/virtualNetworkTaps/Read|Tik op virtueel netwerk ophalen|
|virtualNetworkTaps/schrijven|Maken of bijwerken van virtueel netwerk-Tap|
|/virtualwans/delete|Verwijdert een virtuele Wan|
|/virtualwans/Read|Get a virtuele Wan|
|/virtualWans/virtualHubProxies/delete|Hiermee verwijdert u een virtuele Hub-proxy|
|/virtualWans/virtualHubProxies/read|De definitie van een virtuele Hub proxy ophalen|
|/virtualWans/virtualHubProxies/write|Een proxy virtuele Hub maken of bijwerken van een virtuele Hub-proxy|
|/virtualwans/virtualHubs/Read|Hiermee haalt u alle virtuele-Hubs die gekoppeld aan een virtuele Wan zijn.|
|/virtualwans/vpnconfiguration/Read|Een VPN-configuratie opgehaald|
|/virtualWans/vpnSiteProxies/delete|Hiermee verwijdert u een VPN-Site-proxy|
|/virtualWans/vpnSiteProxies/read|De definitie van een VPN-Site-proxy ophalen|
|/virtualWans/vpnSiteProxies/write|Een VPN-Site-proxy maken of bijwerken van een VPN-Site-proxy|
|/virtualwans/vpnSites/read|Hiermee haalt u alle VPN-Sites die gekoppeld aan een virtuele Wan zijn.|
|virtualwans/schrijven|Maken of bijwerken van een virtuele Wan|
|/vpnGateways/Read|Hiermee wordt een VpnGateway opgehaald.|
|/vpnGateways/vpnConnections/Read|Hiermee haalt u een VPN-verbinding.|
|/vpnGateways/vpnConnections/write|Plaatst een VPN-verbinding.|
|vpnGateways/schrijven|Een VpnGateway plaatst.|
|/vpnsites/DELETE|Hiermee verwijdert u een VPN-Site-resource.|
|/vpnsites/Read|Hiermee haalt u een VPN-Site-resource.|
|vpnsites/schrijven|Maken of bijwerken van een VPN-Site-resource.|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| Bewerking | Beschrijving |
|---|---|
|/ CheckNamespaceAvailability/actie|Hiermee wordt gecontroleerd of de resourcenaam voor de naamruimte beschikbaar is binnen de NotificationHub-service.|
|/ Naamruimten/authorizationRules/actie|De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen.|
|Naamruimten, authorizationRules/verwijderen|Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd. |
|/ Naamruimten/authorizationRules/listkeys/actie|De verbindingstekenreeks naar de naamruimte ophalen|
|Naamruimten/authorizationRules/leestijd|De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen.|
|/ Naamruimten/authorizationRules/regenerateKeys/actie|De verificatieregel voor een naamruimte De primaire/secundaire sleutel opnieuw genereren, De sleutel opgeven die opnieuw moet worden gegenereerd|
|Naamruimten/authorizationRules/schrijftijd|Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/CheckNotificationHubAvailability/action|Hiermee wordt gecontroleerd of de NotificationHub-naam beschikbaar is binnen een naamruimte.|
|Naamruimten, verwijderen|De resource van een naamruimte verwijderen|
|/ Naamruimten/NotificationHubs/authorizationRules/actie|De lijst met verificatieregels voor Notification Hub ophalen|
|/Namespaces/NotificationHubs/authorizationRules/delete|Verificatieregels voor Notification Hub verwijderen|
|/ Naamruimten/NotificationHubs/authorizationRules/listkeys/actie|De verbindingsreeks naar de Notification Hub ophalen|
|Naamruimten/NotificationHubs/authorizationRules/leestijd|De lijst met verificatieregels voor Notification Hub ophalen|
|/ Naamruimten/NotificationHubs/authorizationRules/regenerateKeys/actie|De verificatieregel voor Notification Hub De primaire/secundaire sleutel opnieuw genereren, De sleutel opgeven die opnieuw moet worden gegenereerd|
|Naamruimten/NotificationHubs/authorizationRules/schrijftijd|Notification Hub-autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/NotificationHubs/debugSend/action|Een testpushmelding verzenden.|
|/Namespaces/NotificationHubs/Delete|De resource voor Notification Hub verwijderen|
|Naamruimten/NotificationHubs/metricDefinitions/leestijd|Lijst met Namespace metrische Resource beschrijvingen|
|/Namespaces/NotificationHubs/pnsCredentials/action|Alle Notification Hub PNS referenties ophalen. Dit omvat, WNS, MPNS, APNS, GCM en Baidu-referenties|
|Naamruimten/NotificationHubs/leestijd|De lijst met beschrijvingen van resources voor Notification Hub ophalen|
|Naamruimten/NotificationHubs/schrijftijd|Een Notification Hub maken en bijwerken van de eigenschappen ervan. De eigenschappen zijn hoofdzakelijk PNS-referenties. Autorisatieregels en TTL|
|Naamruimten/leestijd|De lijst met beschrijvingen van resources van naamruimten ophalen|
|Naamruimten/schrijftijd|Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Labels en capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt.|
|registratie-/ actie|Hiermee wordt het abonnement voor de NotificationHubs-resourceprovider geregistreerd en wordt het maken van naamruimten en NotificationHubs mogelijk|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| Bewerking | Beschrijving |
|---|---|
|/linkTargets/Read|Een lijst met bestaande accounts die niet gekoppeld aan een Azure-abonnement zijn. Als deze Azure-abonnement koppeling naar een werkruimte, gebruikt u een klant-id die is geretourneerd door deze bewerking in de eigenschap van de klant-id van de bewerking werkruimte maken.|
|registratie-/ actie|Een abonnement op een resourceprovider registreren.|
|/Workspaces/Analytics/query/Action|Zoeken met nieuwe-engine.|
|/Workspaces/Analytics/query/schema/Read|Search schema V2 worden opgehaald.|
|/Workspaces/API/query/Action|Zoeken met nieuwe-engine.|
|/Workspaces/API/query/schema/Read|Search schema V2 worden opgehaald.|
|/workspaces/configurationScopes/delete|Configuratiebereik verwijderen|
|/Workspaces/configurationScopes/Read|Configuratiebereik ophalen|
|/Workspaces/configurationScopes/Write|Configuratiebereik instellen|
|/Workspaces/Datasources/DELETE|Verwijder de gegevensbronnen onder een werkruimte.|
|/Workspaces/Datasources/Read|Gegevensbronnen ophalen onder een werkruimte.|
|/Workspaces/Datasources/Write|Gegevensbronnen onder een werkruimte maken of bijwerken.|
|/Workspaces/DELETE|Hiermee verwijdert u een werkruimte. Als u de werkruimte is gekoppeld aan een bestaande werkruimte tijdens het maken worden de werkruimte die deze is gekoppeld aan niet verwijderd.|
|/Workspaces/generateregistrationcertificate/Action|Genereert Registratiecertificaat voor de werkruimte. Dit certificaat wordt gebruikt voor Microsoft System Center Operation Manager verbinden met de werkruimte.|
|/Workspaces/intelligencepacks/Disable/Action|Hiermee schakelt u een intelligence pack voor een bepaalde werkruimte.|
|/Workspaces/intelligencepacks/Enable/Action|Hiermee kunt een intelligence pack voor een bepaalde werkruimte.|
|/Workspaces/intelligencepacks/Read|Geeft een lijst van alle intelligence packs die zichtbaar zijn voor een bepaalde worksapce en ook wordt aangegeven of het pakket is ingeschakeld of uitgeschakeld voor die werkruimte.|
|/workspaces/linkedServices/delete|Delete gekoppelde services onder deze werkruimte.|
|/Workspaces/linkedServices/Read|Ophalen van de gekoppelde services onder gegeven van de werkruimte.|
|/Workspaces/linkedServices/Write|Services maken/bijwerken gekoppeld onder deze werkruimte.|
|/Workspaces/listKeys/Action|Hiermee haalt u de lijst met sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte.|
|/Workspaces/listKeys/Read|Hiermee haalt u de lijst met sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte.|
|/Workspaces/managementGroups/Read|Hiermee haalt de namen en de metagegevens voor System Center Operations Manager-beheergroepen die is verbonden met deze werkruimte.|
|/Workspaces/metricDefinitions/Read|Metriek definities onder werkruimte ophalen|
|/workspaces/notificationSettings/delete|Verwijderen van de gebruiker meldingsinstellingen voor de werkruimte.|
|/Workspaces/notificationSettings/Read|Ophalen van de gebruiker meldingsinstellingen voor de werkruimte.|
|/Workspaces/notificationSettings/Write|Instellen van de gebruiker meldingsinstellingen voor de werkruimte.|
|/Workspaces/Purge/Action|Opgegeven gegevens verwijderen uit de werkruimte|
|/Workspaces/Read|Een bestaande werkruimte opgehaald|
|/Workspaces/savedSearches/DELETE|Hiermee verwijdert u een opgeslagen zoekopdracht|
|/Workspaces/savedSearches/Read|Een opgeslagen zoekopdracht opgehaald|
|/Workspaces/savedSearches/Write|Hiermee maakt u een opgeslagen zoekopdracht|
|/Workspaces/schema/Read|Hiermee haalt de search-schema voor de werkruimte.  Search schema bevat de blootgestelde velden en hun typen.|
|/Workspaces/Search/Action|Een zoekopdracht wordt uitgevoerd|
|/Workspaces/sharedKeys/Action|Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte.|
|/Workspaces/sharedKeys/Read|Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte.|
|/Workspaces/storageinsightconfigs/DELETE|Hiermee verwijdert u een configuratie van de opslag. Hiermee stopt u Microsoft Operational Insights van lezen van gegevens van het opslagaccount.|
|/Workspaces/storageinsightconfigs/Read|Hiermee haalt u een configuratie van de opslag.|
|/Workspaces/storageinsightconfigs/Write|Maakt een nieuwe opslagconfiguratie. Deze configuraties worden gebruikt voor het ophalen van gegevens uit een locatie in een bestaand opslagaccount.|
|/Workspaces/usages/Read|Hiermee haalt gebruiksgegevens voor een werkruimte met inbegrip van de hoeveelheid gegevens gelezen door de werkruimte.|
|werkruimten/schrijven|Een nieuwe werkruimte of koppelingen naar een bestaande werkruimte wordt gemaakt door de klant-id van de bestaande werkruimte.|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| Bewerking | Beschrijving |
|---|---|
|/managementAssociations/DELETE|Verwijderen van bestaande Management-koppeling|
|/managementAssociations/Read|Ophalen van bestaande Management-koppeling|
|managementAssociations/schrijven|Een nieuwe Management associatie maken|
|/managementConfigurations/DELETE|Verwijderen van bestaande Management Configuratin|
|/managementConfigurations/Read|Configuratie van bestaande Management ophalen|
|managementConfigurations/schrijven|Maak een nieuwe configuratie voor beheer|
|registratie-/ actie|Een abonnement op een resourceprovider registreren.|
|/Solutions/DELETE|Verwijderen van bestaande OMS-oplossing|
|/Solutions/Read|Ophalen van OMS oplossing afgesloten|
|oplossingen/schrijven|Nieuwe OMS-oplossing maken|

## <a name="microsoftportal"></a>Microsoft.Portal

| Bewerking | Beschrijving |
|---|---|
|/dashboards/DELETE|Het dashboard verwijderen uit het abonnement.|
|/dashboards/Read|De dashboards voor het abonnement lezen.|
|dashboards/schrijven|Dashboard toevoegen aan een abonnement of het dashboard wijzigen.|

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

| Bewerking | Beschrijving |
|---|---|
|/capacities/checkNameAvailability/Action|Controleert of Power BI-specifieke-capaciteit van de opgegeven naam geldig is en niet in gebruik.|
|/capacities/DELETE|Hiermee verwijdert u de Power BI toegewezen capaciteit.|
|/capacities/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor Power BI-specifieke capaciteit.|
|/capacities/Read|Haalt de informatie van de opgegeven Power BI-specifieke capaciteit.|
|capaciteit/schrijven|Maken of bijwerken van de opgegeven Power BI-specifieke capaciteit.|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| Bewerking | Beschrijving |
|---|---|
|/Locations/allocatedStamp/Read|GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service|
|/Locations/allocateStamp/Action|AllocateStamp is een interne bewerking die wordt gebruikt door de service|
|/locations/backupPreValidateProtection/action||
|/Locations/backupStatus/Action|Controleer de Status van back-up voor de Recovery Services-kluizen|
|/Locations/backupValidateFeatures/Action|Functies valideren|
|/Operations/Read|Bewerking resulteert in de lijst met bewerkingen voor een Resource Provider|
|registratie-/ actie|Registreert het abonnement voor de opgegeven Resource Provider|
|Kluizen/backupconfig/leestijd|Retourneert-configuratie voor Recovery Services-kluis.|
|Kluizen/backupconfig/schrijftijd|Configuratie van de updates voor Recovery Services-kluis.|
|Kluizen/backupEngines/leestijd|Hiermee worden alle servers voor back-upbeheer geretourneerd die in de kluis zijn geregistreerd.|
|/Vaults/backupFabrics {fabricName} /protectionContainers/ {containerName} / items/leestijd|Alle items in een container ophalen|
|/Vaults/backupFabrics/backupProtectionIntent/write|Maak een back-beveiliging wordt intentie|
|Kluizen/backupFabrics/operationResults/leestijd|Retourneert de status van de bewerking|
|Kluizen/backupFabrics/protectableContainers/leestijd|Alle beveiligbare containers ophalen|
|/Vaults/backupFabrics/protectionContainers/inquire/action|Voer de query voor werkbelastingen binnen een container|
|/Vaults/backupFabrics/protectionContainers/operationResults/read|Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action|Hiermee wordt een back-up van het beveiligde item gemaakt.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/delete|Hiermee verwijdert u beveiligd Item|
|/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read|Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read|Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/read|Geeft de details van het artikel beveiligd object|
|/ Kluizen/backupFabrics/protectionContainers/protectedItems/provisionInstantItemRecovery-recoveryPoints/actie|Inrichten Instant Item herstel voor beveiligde Item|
|/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read|Herstelpunten voor beveiligde items ophalen.|
|/ Kluizen/backupFabrics/protectionContainers/protectedItems/recoveryPoints/herstelbewerking|Herstelpunten voor beveiligde items herstellen.|
|/Vaults/backupFabrics/protectionContainers/protectedItems/ recoveryPoints/revokeInstantItemRecovery/action|Herstel op directe intrekken voor beveiligde Item|
|/Vaults/backupFabrics/protectionContainers/protectedItems/write|Een back-up beveiligd Item maken|
|Kluizen/backupFabrics/protectionContainers/leestijd|Retourneert alle geregistreerde containers|
|Kluizen/backupFabrics/protectionContainers/schrijftijd|Hiermee maakt u een geregistreerde container|
|/ Kluizen/backupFabrics/refreshContainers/actie|Hiermee vernieuwt u de lijst van de container|
|/ Kluizen/backupJobs / / actie voor annuleren|Annuleer de taak|
|Kluizen/backupJobs/operationResults/leestijd|Hiermee wordt het resultaat van de taakbewerking geretourneerd.|
|Kluizen/backupJobs/leestijd|Retourneert alle objecten van de taak|
|/ Kluizen/backupJobsExport/actie|Exporttaken|
|Kluizen/backupJobsExport/operationResults/leestijd|Retourneert het resultaat van taak exportbewerking.|
|Kluizen/backupManagementMetaData/leestijd|Hiermee worden de metagegevens van back-upbeheer voor een Recovery Services-kluis geretourneerd.|
|Kluizen/backupOperationResults/leestijd|Hiermee wordt het resultaat van de back-upbewerking voor een Recovery Services-kluis geretourneerd.|
|Kluizen/backupOperations/leestijd|Back-upbewerking retourneert Status voor de Recovery Services-kluis.|
|/ Kluizen/backupPolicies/verwijderen|Een beveiligingsbeleid voor verwijderen|
|Kluizen/backupPolicies/operationResults/leestijd|Hiermee worden de resultaten van de beleidsbewerking opgehaald.|
|Kluizen/backupPolicies/operations/leestijd|Status van de beleidsbewerking ophalen.|
|Kluizen/backupPolicies/leestijd|Retourneert alle beleidsregels voor beveiliging|
|Kluizen/backupPolicies/schrijftijd|Hiermee maakt u het beveiligingsbeleid|
|Kluizen/backupProtectableItems/leestijd|Hiermee wordt een lijst met alle beveiligbare items opgehaald.|
|/Vaults/backupProtectedItems/read|Hiermee wordt de lijst met alle beveiligde items geretourneerd.|
|/Vaults/backupProtectionContainers/read|Retourneert alle containers die horen bij het abonnement|
|/ Kluizen/backupSecurityPIN/actie|Beveiliging PINCODE retourneert informatie voor Recovery Services-kluis.|
|Kluizen/backupstorageconfig/leestijd|Retourneert opslagconfiguratie voor Recovery Services-kluis.|
|Kluizen/backupstorageconfig/schrijftijd|De opslagconfiguratie updates voor Recovery Services-kluis.|
|Kluizen/backupUsageSummaries/leestijd|Samenvattingen retourneert voor beveiligde Items en beveiligde Servers voor een Recovery Services.|
|Kluizen/certificaten/schrijftijd|De bewerking Update Resource certificaat updates het referentiecertificaat van de resource/kluis.|
|/Vaults/delete|De bewerking verwijderen kluis verwijdert de opgegeven Azure-resource van het type 'kluis'|
|/Vaults/extendedInformation/delete|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|Kluizen/extendedInformation/leestijd|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|Kluizen/extendedInformation/schrijftijd|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|Kluizen/monitoringAlerts/leestijd|Hiermee haalt de waarschuwingen voor de Recovery services-kluis.|
|Kluizen/monitoringAlerts/schrijftijd|Oplossing voor de waarschuwing.|
|Kluizen/monitoringConfigurations/leestijd|Hiermee haalt u de configuratie voor meldingen van Recovery services-kluis.|
|Kluizen/monitoringConfigurations/schrijftijd|Hiermee configureert u een e-mailmeldingen Recovery services-kluis.|
|/Vaults/providers/Microsoft.Insights/diagnosticSettings/read|Azure Backup diagnostische gegevens|
|/Vaults/providers/Microsoft.Insights/diagnosticSettings/write|Azure Backup diagnostische gegevens|
|/Vaults/providers/Microsoft.Insights/logDefinitions/read|Azure back-uplogboeken|
|/Vaults/providers/Microsoft.Insights/metricDefinitions/read|Azure Backup metrische gegevens|
|Kluizen/leestijd|De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald|
|/Vaults/registeredIdentities/delete|De Container registratie-bewerking kan worden gebruikt om de registratie van een container te.|
|/Vaults/registeredIdentities/operationResults/read|De bewerking ophalen resulteert de bewerking kan worden gebruikt ophalen de bewerkingsstatus van en het resultaat voor de bewerking asynchroon ingediende|
|/Vaults/registeredIdentities/read|De Containers ophalen bewerking kan worden gebruikt, krijgen de containers die zijn geregistreerd voor een resource.|
|/Vaults/registeredIdentities/write|De bewerking servicecontainer registreren kan worden gebruikt om te registreren van een container met Service voor herstel.|
|/vaults/replicationAlertSettings/Read|Alle instellingen voor waarschuwingen lezen|
|/vaults/replicationAlertSettings/Write|Alle waarschuwingsinstellingen maken of bijwerken|
|/vaults/replicationEvents/Read|Alle gebeurtenissen lezen|
|/vaults/replicationFabrics/checkConsistency/Action|Hiermee wordt de consistentie van de infrastructuur gecontroleerd|
|/vaults/replicationFabrics/DELETE|Verwijderen van eventuele Fabrics|
|/vaults/replicationFabrics/deployProcessServerImage/action|Proces serverinstallatiekopie te implementeren|
|/vaults/replicationFabrics/Read|Alle Fabrics lezen|
|/vaults/replicationFabrics/reassociateGateway/Action|Gateway opnieuw koppelen|
|/vaults/replicationFabrics/Remove/Action|Infrastructuur verwijderen|
|/vaults/replicationFabrics/renewcertificate/Action|Certificaat vernieuwen voor de infrastructuur|
|/vaults/replicationFabrics/replicationNetworks/Read|Geen netwerken lezen|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/DELETE|Eventuele Netwerktoewijzingen verwijderen|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/Read|Alle Netwerktoewijzingen lezen|
|/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/Write|Maken of bijwerken van eventuele Netwerktoewijzingen|
|kluizen/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/actie|Beveiligbare Item detecteren|
|/vaults/replicationFabrics/replicationProtectionContainers/Read|Beveiliging-Containers gelezen|
|/vaults/replicationFabrics/replicationProtectionContainers/remove/action|De Beveiligingscontainer verwijderen|
|/ replicationFabrics-kluizen/replicationProtectionContainers/replicationProtectableItems leestijd|Alle beveiligbare objecten lezen|
|/ replicationFabrics-kluizen/replicationProtectionContainers/applyRecoveryPoint-replicationProtectedItems/actie|Herstelpunt toepassen|
|kluizen/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/verwijderen|Verwijder alle beveiligde Items|
|/ replicationFabrics-kluizen/replicationProtectionContainers/failoverCommit-replicationProtectedItems/actie|Failover doorvoeren|
|/ replicationFabrics-kluizen/replicationProtectionContainers/plannedFailover-replicationProtectedItems/actie|Geplande Failover|
|/ replicationFabrics-kluizen/replicationProtectionContainers/replicationProtectedItems leestijd|Alle beveiligde Items lezen|
|/ replicationFabrics-kluizen/replicationProtectionContainers/recoveryPoints replicationProtectedItems leestijd|Replicatie herstelpunten lezen|
|/ replicationFabrics-kluizen/replicationProtectionContainers/replicationProtectedItems/verwijderen/actie|Het beveiligde Item verwijderen|
|/ replicationFabrics-kluizen/replicationProtectionContainers/repairReplication-replicationProtectedItems/actie|Reparatie replicatie|
|/ replicationFabrics-kluizen/replicationProtectionContainers/replicationProtectedItems, beveiligt/actie|Beveiligde Item opnieuw beveiligen|
|/ replicationFabrics-kluizen/replicationProtectionContainers/testFailover-replicationProtectedItems/actie|Failover testen|
|/ replicationFabrics-kluizen/replicationProtectionContainers/testFailoverCleanup-replicationProtectedItems/actie|Het opruimen van testfailover|
|/ replicationFabrics-kluizen/replicationProtectionContainers/unplannedFailover-replicationProtectedItems/actie|Failover|
|/ replicationFabrics-kluizen/replicationProtectionContainers/updateMobilityService-replicationProtectedItems/actie|Bijwerken van de Mobility-Service|
|kluizen/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/schrijven|Maken of bijwerken van alle beveiligde Items|
|kluizen/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/verwijderen|Alle toewijzingen van de Container beveiliging verwijderen|
|/ replicationFabrics-kluizen/replicationProtectionContainers/replicationProtectionContainerMappings leestijd|Alle toewijzingen van de Container beveiliging lezen|
|/ replicationFabrics-kluizen/replicationProtectionContainers/replicationProtectionContainerMappings/verwijderen/actie|Beveiliging Container toewijzing verwijderen|
|kluizen/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/schrijven|Maken of bijwerken van alle toewijzingen van de Container beveiliging|
|/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/Action|Switch Protection Container|
|/vaults/replicationFabrics/replicationProtectionContainers/Write|Maken of bijwerken van alle Containers beveiliging|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete|Verwijderen van een Recovery Services-Providers|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/Read|Een Recovery Services-Providers lezen|
|kluizen/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/actie|Vernieuw de Provider|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action|Recovery Services-Provider verwijderen|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/Write|Maken of bijwerken van een Recovery Services-Providers|
|/vaults/replicationFabrics/replicationStorageClassifications/Read|Alle Opslagclassificaties lezen|
|kluizen/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/verwijderen|Alle toewijzingen van de classificatie opslag verwijderen|
|/ replicationFabrics-kluizen/replicationStorageClassifications/replicationStorageClassificationMappings leestijd|Alle toewijzingen van de classificatie opslag lezen|
|kluizen/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/schrijven|Maken of bijwerken van alle toewijzingen van de classificatie opslag|
|/vaults/replicationFabrics/replicationvCenters/DELETE|Taken verwijderen|
|/vaults/replicationFabrics/replicationvCenters/Read|Lezen van taken|
|/vaults/replicationFabrics/replicationvCenters/Write|Alle taken maken of bijwerken|
|/vaults/replicationFabrics/Write|Maken of bijwerken van een Fabrics|
|/vaults/replicationJobs/Cancel/Action|Taak annuleren|
|/vaults/replicationJobs/Read|Lezen van taken|
|/vaults/replicationJobs/restart/Action|Taak opnieuw starten|
|/vaults/replicationJobs/Resume/Action|Taak hervatten|
|/vaults/replicationPolicies/DELETE|Beleid verwijderen|
|/vaults/replicationPolicies/Read|Lezen van beleid|
|/vaults/replicationPolicies/Write|Beleid maken of bijwerken|
|/vaults/replicationRecoveryPlans/DELETE|Verwijderen van eventuele herstelplannen|
|/vaults/replicationRecoveryPlans/failoverCommit/Action|Herstelplan-failover doorvoeren|
|/vaults/replicationRecoveryPlans/plannedFailover/Action|Plan voor herstel voor geplande Failover|
|/vaults/replicationRecoveryPlans/Read|Alle herstelplannen lezen|
|/vaults/replicationRecoveryPlans/reProtect/Action|Plan voor herstel opnieuw beveiligen|
|/vaults/replicationRecoveryPlans/testFailover/Action|Test Failover herstelplan|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/Action|Test het opruimen van herstelplan|
|/vaults/replicationRecoveryPlans/unplannedFailover/Action|Plan voor herstel van failover|
|/vaults/replicationRecoveryPlans/Write|Maken of bijwerken van een herstelplannen|
|/Vaults/tokenInfo/read|Retourneert token-informatie voor de Recovery Services-kluis.|
|/vaults/usages/Read|Lezen van het gebruik van een kluis|
|Kluizen/gebruik/leestijd|Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd.|
|Kluizen/vaultTokens/leestijd|Het Token kluis opnieuw kan worden gebruikt voor bewerkingen van kluis niveau back-end-kluis Token ophalen.|
|Kluizen/schrijftijd|Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt.|

## <a name="microsoftrelay"></a>Microsoft.Relay

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd.|
|/ checkNamespaceAvailability/actie|Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. Deze API is afgeschaft. Gebruik in plaats hiervan CheckNameAvailabiltiy.|
|/Namespaces/authorizationRules/Action|Updates Namespace-autorisatieregel. Deze API is depricated. Gebruik een PUT-aanroep in plaats daarvan de autorisatieregel Namespace bijwerken... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API.|
|/Namespaces/authorizationRules/DELETE|Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd. |
|/Namespaces/authorizationRules/listkeys/Action|De verbindingstekenreeks naar de naamruimte ophalen|
|/Namespaces/authorizationRules/Read|De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen.|
|/Namespaces/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/authorizationRules/Write|Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/ naamruimten/verwijderen|De resource van een naamruimte verwijderen|
|/Namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/Action|De autorisatie regels sleutels voor de primaire naamruimte voor herstel na noodgevallen opgehaald|
|/Namespaces/disasterRecoveryConfigs/authorizationRules/Read|Disaster Recovery primaire Namespace van autorisatieregels ophalen|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Schakelt Disaster Recovery uit en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten.|
|/Namespaces/disasterrecoveryconfigs/checkNameAvailability/Action|Beschikbaarheid van de controles van naamruimtealias onder het betreffende abonnement.|
|/namespaces/disasterRecoveryConfigs/delete|Hiermee verwijdert u de configuratie van het herstel na noodgevallen die zijn gekoppeld aan de naamruimte. Deze bewerking kan alleen worden aangeroepen via de primaire naamruimte.|
|/Namespaces/disasterRecoveryConfigs/failover/Action|Hiermee wordt een GEO DR-failover aangeroepen en wordt het naamruimtealias zo geconfigureerd dat verwezen wordt naar de secundaire naamruimte.|
|/namespaces/disasterRecoveryConfigs/read|Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort opgehaald.|
|/namespaces/disasterRecoveryConfigs/write|Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort gemaakt of bijgewerkt.|
|/Namespaces/HybridConnections/authorizationRules/Action|De bewerking HybridConnection bijwerken. Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. Autorisatieregels. Gebruik een PUT-aanroep autorisatieregel bijwerken.|
|/Namespaces/HybridConnections/authorizationRules/DELETE|Bewerking HybridConnection autorisatieregels verwijderen|
|/Namespaces/HybridConnections/authorizationRules/listkeys/Action|De verbindingsreeks naar HybridConnection ophalen|
|/Namespaces/HybridConnections/authorizationRules/Read| De lijst met autorisatieregels HybridConnection ophalen|
|/Namespaces/HybridConnections/authorizationRules/regeneratekeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/HybridConnections/authorizationRules/Write|Autorisatieregels HybridConnection maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels kunnen worden bijgewerkt.|
|/Namespaces/HybridConnections/DELETE|Bewerking HybridConnection bron verwijderen|
|/Namespaces/HybridConnections/Read|Lijst met beschrijvingen van de Resource HybridConnection ophalen|
|/Namespaces/HybridConnections/Write|Maken of bijwerken HybridConnection eigenschappen.|
|/Namespaces/messagingPlan/Read|Hiermee wordt de Messaging plannen voor een naamruimte. Deze API is afgeschaft. Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API.|
|/Namespaces/messagingPlan/Write|De Messaging plannen voor een naamruimte-updates. Deze API is afgeschaft. Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API.|
|/Namespaces/operationresults/Read|De status van de naamruimtebewerking ophalen|
|/Namespaces/providers/Microsoft.Insights/metricDefinitions/Read|Lijst met Namespace metrische Resource beschrijvingen|
|/Namespaces/Read|De lijst met beschrijvingen van resources van naamruimten ophalen|
|/Namespaces/WcfRelays/authorizationRules/Action|De bewerking WcfRelay bijwerken. Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. Autorisatieregels. Gebruik een PUT-aanroep autorisatieregel bijwerken.|
|/Namespaces/WcfRelays/authorizationRules/DELETE|Bewerking WcfRelay autorisatieregels verwijderen|
|/Namespaces/WcfRelays/authorizationRules/listkeys/Action|De verbindingsreeks naar WcfRelay ophalen|
|/Namespaces/WcfRelays/authorizationRules/Read| De lijst met autorisatieregels WcfRelay ophalen|
|/Namespaces/WcfRelays/authorizationRules/regeneratekeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/WcfRelays/authorizationRules/Write|Autorisatieregels WcfRelay maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels kunnen worden bijgewerkt.|
|/Namespaces/WcfRelays/DELETE|Bewerking WcfRelay bron verwijderen|
|/Namespaces/WcfRelays/Read|Lijst met beschrijvingen van de Resource WcfRelay ophalen|
|/Namespaces/WcfRelays/Write|Maken of bijwerken WcfRelay eigenschappen.|
|naamruimten/schrijven|Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Labels en capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt.|
|/Operations/Read|Bewerkingen ophalen|
|registratie-/ actie|Hiermee wordt het abonnement voor de Relay-resourceprovider geregistreerd en wordt het maken van Relay-resources mogelijk|
|/ unregister/actie|Hiermee wordt het abonnement voor de Relay-resourceprovider geregistreerd en wordt het maken van Relay-resources mogelijk|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| Bewerking | Beschrijving |
|---|---|
|AvailabilityStatuses/current/leestijd|De beschikbaarheidsstatus ophalen voor de opgegeven resource|
|AvailabilityStatuses/leestijd|De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik|
|/ healthevent/actie|Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid|
|/healthevent/activated/Action|Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid|
|/healthevent/InProgress/Action|Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid|
|/healthevent/Pending/Action|Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid|
|/healthevent/Resolved/Action|Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid|
|/healthevent/Updated/Action|Hiermee wordt de wijziging van de status voor de opgegeven resource aangeduid|
|registratie-/ actie|Hiermee wordt het abonnement geregistreerd voor Microsoft ResourceHealth|

## <a name="microsoftresources"></a>Microsoft.Resources

| Bewerking | Beschrijving |
|---|---|
|/ checkResourceName/actie|Controleer of de resourcenaam geldig is.|
|/Deployments/Cancel/Action|Hiermee kunt u een implementatie annuleren.|
|/Deployments/DELETE|Hiermee kunt u een implementatie verwijderen.|
|/Deployments/Operations/Read|Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst.|
|/Deployments/Read|Hiermee kunt u implementaties ophalen of opnemen in een lijst.|
|/Deployments/Validate/Action|Hiermee kunt u een implementatie valideren.|
|implementaties van/schrijven|Hiermee kunt u een implementatie maken of bijwerken.|
|/links/DELETE|Hiermee kunt u een resourcekoppeling verwijderen.|
|/links/Read|Hiermee kunt u resourcekoppelingen ophalen of opnemen in een lijst.|
|koppelingen/schrijven|Hiermee kunt u een resourcekoppeling maken of bijwerken.|
|/Marketplace/Purchase/Action|Hiermee worden een resource gekocht via de marketplace.|
|/providers/Read|De lijst met providers ophalen.|
|/resources/Read|Hiermee wordt de lijst met resources op basis van filters opgehaald.|
|/Subscriptions/Locations/Read|Hiermee kunt u de lijst met ondersteunde locaties ophalen.|
|/Subscriptions/operationresults/Read|Hiermee worden de resultaten van de abonnementsbewerking opgehaald.|
|/Subscriptions/providers/Read|Hiermee kunt u de resourceproviders ophalen of opnemen in een lijst.|
|/Subscriptions/Read|Hiermee kunt u de lijst met abonnementen ophalen.|
|/Subscriptions/resourceGroups/DELETE|Hiermee kunt u een resourcegroep en de bijhorende resources verwijderen.|
|/Subscriptions/ResourceGroups/Deployments/Operations/Read|Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst.|
|/Subscriptions/ResourceGroups/Deployments/operationstatuses/Read|Hiermee worden lijsten met statussen van implementatiebewerkingen opgehaald.|
|/Subscriptions/ResourceGroups/Deployments/Read|Hiermee kunt u implementaties ophalen of opnemen in een lijst.|
|/Subscriptions/ResourceGroups/Deployments/Write|Hiermee kunt u een implementatie maken of bijwerken.|
|/subscriptions/resourceGroups/moveResources/action|Hiermee kunt u resources van een resourcegroep naar een andere resourcegroep verplaatsen.|
|/Subscriptions/resourceGroups/Read|Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst.|
|/Subscriptions/ResourceGroups/resources/Read|Hiermee worden de resources voor de resourcegroep opgehaald.|
|/Subscriptions/resourceGroups/validateMoveResources/Action|Hiermee wordt de verplaatsing van resources van de ene naar de andere resourcegroep gevalideerd.|
|/Subscriptions/resourceGroups/Write|Hiermee kunt u een resourcegroep maken of bijwerken.|
|/subscriptions/resources/Read|Hiermee kunt u de resources van een abonnement ophalen.|
|/Subscriptions/tagNames/DELETE|Hiermee kunt u een abonnementslabel verwijderen.|
|/Subscriptions/tagNames/Read|Hiermee kunt u abonnementslabels ophalen of opnemen in een lijst.|
|/subscriptions/tagNames/tagValues/delete|Hiermee kunt een abonnementslabelwaarde verwijderen.|
|/Subscriptions/tagNames/tagValues/Read|Hiermee kunt u abonnementslabelwaarden ophalen of opnemen in een lijst.|
|/Subscriptions/tagNames/tagValues/Write|Hiermee kunt u een abonnementslabelwaarde toevoegen.|
|/Subscriptions/tagNames/Write|Hiermee kunt u een abonnementslabel toevoegen.|
|/tenants/Read|Hiermee kunt u de lijst met tenants ophalen.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| Bewerking | Beschrijving |
|---|---|
|/jobcollections/DELETE|Hiermee verwijdert u de taakverzameling.|
|/jobcollections/Disable/Action|Hiermee schakelt u de taakverzameling uit.|
|/jobcollections/Enable/Action|Hiermee schakelt u de taakverzameling in.|
|/jobcollections/Jobs/DELETE|Hiermee verwijdert u een taak.|
|/jobcollections/jobs/generateLogicAppDefinition/action|Hiermee wordt een definitie voor een logische app gegenereerd op basis van een Scheduler-taak.|
|/jobcollections/Jobs/jobhistories/Read|Hiermee haalt u de taakgeschiedenis op.|
|/jobcollections/Jobs/Read|Hiermee haalt u een taak op.|
|/jobcollections/Jobs/Run/Action|De taak wordt uitgevoerd.|
|/jobcollections/Jobs/Write|Hiermee maakt u een taak of werkt u er een bij.|
|/jobcollections/Read|Taakverzameling ophalen|
|taakverzamelingen/schrijven|Hiermee maakt u een taakverzameling of werkt u er een bij.|

## <a name="microsoftsearch"></a>Microsoft.Search

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Controleert de beschikbaarheid van de naam van de service.|
|registratie-/ actie|Het abonnement voor de resource zoekmachine registreert en wordt het maken van de search-services.|
|/searchServices/createQueryKey/action|Maakt de querysleutel.|
|/searchServices/delete|Hiermee verwijdert u de search-service.|
|/searchServices/diagnosticSettings/Read|De diganostic instelling opgehaald voor de resource lezen|
|/searchServices/diagnosticSettings/Write|Maken of bijwerken van de instelling diganostic voor de bron|
|/searchServices/listAdminKeys/action|De administratorsleutels leest.|
|/searchServices/logDefinitions/Read|De beschikbare logboeken opgehaald voor de search-service|
|/searchServices/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor de search-service|
|/searchServices/queryKey/DELETE|Hiermee verwijdert u de querysleutel.|
|/searchServices/queryKey/Read|De querysleutels worden gelezen.|
|/searchServices/Read|Leest de search-service.|
|/searchServices/regenerateAdminKey/Action|De Administrator-code genereert.|
|/searchServices/start/Action|Start de search-service.|
|/searchServices/Stop/Action|De search-service stopt.|
|searchServices/schrijven|Maken of bijwerken van de search-service.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| Bewerking | Beschrijving |
|---|---|
|/Alerts/Read|Alle beschikbare beveiligingswaarschuwingen opgehaald|
|/applicationWhitelistings/Read|De toepassing whitelistings opgehaald|
|applicationWhitelistings/schrijven|Hiermee maakt u een nieuwe toepassing whitelisting of een bestaande bijgewerkt|
|/complianceResults/Read|De compliantieresultaten voor de resource opgehaald|
|/Locations/Alerts/Activate/Action|Een beveiligingswaarschuwing activeren|
|/Locations/Alerts/Dismiss/Action|Een beveiligingswaarschuwing te sluiten|
|/Locations/Alerts/Read|Alle beschikbare beveiligingswaarschuwingen opgehaald|
|/locations/jitNetworkAccessPolicies/initiate/action|Initieert een netwerktoegangsbeleid just in time|
|/locations/jitNetworkAccessPolicies/read|Het toegangsbeleid just in time netwerk opgehaald|
|/locations/jitNetworkAccessPolicies/write|Hiermee maakt u een nieuw netwerk just-in-time-beleid of een bestaande bijgewerkt|
|/Locations/Read|De locatie van beveiliging opgehaald|
|/Locations/Tasks/Activate/Action|Een beveiligingsaanbeveling activeren|
|/Locations/Tasks/Dismiss/Action|Een beveiligingsaanbeveling negeren|
|/Locations/Tasks/Read|Alle beschikbare beveiligingsaanbevelingen opgehaald|
|/Locations/Tasks/Resolve/Action|Een beveiligingsaanbeveling oplossen|
|/Locations/Tasks/start/Action|Start een beveiligingsaanbeveling|
|/Policies/Read|Het beveiligingsbeleid opgehaald|
|/ beleid/schrijven|Updates van het beveiligingsbeleid|
|/pricings/DELETE|Hiermee verwijdert u de prijscategorie instellingen voor het bereik|
|/pricings/Read|De prijscategorie instellingen voor de scope opgehaald|
|pricings/schrijven|De prijscategorie instellingen voor de scope-updates|
|registratie-/ actie|Registreert het abonnement voor Azure Security Center|
|/securityContacts/DELETE|Neem contact op met de beveiliging wordt verwijderd|
|/securityContacts/Read|Neem contact op met de beveiliging opgehaald|
|securityContacts/schrijven|Neem contact op met de beveiliging werkt|
|/securitySolutions/DELETE|Hiermee verwijdert u een beveiligingsoplossing|
|/securitySolutions/Read|De beveiligingsoplossingen opgehaald|
|securitySolutions/schrijven|Maakt een nieuwe beveiligingsoplossing of een bestaande bijgewerkt|
|/securitySolutionsReferenceData/Read|Referentiegegevens voor de beveiligingsoplossingen opgehaald|
|/securityStatuses/Read|De beveiliging van health-statussen voor Azure-resources opgehaald|
|/securityStatusesSummaries/Read|De beveiliging van samenvattingen van de statussen voor de scope opgehaald|
|/Tasks/Read|Alle beschikbare beveiligingsaanbevelingen opgehaald|
|/webApplicationFirewalls/delete|Hiermee verwijdert u een web application firewall|
|/webApplicationFirewalls/read|Het web application firewalls opgehaald|
|/webApplicationFirewalls/write|Hiermee maakt u een nieuwe web application firewall of een bestaande bijgewerkt|
|/workspaceSettings/Connect/Action|Wijzigen van de werkruimte-instellingen opnieuw verbinden|
|/workspaceSettings/DELETE|Hiermee verwijdert u de werkruimte-instellingen|
|/workspaceSettings/Read|De werkruimte-instellingen opgehaald|
|workspaceSettings/schrijven|De werkruimte-instellingen voor updates|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd.|
|/ checkNamespaceAvailability/actie|Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd. Deze API is afgeschaft. Gebruik in plaats hiervan CheckNameAvailabiltiy.|
|/Namespaces/authorizationRules/Action|Updates Namespace-autorisatieregel. Deze API is depricated. Gebruik een PUT-aanroep in plaats daarvan de autorisatieregel Namespace bijwerken... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API.|
|/Namespaces/authorizationRules/DELETE|Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd. |
|/Namespaces/authorizationRules/listkeys/Action|De verbindingstekenreeks naar de naamruimte ophalen|
|/Namespaces/authorizationRules/Read|De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen.|
|/Namespaces/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/authorizationRules/Write|Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/ naamruimten/verwijderen|De resource van een naamruimte verwijderen|
|/Namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/Action|De autorisatie regels sleutels voor de primaire naamruimte voor herstel na noodgevallen opgehaald|
|/Namespaces/disasterRecoveryConfigs/authorizationRules/Read|Disaster Recovery primaire Namespace van autorisatieregels ophalen|
|/namespaces/disasterRecoveryConfigs/breakPairing/action|Schakelt Disaster Recovery uit en stopt het repliceren van wijzigingen van primaire naar secundaire naamruimten.|
|/Namespaces/disasterrecoveryconfigs/checkNameAvailability/Action|Beschikbaarheid van de controles van naamruimtealias onder het betreffende abonnement.|
|/namespaces/disasterRecoveryConfigs/delete|Hiermee verwijdert u de configuratie van het herstel na noodgevallen die zijn gekoppeld aan de naamruimte. Deze bewerking kan alleen worden aangeroepen via de primaire naamruimte.|
|/Namespaces/disasterRecoveryConfigs/failover/Action|Hiermee wordt een GEO DR-failover aangeroepen en wordt het naamruimtealias zo geconfigureerd dat verwezen wordt naar de secundaire naamruimte.|
|/namespaces/disasterRecoveryConfigs/read|Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort opgehaald.|
|/namespaces/disasterRecoveryConfigs/write|Hiermee wordt de Disaster Recovery-configuratie die bij de naamruimte hoort gemaakt of bijgewerkt.|
|/namespaces/eventGridFilters/delete|Hiermee verwijdert u de gebeurtenis raster filteren die zijn gekoppeld aan de naamruimte.|
|/Namespaces/eventGridFilters/Read|Hiermee haalt u de gebeurtenis raster filteren die zijn gekoppeld aan de naamruimte.|
|/Namespaces/eventGridFilters/Write|Maken of bijwerken van de gebeurtenis raster filteren die zijn gekoppeld aan de naamruimte.|
|/Namespaces/eventhubs/Read|Lijst met beschrijvingen van EventHub-Resource|
|/Namespaces/messagingPlan/Read|Hiermee wordt de Messaging plannen voor een naamruimte. Deze API is afgeschaft. Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API.|
|/Namespaces/messagingPlan/Write|De Messaging plannen voor een naamruimte-updates. Deze API is afgeschaft. Eigenschappen beschikbaar via de MessagingPlan resource worden verplaatst naar een Namespace-resource in latere versies van de API (bovenliggend)... Deze bewerking wordt niet ondersteund op versie 2017-04-01-API.|
|/Namespaces/Migrate/Action|Bewerking voor migreren van de naamruimte|
|/Namespaces/operationresults/Read|De status van de naamruimtebewerking ophalen|
|/namespaces/providers/Microsoft.Insights/diagnosticSettings/read|Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen|
|/Namespaces/providers/Microsoft.Insights/diagnosticSettings/Write|Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen|
|/namespaces/providers/Microsoft.Insights/logDefinitions/read|Lijst met Namespace logboeken Resource beschrijvingen|
|/Namespaces/providers/Microsoft.Insights/metricDefinitions/Read|Lijst met Namespace metrische Resource beschrijvingen|
|/Namespaces/Queues/authorizationRules/Action|Bewerking voor het bijwerken van de wachtrij. Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. Autorisatieregels. Gebruik een PUT-aanroep autorisatieregel bijwerken.|
|/namespaces/queues/authorizationRules/delete|Bewerking autorisatieregels wachtrij verwijderen|
|/Namespaces/Queues/authorizationRules/listkeys/Action|De verbindingsreeks naar wachtrij ophalen|
|/Namespaces/Queues/authorizationRules/Read| De lijst met autorisatieregels wachtrij ophalen|
|/Namespaces/Queues/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/Queues/authorizationRules/Write|Autorisatieregels wachtrij maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels kunnen worden bijgewerkt.|
|/Namespaces/Queues/DELETE|Bewerking bron van de wachtrij verwijderen|
|/Namespaces/Queues/Read|Lijst met beschrijvingen van de wachtrij-Resource|
|/Namespaces/Queues/Write|Maken of wachtrijeigenschappen Update.|
|/Namespaces/Read|De lijst met beschrijvingen van resources van naamruimten ophalen|
|/Namespaces/Topics/authorizationRules/Action|Bewerking voor het bijwerken van onderwerp. Deze bewerking wordt niet ondersteund op versie 2017-04-01-API. Autorisatieregels. Gebruik een PUT-aanroep autorisatieregel bijwerken.|
|/Namespaces/Topics/authorizationRules/DELETE|Bewerking verificatieregels onderwerp verwijderen|
|/Namespaces/Topics/authorizationRules/listkeys/Action|De verbindingsreeks naar onderwerp ophalen|
|/Namespaces/Topics/authorizationRules/Read| De lijst met verificatieregels onderwerp ophalen|
|/Namespaces/Topics/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/Topics/authorizationRules/Write|Verificatieregels onderwerp maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels kunnen worden bijgewerkt.|
|/Namespaces/Topics/DELETE|Bewerking onderwerp bron verwijderen|
|/Namespaces/Topics/Read|Lijst met beschrijvingen van de Resource onderwerp|
|/Namespaces/Topics/Subscriptions/DELETE|Bewerking TopicSubscription bron verwijderen|
|/Namespaces/Topics/Subscriptions/Read|Lijst met beschrijvingen van de Resource TopicSubscription ophalen|
|/Namespaces/Topics/Subscriptions/Rules/DELETE|Bewerking bron van de regel verwijderen|
|/Namespaces/Topics/Subscriptions/Rules/Read|Lijst met beschrijvingen van de Resource regel|
|/Namespaces/Topics/Subscriptions/Rules/Write|Maken of de eigenschappen van de regel voor het bijwerken.|
|/Namespaces/Topics/Subscriptions/Write|Maken of bijwerken TopicSubscription eigenschappen.|
|/Namespaces/Topics/Write|Maken of bijwerken onderwerp eigenschappen.|
|naamruimten/schrijven|Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Labels en capaciteit van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt.|
|/Operations/Read|Bewerkingen ophalen|
|registratie-/ actie|Hiermee wordt het abonnement voor de Service Bus-resourceprovider geregistreerd en wordt het maken van Service Bus-resources mogelijk|
|/SKU/Read|Lijst met beschrijvingen van de Resource-Sku ophalen|
|/SKU/Regions/Read|Lijst met beschrijvingen van de Resource SkuRegions ophalen|
|/ unregister/actie|Hiermee wordt het abonnement voor de Service Bus-resourceprovider geregistreerd en wordt het maken van Service Bus-resources mogelijk|

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

| Bewerking | Beschrijving |
|---|---|
|/clusters/Applications/DELETE|Een toepassing verwijderen|
|/clusters/Applications/Read|Een toepassing lezen|
|/clusters/Applications/Services/DELETE|Een service verwijderen|
|/clusters/Applications/Services/Partitions/Read|Een partitie lezen|
|/clusters/Applications/Services/Partitions/Replicas/Read|Een replica lezen|
|/clusters/Applications/Services/Read|Een service lezen|
|/clusters/Applications/Services/statuses/Read|Een servicestatus lezen|
|/clusters/Applications/Services/Write|Een service maken of bijwerken|
|/clusters/Applications/Write|Een toepassing maken of bijwerken|
|/clusters/applicationTypes/DELETE|Een toepassingstype verwijderen|
|/clusters/applicationTypes/Read|Een toepassingstype lezen|
|/clusters/applicationTypes/Versions/DELETE|Een versie van een toepassingstype verwijderen|
|/clusters/applicationTypes/Versions/Read|Een versie van een toepassingstype lezen|
|/clusters/applicationTypes/Versions/Write|Een versie van een toepassingstype maken of bijwerken|
|/clusters/applicationTypes/Write|Een toepassingstype maken of bijwerken|
|/clusters/DELETE|Een cluster verwijderen|
|/clusters/nodes/Read|Een knooppunt lezen|
|/clusters/Read|Een cluster lezen|
|/clusters/statuses/Read|Een clusterstatus lezen|
|clusters/schrijven|Een cluster maken of bijwerken|
|/Locations/clusterVersions/Read|Een clusterversie lezen|
|/Locations/Environments/clusterVersions/Read|Een clusterversie voor een specifieke omgeving lezen|
|/Locations/operationresults/Read|Bewerkingsresultaten lezen|
|/Locations/Operations/Read|Bewerkingen op locatie lezen|
|/Operations/Read|Beschikbare bewerkingen lezen|
|registratie-/ actie|Een actie registreren|

## <a name="microsoftsolutions"></a>Microsoft.Solutions

| Bewerking | Beschrijving |
|---|---|
|/applicationDefinitions/DELETE|Hiermee wordt een toepassingsdefinitie verwijderd.|
|/applicationDefinitions/Read|Hiermee wordt een lijst met toepassingsdefinities opgehaald.|
|applicationDefinitions/schrijven|Hiermee wordt een toepassingsdefinitie toegevoegd of gewijzigd.|
|/Applications/DELETE|Hiermee verwijdert u een toepassing.|
|/Applications/Read|Hiermee wordt een lijst met toepassingen opgehaald.|
|/ applications/schrijven|Hiermee wordt een toepassing gemaakt.|
|/Locations/operationStatuses/Read|Hiermee wordt de bewerkingsstatus voor de resource ingelezen.|
|registratie-/ actie|Registreren bij Solutions.|

## <a name="microsoftsql"></a>Microsoft.Sql

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Controleer of de opgegeven server naam beschikbaar is voor het inrichten van wereldwijd voor een bepaald abonnement.|
|/locations/auditingSettingsAzureAsyncOperation/read|Resultaat van de uitgebreide server blob controle beleid Set-bewerking ophalen|
|/locations/auditingSettingsOperationResults/read|Resultaat van de server blob controle beleid Set-bewerking ophalen|
|/Locations/Capabilities/Read|Hiermee haalt u de mogelijkheden voor dit abonnement in een bepaalde locatie|
|/locations/databaseAzureAsyncOperation/read|Hiermee haalt u de status van een databasebewerking.|
|/Locations/databaseOperationResults/Read|Hiermee haalt u de status van een databasebewerking.|
|/locations/deletedServerAsyncOperation/read|Opgehaald in uitvoering bewerkingen op verwijderde server|
|/locations/deletedServerOperationResults/read|Opgehaald in uitvoering bewerkingen op verwijderde server|
|/Locations/deletedServers/Read|De lijst met verwijderde servers of haalt de eigenschappen voor de opgegeven server verwijderde retourneren.|
|/Locations/deletedServers/recover/Action|Een verwijderde server herstellen|
|/locations/deleteVirtualNetworkOrSubnets/action|Verwijdert virtuele netwerk regels die zijn gekoppeld aan een virtueel netwerk of subnet|
|/locations/elasticPoolAzureAsyncOperation/read|De azure asynchrone bewerking voor een elastische pool asynchrone bewerking opgehaald|
|/locations/elasticPoolOperationResults/read|Hiermee wordt het resultaat van een bewerking voor de elastische groep opgehaald.|
|/locations/extendedAuditingSettingsAzureAsyncOperation/read|Resultaat van de uitgebreide server blob controle beleid Set-bewerking ophalen|
|/locations/extendedAuditingSettingsOperationResults/read|Resultaat van de uitgebreide server blob controle beleid Set-bewerking ophalen|
|/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action|Beheerde database restore-bewerking is voltooid|
|/locations/managedTransparentDataEncryptionAzureAsyncOperation/read|Opgehaald in uitvoering bewerkingen op beheerde database transparante gegevensversleuteling|
|/locations/managedTransparentDataEncryptionOperationResults/read|Opgehaald in uitvoering bewerkingen op beheerde database transparante gegevensversleuteling|
|/Locations/Read|De beschikbare locaties voor een bepaald abonnement opgehaald|
|/locations/syncAgentOperationResults/read|Resultaat van de synchronisatie-agent resource bewerking ophalen|
|/locations/syncDatabaseIds/read|De synchronisatie-database-id's voor een bepaalde regio en een abonnement ophalen|
|/locations/syncGroupOperationResults/read|Resultaat van de synchronisatie groep resource-bewerking ophalen|
|/Locations/syncMemberOperationResults/Read|Resultaat van de synchronisatie lid resource bewerking ophalen|
|/Locations/usages/Read|Hiermee wordt een verzameling van de meetgegevens voor softwaregebruik voor dit abonnement op een locatie|
|/locations/virtualNetworkRulesAzureAsyncOperation/read|Retourneert de gegevens van de opgegeven virtuele netwerk regels azure asynchrone bewerking |
|/locations/virtualNetworkRulesOperationResults/read|Retourneert de gegevens van de opgegeven virtuele netwerk regels bewerking |
|/managedInstances/Administrators/DELETE|Hiermee verwijdert u een bestaande beheerder van beheerde exemplaar.|
|/managedInstances/Administrators/Read|Hiermee wordt een lijst met beheerde exemplaar beheerders opgehaald.|
|/managedInstances/Administrators/Write|Maken of bijwerken van de administrator beheerde exemplaar met de opgegeven parameters.|
|/managedInstances/databases/DELETE|Hiermee verwijdert u een bestaande beheerde database|
|/managedInstances/databases/Read|Beheerde database opgehaald bestaande|
|/managedInstances/databases/securityAlertPolicies/Read|Ophalen van gegevens van de afwijkingsdetectie van de database threat geconfigureerd op een bepaalde beheerde database|
|/managedInstances/databases/securityAlertPolicies/write|Het beleid database threat detectie voor een bepaalde beheerde database wijzigen|
|/managedInstances/databases/securityEvents/Read|Haalt de beheerde database beveiligingsgebeurtenissen|
|/managedInstances/databases/transparentDataEncryption/read|Ophalen van gegevens van de database met transparante gegevensversleuteling op een bepaalde beheerde database|
|/managedInstances/databases/transparentDataEncryption/Write|De database met transparante gegevensversleuteling voor een bepaalde beheerde database wijzigen|
|/managedInstances/databases/Write|Een nieuwe database maken of bijwerken van een bestaande database.|
|/managedInstances/delete|Hiermee verwijdert u een bestaand exemplaar van de beheerde.|
|/managedInstances/metricDefinitions/Read|Beheerde exemplaar metrische definities ophalen|
|/managedInstances/metrics/Read|Ophalen van beheerde exemplaar metrische gegevens|
|/managedInstances/Read|De lijst met beheerde-exemplaren of haalt de eigenschappen voor het opgegeven exemplaar van de beheerde retourneren.|
|/managedInstances/securityAlertPolicies/Read|Ophalen van gegevens van de afwijkingsdetectie van de beheerde server threat geconfigureerd op een bepaalde beheerde server|
|/managedInstances/securityAlertPolicies/Write|Het beleid voor detectie van bedreigingen beheerde server voor een bepaalde beheerde server wijzigen|
|managedInstances/schrijven|Hiermee maakt u een beheerde exemplaar met de opgegeven parameters of bijwerken van de eigenschappen of labels voor het opgegeven exemplaar van de beheerde.|
|/Operations/Read|Beschikbare REST-bewerkingen opgehaald|
|registratie-/ actie|Registreert het abonnement voor de resourceprovider voor Microsoft SQL-Database en wordt het maken van Microsoft SQL-Databases.|
|/servers/administratorOperationResults/read|Opgehaald in uitvoering bewerkingen op de server-beheerders|
|/servers/Administrators/DELETE|Verwijderen van server-beheerder|
|/servers/Administrators/Read|Administrator-servergegevens ophalen|
|/servers/Administrators/Write|Maken of bijwerken van de serverbeheerder|
|/servers/Advisors/Read|Retourneert de lijst met adviseurs beschikbaar voor de server|
|/servers/Advisors/recommendedActions/Read|Retourneert de lijst met aanbevolen acties van opgegeven advisor voor de server|
|/servers/Advisors/recommendedActions/Write|De aanbevolen actie toepassen op de server|
|/servers/Advisors/Write|Updates execute automatisch-status van een advisor op serverniveau.|
|/servers/auditingPolicies/Read|Ophalen van gegevens van de tabel met standaardtijden server controlebeleid geconfigureerd op een bepaalde server|
|/servers/auditingPolicies/Write|Controle van wijzigingen in de standaard server tabel voor een bepaalde server|
|/servers/auditingSettings/operationResults/read|Resultaat van de server blob controle beleid Set-bewerking ophalen|
|/servers/auditingSettings/Read|Ophalen van gegevens van de server blob controlebeleid geconfigureerd op een bepaalde server|
|/servers/auditingSettings/Write|De server blob-controle voor een bepaalde server wijzigen|
|/servers/automaticTuning/Read|Retourneert automatische afstemming van instellingen voor de server|
|/servers/automaticTuning/Write|Prestatieafstemming instellingen voor de server voor automatische updates en bijgewerkte instellingen retourneert|
|/servers/backupLongTermRetentionVaults/delete|Hiermee verwijdert u een bestaande back-upkluis archivering.|
|/servers/backupLongTermRetentionVaults/Read|Deze bewerking wordt gebruikt voor het ophalen van een back-up langdurig bewaren kluis. Gegevens over de kluis geregistreerd met deze server worden geretourneerd|
|/servers/backupLongTermRetentionVaults/Write|Deze bewerking wordt gebruikt voor het registreren van een back-up langdurig bewaren kluis naar een server|
|/servers/communicationLinks/delete|Hiermee verwijdert u een bestaande koppeling van de server-communicatie.|
|/servers/communicationLinks/Read|De lijst met communicatiekoppelingen van de opgegeven server retourneren.|
|/servers/communicationLinks/Write|Maken of bijwerken van een server communicatiekanaal.|
|/servers/connectionPolicies/Read|De lijst met beleidsregels voor server-verbinding van een opgegeven server retourneren.|
|/servers/connectionPolicies/Write|Maken of bijwerken van een beleid van server-verbinding.|
|/servers/databases/Advisors/Read|Retourneert de lijst met adviseurs die beschikbaar zijn voor de database|
|/servers/databases/Advisors/recommendedActions/Read|Retourneert de lijst met aanbevolen acties van de opgegeven advisor voor de database|
|/servers/databases/Advisors/recommendedActions/Write|De aanbevolen actie toepassen op de database|
|/servers/databases/Advisors/Write|Update automatisch-execute status van een advisor op databaseniveau.|
|/servers/databases/auditingPolicies/Read|Ophalen van gegevens van de tabel controlebeleid geconfigureerd op een bepaalde database|
|/servers/databases/auditingPolicies/Write|De tabel controlebeleid voor een bepaalde database wijzigen|
|/servers/databases/auditingSettings/Read|Ophalen van gegevens van de blob-controlebeleid geconfigureerd op een bepaalde database|
|/servers/databases/auditingSettings/Write|De blob controlebeleid voor een bepaalde database wijzigen|
|/servers/databases/auditRecords/Read|De database blob-controlerecords ophalen|
|/servers/databases/automaticTuning/Read|Retourneert automatische afstemming van instellingen voor een database|
|/servers/databases/automaticTuning/Write|Instellingen voor automatische afstemmen voor een database-updates en bijgewerkte instellingen retourneert|
|/servers/databases/azureAsyncOperation/read|Hiermee haalt u de status van een databasebewerking.|
|/servers/databases/backupLongTermRetentionPolicies/Read|De lijst met back-beleid voor archivering van een opgegeven database geretourneerd.|
|/servers/databases/backupLongTermRetentionPolicies/Write|Maken of bijwerken van een database archiveringsbeleid voor de back-up.|
|/servers/databases/connectionPolicies/Read|Ophalen van gegevens van het verbindingsbeleid is geconfigureerd op een bepaalde database|
|/servers/databases/connectionPolicies/Write|Verbindingsbeleid voor een bepaalde database wijzigen|
|/servers/databases/dataMaskingPolicies/Read|De lijst van databasegegevens maskeren beleidsregels retourneren.|
|/servers/databases/dataMaskingPolicies/rules/delete|Gegevensmaskering beleidsregel voor een bepaalde database verwijderen|
|/servers/databases/dataMaskingPolicies/Rules/Read|Ophalen van gegevens van de gegevens die zijn geconfigureerd op een bepaalde database beleidsregel maskeren|
|/servers/databases/dataMaskingPolicies/Rules/Write|Gegevensmaskering beleidsregel voor een bepaalde database wijzigen|
|/servers/databases/dataMaskingPolicies/Write|Beleid voor een bepaalde database gegevensmaskering wijzigen|
|/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/Read|Retourneert de gegevens van de gedistribueerde query-stap van de datawarehouse-query voor de ID voor de geselecteerde stap|
|/servers/databases/dataWarehouseQueries/Read|Retourneert het datawarehouse distributie query-gegevens voor geselecteerde query-ID|
|/servers/databases/dataWarehouseUserActivities/Read|Haalt de gebruikersactiviteiten van een exemplaar van SQL Data Warehouse waaronder query's uitgevoerd en onderbroken|
|/servers/databases/DELETE|Hiermee verwijdert u een bestaande database.|
|/servers/databases/export/Action|Exporteren van Azure SQL Database|
|/servers/databases/extendedAuditingSettings/Read|Ophalen van gegevens van de uitgebreide blob controlebeleid geconfigureerd op een bepaalde database|
|/servers/databases/extendedAuditingSettings/Write|De uitgebreide blob controlebeleid voor een bepaalde database wijzigen|
|/servers/databases/Extensions/Read|Hiermee wordt een verzameling van uitbreidingen voor de database.|
|/servers/databases/Extensions/Write|De extensie voor een bepaalde database wijzigen|
|/servers/databases/geoBackupPolicies/Read|Back-upbeleid voor een bepaalde database geo ophalen|
|/servers/databases/geoBackupPolicies/write|Een database geobackup beleid maken of bijwerken|
|/servers/databases/importExportOperationResults/read|Bewerkingen voor opgehaald in uitvoering voor importeren/exporteren|
|/servers/databases/metricDefinitions/Read|Retourtypen van metrische gegevens die beschikbaar voor databases zijn|
|/servers/databases/metrics/Read|Metrische gegevens voor de databases terug|
|/servers/databases/Move/Action|Wijzig de naam van Azure SQL Database|
|/servers/databases/operationResults/Read|Hiermee haalt u de status van een databasebewerking.|
|/servers/databases/Operations/Cancel/Action|Hiermee annuleert Azure SQL Database in behandeling zijnde asynchrone bewerking die nog niet is voltooid.|
|/servers/databases/Operations/Read|De lijst met bewerkingen die worden uitgevoerd op de database terug|
|/servers/databases/pause/Action|Onderbreken Azure SQL Datawarehouse-Database|
|/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/servers/databases/providers/Microsoft.Insights/logDefinitions/read|De beschikbare logboeken opgehaald voor databases|
|/servers/databases/providers/Microsoft.Insights/metricDefinitions/read|Retourtypen van metrische gegevens die beschikbaar voor databases zijn|
|/servers/databases/queryStore/queryTexts/Read|Retourneert de verzameling van query teksten die overeenkomen met de opgegeven parameters.|
|/servers/databases/queryStore/Read|Retourneert de huidige waarden van de Query Store-instellingen voor de database.|
|/servers/databases/queryStore/Write|Query Store-instelling voor de database-updates|
|/servers/databases/Read|De lijst met databases of haalt de eigenschappen voor de opgegeven database geretourneerd.|
|/servers/databases/replicationLinks/DELETE|Beëindigen van de replicatierelatie geforceerd en met mogelijk gegevensverlies|
|/servers/databases/replicationLinks/failover/Action|Failover na het synchroniseren van alle wijzigingen van de primaire waardoor deze database in de replicatie-relationship\u0027s primaire en het aanbrengen van de externe primaire naar een secundair|
|/servers/databases/replicationLinks/forceFailoverAllowDataLoss/Action|Failover onmiddellijk met mogelijk gegevensverlies, waardoor deze database in de replicatie-relationship\u0027s primaire en het aanbrengen van de externe primaire naar een secundair|
|/servers/databases/replicationLinks/Read|Geretourneerde gegevens over koppelingen voor databasereplicatie tot stand gebracht voor een bepaalde database|
|/servers/databases/replicationLinks/Unlink/Action|Beëindigen van de replicatierelatie geforceerd of na het synchroniseren met de partner|
|/servers/databases/replicationLinks/updateReplicationMode/action|Replicatiemodus voor koppeling naar de modus voor synchroon of asynchroon bijwerken|
|/servers/databases/restorePoints/Action|Hiermee wordt een nieuw herstelpunt gemaakt|
|/servers/databases/restorePoints/Read|Retourneert de herstelpunten voor de database.|
|/servers/databases/Resume/Action|Azure SQL Datawarehouse-Database hervatten|
|/servers/databases/schemas/Read|Ophalen van lijst met schema's van een database|
|/servers/databases/schemas/Tables/Columns/Read|Ophalen van lijst met kolommen van een tabel|
|/servers/databases/schemas/tables/columns/sensitivityLabels/delete|Het gevoeligheidslabel van een bepaalde kolom verwijderen|
|/servers/databases/schemas/Tables/Columns/sensitivityLabels/Read|Het gevoeligheidslabel van een bepaalde kolom ophalen|
|/servers/databases/schemas/Tables/Columns/sensitivityLabels/Write|Maken of bijwerken van het gevoeligheidslabel van een bepaalde kolom|
|/servers/databases/schemas/Tables/Read|Ophalen van lijst met tabellen van een database|
|/servers/databases/schemas/Tables/recommendedIndexes/Read|Lijst met indexaanbevelingen voor een database ophalen|
|/servers/databases/schemas/Tables/recommendedIndexes/Write|Indexaanbeveling toepassen|
|/servers/databases/securityAlertPolicies/Read|Ophalen van gegevens van de threat beleid geconfigureerd op een bepaalde database|
|/servers/databases/securityAlertPolicies/write|Het beleid van de detectie van bedreigingen voor een bepaalde database wijzigen|
|/servers/databases/securityMetrics/Read|Hiermee wordt een verzameling van database beveiliging metrische gegevens|
|/servers/databases/sensitivityLabels/Read|Lijst gevoeligheidslabels van een bepaalde database|
|/servers/databases/serviceTierAdvisors/Read|Database schalen omhoog of omlaag wordt voorgesteld geretourneerd op basis van statistieken van de query kan worden uitgevoerd op de prestaties verbeteren of kosten|
|/servers/databases/syncGroups/cancelSync/Action|Synchronisatie groepssynchronisatie annuleren|
|/servers/databases/syncGroups/DELETE|Hiermee verwijdert u een bestaande groep voor synchronisatie.|
|/servers/databases/syncGroups/hubSchemas/read|De lijst met synchronisatie hub databaseschema retourneren|
|/servers/databases/syncGroups/Logs/Read|De lijst met synchronisatielogboeken groep retourneren|
|/servers/databases/syncGroups/Read|De lijst met synchronisatie groepen of haalt u de eigenschappen voor de groep voor synchronisatie opgegeven retourneren.|
|/servers/databases/syncGroups/refreshHubSchema/Action|Synchronisatie hub databaseschema vernieuwen|
|/servers/databases/syncGroups/refreshHubSchemaOperationResults/Read|Resultaat van de synchronisatiebewerking hub schema vernieuwen ophalen|
|/servers/databases/syncGroups/syncMembers/DELETE|Hiermee verwijdert u een bestaand lid van de synchronisatie.|
|/servers/databases/syncGroups/syncMembers/Read|De lijst met leden van de synchronisatie of haalt de eigenschappen voor een lid van de opgegeven synchronisatie retourneren.|
|/servers/databases/syncGroups/syncMembers/refreshSchema/Action|Synchronisatie lid schema vernieuwen|
|/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/Read|Resultaat van de synchronisatiebewerking lid schema vernieuwen ophalen|
|/servers/databases/syncGroups/syncMembers/schemas/Read|De lijst met synchronisatie lid databaseschema retourneren|
|/servers/databases/syncGroups/syncMembers/Write|Een lid van de synchronisatie maakt met de opgegeven parameters of bijwerken van de eigenschappen voor de opgegeven synchronisatie-lid.|
|/servers/databases/syncGroups/triggerSync/action|Groepssynchronisatie voor trigger sync|
|/servers/databases/syncGroups/Write|Hiermee maakt u een groep voor synchronisatie met de opgegeven parameters of bijwerken van de eigenschappen voor de groep voor synchronisatie opgegeven.|
|/servers/databases/topQueries/queryText/action|Retourneert de Transact-SQL-tekst voor de geselecteerde query-ID|
|/servers/databases/topQueries/read|Retourneert geaggregeerd runtime-statistieken voor de geselecteerde query in de geselecteerde tijdsperiode|
|/servers/databases/topQueries/statistics/read|Retourneert geaggregeerd runtime-statistieken voor de geselecteerde query in de geselecteerde tijdsperiode|
|/servers/databases/transparentDataEncryption/operationResults/read|Opgehaald in uitvoering bewerkingen op transparante gegevensversleuteling|
|/servers/databases/transparentDataEncryption/Read|Status en details van transparent data encryption beveiligingsfunctie voor een bepaalde database ophalen|
|/servers/databases/transparentDataEncryption/Write|Transparent data encryption status wijzigen|
|/servers/databases/upgradeDataWarehouse/Action|Azure SQL Datawarehouse-Database bijwerken|
|/servers/databases/usages/Read|Hiermee haalt u de informatie van Azure SQL Database vormen van gebruik|
|/servers/databases/vulnerabilityAssessments/DELETE|De controle op beveiligingslekken voor een bepaalde database verwijderen|
|/servers/databases/vulnerabilityAssessments/Read|Ophalen van gegevens van de controle op beveiligingslekken geconfigureerd op een bepaalde database|
|/servers/databases/vulnerabilityAssessments/Rules/baselines/DELETE|De basislijn vulnerability assessment regel voor een bepaalde database verwijderen|
|/servers/databases/vulnerabilityAssessments/Rules/baselines/Read|Ophalen van de basislijn vulnerability assessment regel voor een bepaalde database|
|/servers/databases/vulnerabilityAssessments/Rules/baselines/Write|De basislijn vulnerability assessment regel voor een bepaalde database wijzigen|
|/servers/databases/vulnerabilityAssessments/scans/Action|Database-scan voor beoordeling van beveiligingsproblemen worden uitgevoerd.|
|/servers/databases/vulnerabilityAssessments/scans/export/Action|Een bestaand scan resultaat niet converteren naar een menselijke leesbare indeling. Als al er niets gebeurt|
|/servers/databases/vulnerabilityAssessments/scans/Read|De lijst met database vulnerability assessment scan records geretourneerd of ophalen van de scan-record voor de opgegeven scan-ID.|
|/servers/databases/vulnerabilityAssessments/Write|De controle op beveiligingslekken voor een bepaalde database wijzigen|
|/servers/databases/vulnerabilityAssessmentScans/Action|Database-scan voor beoordeling van beveiligingsproblemen worden uitgevoerd.|
|/servers/databases/vulnerabilityAssessmentScans/operationResults/read|Het resultaat van de database vulnerability assessment scan Execute-bewerking ophalen|
|/servers/databases/vulnerabilityAssessmentSettings/Read|Ophalen van gegevens van de controle op beveiligingslekken geconfigureerd op een bepaalde database|
|/servers/databases/vulnerabilityAssessmentSettings/Write|De controle op beveiligingslekken voor een bepaalde database wijzigen|
|/servers/databases/Write|Maakt een database met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven database.|
|/servers/DELETE|Hiermee verwijdert u een bestaande server.|
|/servers/disasterRecoveryConfiguration/delete|Hiermee verwijdert u een bestaande noodherstelconfiguraties voor een bepaalde server|
|/servers/disasterRecoveryConfiguration/failover/Action|Failover een DisasterRecoveryConfiguration|
|/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action|Een DisasterRecoveryConfiguration een failover|
|/servers/disasterRecoveryConfiguration/read|Hiermee wordt een verzameling disaster recovery configuraties met deze server|
|/servers/disasterRecoveryConfiguration/write|Wijziging noodherstelconfiguratie op de server|
|/servers/elasticPoolEstimates/read|Retourneert de lijst van de elastische groep schattingen al is gemaakt voor deze server|
|/servers/elasticPoolEstimates/write|Hiermee maakt u nieuwe elastische pool schatting voor de lijst met databases die zijn opgegeven|
|/servers/elasticPools/advisors/read|Retourneert de lijst met adviseurs die beschikbaar zijn voor de elastische groep|
|/servers/elasticPools/advisors/recommendedActions/read|Retourneert de lijst met aanbevolen acties van opgegeven advisor voor de elastische groep|
|/servers/elasticPools/advisors/recommendedActions/write|De aanbevolen actie toepassen op de elastische groep|
|/servers/elasticPools/advisors/write|Update automatisch-execute status van een advisor op niveau van de elastische groep.|
|/servers/elasticPools/databases/read|Een lijst met databases ophalen voor een elastische pool|
|/servers/elasticPools/delete|Verwijderen van de huidige elastische pool|
|/servers/elasticPools/elasticPoolActivity/read|Ophalen van activiteiten en meer informatie over een bepaalde elastische databasegroep|
|/servers/elasticPools/elasticPoolDatabaseActivity/read|Ophalen van activiteiten en meer informatie over een bepaalde database die deel uitmaakt van een pool voor elastische database|
|/servers/elasticPools/metricDefinitions/read|Retourtypen van metrische gegevens die beschikbaar voor pools voor elastische databases zijn|
|/servers/elasticPools/metrics/Read|Retourneren van metrische gegevens voor pools voor elastische databases|
|/servers/elasticPools/operations/cancel/action|Azure SQL elastische pool in behandeling zijnde asynchrone bewerking die nog niet is voltooid, wordt geannuleerd.|
|/servers/elasticPools/operations/read|Retourneren van de lijst met bewerkingen die worden uitgevoerd op de elastische groep|
|/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read|Retourtypen van metrische gegevens die beschikbaar voor pools voor elastische databases zijn|
|/servers/elasticPools/read|Details van de elastische pool op een bepaalde server ophalen|
|/servers/elasticPools/skus/read|Hiermee wordt een verzameling van SKU's beschikbaar voor deze elastische groep|
|/servers/elasticPools/write|Maak een nieuwe eigenschappen of wijzigen van de huidige elastische pool|
|/servers/encryptionProtector/read|Retourneert een lijst met server versleuteling beveiligingen of haalt u de eigenschappen voor de opgegeven server versleuteling beveiliging.|
|/servers/encryptionProtector/write|De eigenschappen voor de opgegeven Server versleuteling beveiligingstoepassing bijwerken.|
|/servers/extendedAuditingSettings/read|Ophalen van gegevens van de uitgebreide server blob controlebeleid geconfigureerd op een bepaalde server|
|/servers/extendedAuditingSettings/write|De uitgebreide server blob-controle voor een bepaalde server wijzigen|
|/servers/failoverGroups/delete|Hiermee verwijdert u een bestaande groep voor failover.|
|/servers/failoverGroups/failover/Action|Geplande failover worden uitgevoerd in een bestaande groep voor failover.|
|/servers/failoverGroups/forceFailoverAllowDataLoss/action|Geforceerde failover worden uitgevoerd in een bestaande groep voor failover.|
|/servers/failoverGroups/Read|Retourneert de lijst met failover groepen of haalt u de eigenschappen voor de opgegeven failover-groep.|
|/servers/failoverGroups/Write|Hiermee maakt u een failover-groep met de opgegeven parameters of updates van de eigenschappen of labels voor de opgegeven failover-groep.|
|/servers/firewallRules/DELETE|Hiermee verwijdert u een bestaande firewallregel voor de server.|
|/servers/firewallRules/Read|De lijst met serverfirewall regels of haalt u de eigenschappen voor de opgegeven server firewallregel retourneren.|
|/servers/firewallRules/Write|Een serverfirewallregel maakt met de opgegeven parameters bijwerken van de eigenschappen voor de opgegeven regel of alle bestaande regels worden overschreven met de nieuwe server firewall (s).|
|/servers/import/Action|Een nieuwe database op de server maken en implementeren van schema en de gegevens uit een DacPac-pakket|
|/servers/importExportOperationResults/read|Bewerkingen voor opgehaald in uitvoering voor importeren/exporteren|
|/servers/Keys/DELETE|Hiermee verwijdert u een bestaande serversleutel.|
|/servers/Keys/Read|De lijst van server-codes of haalt u de eigenschappen voor de sleutel van de opgegeven server retourneren.|
|/servers/Keys/Write|Maakt een sleutel met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de sleutel van de opgegeven server.|
|/servers/operationResults/Read|Opgehaald in uitvoering serverbewerkingen|
|/servers/providers/Microsoft.Insights/metricDefinitions/read|Retourtypen van metrische gegevens die beschikbaar voor servers zijn|
|/servers/Read|Retourneert de lijst met servers of de eigenschappen voor de opgegeven server opgehaald.|
|/servers/recommendedElasticPools/databases/read|Metrische gegevens voor aanbevolen elastische databases voor een bepaalde server ophalen|
|/servers/recommendedElasticPools/read|Aanbevelingen voor pools voor elastische databases kosten verlagen of verbeteren de prestaties op basis van Resourcegebruik historica ophalen|
|/servers/recoverableDatabases/Read|Deze bewerking wordt gebruikt voor herstel na noodgevallen van live-database voor de database herstellen naar laatst bekende goede back-up. Deze retourneert informatie over de laatste goede back-up, maar doesn\u0027t daadwerkelijk de database herstellen.|
|/servers/restorableDroppedDatabases/read|Een lijst met databases die zijn verwijderd op een bepaalde server die binnen het bewaarbeleid ophalen.|
|/servers/securityAlertPolicies/operationResults/Read|Resultaten van de server threat detectie beleid write-bewerking ophalen|
|/servers/securityAlertPolicies/Read|Ophalen van gegevens van het beleid server threat detectie is geconfigureerd op een bepaalde server|
|/servers/securityAlertPolicies/write|Het beleid van server threat detectie voor een bepaalde server wijzigen|
|/servers/serviceObjectives/Read|Lijst met serviceniveaudoelstellingen (ook wel bekend als prestatielagen) beschikbaar is op een bepaalde server ophalen|
|/servers/syncAgents/delete|Hiermee verwijdert u een bestaande sync-agent.|
|/servers/syncAgents/generateKey/Action|Sync-agent registratie sleutel genereren|
|/servers/syncAgents/linkedDatabases/read|Retourneren van de lijst met databases synchroniseren-agent die zijn gekoppeld|
|/servers/syncAgents/Read|Retourneert de lijst met synchronisatie agents of haalt de eigenschappen voor de opgegeven synchronisatie-agent.|
|/servers/syncAgents/Write|Hiermee maakt u een synchronisatie-agent met de opgegeven parameters of bijwerken van de eigenschappen voor de opgegeven synchronisatie-agent.|
|/servers/usages/Read|DTU-quotum voor server en de huidige DTU consuption geretourneerd door alle databases in de server|
|/servers/virtualNetworkRules/delete|Hiermee verwijdert u een bestaande regel voor het virtuele netwerk|
|/servers/virtualNetworkRules/Read|De lijst van het virtuele netwerk regels of haalt u de eigenschappen voor de opgegeven virtuele netwerk regel retourneren.|
|/servers/virtualNetworkRules/Write|Een virtueel netwerk maken met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de regel voor de opgegeven virtuele netwerk.|
|servers/schrijven|Hiermee maakt u een server met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de opgegeven server.|
|/ unregister/actie|Heft de registratie van het abonnement voor de resourceprovider voor Microsoft SQL-Database en wordt het maken van Microsoft SQL-Databases.|
|/virtualClusters/Read|De lijst met virtuele clusters of haalt de eigenschappen voor de opgegeven virtuele cluster retourneren.|
|virtualClusters/schrijven|Virtueel cluster tags-updates.|

## <a name="microsoftstorage"></a>Microsoft.Storage

| Bewerking | Beschrijving |
|---|---|
|/checknameavailability/Read|Hiermee controleert u of een accountnaam geldig is en niet wordt gebruikt.|
|/locations/deleteVirtualNetworkOrSubnets/action|Hiermee wordt aan Microsoft.Storage doorgegeven dat het virtuele netwerk of subnet wordt verwijderd|
|/Operations/Read|Hiermee wordt de status van een asynchrone bewerking gepeild.|
|registratie-/ actie|Hiermee wordt het abonnement voor de opslagresourceprovider geregistreerd en wordt het maken van opslagaccounts mogelijk.|
|/skus/Read|Hiermee wordt een lijst weergegeven van de SKU's die worden ondersteund door Microsoft.Storage.|
|/storageAccounts/blobServices/containers/clearLegalHold/action|Juridische bewaring van blobcontainer wissen|
|/storageAccounts/blobServices/containers/delete|Hiermee wordt het resultaat van het verwijderen van een container geretourneerd|
|/storageAccounts/blobServices/containers/immutabilityPolicies/delete|Beleid voor onveranderbaarheid van blobcontainer verwijderen|
|/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action|Beleid voor onveranderbaarheid van blobcontainer uitbreiden|
|/storageAccounts/blobServices/containers/immutabilityPolicies/LOCK/Action|Beleid voor onveranderbaarheid van blobcontainer vergrendelen|
|/storageAccounts/blobServices/containers/immutabilityPolicies/Read|Beleid voor onveranderbaarheid van blobcontainer ophalen|
|/storageAccounts/blobServices/containers/immutabilityPolicies/write|Beleid voor onveranderbaarheid van blobcontainer plaatsen|
|/storageAccounts/blobServices/containers/Read|Hiermee wordt een container of een lijst containers geretourneerd|
|/storageAccounts/blobServices/containers/setLegalHold/action|Juridische bewaring voor blobcontainer instellen|
|/storageAccounts/blobServices/containers/write|Hiermee wordt het resultaat van het plaatsen of in lease geven van de blobcontainer geretourneerd|
|/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald.|
|/storageAccounts/blobServices/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt.|
|/storageAccounts/blobServices/providers/Microsoft.Insights/metricDefinitions/read|Vraag een lijst met definities voor metrische Microsoft-opslaggegevens op.|
|/storageAccounts/blobServices/Read|Hiermee worden de eigenschappen van de Blob service of statistieken geretourneerd|
|/storageAccounts/blobServices/write|Hiermee wordt het resultaat van de eigenschappen voor het plaatsen van de blobcontainer geretourneerd|
|/storageAccounts/delete|Hiermee verwijdert u een bestaand opslagaccount.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt.|
|/storageAccounts/fileServices/providers/Microsoft.Insights/metricDefinitions/Read|Vraag een lijst met definities voor metrische Microsoft-opslaggegevens op.|
|/storageAccounts/listAccountSas/action|Hiermee wordt het SAS-token van het account geretourneerd voor het opgegeven opslagaccount.|
|/storageAccounts/listkeys/Action|Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount.|
|/storageAccounts/listServiceSas/action|Hiermee wordt het SAS-token van de service geretourneerd voor het opgegeven opslagaccount.|
|/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald.|
|/storageAccounts/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt.|
|/storageAccounts/providers/Microsoft.Insights/metricDefinitions/Read|Vraag een lijst met definities voor metrische Microsoft-opslaggegevens op.|
|/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt.|
|/storageAccounts/queueServices/providers/Microsoft.Insights/metricDefinitions/read|Vraag een lijst met definities voor metrische Microsoft-opslaggegevens op.|
|/storageAccounts/queueServices/queues/delete|Hiermee wordt het resultaat van een wachtrij verwijderen geretourneerd|
|/storageAccounts/queueServices/Queues/Read|Hiermee wordt een wachtrij of een lijst met wachtrijen geretourneerd.|
|/storageAccounts/queueServices/queues/write|Hiermee wordt het resultaat van het schrijven van een wachtrij geretourneerd|
|/storageAccounts/queueServices/Read|Hiermee worden de eigenschappen of statistieken van de Queue-service geretourneerd.|
|/storageAccounts/queueServices/write|Hiermee wordt het resultaat van het instellen van de eigenschappen van de Queue-service geretourneerd|
|/storageAccounts/Read|Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount.|
|/storageAccounts/regeneratekey/Action|Hiermee genereert u de toegangssleutels voor het opgegeven opslagaccount opnieuw.|
|/storageAccounts/Services/diagnosticSettings/Write|Diagnostische instellingen van opslagaccount maken/bijwerken.|
|/ storageAccounts/storageAccounts/queueServices/providers/Microsoft.Insights/diagnosticSettings/read|Hiermee wordt de diagnostische instelling voor de resource opgehaald.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt.|
|/storageAccounts/tableServices/providers/Microsoft.Insights/metricDefinitions/Read|Vraag een lijst met definities voor metrische Microsoft-opslaggegevens op.|
|/storageAccounts/write|Hiermee maakt u een opslagaccount met de opgegeven parameters, werkt u de eigenschappen of labels bij of voegt u een aangepast domein toe aan het opgegeven opslagaccount.|
|/usages/Read|Retourneert de limiet en het huidige gebruik voor resources in het opgegeven abonnement|

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

| Bewerking | Beschrijving |
|---|---|
|/storageSyncServices/delete|Verwijderen van alle opslag-Synchronisatieservices|
|/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read|Hiermee wordt de beschikbare metrische gegevens voor synchronisatie van opslagservices|
|/storageSyncServices/Read|Lezen van alle opslag-Synchronisatieservices|
|/storageSyncServices/registeredServers/delete|Verwijder alle geregistreerde Server|
|/storageSyncServices/registeredServers/Read|Lees alle geregistreerde Server|
|/storageSyncServices/registeredServers/Write|Maken of bijwerken van een geregistreerde Server|
|/storageSyncServices/syncGroups/cloudEndpoints/delete|Geen eindpunten Cloud verwijderen|
|/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read|Api voor back-aanroepen van asynchrone locatie|
|/storageSyncServices/syncGroups/cloudEndpoints/postbackup/Action|Aanroepen van deze actie na back-up|
|/storageSyncServices/syncGroups/cloudEndpoints/PostRestore/Action|Deze actie na het terugzetten van aanroepen|
|/storageSyncServices/syncGroups/cloudEndpoints/prebackup/Action|Aanroepen van deze actie voor back-up|
|/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action|Aanroepen van deze actie vóór herstellen|
|/storageSyncServices/syncGroups/cloudEndpoints/read|Lezen van elke Cloud-eindpunten|
|/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/Action|Heartbeat herstellen|
|/storageSyncServices/syncGroups/cloudEndpoints/write|Maken of bijwerken van elke Cloud-eindpunten|
|/storageSyncServices/syncGroups/delete|Synchronisatiegroepen verwijderen|
|/storageSyncServices/syncGroups/Read|Alle synchronisatiegroepen lezen|
|/storageSyncServices/syncGroups/serverEndpoints/delete|Geen eindpunten Server verwijderen|
|/storageSyncServices/syncGroups/serverEndpoints/Read|Lezen van elke Server-eindpunten|
|/storageSyncServices/syncGroups/serverEndpoints/recallAction/action|Aanroepen van deze actie om in te trekken van de bestanden naar een server|
|/storageSyncServices/syncGroups/serverEndpoints/write|Maken of bijwerken van elke Server-eindpunten|
|/storageSyncServices/syncGroups/write|Een synchronisatiegroepen maken of bijwerken|
|/storageSyncServices/write|Alle opslag Synchronisatieservices maken of bijwerken|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| Bewerking | Beschrijving |
|---|---|
|/managers/accessControlRecords/delete|De Access Control Records verwijderd|
|/managers/accessControlRecords/Read|Geeft een lijst of de Access Control Records opgehaald|
|/managers/accessControlRecords/Write|De Access Control Records maken of bijwerken|
|/managers/Alerts/Read|Geeft een lijst of de waarschuwingen opgehaald|
|/managers/bandwidthSettings/delete|Hiermee verwijdert u een bestaande bandbreedte-instellingen (alleen 8000-serie)|
|/managers/bandwidthSettings/Read|Lijst van de bandbreedte-instellingen (alleen 8000 serie)|
|/managers/bandwidthSettings/Write|Een nieuwe maken of bijwerken van de bandbreedte-instellingen (alleen 8000-serie)|
|Managers/certificaten/schrijftijd|De bewerking Update Resource certificaat updates het referentiecertificaat van de resource/kluis.|
|/managers/clearAlerts/Action|Schakel alle waarschuwingen die zijn gekoppeld aan de Apparaatbeheer.|
|/managers/cloudApplianceConfigurations/Read|Lijst met het toestel Cloud ondersteunde configuraties|
|/managers/configureDevice/Action|Hiermee configureert u een apparaat|
|/managers/DELETE|Hiermee verwijdert u de Managers voor apparaatregistratie|
|/ Managers/verwijderen|De bewerking verwijderen kluis verwijdert de opgegeven Azure-resource van het type 'kluis'|
|/managers/Devices/alertSettings/Read|Geeft een lijst of opgehaald van de instellingen voor waarschuwingen|
|/managers/Devices/alertSettings/Write|Maken of bijwerken van de instellingen voor waarschuwingen|
|/managers/Devices/backupPolicies/Backup/Action|Een handmatige back-up op aanvraag maken back-up van alle volumes die zijn beveiligd door het beleid te nemen.|
|/managers/Devices/backupPolicies/DELETE|Hiermee verwijdert u een beleid in een bestaande back-up (alleen 8000-serie)|
|/managers/Devices/backupPolicies/Read|Lijst met de back-up beleidsregels (alleen 8000 serie)|
|/managers/Devices/backupPolicies/Schedules/DELETE|Hiermee verwijdert u een bestaande planningen|
|/managers/Devices/backupPolicies/Schedules/Read|Lijst van de planning|
|/managers/Devices/backupPolicies/Schedules/Write|Maakt een nieuw of bijgewerkt schema 's|
|/managers/Devices/backupPolicies/Write|Maakt een nieuw of bijgewerkt beleid van de back-up (alleen 8000-serie)|
|/managers/Devices/backups/DELETE|Hiermee verwijdert u de back-upset|
|/managers/Devices/backups/Elements/Clone/Action|Klonen van een share of het volume met behulp van een back-element.|
|/managers/Devices/backups/Read|Geeft een lijst of back-upset opgehaald|
|/managers/Devices/backups/Restore/Action|Alle volumes terugzetten vanaf een back-upset.|
|/managers/Devices/backupScheduleGroups/DELETE|Hiermee verwijdert u de back-upschema-groepen|
|/managers/Devices/backupScheduleGroups/Read|Geeft een lijst of de back-up schemagroepen opgehaald|
|/managers/Devices/backupScheduleGroups/Write|De back-up schemagroepen maken of bijwerken|
|/managers/Devices/chapSettings/DELETE|Hiermee verwijdert u de Chap-instellingen|
|/managers/Devices/chapSettings/Read|Geeft een lijst of ontvangt de Chap-instellingen|
|/managers/Devices/chapSettings/Write|De Chap-instellingen maken of bijwerken|
|/managers/Devices/Deactivate/Action|Een apparaat wordt gedeactiveerd.|
|/managers/Devices/DELETE|Hiermee verwijdert u de apparaten|
|/managers/Devices/Download/Action|Download updates voor een apparaat.|
|/managers/Devices/failover/Action|Failover van het apparaat.|
|/managers/Devices/fileservers/Backup/Action|Duren back-up van een bestandsserver.|
|/managers/Devices/fileservers/DELETE|Hiermee verwijdert u de bestandsservers|
|/managers/Devices/fileservers/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/Devices/fileservers/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/Devices/fileservers/Read|Geeft een lijst of opgehaald van de bestandsservers|
|/managers/Devices/fileservers/shares/DELETE|Hiermee verwijdert u de Shares|
|/managers/Devices/fileservers/shares/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/Devices/fileservers/shares/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/Devices/fileservers/shares/Read|Geeft een lijst of de Shares opgehaald|
|/managers/Devices/fileservers/shares/Write|Maken of bijwerken van de Shares|
|/managers/Devices/fileservers/Write|Maken of bijwerken van de bestandsservers|
|/managers/devices/hardwareComponentGroups/changeControllerPowerState/action|Controller voedingsstatus van hardware-onderdeelgroepen wijzigen|
|/managers/Devices/hardwareComponentGroups/Read|Lijst van de Hardware-onderdeelgroepen|
|/managers/Devices/Install/Action|Updates installeren op een apparaat.|
|/managers/Devices/installUpdates/Action|Updates zijn geïnstalleerd op de apparaten|
|/managers/Devices/iscsiservers/Backup/Action|Duren back-up van een iSCSI-server.|
|/managers/Devices/iscsiservers/DELETE|Hiermee verwijdert u de iSCSI-Servers|
|/managers/Devices/iscsiservers/Disks/DELETE|Hiermee verwijdert u de schijven|
|/managers/Devices/iscsiservers/Disks/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/Devices/iscsiservers/Disks/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/Devices/iscsiservers/Disks/Read|Geeft een lijst of de schijven opgehaald|
|/managers/Devices/iscsiservers/Disks/Write|Maken of bijwerken van de schijven|
|/managers/Devices/iscsiservers/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/Devices/iscsiservers/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/Devices/iscsiservers/Read|Geeft een lijst of opgehaald van de iSCSI-Servers|
|/managers/Devices/iscsiservers/Write|Maken of bijwerken van de iSCSI-Servers|
|/managers/Devices/Jobs/Cancel/Action|Een actieve taak annuleren|
|/managers/Devices/Jobs/Read|Geeft een lijst of de taken opgehaald|
|/managers/Devices/listFailoverSets/Action|Hiermee stelt u de failover voor een bestaand apparaat een lijst.|
|/managers/Devices/listFailoverTargets/Action|Doelen voor de failover van de apparaten|
|/managers/Devices/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/Devices/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/Devices/migrationSourceConfigurations/confirmMigration/Action|Bevestigt een succesvolle migratie en het doorvoeren.|
|/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action|Haal de status van het bevestigen van de migratie.|
|/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action|De status van de migratietaak schatting ophalen.|
|/managers/Devices/migrationSourceConfigurations/fetchMigrationStatus/Action|Haal de status van de migratie.|
|/managers/Devices/migrationSourceConfigurations/import/Action|Importeren van de bron-configuraties voor migratie|
|/managers/Devices/migrationSourceConfigurations/startMigration/Action|Begint met de migratie met behulp van de bron-configuraties|
|/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action|Een taak voor een schatting van de duur van het migratieproces start.|
|/managers/Devices/networkSettings/Read|Geeft een lijst of de netwerkinstellingen opgehaald|
|/managers/Devices/networkSettings/Write|Een nieuwe maken of bijwerken van netwerkinstellingen|
|/managers/devices/publicEncryptionKey/action|Openbare versleutelingssleutel lijst van Apparaatbeheer|
|/managers/Devices/publishSupportPackage/Action|Publiceer ondersteuningspakket van een apparaat voor het oplossen van Microsoft Support.|
|/managers/Devices/Read|Geeft een lijst of de apparaten opgehaald|
|/managers/Devices/scanForUpdates/Action|Zoeken naar updates in een apparaat.|
|/managers/Devices/securitySettings/Read|Lijst van de beveiligingsinstellingen|
|/managers/devices/securitySettings/syncRemoteManagementCertificate/action|Het certificaat voor extern beheer voor een apparaat worden gesynchroniseerd.|
|/managers/Devices/securitySettings/update/Action|Werk de beveiligingsinstellingen.|
|/managers/Devices/securitySettings/Write|Maakt een nieuw of bijgewerkt beveiligingsinstellingen|
|/managers/Devices/sendTestAlertEmail/Action|Waarschuwing testbericht verzenden naar geconfigureerde e-mailontvangers.|
|/managers/Devices/timeSettings/Read|Geeft een lijst of de instellingen opgehaald|
|/managers/Devices/timeSettings/Write|Maakt een nieuw of bijgewerkt tijdinstellingen|
|/managers/Devices/updateSummary/Read|Geeft een lijst of een overzicht van de Update wordt opgehaald|
|/managers/Devices/volumeContainers/DELETE|Hiermee verwijdert u een bestaande Volumecontainers (alleen 8000-serie)|
|/managers/devices/volumeContainers/listEncryptionKeys/action|De versleutelingssleutels lijst van Volumecontainers|
|/managers/Devices/volumeContainers/metrics/Read|Lijst van de metrische gegevens|
|/managers/Devices/volumeContainers/metricsDefinitions/Read|De definities van de metrische gegevens weergeven|
|/managers/Devices/volumeContainers/Read|Lijst van de Volumecontainers (alleen 8000 serie)|
|/managers/devices/volumeContainers/rolloverEncryptionKey/action|De versleutelingssleutels rollover van Volumecontainers|
|/managers/Devices/volumeContainers/volumes/DELETE|Hiermee verwijdert u een bestaande Volumes|
|/managers/Devices/volumeContainers/volumes/metrics/Read|Lijst van de metrische gegevens|
|/managers/Devices/volumeContainers/volumes/metricsDefinitions/Read|De definities van de metrische gegevens weergeven|
|/managers/Devices/volumeContainers/volumes/Read|Lijst van de Volumes|
|/managers/Devices/volumeContainers/volumes/Write|Een nieuwe maken of bijwerken van Volumes|
|/managers/Devices/volumeContainers/Write|Maakt een nieuw of bijgewerkt Volumecontainers (alleen 8000-serie)|
|/managers/Devices/Write|Maken of bijwerken van de apparaten|
|/managers/encryptionSettings/Read|Geeft een lijst of de instellingen voor codering opgehaald|
|Managers, extendedInformation/verwijderen|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|Managers/extendedInformation/leestijd|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|Managers/extendedInformation/schrijftijd|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|/managers/getActivationKey/Action|Activeringscode voor Apparaatbeheer niet ophalen.|
|/managers/getEncryptionKey/Action|Download coderingssleutel voor Apparaatbeheer.|
|/managers/listActivationKey/Action|Hiermee haalt u de activeringssleutel van de StorSimple-Apparaatbeheer.|
|/managers/listPrivateEncryptionKey/action|Persoonlijke sleutel voor een StorSimple-Apparaatbeheer opgehaald.|
|/managers/listPublicEncryptionKey/action|Lijst met codering met openbare sleutels van een StorSimple-Apparaatbeheer.|
|/managers/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/provisionCloudAppliance/Action|Maak een nieuw toestel in de cloud.|
|/managers/Read|Geeft een lijst of de apparaat-Managers opgehaald|
|Managers/leestijd|De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald|
|/managers/regenarateRegistationCertificate/Action|Registratiecertificaat opnieuw genereren voor de managers voor apparaatregistratie.|
|/managers/regenerateActivationKey/Action|Activeringscode genereren voor Apparaatbeheer.|
|/managers/storageAccountCredentials/delete|Hiermee verwijdert u de Opslagaccountreferenties|
|/managers/storageAccountCredentials/listAccessKey/action|Toegangstoetsen lijst van Opslagaccountreferenties|
|/managers/storageAccountCredentials/read|Geeft een lijst of de Opslagaccountreferenties opgehaald|
|/managers/storageAccountCredentials/write|Maken of bijwerken van de Opslagaccountreferenties|
|/managers/storageDomains/delete|Hiermee verwijdert u de opslag-domeinen|
|/managers/storageDomains/Read|Geeft een lijst of de opslag domeinen opgehaald|
|/managers/storageDomains/write|Maken of bijwerken van de opslag-domeinen|
|managers/schrijven|Maken of bijwerken van de Managers voor apparaatregistratie|
|Managers/schrijftijd|Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt.|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| Bewerking | Beschrijving |
|---|---|
|/Locations/Quotas/Read|Quotum lezen Stream Analytics-abonnement|
|/ operations/lezen|Lees Stream Analytics-bewerkingen|
|/ Registreren/actie|Abonnement met Stream Analytics Resource Provider registreren|
|/streamingjobs/Delete|Verwijderen van de Stream Analytics-taak|
|/streamingjobs/Functions/DELETE|Verwijderen van de Stream Analytics-functie|
|/streamingjobs/Functions/operationresults/Read|Resultaat van de bewerking voor de functie voor Stream Analytics-taak lezen|
|/streamingjobs/Functions/Read|Functie Read Stream Analytics-taak|
|/streamingjobs/Functions/RetrieveDefaultDefinition/Action|Default-definitie van een functie van Stream Analytics-taak ophalen|
|/streamingjobs/Functions/test/Action|Test Stream Analytics-functie|
|/streamingjobs/Functions/Write|Stream Analytics-taakfunctie schrijven|
|/streamingjobs/inputs/Delete|Stream Analytics-taak invoer verwijderen|
|/streamingjobs/inputs/operationresults/Read|Resultaat van de bewerking voor invoer van Stream Analytics-taak lezen|
|/streamingjobs/inputs/Read|Invoer lezen Stream Analytics-taak|
|/streamingjobs/inputs/sample/Action|Voorbeeldinvoer Stream Analytics-taak|
|/streamingjobs/inputs/test/Action|Test Stream Analytics-taak invoer|
|/streamingjobs/inputs/Write|Stream Analytics-taak invoer schrijven|
|/streamingjobs/metricdefinitions/Read|Metrische definities lezen|
|/streamingjobs/operationresults/Read|Resultaat van de bewerking voor Stream Analytics-taak lezen|
|/streamingjobs/outputs/DELETE|Stream Analytics-Taakuitvoer verwijderen|
|/streamingjobs/outputs/operationresults/Read|Resultaat van de bewerking voor Stream Analytics-Taakuitvoer lezen|
|/streamingjobs/outputs/Read|Lees Stream Analytics-Taakuitvoer|
|/streamingjobs/outputs/test/Action|Testuitvoer Stream Analytics-taak|
|/streamingjobs/outputs/Write|Stream Analytics-Taakuitvoer schrijven|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read|Lees de diagnostische instelling.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write|Diagnostische instelling schrijven.|
|/streamingjobs/providers/Microsoft.Insights/logDefinitions/read|De beschikbare logboeken opgehaald voor streamingjobs|
|/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read|Hiermee wordt de beschikbare metrische gegevens voor streamingjobs|
|/ streamingjobs/lezen|Stream Analytics-taak lezen|
|/streamingjobs/start/Action|Stream Analytics-taak starten|
|/streamingjobs/Stop/Action|Stream Analytics-taak stoppen|
|/streamingjobs/Transformations/DELETE|Stream Analytics-taak transformatie verwijderen|
|/streamingjobs/Transformations/Read|Transformatie lezen Stream Analytics-taak|
|/streamingjobs/Transformations/Write|Stream Analytics-taak transformatie schrijven|
|/ streamingjobs/schrijven|Schrijven van Stream Analytics-taak|

## <a name="microsoftsubscription"></a>Microsoft.Subscription

| Bewerking | Beschrijving |
|---|---|
|SubscriptionDefinitions/leestijd|De definitie van een Azure-abonnement in een beheergroep worden opgehaald.|
|/ SubscriptionDefinitions/schrijven|De definitie van een Azure-abonnement maken|

## <a name="microsoftsupport"></a>Microsoft.Support

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Hiermee wordt het item geregistreerd bij de ondersteuningsresourceprovider|
|/supportTickets/Read|Hiermee worden gegevens over het ondersteuningsticket opgehaald (inclusief status, ernst, contactgegevens en communicatie) of wordt de lijst met ondersteuningstickets voor de verschillende abonnementen opgehaald.|
|supportTickets/schrijven|Maken of bijwerken van een Ondersteuningsticket. U kunt een Ondersteuningsticket maken voor technische, facturerings, quota's of beheer van abonnementen problemen met betrekking tot. U kunt de ernst, contactgegevens en communicatie voor bestaande ondersteuningstickets bijwerken.|

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

| Bewerking | Beschrijving |
|---|---|
|/Environments/accesspolicies/DELETE|Hiermee verwijdert u het toegangsbeleid.|
|/Environments/accesspolicies/Read|De eigenschappen van een toegangsbeleid opgehaald.|
|/Environments/accesspolicies/Write|Maakt u een nieuw toegangsbeleid voor een omgeving of updates van een bestaande toegangsbeleid.|
|/Environments/DELETE|Hiermee verwijdert u de omgeving.|
|/Environments/eventsources/DELETE|Hiermee verwijdert u de gegevensbron.|
|/Environments/eventsources/eventsources/providers/Microsoft.Insights/ diagnosticSettings/schrijven|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/Environments/eventsources/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/Environments/eventsources/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor eventsources|
|/Environments/eventsources/Read|De eigenschappen van een gebeurtenisbron opgehaald.|
|/Environments/eventsources/Write|Maakt een nieuwe gebeurtenisbron voor een omgeving, of een bestaande gebeurtenisbron bijgewerkt.|
|/Environments/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/Environments/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/Environments/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor omgevingen|
|/Environments/Read|De eigenschappen van een omgeving opgehaald.|
|/Environments/referencedatasets/DELETE|Hiermee verwijdert u de referentie-gegevensset.|
|/Environments/referencedatasets/Read|De eigenschappen van een gegevensset verwijzing opgehaald.|
|/Environments/referencedatasets/Write|Hiermee maakt u een nieuwe verwijzing gegevens voor een omgeving of updates van een bestaande referentie-gegevensset.|
|/Environments/status/Read|De status van de status van de gekoppelde bewerkingen zoals ingress-omgeving niet ophalen.|
|omgevingen/schrijven|Maakt u een nieuwe omgeving of updates van een bestaande omgeving.|
|registratie-/ actie|Het abonnement voor de Time Series Insights-resourceprovider geregistreerd en wordt het maken van tijd reeks Insights omgevingen.|

## <a name="microsoftweb"></a>microsoft.web

| Bewerking | Beschrijving |
|---|---|
|/apimanagementaccounts/apiacls/Read|Api Management Accounts Apiacls ophalen.|
|/apimanagementaccounts/APIs/apiacls/DELETE|Api Management Accounts API's Apiacls verwijderen.|
|/apimanagementaccounts/APIs/apiacls/Read|Api Management Accounts API's Apiacls ophalen.|
|/apimanagementaccounts/APIs/apiacls/Write|Api Management Accounts API's Apiacls bijwerken.|
|/apimanagementaccounts/APIs/connectionacls/Read|Api Management Accounts API's Connectionacls ophalen.|
|/apimanagementaccounts/APIs/Connections/confirmconsentcode/Action|Controleer of u toestemming Code Api Management Accounts API's verbindingen.|
|/apimanagementaccounts/APIs/Connections/connectionacls/DELETE|Api Management Accounts API's verbindingen Connectionacls verwijderen.|
|/apimanagementaccounts/APIs/Connections/connectionacls/Read|Api Management Accounts API's verbindingen Connectionacls ophalen.|
|/apimanagementaccounts/APIs/Connections/connectionacls/Write|Api Management Accounts API's verbindingen Connectionacls bijwerken.|
|/apimanagementaccounts/APIs/Connections/DELETE|Api Management Accounts API's verbindingen verwijderen.|
|/apimanagementaccounts/APIs/Connections/getconsentlinks/Action|Toestemming koppelingen voor verbindingen van Api Management Accounts API's niet ophalen.|
|/apimanagementaccounts/APIs/Connections/listconnectionkeys/Action|Lijst verbinding sleutels Api Management Accounts API's verbindingen.|
|/apimanagementaccounts/APIs/Connections/listsecrets/Action|Lijst met geheimen Api Management Accounts API's verbindingen.|
|/apimanagementaccounts/APIs/Connections/Read|Api Management Accounts API's verbindingen ophalen.|
|/apimanagementaccounts/APIs/Connections/Write|Api Management Accounts API's verbindingen worden bijgewerkt.|
|/apimanagementaccounts/APIs/DELETE|Api Management-API's Accounts verwijderen.|
|/apimanagementaccounts/APIs/localizeddefinitions/DELETE|Verwijderen van Api Management-API's Accounts gelokaliseerd definities.|
|/apimanagementaccounts/APIs/localizeddefinitions/Read|Ophalen van Api Management-API's Accounts gelokaliseerd definities.|
|/apimanagementaccounts/APIs/localizeddefinitions/Write|Update Api Management Accounts-API's gelokaliseerd definities.|
|/apimanagementaccounts/APIs/Read|Api Management-API's Accounts worden opgehaald.|
|/apimanagementaccounts/APIs/Write|Api Management-API's Accounts bijwerken.|
|/apimanagementaccounts/connectionacls/Read|Api Management Accounts Connectionacls ophalen.|
|/availablestacks/Read|Beschikbare Stacks ophalen.|
|/billingmeters/Read|Lijst met meters facturering ophalen.|
|certificaten/verwijderen|Een bestaand certificaat verwijderen.|
|certificaten/lezen|Haal de lijst van certificaten.|
|certificaten/schrijven|Voeg een nieuw certificaat of een bestaande bijgewerkt.|
|/checknameavailability/Read|Controleer of de resourcenaam van de beschikbaar is.|
|/classicmobileservices/Read|Klassieke mobiele Services krijgen.|
|/ connectionGateways/verwijderen|Hiermee verwijdert u een Gateway verbinding.|
|/connectionGateways/join/Action|Een Gateway verbinding koppelt.|
|/connectiongateways/liststatus/Action|Lijst met Status verbinding Gateways.|
|/connectionGateways/ListStatus/Action|Hiermee wordt de status van een Gateway-verbinding.|
|/connectionGateways/Move/Action|Hiermee verplaatst u een Gateway verbinding.|
|/ connectionGateways/lezen|De lijst van Gateways verbinding ophalen.|
|/ connectionGateways/schrijven|Maken of bijwerken van een Gateway-verbinding.|
|/Connections/confirmconsentcode/Action|Controleer de verbindingen toestemming Code.|
|/ verbindingen/verwijderen|Hiermee verwijdert u een verbinding.|
|/Connections/join/Action|Lid van een verbinding.|
|/Connections/listconsentlinks/Action|Lijst met toestemming koppelingen voor verbindingen.|
|/Connections/Move/Action|Hiermee verplaatst u een verbinding.|
|/ verbindingen/lezen|De lijst met verbindingen opgehaald.|
|/ verbindingen/schrijven|Maken of bijwerken van een verbinding.|
|/customApis/Delete|Hiermee verwijdert u een aangepaste API gebruiken.|
|/customApis/extractApiDefinitionFromWsdl/Action|Haalt de API-definitie van een WSDL.|
|/customApis/join/Action|Een aangepaste API koppelt.|
|/customApis/listWsdlInterfaces/Action|Geeft een lijst van WSDL-interfaces voor een aangepaste API.|
|/customApis/Move/Action|Hiermee verplaatst u een aangepaste API gebruiken.|
|/ customApis/lezen|De lijst met aangepaste API ophalen.|
|/ customApis/schrijven|Maken of bijwerken van een aangepaste API.|
|/deploymentlocations/Read|Implementatie locaties ophalen.|
|/ geoRegions/lezen|De lijst met Geo regions ophalen.|
|/hostingenvironments/capacities/Read|Hosting-omgevingen capaciteiten ophalen.|
|/ hostingEnvironments/verwijderen|Een App-serviceomgeving verwijderen|
|/hostingenvironments/Diagnostics/Read|Hosting-omgevingen diagnostische gegevens ophalen.|
|/hostingenvironments/inboundnetworkdependenciesendpoints/Read|De netwerkeindpunten van alle inkomende afhankelijkheden ophalen.|
|/hostingenvironments/metricdefinitions/Read|Hosting-omgevingen metrische definities ophalen.|
|/hostingenvironments/multirolepools/metricdefinitions/Read|Hosting-omgevingen MultiRole Pools metrische definities ophalen.|
|/hostingenvironments/multirolepools/metrics/Read|Hosting-omgevingen MultiRole Pools metrische gegevens ophalen.|
|/hostingEnvironments/multiRolePools/providers/Microsoft.Insights/ metricDefinitions leestijd|Hiermee worden de beschikbare metrische gegevens voor App Service-omgeving MultiRole opgehaald|
|/hostingEnvironments/multiRolePools/Read|De eigenschappen van een FrontEnd-toepassingen in een App-serviceomgeving opgehaald|
|/hostingenvironments/multirolepools/skus/Read|Hosting-omgevingen MultiRole Pools SKU's ophalen.|
|/hostingenvironments/multirolepools/usages/Read|Hosting-omgevingen MultiRole Pools gebruik ophalen.|
|/hostingEnvironments/multiRolePools/Write|Een nieuwe FrontEnd-toepassingen in een App Service-omgeving maken of bijwerken van een bestaande|
|/hostingenvironments/Operations/Read|Hosting-omgevingen bewerkingen ophalen.|
|/hostingenvironments/outboundnetworkdependenciesendpoints/Read|De netwerkeindpunten van alle uitgaande afhankelijkheden ophalen.|
|/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/hostingenvironments/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/ hostingEnvironments/lezen|De eigenschappen van een App-serviceomgeving opgehaald|
|/hostingEnvironments/reboot/Action|Start opnieuw op alle machines in een App-serviceomgeving|
|/hostingenvironments/Resume/Action|Hervat de hostomgevingen.|
|/hostingenvironments/serverfarms/Read|Hosting-omgevingen App Service-abonnementen worden opgehaald.|
|/hostingenvironments/sites/Read|Hosting-omgevingen Web-Apps worden opgehaald.|
|/hostingenvironments/Suspend/Action|Onderbreken hostomgevingen.|
|/hostingenvironments/usages/Read|Hosting-omgevingen gebruik ophalen.|
|/hostingenvironments/workerpools/metricdefinitions/Read|Hosting-omgevingen Workerpools metriek definities ophalen.|
|/hostingenvironments/workerpools/metrics/Read|Hosting-omgevingen Workerpools metrische gegevens ophalen.|
|/hostingEnvironments/workerPools/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee worden de beschikbare metrische gegevens voor App Service-omgeving WorkerPool opgehaald|
|/hostingEnvironments/workerPools/Read|De eigenschappen van een werknemersgroep in een App-serviceomgeving opgehaald|
|/hostingenvironments/workerpools/skus/Read|Hosting-omgevingen Workerpools SKU's ophalen.|
|/hostingenvironments/workerpools/usages/Read|Hosting-omgevingen Workerpools gebruik ophalen.|
|/hostingEnvironments/workerPools/Write|Een nieuwe Worker-groep in een App Service-omgeving maken of bijwerken van een bestaande|
|/ hostingEnvironments/schrijven|Een nieuwe App Service-omgeving maken of bijwerken van bestaande|
|/ishostingenvironmentnameavailable/Read|Get als de naam van de Hosting-omgeving beschikbaar is.|
|/ishostnameavailable/Read|Controleer of de hostnaam beschikbaar is.|
|/isusernameavailable/Read|Controleer of de gebruikersnaam beschikbaar is.|
|/listSitesAssignedToHostName/Read|Namen van sites die zijn toegewezen aan de hostnaam ophalen.|
|/Locations/apioperations/Read|Locaties API-bewerkingen ophalen.|
|/locations/connectiongatewayinstallations/read|Locaties verbinding Gateway installaties worden opgehaald.|
|/Locations/extractapidefinitionfromwsdl/Action|Pak de Api-definitie van WSDL voor locaties.|
|/Locations/listwsdlinterfaces/Action|Lijst met WSDL-Interfaces voor locaties.|
|/Locations/managedapis/apioperations/Read|Locaties Managed API-bewerkingen ophalen.|
|/Locations/managedapis/join/Action|Een beheerde API koppelt.|
|/Locations/managedapis/Read|Locaties beheerde API's worden opgehaald.|
|/Operations/Read|Bewerkingen ophalen.|
|/publishingusers/Read|Publiceren van gebruikers ophalen.|
|publishingusers/schrijven|Bijwerken van de gebruikers te publiceren.|
|/ aanbevelingen/lezen|De lijst met aanbevelingen voor abonnementen ophalen.|
|registratie-/ actie|De registerbronprovider Microsoft.Web voor het abonnement.|
|/resourcehealthmetadata/Read|Bron Health-metagegevens worden opgehaald.|
|/serverfarms/Capabilities/Read|Mogelijkheden van App Service-abonnementen ophalen.|
|/ serverfarms/verwijderen|Verwijder een bestaande App Service-Plan|
|/serverfarms/firstpartyapps/settings/delete|App Service-abonnementen eerste partijen Apps instellingen verwijderd.|
|/serverfarms/firstpartyapps/Settings/Read|App Service-abonnementen eerste partijen Apps-instellingen ophalen.|
|/serverfarms/firstpartyapps/Settings/Write|App Service-abonnementen eerste partijen Apps instellingen worden bijgewerkt.|
|/serverfarms/hybridconnectionnamespaces/relays/DELETE|App Service-abonnementen hybride verbinding naamruimten Relays verwijderen.|
|/serverfarms/hybridconnectionnamespaces/relays/Read|App Service-abonnementen hybride verbinding naamruimten Relays ophalen.|
|/serverfarms/hybridconnectionnamespaces/relays/sites/Read|App Service-plannen voor hybride verbinding naamruimten Relays WebApps ophalen.|
|/serverfarms/hybridconnectionplanlimits/Read|Ophalen van App Service-abonnementen hybride verbindingslimieten-Plan.|
|/serverfarms/hybridconnectionrelays/read|App Service-abonnementen hybride verbinding Relays ophalen.|
|/serverfarms/metricdefinitions/Read|App Service-abonnementen metrische definities worden opgehaald.|
|/serverfarms/metrics/Read|App Service-abonnementen metrische gegevens worden opgehaald.|
|/serverfarms/operationresults/Read|App Service-abonnementen Bewerkingsresultaten krijgt.|
|/serverfarms/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/serverfarms/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/serverfarms/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor het App Service-Plan|
|/ serverfarms/lezen|Opvragen van de eigenschappen voor een App Service-Plan|
|/serverfarms/restartSites/Action|Start opnieuw op alle Web-Apps in een App Service-Plan|
|/serverfarms/sites/Read|Web-Apps van App Service-abonnementen ophalen.|
|/serverfarms/skus/Read|App Service-abonnementen SKU's worden opgehaald.|
|/serverfarms/usages/Read|Het gebruik van App Service-abonnementen ophalen.|
|/serverfarms/virtualnetworkconnections/gateways/Write|App Service-abonnementen virtueel netwerk verbindingen Gateways bijwerken.|
|/serverfarms/virtualnetworkconnections/Read|App Service-abonnementen virtuele netwerkverbindingen worden opgehaald.|
|/serverfarms/virtualnetworkconnections/routes/DELETE|App Service-abonnementen virtueel netwerk verbindingen Routes verwijderen.|
|/serverfarms/virtualnetworkconnections/routes/Read|App Service-abonnementen virtueel netwerk verbindingen Routes worden opgehaald.|
|/serverfarms/virtualnetworkconnections/routes/Write|App Service-abonnementen virtueel netwerk verbindingen Routes bijwerken.|
|/serverfarms/workers/reboot/Action|Start opnieuw op App Service-abonnementen werknemers.|
|/ serverfarms/schrijven|Maak een nieuwe App Service-abonnement of een bestaande bijgewerkt|
|/sites/analyzecustomhostname/Read|Analyseer de aangepaste hostnaam.|
|/sites/applySlotConfig/Action|Web-app sleuf configuratie van doel-sleuf in de huidige web-app toe te passen|
|/sites/Backup/Action|Een nieuwe web-app back-up maken|
|/ sites/back-up/lezen|Back-up van Web Apps ophalen.|
|/sites/Backup/Write|Back-up van Web-Apps worden bijgewerkt.|
|/sites/backups/DELETE|Back-ups van Web-Apps verwijderen.|
|/sites/backups/List/Action|Lijst met Web-Apps back-ups.|
|/sites/backups/Read|De eigenschappen van back-up van een web-app|
|/sites/backups/Restore/Action|Terugzetten van back-ups van Web-Apps.|
|/sites/config/delete|Webconfiguratie Apps verwijderen.|
|/sites/config/List/Action|Web-App gevoelige beveiligingsinstellingen, zoals het publiceren van referenties, app-instellingen en verbindingsreeksen weergeven|
|/sites/config/Read|Ophalen van configuratie-instellingen voor Web-App|
|/sites/config/Write|Configuratie-instellingen voor Web-App bijwerken|
|/sites/continuouswebjobs/delete|Web-Apps doorlopende webtaken verwijderen.|
|/sites/continuouswebjobs/Read|Web-Apps doorlopende webtaken worden opgehaald.|
|/sites/continuouswebjobs/start/Action|Web-Apps doorlopende webtaken starten.|
|/sites/continuouswebjobs/Stop/Action|Web-Apps doorlopende webtaken stoppen.|
|/ sites/verwijderen|Een bestaande Web-App verwijderen|
|/sites/Deployments/DELETE|Implementaties van Web-Apps verwijderd.|
|/sites/Deployments/log/Read|Web-Apps implementaties logboek ophalen.|
|/sites/Deployments/Read|Implementaties van Web-Apps worden opgehaald.|
|/sites/Deployments/Write|Update-implementaties van Web-Apps.|
|/sites/Diagnostics/analyses/Execute/Action|Web-Apps Diagnostics analyse uitvoeren.|
|/sites/Diagnostics/analyses/Read|Analyse van Web Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/aspnetcore/Read|Diagnostische gegevens van Web-Apps voor ASP.NET Core app worden opgehaald.|
|/sites/Diagnostics/autoheal/Read|Web-Apps Diagnostics Autoheal ophalen.|
|/sites/Diagnostics/Deployment/Read|Implementatie van Web Apps diagnostische gegevens ophalen.|
|/sites/Diagnostics/Deployments/Read|Implementaties van Web Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/detectors/Execute/Action|Voer Web Apps diagnostische gegevens uit.|
|/sites/Diagnostics/detectors/Read|Detectie van Web Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/failedrequestsperuri/Read|Web-Apps Diagnostics mislukte aanvragen Per Uri ophalen.|
|/sites/Diagnostics/frebanalysis/Read|Web-Apps Diagnostics FREB analyse worden opgehaald.|
|/sites/Diagnostics/loganalyzer/Read|Web-Apps Diagnostics logboek Analyzer ophalen.|
|/sites/Diagnostics/Read|Categorieën voor Web-Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/runtimeavailability/Read|Runtime-beschikbaarheid van Web-Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/servicehealth/Read|Servicestatus van Web Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/sitecpuanalysis/Read|Web-Apps Diagnostics Site CPU analyse worden opgehaald.|
|/sites/Diagnostics/sitecrashes/Read|Web-Apps Diagnostics Site Crashes worden opgehaald.|
|/sites/Diagnostics/sitelatency/Read|Latentie van de Site Web Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/sitememoryanalysis/Read|Web-Apps Diagnostics Site geheugenanalyse worden opgehaald.|
|/sites/Diagnostics/siterestartsettingupdate/Read|Web-Apps Diagnostics Site opnieuw opstarten instelling Update downloaden.|
|/sites/Diagnostics/siterestartuserinitiated/Read|Web-Apps Diagnostics Site opnieuw opstarten gebruiker gestarte ophalen.|
|/sites/Diagnostics/siteswap/Read|Site wisseling van Web Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/ThreadCount/Read|Aantal threads van Web Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/workeravailability/Read|Web-Apps Diagnostics Workeravailability ophalen.|
|/sites/Diagnostics/workerprocessrecycle/Read|Recyclen van Web Apps Diagnostics Worker proces worden opgehaald.|
|/sites/domainownershipidentifiers/Read|Web-Apps domein eigenaar-id's worden opgehaald.|
|/sites/domainownershipidentifiers/Write|Web-Apps domein eigenaar-id's worden bijgewerkt.|
|/sites/Functions/Action|Functies van Web-Apps.|
|/sites/Functions/DELETE|Functies voor Web-Apps verwijderen.|
|/sites/Functions/listsecrets/Action|Lijst met geheimen Web Apps functies.|
|/sites/Functions/masterKey/Read|Web-Apps functies hoofdsleutel ophalen.|
|/sites/Functions/Read|Functies voor Web-Apps worden opgehaald.|
|/sites/Functions/token/Read|Get-Web-Apps functies Token.|
|/sites/Functions/Write|Bijwerken van Web Apps functies.|
|/sites/hostnamebindings/DELETE|Hostnaambindings voor Web-Apps verwijderen.|
|/sites/hostnamebindings/Read|Web-Apps Hostnaambindings ophalen.|
|/sites/hostnamebindings/Write|Web-Apps Hostnaambindings bijwerken.|
|/sites/hybridconnection/DELETE|Web-Apps hybride verbinding verwijderen.|
|/sites/hybridconnection/Read|Verbinding voor Web-Apps hybride ophalen.|
|/sites/hybridconnection/Write|Web-Apps hybride verbinding bijwerken.|
|/sites/hybridconnectionnamespaces/relays/DELETE|Web-Apps hybride verbinding naamruimten Relays verwijderen.|
|/sites/hybridconnectionnamespaces/relays/listkeys/Action|Lijst met sleutels Web Apps hybride verbinding naamruimten Relays.|
|/sites/hybridconnectionnamespaces/relays/Read|Web-Apps hybride verbinding naamruimten Relays ophalen.|
|/sites/hybridconnectionnamespaces/relays/Write|Web-Apps hybride verbinding naamruimten Relays bijwerken.|
|/sites/hybridconnectionrelays/Read|Web-Apps hybride verbinding Relays ophalen.|
|/sites/Instances/Deployments/DELETE|Web-Apps exemplaren implementaties verwijderd.|
|/sites/Instances/Deployments/Read|Implementaties van Web Apps instanties worden opgehaald.|
|/sites/Instances/Extensions/log/Read|Web-Apps exemplaren extensies logboek ophalen.|
|/sites/Instances/Extensions/Read|Web-Apps exemplaren extensies worden opgehaald.|
|/sites/Instances/processes/DELETE|Web-Apps exemplaren processen verwijderen.|
|/sites/Instances/processes/Read|Web-Apps exemplaren processen worden opgehaald.|
|/sites/Instances/Read|Web-Apps instanties worden opgehaald.|
|/sites/listsyncfunctiontriggerstatus/Action|Lijst met synchronisatie functie Trigger Status Web-Apps.|
|/sites/metricdefinitions/Read|Web-Apps metriek definities worden opgehaald.|
|/sites/metrics/Read|Web-Apps metrische gegevens worden opgehaald.|
|/sites/metricsdefinitions/Read|Web Apps metrische definities worden opgehaald.|
|/sites/migratemysql/Action|MySql-Web-Apps worden gemigreerd.|
|/sites/migratemysql/Read|Ophalen van Web Apps MySql migreren.|
|/sites/networktrace/Action|Netwerk Trace Web-Apps.|
|/sites/NewPassword/Action|NieuwWachtwoord Web-Apps.|
|/sites/operationresults/Read|Web-Apps Bewerkingsresultaten krijgt.|
|/sites/Operations/Read|Bewerkingen voor Web-Apps ophalen.|
|/sites/perfcounters/Read|Prestatiemeteritems voor Web-Apps worden opgehaald.|
|/sites/premieraddons/DELETE|Web Apps Premier invoegtoepassingen verwijderen.|
|/sites/premieraddons/Read|Web Apps Premier invoegtoepassingen ophalen.|
|/sites/premieraddons/Write|Bijwerken van Web Apps Premier invoegtoepassingen.|
|/sites/processes/Read|Web-Apps processen worden opgehaald.|
|/sites/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/sites/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/sites/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor Web-App|
|/sites/publiccertificates/delete|Web-Apps openbare certificaten verwijderen.|
|/sites/publiccertificates/Read|Web-Apps openbare certificaten ophalen.|
|/sites/publiccertificates/Write|Web-Apps openbare certificaten bijwerken.|
|/sites/Publish/Action|Een Web-App publiceren|
|/sites/publishxml/Action|Ophalen van de xml van profiel voor een Web-App publiceren|
|/sites/publishxml/read|Web-Apps publiceren XML ophalen.|
|/ sites/lezen|De eigenschappen van een Web-App opgehaald|
|/sites/recommendationhistory/Read|Web-Apps aanbeveling geschiedenis ophalen.|
|/sites/Recommendations/Disable/Action|Web-Apps aanbevelingen uitschakelen.|
|/sites/Recommendations/Read|Haal de lijst met aanbevelingen voor web-app.|
|/sites/recover/Action|Herstellen van Web-Apps.|
|/sites/resetSlotConfig/Action|Web-app-configuratie opnieuw instellen|
|/sites/resourcehealthmetadata/Read|Web-Apps Resource Health metagegevens ophalen.|
|/sites/restart/Action|Een Web-App starten|
|/sites/Restore/Read|Terugzetten van de Web-Apps worden opgehaald.|
|/sites/Restore/Write|Herstellen van Web-Apps.|
|/sites/siteextensions/DELETE|Verwijderen van Web Apps Site-uitbreidingen.|
|/sites/siteextensions/Read|Web-Apps-Site-uitbreidingen worden opgehaald.|
|/sites/siteextensions/Write|Web-Apps-Site-uitbreidingen worden bijgewerkt.|
|/sites/slots/analyzecustomhostname/Read|Ophalen van Web Apps sleuven analyseren aangepaste hostnaam.|
|/sites/slots/applySlotConfig/Action|Web-app siteconfiguratie van doel sleuf naar de huidige sleuf toepassen.|
|/sites/slots/Backup/Action|Nieuwe Web-Appsite back-up maken.|
|/sites/slots/Backup/Read|Back-up-sleuven voor Web-Apps worden opgehaald.|
|/sites/slots/Backup/Write|Bijwerken van Web Apps sleuven back-up.|
|/sites/slots/backups/DELETE|Back-ups sleuven voor Web-Apps verwijderen.|
|/sites/slots/backups/List/Action|Lijst met Web-Apps sleuven back-ups.|
|/sites/slots/backups/Read|De eigenschappen van een web-appsites back-up|
|/sites/slots/backups/Restore/Action|Web-Apps sleuven back-ups herstellen.|
|/sites/slots/config/DELETE|Webconfiguratie Apps sleuven verwijderen.|
|/sites/slots/config/List/Action|Web-Appsite van gevoelige beveiligingsinstellingen, zoals het publiceren van referenties, app-instellingen en verbindingsreeksen weergeven|
|/sites/slots/config/Read|Web-Appsite van configuratie-instellingen ophalen|
|/sites/slots/config/Write|Web-Appsite van configuratie-instellingen bijwerken|
|/sites/slots/continuouswebjobs/DELETE|Web-Apps sleuven doorlopende webtaken verwijderen.|
|/sites/slots/continuouswebjobs/Read|Web-Apps sleuven doorlopende webtaken worden opgehaald.|
|/sites/slots/continuouswebjobs/start/Action|Web-Apps sleuven doorlopende webtaken starten.|
|/sites/slots/continuouswebjobs/Stop/Action|Web-Apps sleuven doorlopende webtaken stoppen.|
|/sites/slots/DELETE|Een bestaande Web-Appsite verwijderen|
|/sites/slots/Deployments/DELETE|Web-Apps sleuven implementaties verwijderd.|
|/sites/slots/Deployments/log/Read|Web-Apps sleuven implementaties logboek ophalen.|
|/sites/slots/Deployments/Read|Web-Apps sleuven implementaties ophalen.|
|/sites/slots/Deployments/Write|Update-implementaties van Web Apps sleuven.|
|/sites/slots/Diagnostics/analyses/Execute/Action|Web-Apps sleuven Diagnostics analyse uitvoeren.|
|/sites/slots/Diagnostics/analyses/Read|Analyse van Web Apps sleuven diagnostische gegevens worden opgehaald.|
|/sites/slots/Diagnostics/aspnetcore/Read|Web-Apps sleuven diagnostische gegevens voor ASP.NET Core app worden opgehaald.|
|/sites/slots/Diagnostics/autoheal/Read|Web-Apps sleuven Diagnostics Autoheal ophalen.|
|/sites/slots/Diagnostics/Deployment/Read|Implementatie van Web Apps sleuven diagnostische gegevens ophalen.|
|/sites/slots/Diagnostics/Deployments/Read|Implementaties van Web Apps sleuven diagnostische gegevens worden opgehaald.|
|/sites/slots/Diagnostics/detectors/Execute/Action|Voer Web Apps sleuven diagnostische gegevens uit.|
|/sites/slots/Diagnostics/detectors/Read|Detectie van Web Apps sleuven diagnostische gegevens worden opgehaald.|
|/sites/slots/Diagnostics/frebanalysis/Read|Web-Apps sleuven Diagnostics FREB analyse worden opgehaald.|
|/sites/slots/Diagnostics/loganalyzer/Read|Web-Apps sleuven Diagnostics logboek Analyzer ophalen.|
|/sites/slots/Diagnostics/Read|Web-Apps sleuven diagnostische gegevens worden opgehaald.|
|/sites/slots/Diagnostics/runtimeavailability/Read|Web-Apps sleuven Diagnostics Runtime-beschikbaarheid ophalen.|
|/sites/slots/Diagnostics/servicehealth/Read|Servicestatus van Web Apps sleuven diagnostische gegevens worden opgehaald.|
|/sites/slots/Diagnostics/sitecpuanalysis/Read|Get Web Apps sleuven Diagnostics Site CPU-analyse.|
|/sites/slots/Diagnostics/sitecrashes/Read|Web-Apps sleuven Diagnostics Site Crashes worden opgehaald.|
|/sites/slots/Diagnostics/sitelatency/Read|Latentie van Web Apps sleuven Diagnostics Site ophalen.|
|/sites/slots/Diagnostics/sitememoryanalysis/Read|Web-Apps sleuven Diagnostics Site geheugenanalyse worden opgehaald.|
|/sites/slots/Diagnostics/siterestartsettingupdate/Read|Web-Apps sleuven Diagnostics Site opnieuw opstarten instelling Update downloaden.|
|/sites/slots/Diagnostics/siterestartuserinitiated/Read|Web-Apps sleuven Diagnostics Site opnieuw opstarten gebruiker gestarte ophalen.|
|/sites/slots/Diagnostics/siteswap/Read|Site wisseling van Web Apps sleuven diagnostische gegevens worden opgehaald.|
|/sites/slots/Diagnostics/ThreadCount/Read|Threads voor Web-Apps sleuven diagnostische gegevens worden opgehaald.|
|/sites/slots/Diagnostics/workeravailability/Read|Web-Apps sleuven Diagnostics Workeravailability ophalen.|
|/sites/slots/Diagnostics/workerprocessrecycle/Read|Recyclen van Web Apps sleuven Diagnostics Worker proces worden opgehaald.|
|/sites/slots/domainownershipidentifiers/Read|Web-Apps sleuven domein eigenaar-id's worden opgehaald.|
|/sites/slots/hostnamebindings/DELETE|Web-Apps sleuven Hostnaambindings verwijderen.|
|/sites/slots/hostnamebindings/Read|Web-Apps sleuven Hostnaambindings ophalen.|
|/sites/slots/hostnamebindings/Write|Web-Apps sleuven Hostnaambindings bijwerken.|
|/sites/slots/hybridconnection/DELETE|Sleuven hybride verbinding voor Web-Apps verwijderen.|
|/sites/slots/hybridconnection/Read|Hybride verbinding voor Web-Apps sleuven ophalen.|
|/sites/slots/hybridconnection/Write|Hybride verbinding voor Web-Apps sleuven bijwerken.|
|/sites/slots/hybridconnectionnamespaces/relays/DELETE|Web-Apps sleuven hybride verbinding naamruimten Relays verwijderen.|
|/sites/slots/hybridconnectionnamespaces/relays/Write|Web-Apps sleuven hybride verbinding naamruimten Relays bijwerken.|
|/sites/slots/hybridconnectionrelays/Read|Web-Apps sleuven hybride verbinding Relays ophalen.|
|/sites/slots/Instances/Deployments/Read|Implementaties van Web Apps sleuven instanties worden opgehaald.|
|/sites/slots/Instances/processes/DELETE|Web-Apps sleuven exemplaren processen verwijderen.|
|/sites/slots/Instances/processes/Read|Web-Apps sleuven exemplaren processen worden opgehaald.|
|/sites/slots/Instances/Read|Web-Apps sleuven instanties worden opgehaald.|
|/sites/slots/metricdefinitions/Read|Web-Apps sleuven metrische definities worden opgehaald.|
|/sites/slots/metrics/Read|Web-Apps sleuven metrische gegevens worden opgehaald.|
|/sites/slots/migratemysql/Read|Ophalen van Web Apps sleuven MySql migreren.|
|/sites/slots/networktrace/Action|Netwerk Trace Web Apps sleuven.|
|/sites/slots/NewPassword/Action|Sleuven NieuwWachtwoord Web-Apps.|
|/sites/slots/operationresults/Read|Web-Apps sleuven Bewerkingsresultaten krijgt.|
|/sites/slots/Operations/Read|Web-Apps sleuven bewerkingen ophalen.|
|/sites/slots/perfcounters/Read|Prestatiemeteritems van Web Apps sleuven ophalen.|
|/sites/slots/phplogging/Read|Web-Apps sleuven Phplogging ophalen.|
|/sites/slots/premieraddons/DELETE|Web Apps sleuven Premier invoegtoepassingen verwijderen.|
|/sites/slots/premieraddons/Read|Web Apps sleuven Premier invoegtoepassingen ophalen.|
|/sites/slots/premieraddons/Write|Bijwerken van Web Apps sleuven Premier invoegtoepassingen.|
|/sites/slots/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/sites/slots/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/sites/slots/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor Web-Appsite|
|/sites/slots/publiccertificates/Read|Web-Apps sleuven openbare certificaten ophalen.|
|/sites/slots/publiccertificates/Write|Maken of bijwerken van Web Apps sleuven openbare certificaten.|
|/sites/slots/Publish/Action|Een Web-Appsite publiceren|
|/sites/slots/publishxml/Action|Profiel xml publiceren voor Web-Appsite ophalen|
|/sites/slots/Read|De eigenschappen van een Web-App implementatiesleuf opgehaald|
|/sites/slots/resetSlotConfig/Action|Web-app sleuf configuratie opnieuw instellen|
|/sites/slots/resourcehealthmetadata/Read|Web-Apps sleuven Resource Health metagegevens ophalen.|
|/sites/slots/restart/Action|Een Web-Appsite starten|
|/sites/slots/Restore/Read|Web-Apps sleuven terugzetten ophalen.|
|/sites/slots/Restore/Write|Web-Apps sleuven herstellen.|
|/sites/slots/siteextensions/DELETE|Verwijderen van Web Apps sleuven Site-uitbreidingen.|
|/sites/slots/siteextensions/Read|Web-Apps sleuven Site-uitbreidingen worden opgehaald.|
|/sites/slots/siteextensions/Write|Web-Apps sleuven Site-uitbreidingen worden bijgewerkt.|
|/sites/slots/slotsdiffs/Action|Verschillen in de configuratie tussen web-app en sleuven ophalen|
|/sites/slots/slotsswap/Action|Implementatie Web-appsites wisselen|
|/sites/slots/Snapshots/Read|Momentopnamen van Web Apps sleuven ophalen.|
|/sites/slots/sourcecontrols/DELETE|Instellingen voor Web-Appsite van bronbeheer configuratie verwijderen|
|/sites/slots/sourcecontrols/Read|Configuratie-instellingen voor Web-Appsite van broncodebeheer ophalen|
|/sites/slots/sourcecontrols/Write|Instellingen voor Web-Appsite van bronbeheer configuratie bijwerken|
|/sites/slots/start/Action|Een Web-Appsite starten|
|/sites/slots/Stop/Action|Een Web-Appsite stoppen|
|/sites/slots/Sync/Action|Synchronisatie Web Apps sleuven.|
|/sites/slots/triggeredwebjobs/delete|Web-Apps sleuven Triggered webtaken verwijderen.|
|/sites/slots/triggeredwebjobs/Read|Web-Apps sleuven Triggered webtaken worden opgehaald.|
|/sites/slots/triggeredwebjobs/Run/Action|Web-Apps sleuven Triggered webtaken worden uitgevoerd.|
|/sites/slots/usages/Read|Het gebruik van Web Apps sleuven ophalen.|
|/sites/slots/virtualnetworkconnections/DELETE|Web-Apps sleuven virtuele netwerkverbindingen verwijderen.|
|/sites/slots/virtualnetworkconnections/gateways/Write|Web-Apps sleuven virtueel netwerk verbindingen Gateways bijwerken.|
|/sites/slots/virtualnetworkconnections/Read|Web-Apps sleuven virtuele netwerkverbindingen worden opgehaald.|
|/sites/slots/virtualnetworkconnections/Write|Web-Apps sleuven virtuele netwerkverbindingen worden bijgewerkt.|
|/sites/slots/webjobs/Read|Web-Apps sleuven webtaken worden opgehaald.|
|/sites/slots/Write|Een nieuwe Web-Appsite maken of bijwerken van een bestaande|
|/sites/slotsdiffs/Action|Verschillen in de configuratie tussen web-app en sleuven ophalen|
|/sites/slotsswap/Action|Implementatie Web-appsites wisselen|
|/sites/Snapshots/Read|Momentopnamen van de Web-Apps ophalen.|
|/sites/sourcecontrols/DELETE|Instellingen voor Web-App bronbeheer configuratie verwijderen|
|/sites/sourcecontrols/Read|Broncodebeheer voor Web-App-configuratie-instellingen ophalen|
|/sites/sourcecontrols/Write|Instellingen voor Web-App bronbeheer configuratie bijwerken|
|/sites/start/Action|Een Web-App starten|
|/sites/Stop/Action|Een Web-App stoppen|
|/sites/Sync/Action|Synchronisatie van Web-Apps.|
|/sites/syncfunctiontriggers/Action|Synchronisatie functie Triggers voor Web-Apps.|
|/sites/triggeredwebjobs/delete|Web-Apps Triggered webtaken verwijderen.|
|/sites/triggeredwebjobs/history/Read|Web Apps Triggered webtaken geschiedenis ophalen.|
|/sites/triggeredwebjobs/read|Web-Apps Triggered webtaken worden opgehaald.|
|/sites/triggeredwebjobs/Run/Action|Geactiveerde WebJobs voor Web-Apps worden uitgevoerd.|
|/sites/usages/Read|Het gebruik van Web Apps ophalen.|
|/sites/virtualnetworkconnections/DELETE|Verwijder virtuele netwerkverbindingen van Web Apps.|
|/sites/virtualnetworkconnections/gateways/Read|Web-Apps virtuele verbindingen netwerkgateways ophalen.|
|/sites/virtualnetworkconnections/gateways/Write|Web-Apps virtueel netwerk verbindingen Gateways bijwerken.|
|/sites/virtualnetworkconnections/Read|Web-Apps virtuele netwerkverbindingen worden opgehaald.|
|/sites/virtualnetworkconnections/Write|Web-Apps virtuele netwerkverbindingen worden bijgewerkt.|
|/sites/webjobs/Read|Web-Apps webtaken worden opgehaald.|
|/ sites/schrijven|Een nieuwe Web-App maken of bijwerken van een bestaande|
|/skus/Read|SKU's worden opgehaald.|
|/sourcecontrols/Read|Besturingselementen voor gegevensbronnen worden opgehaald.|
|sourcecontrols/schrijven|Besturingselementen voor gegevensbronnen bijwerken.|
|/ unregister/actie|Hef de registratie Microsoft.Web resourceprovider voor het abonnement.|
|valideren/actie|Valideren.|
|/ verifyhostingenvironmentvnet/actie|Controleer of de Hosting-omgeving Vnet.|

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

| Bewerking | Beschrijving |
|---|---|
|/Components/Read|Bewerkingen bronnen lezen|
|/healthInstances/Read|Bewerkingen bronnen lezen|
|Bewerkingen/leestijd|Bewerkingen bronnen lezen|
|/workloads/DELETE|Hiermee verwijdert u een resource werkbelasting|
|/workloads/Read|Een resource werkbelasting leest|
|werkbelastingen/schrijven|Schrijft een workload resource|

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [maakt u een aangepaste rol](custom-roles.md).
- Controleer de [ingebouwde RBAC-rollen](built-in-roles.md).
- Informatie over het beheren van toegangstoewijzingen [door gebruiker](role-assignments-users.md) of [door resource](role-assignments-portal.md) 
- Meer informatie over hoe [activiteitenlogboeken wilt controleren acties op resources bekijken](~/articles/azure-resource-manager/resource-group-audit.md)
