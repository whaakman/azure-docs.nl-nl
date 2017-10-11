---
title: Azure Resource Manager-Provider Operations | Microsoft Docs
description: Details van de bewerkingen die beschikbaar zijn op de Microsoft Azure Resource Manager-resourceproviders
services: active-directory
documentationcenter: 
author: jboeshart
manager: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: jaboes
ms.openlocfilehash: 95c6a1e9cfadeb19a0ec5a53fffb15ef220bfec0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Azure Resource Manager Resource Provider-bewerkingen

Dit document vindt u de bewerkingen die beschikbaar zijn voor elke resourceprovider van Microsoft Azure Resource Manager. Deze kunnen worden gebruikt in aangepaste rollen gedetailleerde op rollen gebaseerde toegangsbeheer (RBAC) machtigingen opgeven voor resources in Azure. Houd er rekening mee dat niet een uitgebreide lijst en bewerkingen kunnen worden toegevoegd of verwijderd als elke provider wordt bijgewerkt. Voer de bewerking tekenreeksen de indeling van `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. Gebruik voor een lijst met uitgebreide en huidige `Get-AzureRmProviderOperation` (in PowerShell) of `azure provider operations show` (in de Azure CLI) aan de bewerkingen na opvragen van de Azure-resourceproviders.

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| Bewerking | Beschrijving |
|---|---|
|/ configuration/actie|Configuratie van de Tenant updates.|
|/ services/actie|Een service-exemplaar in de tenant wordt bijgewerkt.|
|/ configuration/schrijven|De Tenantconfiguratie van een maakt.|
|/Configuration/Read|Leest de Tenantconfiguratie van de.|
|/ services/schrijven|Maakt een service-exemplaar in de tenant.|
|/Services/Read|Leest de service-exemplaren in de tenant.|
|/Services/DELETE|Hiermee verwijdert u een service-exemplaar in de tenant.|
|/Services/servicemembers/Action|Maakt een exemplaar van de service lid in de service.|
|/Services/servicemembers/Read|Het lid service-exemplaar in de service worden gelezen.|
|/Services/servicemembers/DELETE|Hiermee verwijdert u een exemplaar van de service lid in de service.|
|/Services/servicemembers/Alerts/Read|Leest de waarschuwingen voor een lid van de service.|
|/Services/Alerts/Read|Leest de waarschuwingen voor een service.|
|/Services/Alerts/Read|Leest de waarschuwingen voor een service.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| Bewerking | Beschrijving |
|---|---|
|/ generateRecommendations/actie|Geeft aanbevelingen|
|/ suppressions/actie|Bijgewerkte suppressions|
|registratie-/ actie|Registreert het abonnement voor Microsoft Advisor|
|/generateRecommendations/Read|Haalt status aanbevelingen genereren|
|/Recommendations/Read|Leest aanbevelingen|
|/suppressions/Read|Suppressions opgehaald|
|/suppressions/DELETE|Hiermee verwijdert u de onderdrukking|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| Bewerking | Beschrijving |
|---|---|
|/servers/Read|Haalt de informatie van de opgegeven Analysis-Server.|
|servers/schrijven|Maken of bijwerken van de opgegeven Analysis-Server.|
|/servers/DELETE|Hiermee verwijdert u de Analysis-Server.|
|/servers/Suspend/Action|Onderbreekt de Analysis-Server.|
|/servers/Resume/Action|De Analysis-Server hervat.|
|servers/checkNameAvailability<br>/ Action|Controleert dat de opgegeven Analysis Server naam geldig is en niet in gebruik.|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Controleert of de opgegeven servicenaam is beschikbaar|
|registratie-/ actie|Abonnement voor Microsoft.ApiManagement resourceprovider registreren|
|/ unregister/actie|Abonnement voor Microsoft.ApiManagement resourceprovider van de registratie ongedaan maken|
|/ service/schrijven|Maak een nieuw exemplaar van API Management-Service|
|/Service/Read|Lezen van metagegevens voor een exemplaar van API Management-Service|
|/Service/DELETE|Verwijderen van exemplaar van API Management-Service|
|/Service/updatehostname/Action|Setup, bijwerken of verwijderen van aangepaste domeinnamen voor een API Management-Service|
|/Service/uploadcertificate/Action|SSL-certificaat uploaden voor een API Management-Service|
|/Service/Backup/Action|Back-API Management-Service aan de opgegeven container in een gebruiker opgegeven storage-account|
|/Service/Restore/Action|API Management-Service van de opgegeven container in het opgegeven opslagaccount van een gebruiker herstellen|
|/Service/managedeployments/Action|SKU/eenheden toevoegen of verwijderen van regionale implementaties van API Management-Service wijzigen|
|/Service/getssotoken/Action|Opgehaald SSO-token dat kan worden gebruikt om aan te melden bij API Management-Service Legacy-portal als beheerder|
|/Service/applynetworkconfigurationupdates/Action|De Microsoft.ApiManagement resources voor uitvoering in het virtuele netwerk kiest bijgewerkte netwerkinstellingen-updates.|
|/Service/operationresults/Read|Huidige status van een langdurige bewerking opgehaald|
|/Service/networkStatus/Read|Hiermee haalt u de netwerk-toegangsstatus van resources.|
|/Service/loggers/Read|Ophalen van lijst met voorkomen of details van berichtenlogboek ophalen|
|/Service/loggers/Write|Nieuwe berichtenlogboek toevoegen of bestaande berichtenlogboek details bijwerken|
|/Service/loggers/DELETE|Bestaande Berichtenlogboek verwijderen|
|/Service/Users/Read|Een lijst met geregistreerde gebruikers of accountdetails van de van een gebruiker ophalen|
|/Service/Users/Write|Een nieuwe gebruiker of de details van de Update-account van een bestaande gebruiker registreren|
|/Service/Users/DELETE|Gebruikersaccount verwijderen|
|/Service/Users/generateSsoUrl/Action|URL SSO genereren. De URL kan worden gebruikt voor toegang tot de beheerportal|
|/Service/Users/Subscriptions/Read|Lijst met gebruikersabonnementen ophalen|
|/Service/Users/Keys/Read|Lijst met gebruikerssleutels|
|/Service/Users/Groups/Read|Ophalen van lijst met gebruikersgroepen|
|/Service/tenant/operationResults/Read|Ophalen van lijst met Bewerkingsresultaten of resultaat van een specifieke bewerking ophalen|
|/Service/tenant/Policy/Read|Beleidsconfiguratie van voor de tenant ophalen|
|/Service/tenant/Policy/Write|Stel de beleidsconfiguratie van voor de tenant|
|/Service/tenant/Policy/DELETE|Configuratie van beleid voor de tenant verwijderen|
|/Service/tenant/Configuration/Save/Action|Hiermee maakt u doorvoeren met configuratie-momentopname voor de opgegeven vertakking in de opslagplaats|
|/Service/tenant/Configuration/Deploy/Action|Voert een taak voor implementatie om de wijzigingen van de vertakking opgegeven git toepassen op de configuratie in de database.|
|/Service/tenant/Configuration/Validate/Action|Wijzigingen van de vertakking opgegeven git valideert|
|/Service/tenant/Configuration/operationResults/Read|Ophalen van lijst met Bewerkingsresultaten of resultaat van een specifieke bewerking ophalen|
|/Service/tenant/Configuration/syncState/Read|Status van laatste git-synchronisatie ophalen|
|/Service/tenant/Access/Read|Tenant toegang informatie details ophalen|
|/Service/tenant/Access/Write|Tenant toegang informatie details bijwerken|
|/Service/tenant/Access/regeneratePrimaryKey/Action|Primaire sleutel opnieuw genereren|
|/Service/tenant/Access/regenerateSecondaryKey/Action|Secundaire toegangssleutel opnieuw genereren|
|/Service/identityProviders/Read|Ophalen van lijst met identiteitsproviders of Get-details van de identiteitsprovider|
|/Service/identityProviders/Write|Een nieuwe id-Provider of Update details van een bestaande id-Provider maken|
|/Service/identityProviders/DELETE|Bestaande id-Provider verwijderen|
|/Service/Subscriptions/Read|Een lijst met abonnementen product of details van het product abonnement ophalen|
|/Service/Subscriptions/Write|Een bestaande gebruiker aan een bestaand product abonneren of bijwerken van bestaande abonnementsgegevens. Deze bewerking kan worden gebruikt voor het abonnement te verlengen|
|/Service/Subscriptions/DELETE|Het abonnement verwijderd. Deze bewerking kan worden gebruikt voor het abonnement verwijderen|
|/Service/Subscriptions/regeneratePrimaryKey/Action|Abonnement primaire sleutel opnieuw genereren|
|/Service/Subscriptions/regenerateSecondaryKey/Action|Abonnement secundaire sleutel opnieuw genereren|
|/Service/backends/Read|Ophalen van lijst met back-ends of details van back-end ophalen|
|/Service/backends/Write|Een nieuwe back-end toevoegen of bijwerken van bestaande back-end-details|
|/Service/backends/DELETE|Verwijder de bestaande back-end|
|/Service/APIs/Read|Lijst met alle geregistreerde API's of Get details van de API|
|/Service/APIs/Write|Nieuwe API maken of bijwerken van bestaande API-details|
|/Service/APIs/DELETE|Verwijder de bestaande API|
|/Service/APIs/Policy/Read|Configuratiedetails van beleid voor API ophalen|
|/Service/APIs/Policy/Write|Configuratiedetails van beleid voor API instellen|
|/Service/APIs/Policy/DELETE|Verwijder voor beleidsconfiguratie van de API|
|/Service/APIs/Operations/Read|Ophalen van lijst met bewerkingen voor bestaande API of details van API-bewerking ophalen|
|/Service/APIs/Operations/Write|Nieuwe API-bewerkingen voor het maken of bijwerken van bestaande API-bewerking|
|/Service/APIs/Operations/DELETE|Verwijder de bestaande API-bewerking|
|/Service/APIs/Operations/Policy/Read|Configuratiedetails van beleid voor bewerking ophalen|
|/Service/APIs/Operations/Policy/Write|Configuratiedetails van beleid voor opnieuw instellen|
|/Service/APIs/Operations/Policy/DELETE|Voor beleidsconfiguratie van de bewerking verwijderen|
|/Service/Products/Read|Ophalen van lijst met producten of details van product ophalen|
|/Service/Products/Write|Nieuw product maken of bijwerken van bestaande Productgegevens|
|/Service/Products/DELETE|Verwijder bestaande product|
|/Service/Products/Subscriptions/Read|Lijst met abonnementen product ophalen|
|/Service/Products/APIs/Read|Lijst met API's toegevoegd aan bestaande product ophalen|
|/Service/Products/APIs/Write|Bestaande API toevoegen aan bestaande product|
|/Service/Products/APIs/DELETE|Bestaande API verwijderen uit bestaande product|
|/Service/Products/Policy/Read|Voor beleidsconfiguratie van bestaande product ophalen|
|/Service/Products/Policy/Write|Stel de beleidsconfiguratie van voor bestaande product|
|/Service/Products/Policy/DELETE|Configuratie van beleid verwijderen uit bestaande product|
|/Service/Products/Groups/Read|Lijst met groepen van ontwikkelaars die zijn gekoppeld aan het product verkrijgen|
|/Service/Products/Groups/Write|Bestaande developer-groep koppelen aan bestaande product|
|/Service/Products/Groups/DELETE|Koppeling van bestaande developer-groep met bestaande product verwijderen|
|/Service/openidConnectProviders/Read|Lijst met providers OpenID Connect of details van OpenID Connect-Provider opvragen|
|/Service/openidConnectProviders/Write|Een nieuwe OpenID Connect-Provider of Update details van een bestaande OpenID Connect-Provider maken|
|/Service/openidConnectProviders/DELETE|Bestaande OpenID Connect Provider verwijderen|
|/Service/Certificates/Read|Ophalen van lijst met certificaten of details van een certificaat ophalen|
|/Service/Certificates/Write|Nieuw certificaat toevoegen|
|/Service/Certificates/DELETE|Verwijder bestaande certificaat|
|/Service/Properties/Read|Lijst van alle eigenschappen opgehaald of details van de opgegeven eigenschap opgehaald|
|/Service/Properties/Write|Een nieuwe eigenschap maken of bijwerken van waarde voor de opgegeven eigenschap|
|/Service/Properties/DELETE|Hiermee verwijdert u het bestaande eigenschap|
|/Service/Groups/Read|Lijst met groepen of informatie opgehaald van een groep|
|/Service/Groups/Write|Nieuwe groep maken of bijwerken van bestaande groepsnaam details|
|/Service/Groups/DELETE|Verwijder bestaande groep|
|/Service/Groups/Users/Read|Lijst van de groepsgebruikers ophalen|
|/Service/Groups/Users/Write|Bestaande gebruiker toevoegen aan bestaande groep|
|/Service/Groups/Users/DELETE|Bestaande gebruiker verwijderen uit bestaande groep|
|/Service/authorizationServers/Read|Ophalen van lijst met autorisatieservers of details van de autorisatie-server ophalen|
|/Service/authorizationServers/Write|Een nieuwe autorisatie-server of de details van de Update van een bestaande autorisatie-server maken|
|/Service/authorizationServers/DELETE|Verwijderen van bestaande autorisatie-server|
|/Service/Reports/bySubscription/Read|Rapport dat is samengevoegd per abonnement ophalen.|
|/Service/Reports/byRequest/Read|Rapportagegegevens van aanvragen|
|/Service/Reports/byOperation/Read|Rapport samenvoegen met de bewerkingen ophalen|
|/Service/Reports/byGeo/Read|Rapport samenvoegen met geografische regio ophalen|
|/Service/Reports/byUser/Read|Rapport geaggregeerd door ontwikkelaars ophalen.|
|/Service/Reports/byTime/Read|Rapport samenvoegen met perioden ophalen|
|/Service/Reports/byApi/Read|Rapport samenvoegen met API's ophalen|
|/Service/Reports/byProduct/Read|Rapport samenvoegen met producten ophalen.|

## <a name="microsoftappservice"></a>Microsoft.AppService

| Bewerking | Beschrijving |
|---|---|
|/ appidentities/lezen|Retourneert de resource (website) geregistreerd bij de Gateway.|
|/ appidentities/schrijven|Maakt een nieuwe App-id.|
|/ appidentities/verwijderen|Hiermee verwijdert u een bestaande App-identiteit.|
|/deploymenttemplates/listMetadata/Action|Een lijst met gebruikersinterface-metagegevens zijn gekoppeld aan de API-App-pakket.|
|/deploymenttemplates/Generate/Action|Retourneert een implementatiesjabloon in te richten API-App-exemplaren.|
|/ gateways/lezen|Retourneert het Gateway-exemplaar.|
|/ gateways/schrijven|Een nieuwe Gateway maken of bijwerken van bestaande.|
|/ gateways/verwijderen|Hiermee verwijdert u een bestaand gatewayexemplaar.|
|/gateways/listLoginUris/Action|Vult tokenopslag en retourneert OAuth-aanmelding URI's.|
|/gateways/listKeys/Action|Retourneert de geheimen van de Gateway.|
|/gateways/tokens/Write|Maakt een nieuw Zumo-Token met de opgegeven naam.|
|/gateways/Registrations/Read|Retourneert de resource (website) geregistreerd bij de Gateway.|
|/gateways/Registrations/Write|Registreert een bron (web site) met de Gateway.|
|/gateways/Registrations/DELETE|Heft de registratie van een resource (website) van de Gateway.|
|/ apiapps/lezen|Retourneert de API-App-exemplaar.|
|/ apiapps/schrijven|Een nieuwe API-App maken of bijwerken van bestaande.|
|/ apiapps/verwijderen|Hiermee verwijdert u een bestaand exemplaar van API-App.|
|/apiapps/listStatus/Action|Retourneert de status van de API-App.|
|/apiapps/listKeys/Action|API-App retourneert geheimen.|
|/apiapps/apidefinitions/Read|Retourneert de API-definitie-API-App.|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| Bewerking | Beschrijving |
|---|---|
|/ elevateAccess/actie|Hiermee wordt oproepende functie de rechten van Administrator voor gebruikerstoegang gegeven voor het tenantbereik|
|/classicAdministrators/Read|Hiermee worden de beheerders voor het abonnement ingelezen.|
|classicAdministrators/schrijven|Hiermee wordt de beheerder gewijzigd of aan een abonnement toegevoegd.|
|/classicAdministrators/DELETE|Hiermee wordt de beheerder verwijderd uit het abonnement.|
|/locks/Read|De vergrendelingen bij het opgegeven bereik worden opgehaald.|
|vergrendelingen/schrijven|Hiermee worden vergrendelingen toegevoegd bij het opgegeven bereik.|
|/locks/DELETE|De vergrendelingen bij het opgegeven bereik worden verwijderd.|
|/policyAssignments/Read|Informatie ophalen over een beleidstoewijzing.|
|policyAssignments/schrijven|Een beleidstoewijzing maken bij het opgegeven bereik.|
|/policyAssignments/DELETE|Een beleidstoewijzing verwijderen bij het opgegeven bereik.|
|/permissions/Read|Hiermee worden alle machtigingen van de beller voor een bepaald bereik weergegeven in een lijst.|
|/roleDefinitions/Read|Informatie ophalen over een roldefinitie.|
|roleDefinitions/schrijven|Een aangepaste roldefinitie maken of bijwerken met opgegeven machtigingen of toewijsbare bereiken.|
|/roleDefinitions/DELETE|De opgegeven aangepaste roldefinitie verwijderen.|
|/providerOperations/Read|Bewerkingen ophalen voor alle resourceproviders die kunnen worden gebruikt in roldefinities.|
|/policyDefinitions/Read|Informatie ophalen over een beleidsdefinitie.|
|policyDefinitions/schrijven|De definitie van een aangepast beleid maken.|
|/policyDefinitions/DELETE|Een beleidsdefinitie verwijderen.|
|/roleAssignments/Read|Informatie ophalen over een roltoewijzing.|
|roleAssignments/schrijven|Een roltoewijzing bij het opgegeven bereik maken.|
|/roleAssignments/DELETE|Een roltoewijzing bij het opgegeven bereik verwijderen.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| Bewerking | Beschrijving |
|---|---|
|/automationAccounts/Read|Hiermee ontvangt u een Azure Automation-account|
|automationAccounts/schrijven|Maken of bijwerken van een Azure Automation-account|
|/automationAccounts/DELETE|Hiermee verwijdert u een Azure Automation-account|
|/automationAccounts/Configurations/readContent/Action|Hiermee wordt de inhoud van een Azure Automation DSC opgehaald|
|/automationAccounts/hybridRunbookWorkerGroups/Read|Resources voor de Hybrid Runbook Worker gelezen|
|/automationAccounts/hybridRunbookWorkerGroups/DELETE|Resources voor de Hybrid Runbook Worker worden verwijderd|
|/automationAccounts/jobSchedules/Read|Een Azure Automation-taakschema opgehaald|
|/automationAccounts/jobSchedules/Write|Hiermee maakt u een Azure Automation-taakschema|
|/automationAccounts/jobSchedules/DELETE|Hiermee wordt een Azure Automation-taakschema verwijderd|
|/automationAccounts/connectionTypes/Read|Een Azure Automation verbindingstype-asset opgehaald|
|/automationAccounts/connectionTypes/Write|Hiermee maakt u een Azure Automation verbindingstype-asset|
|/automationAccounts/connectionTypes/DELETE|Hiermee wordt een Azure Automation verbindingstype-asset verwijderd|
|/automationAccounts/modules/Read|Een Azure Automation-module opgehaald|
|/automationAccounts/modules/Write|Maken of bijwerken van een Azure Automation-module|
|/automationAccounts/modules/DELETE|Hiermee verwijdert u een Azure Automation-module|
|/automationAccounts/credentials/Read|Een Azure Automation-referentieasset opgehaald|
|/automationAccounts/credentials/Write|Maken of bijwerken van een Azure Automation-referentieasset|
|/automationAccounts/credentials/DELETE|Hiermee wordt een Azure Automation-referentieasset verwijderd|
|/automationAccounts/Certificates/Read|Een Azure Automation-certificaatasset opgehaald|
|/automationAccounts/Certificates/Write|Maken of bijwerken van een Azure Automation-certificaatactivum|
|/automationAccounts/Certificates/DELETE|Een Azure Automation-certificaatactivum verwijderen|
|/automationAccounts/Schedules/Read|Een Azure Automation-planningsasset opgehaald|
|/automationAccounts/Schedules/Write|Maken of bijwerken van een Azure Automation-planningsasset|
|/automationAccounts/Schedules/DELETE|Hiermee wordt een Azure Automation-planningsasset verwijderd|
|/automationAccounts/Jobs/Read|Een Azure Automation-taak opgehaald|
|/automationAccounts/Jobs/Write|Hiermee maakt u een Azure Automation-taak|
|/automationAccounts/Jobs/Stop/Action|Hiermee wordt een Azure Automation-taak gestopt|
|/automationAccounts/Jobs/Suspend/Action|Een Azure Automation-taak wordt onderbroken|
|/automationAccounts/Jobs/Resume/Action|Een Azure Automation-taak hervatten|
|/automationAccounts/Jobs/runbookContent/Action|De inhoud van het Azure Automation-runbook opgehaald op het moment van de taak wordt uitgevoerd|
|/automationAccounts/Jobs/output/Action|De uitvoer van een taak opgehaald|
|/automationAccounts/Jobs/Read|Een Azure Automation-taak opgehaald|
|/automationAccounts/Jobs/Write|Hiermee maakt u een Azure Automation-taak|
|/automationAccounts/Jobs/Stop/Action|Hiermee wordt een Azure Automation-taak gestopt|
|/automationAccounts/Jobs/Suspend/Action|Een Azure Automation-taak wordt onderbroken|
|/automationAccounts/Jobs/Resume/Action|Een Azure Automation-taak hervatten|
|/automationAccounts/Jobs/streams/Read|Een Azure Automation-taakstroom opgehaald|
|/automationAccounts/Connections/Read|Een Azure Automation-verbindingsasset opgehaald|
|/automationAccounts/Connections/Write|Maken of bijwerken van een Azure Automation-verbindingsasset|
|/automationAccounts/Connections/DELETE|Hiermee wordt een Azure Automation verbindingstype-asset verwijderd|
|/automationAccounts/Variables/Read|Hiermee wordt een variabel Azure Automation-asset gelezen|
|/automationAccounts/Variables/Write|Maken of bijwerken van een Azure Automation-variabelenactivum|
|/automationAccounts/Variables/DELETE|Hiermee wordt een variabel Azure Automation-asset verwijderd|
|/automationAccounts/runbooks/readContent/Action|De inhoud van een Azure Automation-runbook opgehaald|
|/automationAccounts/runbooks/Read|Een Azure Automation-runbook opgehaald|
|/automationAccounts/runbooks/Write|Maken of bijwerken van een Azure Automation-runbook|
|/automationAccounts/runbooks/DELETE|Hiermee verwijdert u een Azure Automation-runbook|
|/automationAccounts/runbooks/Draft/readContent/Action|Hiermee wordt de inhoud van een Azure Automation-runbookconcept opgehaald|
|/automationAccounts/runbooks/Draft/writeContent/Action|De inhoud van een Azure Automation-runbookconcept gemaakt|
|/automationAccounts/runbooks/Draft/Read|Een Azure Automation-runbookconcept opgehaald|
|/automationAccounts/runbooks/Draft/Publish/Action|Een Azure Automation-runbookconcept gepubliceerd|
|/automationAccounts/runbooks/Draft/undoEdit/Action|Bewerkingen aan een Azure Automation-runbookconcept ongedaan maken|
|/automationAccounts/runbooks/Draft/testJob/Read|Een Azure Automation-runbookconcept opgehaald|
|/automationAccounts/runbooks/Draft/testJob/Write|Hiermee maakt u een Azure Automation-runbookconcept|
|/automationAccounts/runbooks/Draft/testJob/Stop/Action|Hiermee wordt een Azure Automation-runbookconcept gestopt|
|/automationAccounts/runbooks/Draft/testJob/Suspend/Action|Wordt een Azure Automation-runbookconcept onderbroken|
|/automationAccounts/runbooks/Draft/testJob/Resume/Action|Een Azure Automation-runbookconcept hervatten|
|/automationAccounts/webhooks/Read|Hiermee wordt een Azure Automation-webhook gelezen|
|/automationAccounts/webhooks/Write|Maken of bijwerken van een Azure Automation-webhook|
|/automationAccounts/webhooks/DELETE|Hiermee wordt een Azure Automation-webhook verwijderd |
|/automationAccounts/webhooks/generateUri/Action|Een URI voor een Azure Automation-webhook gegenereerd|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

Deze provider is niet een volledige ARM-provider en biedt geen bewerkingen ARM.

## <a name="microsoftbatch"></a>Microsoft.Batch

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Het abonnement voor de Batch-Resourceprovider geregistreerd en wordt het maken van de Batch-accounts|
|batchAccounts/schrijven|Een nieuwe Batch-account maken of bijwerken van een bestaande Batch-account|
|/batchAccounts/Read|Een lijst met Batch-accounts of haalt u de eigenschappen van een Batch-account|
|/batchAccounts/DELETE|Hiermee verwijdert u een Batch-account|
|/batchAccounts/listkeys/Action|Een lijst met sleutels voor een Batch-account toegang tot|
|/batchAccounts/regeneratekeys/Action|Opnieuw toegangssleutels voor een Batch-account|
|/batchAccounts/syncAutoStorageKeys/Action|Toegangssleutels voor het opslagaccount automatisch is geconfigureerd voor een Batch-account wordt gesynchroniseerd|
|/batchAccounts/Applications/Read|Een lijst met toepassingen of haalt u de eigenschappen van een toepassing|
|/batchAccounts/Applications/Write|Een nieuwe toepassing maken of bijwerken van een bestaande toepassing|
|/batchAccounts/Applications/DELETE|Een toepassing verwijderen|
|/batchAccounts/Applications/Versions/Read|De eigenschappen van een toepassingspakket opgehaald|
|/batchAccounts/Applications/Versions/Write|Een nieuw pakket maken of bijwerken van een bestaand toepassingspakket|
|/batchAccounts/Applications/Versions/Activate/Action|Hiermee activeert u een toepassingspakket|
|/batchAccounts/Applications/Versions/DELETE|Hiermee verwijdert u een toepassingspakket|
|/Locations/Quotas/Read|Batch-quota van het opgegeven abonnement bij de opgegeven Azure-regio worden opgehaald|

## <a name="microsoftbilling"></a>Microsoft.Billing

| Bewerking | Beschrijving |
|---|---|
|/Invoices/Read|Een lijst met beschikbare facturen|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| Bewerking | Beschrijving |
|---|---|
|/ mapApis/lezen|Leesbewerking|
|/ mapApis/schrijven|Schrijfbewerking|
|/ mapApis/verwijderen|Het verwijderen|
|/mapApis/regenerateKey/Action|De sleutel opnieuw genereren|
|/mapApis/listSecrets/Action|Lijst met geheimen weergeven|
|/mapApis/listSingleSignOnToken/Action|Verificatietoken voor eenmalige aanmelding voor de resource lezen|
|Bewerkingen/leestijd|Beschrijving van de bewerking.|

## <a name="microsoftcache"></a>Microsoft.Cache

| Bewerking | Beschrijving |
|---|---|
|/ checknameavailability/actie|Controleert of een naam beschikbaar voor gebruik met een nieuwe Redis-Cache|
|registratie-/ actie|De provider van de resource 'Microsoft.Cache' wordt geregistreerd met een abonnement|
|/ unregister/actie|Heft de registratie van de resourceprovider 'Microsoft.Cache' met een abonnement|
|redis/schrijven|De instellingen en configuratie van Redis-cache wijzigen in de beheerportal|
|/redis/Read|De instellingen en configuratie van Redis-cache weergeven in de beheerportal|
|/redis/DELETE|Volledige Redis-cache verwijderen|
|/redis/listKeys/Action|De waarde van de toegangssleutels voor Redis-cache weergeven in de beheerportal|
|/redis/regenerateKey/Action|De waarde van de toegangssleutels voor Redis-cache wijzigen in de beheerportal|
|/redis/import/Action|Gegevens met een opgegeven indeling in Redis importeren vanuit meerdere blobs|
|/redis/export/Action|Redis-gegevens exporteren naar vooraf ingestelde opslag-blobs in een opgegeven indeling|
|/redis/forceReboot/Action|Een cache-instantie geforceerd opnieuw opstarten met mogelijk gegevensverlies.|
|/redis/Stop/Action|Stoppen van een cache-exemplaar.|
|/redis/start/Action|Start een cache-exemplaar.|
|/redis/metricDefinitions/Read|Hiermee worden de beschikbare metrische gegevens voor een Redis-cache opgehaald|
|/redis/firewallRules/Read|Ophalen van de IP-firewallregels van een Redis-Cache|
|/redis/firewallRules/Write|Bewerk de IP-firewall-regels van een Redis-Cache|
|/redis/firewallRules/DELETE|IP-firewallregels van een Redis-Cache verwijderen|
|/redis/listUpgradeNotifications/Read|De nieuwste upgrademeldingen weergeven voor de cachetenant.|
|/redis/linkedservers/Read|Gekoppelde Servers die zijn gekoppeld aan een redis-cache niet ophalen.|
|/redis/linkedservers/Write|Gekoppelde Server toevoegen aan een Redis-Cache|
|/redis/linkedservers/DELETE|Gekoppelde Server verwijderen uit een Redis-Cache|
|/redis/patchSchedules/Read|De toepassing van patches planning van een Redis-Cache opgehaald|
|/redis/patchSchedules/Write|Wijzigen van de toepassing van patches planning van een Redis-Cache|
|/redis/patchSchedules/DELETE|Verwijderen van de patch-planning van een Redis-Cache|

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| Bewerking | Beschrijving |
|---|---|
|/ provisionGlobalAppServicePrincipalInUserTenant/actie|Service-principal voor service-principal voor app inrichten|
|/ validateCertificateRegistrationInformation/actie|Aankoop certificaatobject valideren zonder deze te verzenden|
|registratie-/ actie|Registreer de provider van Microsoft-Certificates resource voor het abonnement|
|/ certificateOrders/schrijven|Een nieuwe certificateOrder toevoegen of een bestaande bijgewerkt|
|/ certificateOrders/verwijderen|Een bestaande AppServiceCertificate verwijderen|
|/ certificateOrders/lezen|De lijst met CertificateOrders ophalen|
|/certificateOrders/reissue/Action|Geef een bestaande certificateorder|
|/certificateOrders/renew/Action|Een bestaande certificateorder vernieuwen|
|/certificateOrders/retrieveCertificateActions/Action|De lijst met acties certificaat ophalen|
|/certificateOrders/retrieveEmailHistory/Action|Geschiedenis van de e-certificaat ophalen|
|/certificateOrders/resendEmail/Action|Certificaat-e-mail opnieuw verzenden|
|/certificateOrders/verifyDomainOwnership/Action|Domein verifiëren|
|/certificateOrders/resendRequestEmails/Action|Aanvraag e-mailberichten naar een ander e-mailadres verzenden|
|/certificateOrders/resendRequestEmails/Action|Site zegel voor een uitgegeven certificaat van de App-Service ophalen|
|/certificateOrders/Certificates/Write|Toevoegen van een nieuw certificaat of een bestaande bijgewerkt|
|/certificateOrders/Certificates/DELETE|Een bestaand certificaat verwijderen|
|/certificateOrders/Certificates/Read|De lijst met certificaten ophalen|

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Registreren bij klassieke berekening|
|/ checkDomainNameAvailability/actie|Hiermee wordt gecontroleerd of een bepaald domeinnaam beschikbaar is.|
|/ moveSubscriptionResources/actie|Verplaats alle klassieke resources naar een ander abonnement.|
|/ validateSubscriptionMoveAvailability/actie|De beschikbaarheid van het abonnement voor de klassieke verplaatsingsbewerking valideren.|
|/operatingSystemFamilies/Read|Geeft een lijst van de Gast besturingssystemen beschikbaar is in Microsoft Azure en vermeldt tevens de besturingssysteemversies beschikbaar voor elke f
|/Capabilities/Read|Hiermee worden de mogelijkheden weergegeven|
|/operatingSystems/Read|Hiermee wordt een lijst van de versies van gastbesturingssystemen gemaakt die momenteel beschikbaar zijn in Microsoft Azure.|
|/resourceTypes/skus/Read|Haalt de SKU-lijst op voor ondersteunde resourcetypen.|
|/domainNames/Read|De domeinnamen voor resources retourneren.|
|domainNames/schrijven|Hiermee worden de domeinnamen voor resources toegevoegd of gewijzigd.|
|/domainNames/DELETE|De domeinnamen voor resources verwijderen.|
|/domainNames/swap/Action|Hiermee wordt de faseringssleuf gewisseld naar de productiesleuf.|
|/domainNames/serviceCertificates/Read|Hiermee worden de gebruikte servicecertificaten geretourneerd.|
|/domainNames/serviceCertificates/Write|Hiermee worden de gebruikte servicecertificaten toegevoegd of gewijzigd.|
|/domainNames/serviceCertificates/DELETE|De gebruikte servicecertificaten verwijderen.|
|/domainNames/serviceCertificates/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de servicecertificaten van domeinnamen gelezen.|
|/domainNames/Capabilities/Read|Geeft de mogelijkheden voor domeinnaamgeving weer|
|/domainNames/Extensions/Read|Hiermee kunt u de domeinnaamextensies retourneren.|
|/domainNames/Extensions/Write|De domeinnaamextensies toevoegen.|
|/domainNames/Extensions/DELETE|De domeinnaamextensies verwijderen.|
|/domainNames/Extensions/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de domeinnaamextensies gelezen.|
|/domainNames/Active/Write|Hiermee wordt de naam ingesteld van het actieve domein.|
|/domainNames/slots/Read|Hiermee worden de implementatiesleuven weergegeven.|
|/domainNames/slots/Write|Hiermee wordt de implementatie gemaakt of bijgewerkt.|
|/domainNames/slots/DELETE|Hiermee wordt een bepaald implementatiesleuf verwijderd.|
|/domainNames/slots/start/Action|Hiermee wordt een implementatiesleuf gestart.|
|/domainNames/slots/Stop/Action|Hiermee wordt de implementatiesleuf tijdelijk stopgezet.|
|/domainNames/slots/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de domeinnaamsleuven gelezen.|
|/domainNames/slots/roles/Read|Hiermee wordt de rol van de implementatiesleuf opgehaald.|
|/domainNames/slots/roles/extensionReferences/Read|Hiermee kunt u de extensieverwijzing voor de implementatiesiterol retourneren.|
|/domainNames/slots/roles/extensionReferences/Write|Hiermee kunt u de extensieverwijzing voor de implementatiesiterol toevoegen of aanpassen.|
|/domainNames/slots/roles/extensionReferences/DELETE|De extensieverwijzing voor de implementatiesite verwijderen.|
|/domainNames/slots/roles/extensionReferences/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de extensieverwijzing van de rollen voor domeinnaamsleuven gelezen.|
|/domainNames/slots/roles/roleInstances/Read|Hiermee wordt het rolexemplaar opgehaald.|
|/domainNames/slots/roles/roleInstances/restart/Action|Rolinstanties opnieuw is opgestart.|
|/domainNames/slots/roles/roleInstances/reimage/Action|Hiermee wordt de installatiekopie van het rolexemplaar opnieuw toegepast.|
|/domainNames/slots/roles/roleInstances/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van rolinstanties van rollen voor domeinnaamsleuven gelezen.|
|/domainNames/slots/State/start/Write|Hiermee wordt de status van de implementatiesleuf gewijzigd naar Gestopt.|
|/domainNames/slots/State/Stop/Write|Hiermee wordt de status van de implementatiesleuf gewijzigd naar Gestart.|
|/domainNames/slots/upgradeDomain/Write|Hiermee wordt stap voor stap een domeinupgrade uitgevoerd.|
|/domainNames/internalLoadBalancers/Read|De interne load balancers worden opgehaald.|
|/domainNames/internalLoadBalancers/Write|Hiermee wordt een nieuwe interne taakverdeling gemaakt.|
|/domainNames/internalLoadBalancers/DELETE|Een nieuwe interne taakverdeling verwijderen.|
|/domainNames/internalLoadBalancers/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de interne load balancers van domeinnamen gelezen.|
|/domainNames/loadBalancedEndpointSets/Read|Hiermee worden de eindpuntsets van de taakverdeling weergegeven|
|/domainNames/loadBalancedEndpointSets/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de eindpuntsets voor de taakverdeling van de domeinnamen gelezen.|
|/domainNames/availabilitySets/Read|De beschikbaarheidsset voor de resource wordt weergegeven.|
|/Quotas/Read|Hiermee worden de quota voor het abonnement opgehaald.|
|/virtualMachines/Read|Hiermee wordt een lijst met virtuele machines opgehaald.|
|virtuele machines/schrijven|Hiermee worden virtuele machines toegevoegd of gewijzigd.|
|/virtualMachines/DELETE|Hiermee verwijdert u virtuele machines.|
|/virtualMachines/start/Action|Hiermee wordt de virtuele machine gestart.|
|/virtualMachines/redeploy/Action|De virtuele machine redeploys.|
|/virtualMachines/restart/Action|Virtuele machines opnieuw is opgestart.|
|/virtualMachines/Stop/Action|Hiermee wordt de virtuele machine gestopt.|
|/virtualMachines/Shutdown/Action|Hiermee wordt de virtuele machine afgesloten.|
|/virtualMachines/attachDisk/Action|Hiermee wordt een gegevensschijf gekoppeld aan een virtuele machine.|
|/virtualMachines/detachDisk/Action|Hiermee wordt een gegevensschijf losgekoppeld van een virtuele machine.|
|/virtualMachines/downloadRemoteDesktopConnectionFile/Action|Hiermee wordt het RDP-bestand voor de virtuele machine gedownload.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups leestijd|Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan de netwerkinterface is gekoppeld.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups/schrijven|Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan de netwerkinterface is gekoppeld.|
|/virtualMachines/networkInterfaces /<br>associatedNetworkSecurityGroups/verwijderen|Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan de netwerkinterface is gekoppeld.|
|/virtualMachines/networkInterfaces /<br>operationStatuses associatedNetworkSecurityGroups leestijd|Hiermee wordt de bewerkingsstatus van virtuele machines die zijn gekoppeld aan netwerkbeveiligingsgroepen gelezen.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee worden de metrische definities opgehaald.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee worden de diagnostische instellingen opgehaald.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee worden diagnostische instellingen toegevoegd of gewijzigd.|
|/virtualMachines/metrics/Read|Hiermee worden de metrische gegevens opgehaald.|
|/virtualMachines/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van virtuele machines gelezen.|
|/virtualMachines/Extensions/Read|Hiermee wordt de extensie van de virtuele machine opgehaald.|
|/virtualMachines/Extensions/Write|Hiermee wordt de extensie voor de virtuele machine ingesteld.|
|/virtualMachines/Extensions/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van extensies van virtuele machines gelezen.|
|/virtualMachines/asyncOperations/Read|Hiermee worden de mogelijk asynchrone bewerkingen opgehaald|
|/virtualMachines/Disks/Read|Hiermee wordt een lijst met gegevensschijven opgehaald|
|/virtualMachines/associatedNetworkSecurityGroups/Read|Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan de virtuele machine is gekoppeld.|
|/virtualMachines/associatedNetworkSecurityGroups/Write|Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan de virtuele machine is gekoppeld.|
|/virtualMachines/associatedNetworkSecurityGroups/DELETE|Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan de virtuele machine is gekoppeld.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van virtuele machines die zijn gekoppeld aan netwerkbeveiligingsgroepen gelezen.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Registreren bij klassiek netwerk|
|/gatewaySupportedDevices/Read|De lijst met ondersteunde apparaten wordt opgehaald.|
|/reservedIps/Read|Hiermee worden de gereserveerde IP's opgehaald.|
|reservedip/schrijven|Een nieuw gereserveerd IP toevoegen|
|/reservedIps/DELETE|Hiermee wordt een gereserveerd IP verwijderd.|
|/reservedIps/link/Action|Een gereserveerde IP koppelen|
|/reservedIps/join/Action|Deelnemen aan een gereserveerde IP|
|/reservedIps/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de gereserveerde IP's gelezen.|
|/virtualNetworks/Read|Het virtuele netwerk wordt opgehaald.|
|virtualNetworks/schrijven|Een nieuw virtueel netwerk wordt toegevoegd.|
|/virtualNetworks/DELETE|Hiermee wordt het virtuele netwerk verwijderd.|
|/virtualNetworks/peer/Action|Hiermee wordt een virtueel netwerk via peercommunicatie gebruikt met een ander virtueel netwerk.|
|/virtualNetworks/join/Action|Hiermee wordt de deelname aan het virtuele netwerk uitgevoerd.|
|/virtualNetworks/checkIPAddressAvailability/Action|Hiermee wordt de beschikbaarheid van een bepaald IP-adres in een virtueel netwerk gecontroleerd.|
|/virtualNetworks/Capabilities/Read|Hiermee worden de mogelijkheden weergegeven|
|/virtualNetworks/subnetten /<br>associatedNetworkSecurityGroups leestijd|Hiermee wordt de netwerkbeveiligingsgroep opgehaald die aan het subnet is gekoppeld.|
|/virtualNetworks/subnetten /<br>associatedNetworkSecurityGroups/schrijven|Hiermee wordt een netwerkbeveiligingsgroep toegevoegd die aan het subnet is gekoppeld.|
|/virtualNetworks/subnetten /<br>associatedNetworkSecurityGroups/verwijderen|Hiermee wordt de netwerkbeveiligingsgroep verwijderd die aan het subnet is gekoppeld.|
|/virtualNetworks/subnetten /<br>operationStatuses associatedNetworkSecurityGroups leestijd|Hiermee wordt de bewerkingsstatus van de netwerkbeveiligingsgroep die aan het subnet van het virtueel netwerk is gekoppeld gelezen.|
|/virtualNetworks/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de virtuele netwerken gelezen.|
|/virtualNetworks/gateways/Read|De gateways voor het virtuele netwerk worden opgehaald.|
|/virtualNetworks/gateways/Write|Een gateway voor het virtuele netwerk wordt toegevoegd.|
|/virtualNetworks/gateways/DELETE|De gateway van het virtuele netwerk wordt verwijderd.|
|/virtualNetworks/gateways/startDiagnostics/Action|Hiermee wordt de diagnostische test voor de gateway van het virtuele netwerk gestart.|
|/virtualNetworks/gateways/stopDiagnostics/Action|Hiermee wordt de diagnostische test voor de gateway van het virtuele netwerk gestopt.|
|/virtualNetworks/gateways/downloadDiagnostics/Action|Hiermee worden de diagnotische gegevens van de gateway gedownload.|
|/virtualNetworks/gateways/listCircuitServiceKey/Action|Hiermee wordt de circuitservicesleutel opgehaald.|
|/virtualNetworks/gateways/downloadDeviceConfigurationScript/Action|Het configuratiescript voor het apparaat wordt gedownload.|
|/virtualNetworks/gateways/listPackage/Action|Het gatewaypakket voor het virtuele netwerk wordt weergegeven.|
|/virtualNetworks/gateways/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de virtuele netwerkgateways gelezen.|
|/virtualNetworks/gateways/Packages/Read|Het gatewaypakket voor het virtuele netwerk wordt opgehaald.|
|/virtualNetworks/gateways/Connections/Read|Hiermee wordt de lijst met verbindingen opgehaald.|
|/virtualNetworks/gateways/Connections/Connect/Action|Hiermee wordt een gatewayverbinding tussen sites ingesteld.|
|/virtualNetworks/gateways/Connections/Disconnect/Action|Hiermee wordt een gatewayverbinding tussen sites verbroken.|
|/virtualNetworks/gateways/Connections/test/Action|Hiermee wordt een gatewayverbinding tussen sites getest.|
|/virtualNetworks/gateways/clientRevokedCertificates/Read|De ingetrokken clientcertificaten lezen.|
|/virtualNetworks/gateways/clientRevokedCertificates/Write|Hiermee wordt een clientcertificaat ingetrokken.|
|/virtualNetworks/gateways/clientRevokedCertificates/DELETE|Hiermee wordt de intrekking van een clientcertificaat ongedaan gemaakt.|
|/virtualNetworks/gateways/clientRootCertificates/Read|Clienthoofdcertificaten zoeken.|
|/virtualNetworks/gateways/clientRootCertificates/Write|Hiermee wordt een nieuw clienthoofdcertificaat geüpload.|
|/virtualNetworks/gateways/clientRootCertificates/DELETE|Hiermee wordt het clientcertificaat voor de gateway van het virtuele netwerk verwijderd.|
|/virtualNetworks/gateways/clientRootCertificates/Download/Action|Hiermee wordt een certificaat gedownload op basis van miniatuur.|
|/virtualNetworks/gateways/clientRootCertificates/listPackage/Action|Het certificaatpakket van de virtuele netwerkgateway wordt weergegeven.|
|/networkSecurityGroups/Read|Hiermee wordt de netwerkbeveiligingsgroep opgehaald.|
|networkSecurityGroups/schrijven|Hiermee wordt een nieuwe netwerkbeveiligingsgroep toegevoegd.|
|/networkSecurityGroups/DELETE|Hiermee wordt de netwerkbeveiligingsgroep verwijderd.|
|/networkSecurityGroups/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de netwerkbeveiligingsgroep gelezen.|
|/networkSecurityGroups/securityRules/Read|Hiermee wordt de beveiligingsregel opgehaald.|
|/networkSecurityGroups/securityRules/Write|Hiermee wordt een beveiligingsregel toegevoegd of bijgewerkt.|
|/networkSecurityGroups/securityRules/DELETE|Hiermee wordt de beveiligingsregel verwijderd.|
|/networkSecurityGroups/securityRules/operationStatuses/Read|Hiermee wordt de bewerkingsstatus van de beveiligingsregels voor de netwerkbeveiligingsgroep gelezen.|
|/Quotas/Read|Hiermee worden de quota voor het abonnement opgehaald.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Registreren bij klassieke opslag|
|/ checkStorageAccountAvailability/actie|Hiermee wordt gecontroleerd of een opslagaccount beschikbaar is.|
|/Capabilities/Read|Hiermee worden de mogelijkheden weergegeven|
|/publicImages/Read|Hiermee wordt de openbare installatiekopie voor de virtuele machine opgehaald.|
|/images/Read|Hiermee wordt de afbeelding geretourneerd.|
|/storageAccounts/Read|Opslagaccount met het opgegeven account wordt geretourneerd.|
|storageAccounts/schrijven|Hiermee wordt een nieuw opslagaccount toegevoegd.|
|/storageAccounts/DELETE|Hiermee wordt het opslagaccount verwijderd.|
|/storageAccounts/listKeys/Action|Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven.|
|/storageAccounts/regenerateKey/Action|De bestaande toegangssleutels voor het opslagaccount worden geregenereerd.|
|/storageAccounts/operationStatuses/Read|Hiermee wordt de bewerkingsstatus voor de resource ingelezen.|
|/storageAccounts/images/Read|Hiermme wordt de installatiekopie van het opslagaccount hersteld.|
|/storageAccounts/images/DELETE|Hiermee wordt de betreffende installatiekopie van het opslagaccount verwijderd.|
|/storageAccounts/Disks/Read|Hiermee wordt de opslagaccountschijf geretourneerd.|
|/storageAccounts/Disks/Write|Hiermee wordt een opslagaccountschijf toegevoegd.|
|/storageAccounts/Disks/DELETE|Hiermee wordt een opgegeven opslagaccountschijf verwijderd.|
|/storageAccounts/Disks/operationStatuses/Read|Hiermee wordt de bewerkingsstatus voor de resource ingelezen.|
|/storageAccounts/osImages/Read|Hiermee wordt de installatiekopie van het besturingssysteem van een opslagaccount hersteld.|
|/storageAccounts/osImages/DELETE|Hiermee wordt de betreffende installatiekopie van het besturingssysteem van een opslagaccount verwijderd.|
|/storageAccounts/Services/Read|De beschikbare services ophalen.|
|/storageAccounts/Services/metricDefinitions/Read|Hiermee worden de metrische definities opgehaald.|
|/storageAccounts/Services/metrics/Read|Hiermee worden de metrische gegevens opgehaald.|
|/storageAccounts/Services/diagnosticSettings/Read|Hiermee worden de diagnostische instellingen opgehaald.|
|/storageAccounts/Services/diagnosticSettings/Write|Hiermee worden diagnostische instellingen toegevoegd of gewijzigd.|
|/Disks/Read|Hiermee wordt de opslagaccountschijf geretourneerd.|
|/osImages/Read|De installatiekopie van het besturingssysteem herstellen.|
|/Quotas/Read|Hiermee worden de quota voor het abonnement opgehaald.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| Bewerking | Beschrijving |
|---|---|
|/Accounts/read|API-accounts worden gelezen.|
|accounts/schrijven|Schrijft API-Accounts.|
|/accounts/DELETE|Hiermee verwijdert u de API-accounts|
|/accounts/listKeys/Action|Lijst met sleutels|
|/accounts/regenerateKey/Action|Sleutel opnieuw genereren|
|/accounts/skus/Read|Beschikbare SKU's voor een bestaande resource leest.|
|/accounts/usages/Read|Het gebruik van de quota voor een bestaande resource niet ophalen.|
|Bewerkingen/leestijd|Beschrijving van de bewerking.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| Bewerking | Beschrijving |
|---|---|
|RateCard/leestijd|Retourneert bieden gegevens, metagegevens van de resource/meter en tarieven voor het opgegeven abonnement.|
|UsageAggregates/leestijd|Haalt de Microsoft Azure verbruik door een abonnement. Het resultaat bevat statistische gegevens over het gebruik, abonnement en de resource verwante gegevens, op een bepaalde periode.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Hiermee wordt het abonnement bij de Microsoft.Compute-resourceprovider geregistreerd|
|/restorePointCollections/Read|De eigenschappen van een verzameling van de punt herstellen|
|restorePointCollections/schrijven|Maakt een nieuwe verzameling van de restore-punt of een bestaande bijgewerkt|
|/restorePointCollections/DELETE|Hiermee verwijdert u de herstelbewerking wijst verzameling en opgenomen herstelpunten|
|/restorePointCollections/restorePoints/Read|De eigenschappen van een herstelpunt opgehaald|
|/restorePointCollections/restorePoints/Write|Een nieuw herstelpunt wordt gemaakt|
|/restorePointCollections/restorePoints/DELETE|Hiermee verwijdert u het herstelpunt|
|/restorePointCollections/restorePoints/retrieveSasUris/Action|De eigenschappen van een herstelpunt samen met de blob-SAS URI's opgehaald|
|/virtualMachineScaleSets/Read|Hiermee worden de eigenschappen van de virtuele-machineschaalset opgehaald|
|virtualMachineScaleSets/schrijven|Hiermee wordt een nieuwe virtuele-machineschaalset gemaakt of een bestaande bijgewerkt|
|/virtualMachineScaleSets/DELETE|Hiermee wordt de virtuele-machineschaalset verwijderd|
|/virtualMachineScaleSets/start/Action|Hiermee worden de instanties van de virtuele-machineschaalset gestart|
|/virtualMachineScaleSets/powerOff/Action|Hiermee worden de instanties van de virtuele-machineschaalset uitgeschakeld|
|/virtualMachineScaleSets/restart/Action|Hiermee worden de instanties van de virtuele-machineschaalset opnieuw gestart|
|/virtualMachineScaleSets/deallocate/Action|Hiermee worden de rekenresources voor de instanties van de virtuele-machineschaalset uitgeschakeld en vrijgegeven |
|/virtualMachineScaleSets/manualUpgrade/Action|Hiermee worden instanties handmatig bijgewerkt naar het laatste model van de virtuele-machineschaalset|
|/virtualMachineScaleSets/Scale/Action|Schalen / uitschalen aantal exemplaren van een bestaande virtuele-machineschaalset instellen|
|/virtualMachineScaleSets/instanceView/Read|Hiermee wordt de instantieweergave van de virtuele-machineschaalset opgehaald|
|/virtualMachineScaleSets/skus/Read|Hiermee wordt een lijst weergegeven van de geldige SKU's voor een bestaande virtuele-machineschaalset|
|/virtualMachineScaleSets/virtualMachines/Read|Hiermee worden de eigenschappen van een virtuele machine in een VM-schaalset opgehaald|
|/virtualMachineScaleSets/virtualMachines/DELETE|Een specifieke virtuele machine uit een VM-schaalset verwijderen.|
|/virtualMachineScaleSets/virtualMachines/start/Action|Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset gestart.|
|/virtualMachineScaleSets/virtualMachines/powerOff/Action|Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset uitgeschakeld.|
|/virtualMachineScaleSets/virtualMachines/restart/Action|Hiermee wordt de instantie van een specifieke virtuele machine in een VM-schaalset opnieuw gestart.|
|/virtualMachineScaleSets/virtualMachines/deallocate/Action|Hiermee worden de rekenresources voor een virtuele machine in een VM-schaalset uitgeschakeld en vrijgegeven|
|/virtualMachineScaleSets/virtualMachines/instanceView/Read|Hiermee wordt de instantieweergave van een virtuele machine in een VM-schaalset opgehaald.|
|/images/Read|De eigenschappen van de afbeelding opgehaald|
|installatiekopieën van/schrijven|Een nieuwe installatiekopie gemaakt of een bestaande bijgewerkt|
|/images/DELETE|Hiermee verwijdert u de installatiekopie|
|/Operations/Read|Hiermee wordt een lijst met bewerkingen weergegeven die beschikbaar zijn op de Microsoft.Compute-resourceprovider|
|/Disks/Read|Hiermee worden de eigenschappen van een schijf opgehaald|
|schijven/schrijven|Hiermee wordt een nieuwe schijf gemaakt of een bestaande bijgewerkt|
|/Disks/DELETE|Hiermee wordt de schijf verwijderd|
|/Disks/beginGetAccess/Action|Hiermee wordt de SAS-URI van de schijf opgehaald voor blob-toegang|
|/Disks/endGetAccess/Action|Hiermee wordt de SAS-URI van de schijf ingetrokken|
|/Snapshots/Read|Hiermee worden de eigenschappen van een momentopname opgehaald|
|momentopnamen van/schrijven|Hiermee wordt een nieuwe momentopname gemaakt of een bestaande bijgewerkt|
|/Snapshots/DELETE|Hiermee wordt een momentopname verwijderd|
|/availabilitySets/Read|Hiermee worden de eigenschappen van een beschikbaarheidsset opgehaald|
|/ availabilitySets/schrijven|Hiermee wordt een nieuwe beschikbaarheidsset gemaakt of een bestaande bijgewerkt|
|/availabilitySets/DELETE|Hiermee wordt de beschikbaarheidsset verwijderd|
|/availabilitySets/vmSizes/Read|Lijst met beschikbare grootten voor het maken of bijwerken van een virtuele machine in de beschikbaarheidsset weergeven|
|/virtualMachines/Read|Hiermee worden de eigenschappen van een virtuele machine opgehaald|
|virtuele machines/schrijven|Hiermee wordt een nieuwe virtuele machine gemaakt of een bestaande bijgewerkt|
|/virtualMachines/DELETE|Hiermee wordt de virtuele machine verwijderd|
|/virtualMachines/start/Action|Hiermee wordt de virtuele machine gestart|
|/virtualMachines/powerOff/Action|Hiermee wordt de virtuele machine uitgeschakeld. Houd er rekening mee dat de virtuele machine wordt nog wel gefactureerd.|
|/virtualMachines/redeploy/Action|Virtuele machine redeploys|
|/virtualMachines/restart/Action|Hiermee wordt de virtuele machine opnieuw gestart|
|/virtualMachines/deallocate/Action|Hiermee wordt de virtuele machine uitgeschakeld en worden de rekenresources vrijgegeven|
|/virtualMachines/generalize/Action|Hiermee wordt de status van de virtuele machine ingesteld op Gegeneraliseerd en wordt de virtuele machine voorbereid voor vastleggen|
|/virtualMachines/Capture/Action|Hiermee wordt de virtuele machine vastgelegd via het kopiëren van virtuele harde schijven en wordt een sjabloon gegenereerd waarmee soortgelijke virtuele machines kunnen worden gemaakt|
|/virtualMachines/convertToManagedDisks/Action|De blobs gebaseerde schijven van de virtuele machine converteert naar beheerde schijven|
|/virtualMachines/vmSizes/Read|Hiermee wordt een lijst weergegeven met de beschikbare grootten waarnaar de virtuele machine kan worden bijgewerkt|
|/virtualMachines/instanceView/Read|Hiermee wordt de gedetailleerde runtimestatus van de virtuele machine en bijbehorende resources opgehaald|
|/virtualMachines/Extensions/Read|Hiermee worden de eigenschappen van de extensie van een virtuele machine opgehaald|
|/virtualMachines/Extensions/Write|Hiermee wordt een nieuwe extensie van een virtuele machine gemaakt of een bestaande extensie bijgewerkt|
|/virtualMachines/Extensions/DELETE|Hiermee wordt de extensie van de virtuele machine verwijderd|
|/Locations/vmSizes/Read|Hiermee wordt een lijst weergegeven met de beschikbare grootten van virtuele machines op een locatie|
|/Locations/usages/Read|Hiermee worden servicelimieten en huidige gebruikshoeveelheden opgehaald voor de rekenresources van het abonnement op een locatie|
|/Locations/Operations/Read|Hiermee wordt de status van een asynchrone bewerking opgehaald|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Het abonnement voor de container register-resourceprovider geregistreerd en het maken van de container registers maakt.|
|/checknameavailability/Read|Controleert die registernaam geldig is en niet wordt gebruikt.|
|/registries/Read|Retourneert de lijst met container registers of haalt u de eigenschappen voor het register opgegeven container.|
|registers/schrijven|Een container register maakt met de opgegeven parameters of bijwerken van de eigenschappen of labels voor het register opgegeven container.|
|/registries/DELETE|Hiermee verwijdert u een bestaande container-register.|
|/registries/listCredentials/Action|Hier worden de aanmeldingsreferenties voor het register opgegeven container.|
|/registries/regenerateCredential/Action|Genereert de aanmeldingsreferenties voor het register opgegeven container.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| Bewerking | Beschrijving |
|---|---|
|/containerServices/Subscriptions/Read|De opgegeven containerservices ophalen op basis van abonnement|
|/containerServices/resourceGroups/Read|De opgegeven containerservices ophalen op basis van resourcegroep|
|/containerServices/resourceGroups/ContainerServiceName/Read|De opgegeven containerservice ophalen|
|/containerServices/resourceGroups/ContainerServiceName/Write|De opgegeven containerservice instellen of bijwerken|
|/containerServices/resourceGroups/ContainerServiceName/DELETE|De opgegeven containerservice verwijderen|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| Bewerking | Beschrijving |
|---|---|
|/ updateCommunicationPreference/actie|Communicatievoorkeur bijwerken|
|/ listCommunicationPreference/actie|Lijst communicatievoorkeur|
|/Applications/Read|Leesbewerking|
|/ applications/schrijven|Schrijfbewerking|
|/ applications/schrijven|Schrijfbewerking|
|/Applications/DELETE|Het verwijderen|
|/Applications/listSecrets/Action|Lijst met geheimen|
|/Applications/listSingleSignOnToken/Action|Eenmalige aanmelding Tokens lezen|
|/Operations/Read|leesbewerkingen|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| Bewerking | Beschrijving |
|---|---|
|/hubs/Read|Een Azure klant Insights Hub lezen|
|hubs/schrijven|Maken of bijwerken van een Azure klant Insights Hub|
|/hubs/DELETE|Verwijderen van een Azure klant Insights Hub|
|/hubs/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee worden de beschikbare metrische gegevens voor de resource opgehaald|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/Read|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/Write|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/hubs/providers/Microsoft.Insights/logDefinitions/Read|De beschikbare logboeken opgehaald voor resource|
|/hubs/authorizationPolicies/Read|Geen inzichten Azure klant gedeeld toegangsbeleid handtekening lezen|
|/hubs/authorizationPolicies/Write|Een Azure klant Insights Shared Access Signature beleid maken of bijwerken|
|/hubs/authorizationPolicies/DELETE|Een Azure klant Insights Shared Access Signature-beleid verwijderen|
|/hubs/authorizationPolicies/regeneratePrimaryKey/Action|Azure klant Insights handtekening beleid voor gedeelde toegang primaire sleutel opnieuw genereren|
|/hubs/authorizationPolicies/regenerateSecondaryKey/Action|Azure klant Insights handtekening beleid voor gedeelde toegang secundaire sleutel opnieuw genereren|
|/hubs/Profiles/Read|Een Azure klant Insights profiel lezen|
|/hubs/Profiles/Write|Schrijven van een Azure klant Insights-profiel|
|/hubs/KPI/Read|Een Azure klant Insights Key Performance Indicator lezen|
|/hubs/KPI/Write|Maken of bijwerken van een Azure klant Insights Key Performance Indicator|
|/hubs/KPI/DELETE|Een Azure klant Insights Key Performance Indicator verwijderen|
|/hubs/Views/Read|Elke weergave Azure klant Insights-App lezen|
|/hubs/Views/Write|Maken of bijwerken van een Azure klant Insights App weergeven|
|/hubs/Views/DELETE|Elke klant Azure Insights App weergave verwijderen|
|/hubs/interactions/Read|Elke klant Azure Insights interactie lezen|
|/hubs/interactions/Write|Maken of bijwerken van elke klant Azure Insights interactie|
|/hubs/roleAssignments/Read|Lezen van een Azure klant Insights Rbac-toewijzing|
|/hubs/roleAssignments/Write|Maken of bijwerken van een Azure klant Insights Rbac-toewijzing|
|/hubs/roleAssignments/DELETE|Verwijderen van een Azure klant Insights Rbac-toewijzing|
|/hubs/connectors/Read|Lezen van een Azure klant Insights-Connector|
|/hubs/connectors/Write|Maken of bijwerken van een Azure klant Insights-Connector|
|/hubs/connectors/DELETE|Elke klant Azure Insights-Connector verwijderen|
|/hubs/connectors/mappings/Read|Lezen van een toewijzing van Azure klant Insights-Connector|
|/hubs/connectors/mappings/Write|Maken of bijwerken van een toewijzing van Azure klant Insights-Connector|
|/hubs/connectors/mappings/DELETE|Een toewijzing van Azure klant Insights-Connector verwijderen|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Controleert de beschikbaarheid van de catalogus-naam voor de tenant.|
|/Catalogs/Read|Eigenschappen voor catalogi onder het abonnement of resourcegroep worden opgehaald.|
|catalogussen/schrijven|Catalogus maken of bijwerken van de labels en eigenschappen voor de catalogus.|
|/Catalogs/DELETE|Hiermee verwijdert u de catalogus.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| Bewerking | Beschrijving |
|---|---|
|/datafactories/Read|Data Factory leest.|
|datafactories/schrijven|Maken of bijwerken van de Data Factory|
|/datafactories/DELETE|Hiermee verwijdert u Data Factory.|
|/datafactories/datapipelines/Read|Leest de pijplijn.|
|/datafactories/datapipelines/DELETE|Hiermee verwijdert u een pijplijn.|
|/datafactories/datapipelines/pause/Action|De pijplijn wordt onderbroken.|
|/datafactories/datapipelines/Resume/Action|De pijplijn wordt hervat.|
|/datafactories/datapipelines/update/Action|Pijplijn van updates.|
|/datafactories/datapipelines/Write|Maken of bijwerken van de pijplijn|
|/datafactories/linkedServices/Read|Leest de gekoppelde service.|
|/datafactories/linkedServices/DELETE|Hiermee verwijdert u een gekoppelde service.|
|/datafactories/linkedServices/Write|Maken of bijwerken gekoppelde service|
|/datafactories/Tables/Read|Tabel leest.|
|/datafactories/Tables/DELETE|Hiermee verwijdert u een tabel.|
|/datafactories/Tables/Write|Maken of bijwerken|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| Bewerking | Beschrijving |
|---|---|
|/Accounts/read|Informatie ophalen over de DataLakeAnalytics-account.|
|accounts/schrijven|Maken of bijwerken van het account DataLakeAnalytics.|
|/accounts/DELETE|De account DataLakeAnalytics verwijderen.|
|/accounts/firewallRules/Read|Informatie ophalen over een firewallregel.|
|/accounts/firewallRules/Write|Maken of bijwerken van een firewallregel.|
|/accounts/firewallRules/DELETE|Een firewallregel verwijderen.|
|/accounts/storageAccounts/Read|Gekoppelde Storage-account voor het account DataLakeAnalytics niet ophalen.|
|/accounts/storageAccounts/Write|Storage-account koppelen aan de DataLakeAnalytics-account.|
|/accounts/storageAccounts/DELETE|Een opslagaccount van het account DataLakeAnalytics ontkoppelen.|
|/accounts/storageAccounts/containers/Read|Containers onder het Storage-account ophalen|
|/accounts/storageAccounts/containers/listSasTokens/Action|Lijst met SAS-Tokens voor de Storage-container|
|/accounts/dataLakeStoreAccounts/Read|Gekoppelde DataLakeStore-account voor het account DataLakeAnalytics niet ophalen.|
|/accounts/dataLakeStoreAccounts/Write|Koppel een account DataLakeStore aan het account DataLakeAnalytics.|
|/accounts/dataLakeStoreAccounts/DELETE|Een account DataLakeStore van het account DataLakeAnalytics ontkoppelen.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| Bewerking | Beschrijving |
|---|---|
|/Accounts/read|Informatie ophalen over een bestaand DataLakeStore-account.|
|accounts/schrijven|Een nieuw DataLakeStore-account maken of bijwerken van een bestaand DataLakeStore-account.|
|/accounts/DELETE|Een bestaand DataLakeStore-account verwijderen.|
|/accounts/firewallRules/Read|Informatie ophalen over een firewallregel.|
|/accounts/firewallRules/Write|Maken of bijwerken van een firewallregel.|
|/accounts/firewallRules/DELETE|Een firewallregel verwijderen.|
|/accounts/trustedIdProviders/Read|Informatie ophalen over een vertrouwde id-provider.|
|/accounts/trustedIdProviders/Write|Maken of bijwerken van een vertrouwde id-provider.|
|/accounts/trustedIdProviders/DELETE|Verwijderen van een vertrouwde id-provider.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Het abonnement voor de IotHub-resourceprovider registreren en wordt het maken van resources met IotHub|
|/ checkNameAvailability/actie|Controleer als IotHub naam beschikbaar is|
|Gebruik/lezen|Neem abonnement informatie over het gebruik voor deze provider.|
|/ operations/lezen|Alle bewerkingen van de ResourceProvider ophalen|
|/ iotHubs/lezen|De IotHub-resources opgehaald|
|/ iotHubs/schrijven|Maken of bijwerken van de IotHub-bron|
|/ iotHubs/verwijderen|IotHub-bron verwijderen|
|/iotHubs/listkeys/Action|Alle IotHub-sleutels ophalen|
|/iotHubs/exportDevices/Action|Exporteren van apparaten|
|/iotHubs/importDevices/Action|Apparaten importeren|
|IotHubs/metricDefinitions/leestijd|Hiermee wordt de beschikbare metrische gegevens voor de IotHub-service|
|/iotHubs/iotHubKeys/listkeys/Action|Ophalen van IotHub Key voor de opgegeven naam|
|/iotHubs/iotHubStats/Read|IotHub-statistieken opvragen|
|/iotHubs/quotaMetrics/Read|Quotum metrische gegevens ophalen|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|EventHub Consumergroep maken|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|EventHub Consumer groep(en) ophalen|
|/iotHubs/eventHubEndpoints/consumerGroups/DELETE|EventHub Consumer-groep verwijderen|
|/iotHubs/Routing/routes/$ MATLAB/actie|Een bericht met alle bestaande Routes testen|
|/iotHubs/Routing/routes/$ testnew/actie|Een bericht met een opgegeven test Route testen|
|IotHubs/diagnosticSettings/leestijd|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/ IotHubs/diagnosticSettings/schrijven|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/iotHubs/skus/Read|Geldige IotHub Skus ophalen|
|/iotHubs/Jobs/Read|Details van ingediend op gegeven IotHub taken ophalen|
|/iotHubs/routingEndpointsHealth/Read|Hiermee wordt de status van alle routering eindpunten voor een IotHub|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| Bewerking | Beschrijving |
|---|---|
|/ Abonnement/registreren/actie|Registreert het abonnement|
|/Labs/DELETE|Labs verwijderen.|
|/Labs/Read|Labs lezen.|
|labs/schrijven|Toevoegen of wijzigen van labs.|
|/Labs/ListVhds/Action|Lijst van installatiekopieën van de schijven beschikbaar zijn voor het maken van aangepaste installatiekopie.|
|/Labs/GenerateUploadUri/Action|Een URI genereren voor het aangepaste schijfkopieën uploaden naar een testomgeving.|
|/Labs/CreateEnvironment/Action|Virtuele machines maken in een testomgeving.|
|/Labs/ClaimAnyVm/Action|Een willekeurige claimable virtuele machine in de testomgeving claimen.|
|/Labs/ExportResourceUsage/Action|Het brongebruik lab exporteert naar een opslagaccount|
|/Labs/Users/DELETE|Gebruikersprofielen verwijderen.|
|/Labs/Users/Read|Gebruikersprofielen worden gelezen.|
|/Labs/Users/Write|Toevoegen of wijzigen van gebruikersprofielen.|
|/Labs/Users/secrets/DELETE|Geheimen verwijderen.|
|/Labs/Users/secrets/Read|Lezen van geheimen.|
|/Labs/Users/secrets/Write|Toevoegen of wijzigen van geheimen.|
|/Labs/Users/Environments/DELETE|Verwijder omgevingen.|
|/Labs/Users/Environments/Read|Lezen omgevingen.|
|/Labs/Users/Environments/Write|Toevoegen of wijzigen van omgevingen.|
|/Labs/Users/Disks/DELETE|Schijven worden verwijderd.|
|/Labs/Users/Disks/Read|Schijven worden gelezen.|
|/Labs/Users/Disks/Write|Toevoegen of wijzigen van de schijven.|
|/Labs/Users/Disks/Attach/Action|Koppelen en maken van de lease van de schijf voor de virtuele machine.|
|/Labs/Users/Disks/detach/Action|Ontkoppel en de lease van de schijf die is gekoppeld aan de virtuele machine wordt verbroken.|
|/Labs/customImages/DELETE|Verwijderen van aangepaste installatiekopieën.|
|/Labs/customImages/Read|Lezen van aangepaste installatiekopieën.|
|/Labs/customImages/Write|Toevoegen of wijzigen van aangepaste installatiekopieën.|
|/Labs/serviceRunners/DELETE|Service uitlopers verwijderen.|
|/Labs/serviceRunners/Read|Service uitlopers lezen.|
|/Labs/serviceRunners/Write|Toevoegen of wijzigen van de service uitlopers.|
|/Labs/artifactSources/DELETE|Verwijderen van artefacten gegevensbronnen.|
|/Labs/artifactSources/Read|Artefacten bronnen lezen.|
|/Labs/artifactSources/Write|Toevoegen of wijzigen van artefacten bronnen.|
|/Labs/artifactSources/artifacts/Read|Lezen van artefacten.|
|/Labs/artifactSources/artifacts/GenerateArmTemplate/Action|Genereert een ARM-sjabloon voor het opgegeven artefact, uploadt de vereiste bestanden naar een opslagaccount en valideert het gegenereerde artefact.|
|/Labs/artifactSources/armTemplates/Read|Lezen van azure resource manager-sjablonen.|
|/Labs/costs/Read|Kosten worden gelezen.|
|/Labs/costs/Write|Toevoegen of wijzigen van de kosten.|
|/Labs/virtualNetworks/DELETE|Verwijder de virtuele netwerken.|
|/Labs/virtualNetworks/Read|Virtuele netwerken gelezen.|
|/Labs/virtualNetworks/Write|Toevoegen of wijzigen van virtuele netwerken.|
|/Labs/Formulas/DELETE|Verwijder de formules.|
|/Labs/Formulas/Read|Lees de formules.|
|/Labs/Formulas/Write|Toevoegen of wijzigen van formules.|
|/Labs/Schedules/DELETE|Verwijderen van schema's.|
|/Labs/Schedules/Read|Schema's worden gelezen.|
|/Labs/Schedules/Write|Toevoegen of wijzigen van schema's.|
|/Labs/Schedules/Execute/Action|Een schema worden uitgevoerd.|
|/Labs/Schedules/ListApplicable/Action|Geeft een lijst van alle toepasselijke schema 's|
|/Labs/galleryImages/Read|Afbeeldingen worden gelezen.|
|/Labs/policySets/EvaluatePolicies/Action|Lab beleid evalueert.|
|/Labs/policySets/Policies/DELETE|Beleid verwijderen.|
|/Labs/policySets/Policies/Read|Lezen van beleid.|
|/Labs/policySets/Policies/Write|Toevoegen of wijzigen van beleid.|
|/Labs/virtualMachines/DELETE|Verwijder de virtuele machines.|
|/Labs/virtualMachines/Read|Virtuele machines gelezen.|
|/Labs/virtualMachines/Write|Hiermee worden virtuele machines toegevoegd of gewijzigd.|
|/Labs/virtualMachines/start/Action|Een virtuele machine start.|
|/Labs/virtualMachines/Stop/Action|Een virtuele machine stoppen|
|/Labs/virtualMachines/ApplyArtifacts/Action|Artefacten van toepassing op virtuele machine.|
|/Labs/virtualMachines/AddDataDisk/Action|Een nieuwe of bestaande gegevensschijf koppelen aan de virtuele machine.|
|/Labs/virtualMachines/DetachDataDisk/Action|Loskoppelen van de opgegeven schijf van de virtuele machine.|
|/Labs/virtualMachines/claim/Action|Eigenaar worden van een bestaande virtuele machine|
|/Labs/virtualMachines/ListApplicableSchedules/Action|Geeft een lijst van alle toepasselijke schema 's|
|/Labs/virtualMachines/Schedules/DELETE|Verwijderen van schema's.|
|/Labs/virtualMachines/Schedules/Read|Schema's worden gelezen.|
|/Labs/virtualMachines/Schedules/Write|Toevoegen of wijzigen van schema's.|
|/Labs/virtualMachines/Schedules/Execute/Action|Een schema worden uitgevoerd.|
|/Labs/notificationChannels/DELETE|Notificationchannels verwijderen.|
|/Labs/notificationChannels/Read|Notificationchannels lezen.|
|/Labs/notificationChannels/Write|Toevoegen of wijzigen van notificationchannels.|
|/Labs/notificationChannels/Notify/Action|Melding verzonden naar opgegeven kanaal.|
|/Schedules/DELETE|Verwijderen van schema's.|
|/Schedules/Read|Schema's worden gelezen.|
|schema's / schrijven|Toevoegen of wijzigen van schema's.|
|/Schedules/Execute/Action|Een schema worden uitgevoerd.|
|/Schedules/Retarget/Action|Updates van een planning doelresource-id.|
|/Locations/Operations/Read|Lees-en schrijfopdrachten.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| Bewerking | Beschrijving |
|---|---|
|/databaseAccountNames/Read|Controleert of de naam van beschikbaarheid.|
|/databaseAccounts/Read|Een databaseaccount leest.|
|databaseAccounts/schrijven|Bijwerken van een database-accounts.|
|/databaseAccounts/listKeys/Action|Lijst met sleutels van een databaseaccount|
|/databaseAccounts/regenerateKey/Action|Sleutels van een databaseaccount draaien|
|/databaseAccounts/listConnectionStrings/Action|De verbindingsreeksen niet ophalen voor een databaseaccount|
|/databaseAccounts/changeResourceGroup/Action|Resourcegroep van een databaseaccount wijzigen|
|/databaseAccounts/failoverPriorityChange/Action|Failover prioriteiten van regio's van een databaseaccount wijzigen. Dit wordt gebruikt om handmatige failover uit te voeren|
|/databaseAccounts/DELETE|Hiermee verwijdert u de database-accounts.|
|/databaseAccounts/metricDefinitions/Read|Leest de database account metrische definities.|
|/databaseAccounts/metrics/Read|Hiermee wordt de database-account metrische gegevens gelezen.|
|/databaseAccounts/usages/Read|Het gebruik van de account van de database gelezen.|
|/databaseAccounts/databases/Collections/metricDefinitions/Read|De verzameling leest metrische definities.|
|/databaseAccounts/databases/Collections/metrics/Read|Hiermee wordt de verzameling metrische gegevens gelezen.|
|/databaseAccounts/databases/Collections/usages/Read|Leest het gebruik van de verzameling.|
|/databaseAccounts/databases/metricDefinitions/Read|Metrische definities van de database gelezen|
|/databaseAccounts/databases/metrics/Read|Hiermee wordt de database metrische gegevens gelezen.|
|/databaseAccounts/databases/usages/Read|Hiermee wordt het gebruik van de database gelezen.|
|/databaseAccounts/readonlykeys/Read|Leest de database alleen-lezen sleutels.|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| Bewerking | Beschrijving |
|---|---|
|/ generateSsoRequest/actie|Genereren van een aanvraag voor het aanmelden bij domein control center.|
|/ validateDomainRegistrationInformation/actie|Aankoop domeinobject valideren zonder deze te verzenden|
|/ checkDomainAvailability/actie|Controleer of een domein aanschaffen is|
|/ listDomainRecommendations/actie|Ophalen van de lijst domein aanbevelingen op basis van trefwoorden|
|registratie-/ actie|Registreer de provider van Microsoft Domains resource voor het abonnement|
|/ domeinen/lezen|De lijst met domeinen ophalen|
|/ domeinen/schrijven|Een nieuw domein toevoegen of een bestaande bijgewerkt|
|/ domeinen/verwijderen|Een bestaand domein verwijdert.|
|/Domains/operationresults/Read|De bewerking van een domein ophalen|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| Bewerking | Beschrijving |
|---|---|
|/lcsprojects/Read|Microsoft Dynamics Lifecycle Services-projecten die deel uitmaken van een gebruiker weergeven|
|lcsprojects/schrijven|Maken en bijwerken van de levenscyclus van Services van Microsoft Dynamics-projecten die deel uitmaken van de gebruiker. Alleen de naam en beschrijving van eigenschappen kunnen worden bijgewerkt. Het abonnement en de locatie die is gekoppeld aan het project kunnen niet worden bijgewerkt nadat het maken|
|/lcsprojects/DELETE|Microsoft Dynamics Lifecycle Services-projecten die deel uitmaken van de gebruiker verwijderen|
|/lcsprojects/clouddeployments/Read|Microsoft Dynamics AX 2012 R3 Evaluation-implementaties in een Microsoft Dynamics Lifecycle Services-project die deel uitmaken van een gebruiker weergeven|
|/lcsprojects/clouddeployments/Write|Microsoft Dynamics AX 2012 R3 evaluatie-implementatie in een project Microsoft Dynamics levenscyclus van Services die deel uitmaken van een gebruiker maken. Implementaties kunnen worden beheerd vanaf de Azure-beheerportal|
|/lcsprojects/connectors/Read|Lezen van connectors die deel uitmaken van een project Microsoft Dynamics levenscyclus van Services|
|/lcsprojects/connectors/Write|Maken en bijwerken van connectors die deel uitmaken van een project Microsoft Dynamics levenscyclus van Services|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd.|
|registratie-/ actie|Hiermee wordt het abonnement voor de EventHub-resourceprovider geregistreerd en wordt het maken van EventHub-resources mogelijk|
|naamruimten/schrijven|Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Tags en status van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt.|
|/Namespaces/Read|De lijst met beschrijvingen van resources van naamruimten ophalen|
|/ naamruimten/verwijderen|Namespace-bron verwijderen|
|/Namespaces/metricDefinitions/Read|Lijst met Namespace metrische Resource beschrijvingen|
|/Namespaces/authorizationRules/Read|De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen.|
|/Namespaces/authorizationRules/Write|Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/authorizationRules/DELETE|Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd. |
|/Namespaces/authorizationRules/listkeys/Action|De verbindingstekenreeks naar de naamruimte ophalen|
|/Namespaces/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/eventhubs/Write|Maken of bijwerken EventHub-eigenschappen.|
|/Namespaces/eventhubs/Read|Lijst met beschrijvingen van EventHub-Resource|
|/Namespaces/eventhubs/DELETE|Bewerking EventHub-bron verwijderen|
|/Namespaces/eventHubs/consumergroups/Write|Maken of bijwerken ConsumerGroup eigenschappen.|
|/Namespaces/eventHubs/consumergroups/Read|Lijst met beschrijvingen van de Resource ConsumerGroup ophalen|
|/Namespaces/eventHubs/consumergroups/DELETE|Bewerking ConsumerGroup bron verwijderen|
|/Namespaces/eventhubs/authorizationRules/Read| Haal de lijst van EventHub-autorisatieregels|
|/Namespaces/eventhubs/authorizationRules/Write|Autorisatieregels EventHub maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/eventhubs/authorizationRules/DELETE|Bewerking voor het verwijderen van EventHub-autorisatieregels|
|/Namespaces/eventhubs/authorizationRules/listkeys/Action|De verbindingsreeks naar EventHub ophalen|
|/Namespaces/eventhubs/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/diagnosticSettings/Read|Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen|
|/Namespaces/diagnosticSettings/Write|Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen|
|/Namespaces/logDefinitions/Read|Lijst met Namespace logboeken Resource beschrijvingen|

## <a name="microsoftfeatures"></a>Microsoft.Features

| Bewerking | Beschrijving |
|---|---|
|/providers/Features/Read|Hiermee kunt u de functie van een abonnement voor een opgegeven resourceprovider ophalen.|
|/providers/Features/register/Action|Hiermee kunt u de functie van een abonnement voor een opgegeven resourceprovider registreren.|
|/Features/Read|Hiermee kunt u de functies van een abonnement ophalen.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| Bewerking | Beschrijving |
|---|---|
|clusters/schrijven|Maken of bijwerken van HDInsight-Cluster|
|/clusters/Read|Meer informatie over de HDInsight-Cluster|
|/clusters/DELETE|Een HDInsight-Cluster verwijderen|
|/clusters/changerdpsetting/Action|RDP-instelling voor HDInsight-Cluster wijzigen|
|/clusters/Configurations/Action|Werk de configuratie van de HDInsight-Cluster|
|/clusters/Configurations/Read|HDInsight-Cluster configuraties ophalen|
|/clusters/roles/Resize/Action|Een HDInsight-Cluster vergroten of verkleinen|
|/Locations/Capabilities/Read|Ophalen van abonnement mogelijkheden|
|/Locations/checkNameAvailability/Read|Controleer de naam van de beschikbaarheid|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Registreert het abonnement voor de resourceprovider voor importeren/exporteren en wordt het maken van de taken voor importeren/exporteren.|
|taken/schrijven|Maakt een taak met de opgegeven parameters of bijwerken van de eigenschappen of labels voor de gespecificeerde taak.|
|/jobs/Read|Hiermee wordt de eigenschappen voor de opgegeven taak of de lijst met taken geretourneerd.|
|/jobs/listBitLockerKeys/Action|Hiermee haalt u de BitLocker-sleutels voor de gespecificeerde taak.|
|/jobs/DELETE|Hiermee verwijdert u een bestaande taak.|
|/Locations/Read|Haalt u de eigenschappen voor de opgegeven locatie of de lijst met locaties geretourneerd.|

## <a name="microsoftinsights"></a>Microsoft.Insights

| Bewerking | Beschrijving |
|---|---|
|/ Registreren/actie|De Microsoft Insights-provider registeren|
|/ AlertRules/schrijven|Naar de configuratie van een waarschuwingsregel schrijven|
|/ AlertRules/verwijderen|Configuratie van een waarschuwingsregel verwijderen|
|AlertRules/leestijd|Configuratie van een waarschuwingsregel lezen|
|/ AlertRules/geactiveerd/actie|Waarschuwingsregel geactiveerd|
|/ AlertRules/opgelost/actie|Waarschuwingsregel opgelost|
|/ AlertRules/beperkt/actie|Waarschuwingsregel is beperkt|
|AlertRules/incidenten/leestijd|Incidentconfiguratie van een waarschuwingsregel lezen|
|MetricDefinitions/leestijd|Metrische definities lezen|
|/EventTypes/Values/Read|Waarden beheergebeurtenistype lezen|
|/EventTypes/digestevents/Read|Samenvatting beheergebeurtenistype lezen|
|Metrische gegevens/leestijd|Metrische gegevens lezen|
|/ LogProfiles/schrijven|Schrijven naar een logboek-profielconfiguratie|
|/ LogProfiles/verwijderen|Logboek profielen configuratie verwijderen|
|LogProfiles/leestijd|Profielen voor lees-logboek|
|/ AutoscaleSettings/schrijven|Naar een configuratie-instelling voor automatisch schalen schrijven|
|/ AutoscaleSettings/verwijderen|Configuratie-instelling voor automatisch schalen verwijderen|
|AutoscaleSettings/leestijd|Configuratie-instelling voor automatisch schalen lezen|
|/ AutoscaleSettings/Scaleup/actie|Bewerking Omhoog schalen via automatisch schalen|
|/ AutoscaleSettings/Scaledown/actie|Schaal omlaag bewerking automatisch schalen|
|/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read|Metrische definities lezen|
|/ ActivityLogAlerts/geactiveerd/actie|De activiteit logboek waarschuwing is geactiveerd|
|/ DiagnosticSettings/schrijven|Schrijven naar de configuratie voor de diagnostische instellingen|
|/ DiagnosticSettings/verwijderen|Verwijderen van configuratie van diagnostische instellingen|
|DiagnosticSettings/leestijd|Een configuratie voor de diagnostische instellingen lezen|
|LogDefinitions/leestijd|Logboekdefinities lezen|
|/ ExtendedDiagnosticSettings/schrijven|Schrijven naar de uitgebreide diagnostische instellingen voor configuratie|
|/ ExtendedDiagnosticSettings/verwijderen|Configuratie van diagnostische instellingen voor uitgebreide verwijderd|
|ExtendedDiagnosticSettings/leestijd|Lezen van de configuratie van een uitgebreide diagnostische instellingen|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Registreert een abonnement|
|/checkNameAvailability/Read|Hiermee wordt gecontroleerd of de opgegeven sleutelkluisnaam geldig is en niet wordt gebruikt|
|/vaults/Read|De eigenschappen van een sleutelkluis weergeven|
|kluizen/schrijven|Een nieuwe sleutelkluis maken of de eigenschappen van een bestaande sleutelkluis bijwerken|
|/vaults/DELETE|Een sleutelkluis verwijderen|
|/vaults/Deploy/Action|Geeft toegang tot geheimen in een sleutelkluis bij het implementeren van Azure-resources|
|/vaults/secrets/Read|De eigenschappen van een geheim weergeven, maar niet de waarde|
|/vaults/secrets/Write|Een nieuw geheim maken of de waarde van een bestaand geheim bijwerken|
|/vaults/accessPolicies/Write|Een bestaand toegangsbeleid bijwerken door samenvoegen of vervangen, of een nieuw toegangsbeleid toevoegen aan een kluis.|
|/deletedVaults/Read|De eigenschappen van voorlopig verwijderde sleutelkluizen weergeven|
|/Locations/operationResults/Read|Controleer het resultaat van een bewerking voor de lange termijn|
|/Locations/deletedVaults/Read|De eigenschappen van een voorlopig verwijderde sleutelkluis weergeven|
|/Locations/deletedVaults/Purge/Action|Een voorlopig verwijderde sleutelkluis opschonen|

## <a name="microsoftlogic"></a>Microsoft.Logic

| Bewerking | Beschrijving |
|---|---|
|/workflows/Read|Hiermee wordt de werkstroom gelezen.|
|werkstromen/schrijven|Hiermee wordt de werkstroom gemaakt of bijgewerkt.|
|/workflows/DELETE|Hiermee wordt de werkstroom verwijderd.|
|/workflows/Run/Action|Hiermee wordt de werkstroomuitvoering gestart.|
|/workflows/Disable/Action|Hiermee wordt de werkstroom uitgeschakeld.|
|/workflows/Enable/Action|Hiermee wordt de werkstroom ingeschakeld.|
|/workflows/Validate/Action|Hiermee wordt de werkstroom gevalideerd.|
|/workflows/Move/Action|Hiermee wordt de werkstroom verplaatst van de bestaande abonnements-id, resourcegroep en/of naam naar een andere abonnements-id, resourcegroep en/of naam.|
|/workflows/listSwagger/Action|Hiermee haalt u de werkstroom swagger definities.|
|/workflows/regenerateAccessKey/Action|Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd.|
|/workflows/listCallbackUrl/Action|Hiermee wordt de callback-URL voor de werkstroom.|
|/workflows/Versions/Read|Hiermee wordt de werkstroomversie gelezen.|
|/workflows/Versions/triggers/listCallbackUrl/Action|Hiermee wordt de callback-URL voor de trigger.|
|werkstromen/wordt uitgevoerd/lezen|Hiermee wordt de werkstroomuitvoering gelezen.|
|/workflows/runs/Cancel/Action|Hiermee wordt de werkstroomuitvoering geannuleerd.|
|/workflows/runs/Actions/Read|Hiermee wordt de uitvoeringsactie van de werkstroom gelezen.|
|/workflows/runs/Operations/Read|Hiermee wordt de bewerkingsstatus van de werkstroomuitvoering gelezen.|
|/workflows/triggers/Read|Hiermee wordt de trigger gelezen.|
|/workflows/triggers/Run/Action|Hiermee wordt de trigger uitgevoerd.|
|/workflows/triggers/listCallbackUrl/Action|Hiermee wordt de callback-URL voor de trigger.|
|/workflows/triggers/histories/Read|Hiermee wordt de geschiedenis van de triggers gelezen.|
|/workflows/triggers/histories/resubmit/Action|De trigger werkstroom opnieuw wordt verzonden.|
|/workflows/accessKeys/Read|Hiermee wordt de toegangssleutel gelezen.|
|/workflows/accessKeys/Write|Hiermee wordt de toegangssleutel opnieuw gemaakt of bijgewerkt.|
|/workflows/accessKeys/DELETE|Hiermee wordt de toegangssleutel verwijderd.|
|/workflows/accessKeys/List/Action|Hiermee wordt een lijst met toegangssleutelgeheimen gemaakt.|
|/workflows/accessKeys/Regenerate/Action|Hiermee wordt de toegangssleutelgeheimen opnieuw gegenereerd.|
|/Locations/workflows/Validate/Action|Hiermee wordt de werkstroom gevalideerd.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Het abonnement voor de machine learning web service resourceprovider registreert en wordt het maken van webservices.|
|webServices/actie|Regionale Web Service-eigenschappen voor ondersteunde regio's maken|
|/commitmentPlans/Read|Een Machine streven trainingsplan lezen|
|commitmentPlans/schrijven|Maken of bijwerken van een Machine Learning streven plannen|
|/commitmentPlans/DELETE|Een Machine streven trainingsplan verwijderen|
|/commitmentPlans/join/Action|Lid worden van een Machine streven trainingsplan|
|/commitmentPlans/commitmentAssociations/Read|Lezen van een Machine Learning-streven Plan koppeling|
|/commitmentPlans/commitmentAssociations/Move/Action|Verplaatsen van een Machine Learning-streven Plan koppeling|
|Werkruimten/leestijd|Lezen van een Machine Learning-werkruimte|
|/ Werkruimten/schrijven|Een Machine Learning-werkruimte gemaakt of bijgewerkt|
|-Werkruimten, verwijderen|Verwijderen van een Machine Learning-werkruimte|
|/ Werkruimten/listworkspacekeys/actie|Lijst met sleutels voor een Machine Learning-werkruimte|
|/ Werkruimten/resyncstoragekeys/actie|Sleutels van opslagaccount geconfigureerd voor een Machine Learning-werkruimte synchroniseren|
|/webServices/Read|Lezen van een Machine Learning-webservice|
|webServices/schrijven|Maken of bijwerken van een Machine Learning-webservice|
|/webServices/DELETE|Verwijderen van een Machine Learning-webservice|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| Bewerking | Beschrijving |
|---|---|
|/agreements/Offers/plans/Read|Retourneren van een overeenkomst voor een bepaalde marketplace-item|
|/agreements/Offers/plans/Sign/Action|Meld u aan een overeenkomst voor een bepaalde marketplace-item|
|/agreements/Offers/plans/Cancel/Action|Annuleren van een overeenkomst voor een bepaalde marketplace-item|

## <a name="microsoftmedia"></a>Microsoft.Media

| Bewerking | Beschrijving |
|---|---|
|/mediaservices/Read||
|mediaservices/schrijven||
|/mediaservices/DELETE||
|/mediaservices/regenerateKey/Action||
|/mediaservices/listKeys/Action||
|/mediaservices/syncStorageKeys/Action||

## <a name="microsoftnetwork"></a>Microsoft.Network

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Registreert het abonnement|
|/ unregister/actie|Heft de registratie van het abonnement|
|/ checkTrafficManagerNameAvailability/actie|Controleert de beschikbaarheid van een Traffic Manager ten opzichte van DNS-naam.|
|/dnszones/Read|Ophalen van de DNS-zone, in JSON-indeling. De zone-eigenschappen zijn labels, etag, numberOfRecordSets en maxNumberOfRecordSets. Houd er rekening mee dat deze opdracht niet de recordsets in de zone ophaalt.|
|dnszones/schrijven|Maken of bijwerken van een DNS-zone binnen een resourcegroep.  Gebruikt voor het bijwerken van de labels van een DNS-zone-resource. Houd er rekening mee dat deze opdracht kan niet worden gebruikt om te maken of bijwerken van recordsets binnen de zone.|
|/dnszones/DELETE|Verwijder de DNS-zone, in JSON-indeling. De zone-eigenschappen zijn labels, etag, numberOfRecordSets en maxNumberOfRecordSets.|
|/dnszones/MX/Read|De recordset van het type 'MX', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/MX/Write|Maken of bijwerken van een recordset van het type 'MX' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/MX/DELETE|Verwijder de recordset met een bepaalde naam en typ 'MX' uit een DNS-zone.|
|/dnszones/NS/Read|DNS-recordset van het type NS opgehaald|
|/dnszones/NS/Write|Maken of bijwerken van DNS-recordset van het type NS|
|/dnszones/NS/DELETE|Hiermee verwijdert u de DNS-recordset van het type NS|
|/dnszones/AAAA/Read|De recordset van het type 'AAAA', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/AAAA/Write|Maken of bijwerken van een recordset van het type 'AAAA' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/AAAA/DELETE|Verwijder de recordset met een bepaalde naam en typ 'AAAA' uit een DNS-zone.|
|/dnszones/CNAME/Read|De recordset van het type 'CNAME', in JSON-indeling ophalen. De recordset omvat de TTL, labels en etag.|
|/dnszones/CNAME/Write|Maken of bijwerken van een recordset van het type 'CNAME' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/CNAME/DELETE|Verwijder de recordset met een bepaalde naam en typ 'CNAME' uit een DNS-zone.|
|/dnszones/SOA/Read|DNS-recordset van het type SOA opgehaald|
|/dnszones/SOA/Write|Maken of bijwerken van DNS-recordset van het type SOA|
|/dnszones/SRV/Read|De recordset van het type 'SRV', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/SRV/Write|Maken of bijwerken van de recordset van het type SRV|
|/dnszones/SRV/DELETE|Verwijder de recordset met een bepaalde naam en typ 'SRV' uit een DNS-zone.|
|/dnszones/PTR/Read|De recordset van het type 'PTR', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/PTR/Write|Maken of bijwerken van een recordset van het type 'PTR' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/PTR/DELETE|Verwijder de recordset met een bepaalde naam en typ 'PTR' uit een DNS-zone.|
|/dnszones/A/Read|De recordset van het type "A", in JSON-indeling verkrijgen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/A/Write|Maken of bijwerken van een recordset van het type "A" binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/A/DELETE|Verwijder de recordset met een bepaalde naam en typt u "A" uit een DNS-zone.|
|/dnszones/txt/Read|De recordset van het type 'TXT', in JSON-indeling ophalen. De recordset omvat een lijst met records, alsmede de TTL, labels en etag.|
|/dnszones/txt/Write|Maken of bijwerken van een recordset van het type 'TXT' binnen een DNS-zone. De opgegeven records vervangen de huidige records in de recordset.|
|/dnszones/txt/DELETE|Verwijder de recordset met een bepaalde naam en typ 'TXT' uit een DNS-zone.|
|/dnszones/recordsets/Read|DNS-recordsets in verschillende typen opgehaald|
|/networkInterfaces/Read|Haalt de definitie van een netwerkinterface. |
|networkInterfaces/schrijven|Een netwerkinterface maken of bijwerken van een bestaande netwerkinterface. |
|/networkInterfaces/join/Action|Een virtuele Machine koppelt aan een netwerkinterface|
|/networkInterfaces/DELETE|Hiermee verwijdert u een netwerkinterface|
|/networkInterfaces/effectiveRouteTable/Action|De routetabel is geconfigureerd op de netwerkinterface van de virtuele machine ophalen|
|/networkInterfaces/effectiveNetworkSecurityGroups/Action|Netwerkbeveiligingsgroepen geconfigureerde op Network Interface van de Vm ophalen|
|/networkInterfaces/loadBalancers/Read|Alle load balancers die de netwerkinterface deel van uitmaakt opgehaald|
|/networkInterfaces/ipconfigurations/Read|Hiermee haalt u de definitie van de IP-configuratie voor een netwerk-interface. |
|/publicIPAddresses/Read|De definitie van een openbaar IP-adres ophalen.|
|publicIPAddresses/schrijven|Een openbaar IP-adres maken of bijwerken van een bestaand openbaar IP-adres. |
|/publicIPAddresses/DELETE|Hiermee verwijdert u een openbaar IP-adres.|
|/publicIPAddresses/join/Action|Koppelt een openbare IP-adres|
|/routeFilters/Read|De definitie van een route filter ophalen|
|/routeFilters/join/Action|Lid wordt van een routefilter|
|/routeFilters/DELETE|Hiermee verwijdert u een route filterdefinitie|
|routeFilters/schrijven|Een routefilter maken of bijwerken van een bestaand rotue-filter|
|/routeFilters/Rules/Read|De definitie van een route filter regel ophalen|
|/routeFilters/Rules/Write|Een filterregel route maken of bijwerken van een bestaande route filterregel|
|/routeFilters/Rules/DELETE|Hiermee verwijdert u een route regel filterdefinitie|
|/networkWatchers/Read|Ophalen van de definitie van het netwerk-watcher|
|networkWatchers/schrijven|Een netwerk-watcher maken of bijwerken van een bestaande netwerk-watcher|
|/networkWatchers/DELETE|Hiermee verwijdert u een netwerk-watcher|
|/networkWatchers/configureFlowLog/Action|Hiermee configureert u stroom logboekregistratie voor een doelbron.|
|/networkWatchers/ipFlowVerify/Action|Retourneert of het pakket wordt toegestaan of geweigerd of naar een specifieke bestemming.|
|/networkWatchers/nextHop/Action|Het volgende hoptype retourneren en vervolgens hopen dat IP-adres voor een opgegeven doel- en IP-doeladres.|
|/networkWatchers/queryFlowLogStatus/Action|Hiermee haalt u de status van logboekregistratie voor een bron-stroom.|
|/networkWatchers/queryTroubleshootResult/Action|Het probleemoplossing resultaat van de eerder uitgevoerde of momenteel haalt voor probleemoplossing bewerking uitgevoerd.|
|/networkWatchers/securityGroupView/Action|De geconfigureerde en effectieve netwerkbeveiligingsgroepen toegepast op een virtuele machine weergeven.|
|/networkWatchers/Topology/Action|Hiermee haalt de weergave van resources en hun relaties in een netwerk-niveau in een resourcegroep.|
|/networkWatchers/Troubleshoot/Action|Start het oplossen van een resource netwerken in Azure.|
|/networkWatchers/packetCaptures/queryStatus/Action|Hiermee haalt u informatie over eigenschappen en de status van een pakket vastleggen resource.|
|/networkWatchers/packetCaptures/Stop/Action|Stop de actieve opnamesessie van het pakket.|
|/networkWatchers/packetCaptures/Read|De definitie voor het vastleggen van pakket ophalen|
|/networkWatchers/packetCaptures/Write|Maakt een pakketopname|
|/networkWatchers/packetCaptures/DELETE|Hiermee verwijdert u een pakketopname|
|/loadBalancers/Read|De definitie van een load balancer ophalen|
|loadBalancers/schrijven|Een load balancer maken of bijwerken van een bestaande load balancer|
|/loadBalancers/DELETE|Hiermee verwijdert u een load balancer|
|/loadBalancers/networkInterfaces/Read|Verwijzingen naar alle netwerkinterfaces onder een load balancer ophalen|
|/loadBalancers/loadBalancingRules/Read|Een load balancer load taakverdeling ophalen|
|/loadBalancers/backendAddressPools/Read|Een definitie van het groep back-end-adres van load balancer ophalen|
|/loadBalancers/backendAddressPools/join/Action|Lid wordt van een back-endadresgroep voor load balancer|
|/loadBalancers/inboundNatPools/Read|Een load balancer ophalen inkomende nat-pooldefinitie|
|/loadBalancers/inboundNatPools/join/Action|Lid wordt van een load balancer binnenkomende nat-pool|
|/loadBalancers/inboundNatRules/Read|Een load balancer ophalen inkomende nat-regel definiëren|
|/loadBalancers/inboundNatRules/Write|Een binnenkomende nat-regel van load balancer maken of bijwerken van een bestaande load balancer binnenkomende nat-regel|
|/loadBalancers/inboundNatRules/DELETE|Een binnenkomende nat-regel van load balancer verwijderen|
|/loadBalancers/inboundNatRules/join/Action|Lid wordt van een load balancer binnenkomende nat-regel|
|/loadBalancers/outboundNatRules/Read|Een definitie voor uitgaande nat-regel van load balancer opgehaald|
|/loadBalancers/probes/Read|Een load balancer-test opgehaald|
|/loadBalancers/virtualMachines/Read|Verwijzingen naar alle virtuele machines onder een load balancer ophalen|
|/loadBalancers/frontendIPConfigurations/Read|Een definitie van load balancer frontend IP-configuratie ophalen|
|/trafficManagerGeographicHierarchies/Read|Het Traffic Manager geografisch hiërarchie met regio's die kunnen worden gebruikt met de geografische verkeersrouteringsmethode opgehaald|
|/bgpServiceCommunities/Read|Bgp-Service-community's ophalen|
|/applicationGatewayAvailableWafRuleSets/Read|Toepassingsgateway beschikbaar Waf regelsets opgehaald|
|/virtualNetworks/Read|De definitie van het virtuele netwerk ophalen|
|virtualNetworks/schrijven|Een virtueel netwerk maken of bijwerken van een bestaand virtueel netwerk|
|/virtualNetworks/DELETE|Hiermee verwijdert u een virtueel netwerk|
|/virtualNetworks/peer/Action|Een virtueel netwerk met een ander virtueel netwerk samenwerkt|
|/virtualNetworks/virtualNetworkPeerings/Read|De definitie van een virtueel netwerk peering ophalen|
|/virtualNetworks/virtualNetworkPeerings/Write|Een virtueel netwerk peering of een bestaande virtuele netwerk-peering bijwerken|
|/virtualNetworks/virtualNetworkPeerings/DELETE|Hiermee verwijdert u een virtueel netwerk peering|
|/virtualNetworks/subnets/Read|De subnet-definitie van een virtueel netwerk ophalen|
|/virtualNetworks/subnets/Write|Een virtueel netwerksubnet maken of bijwerken van een bestaand virtueel netwerksubnet|
|/virtualNetworks/subnets/DELETE|Hiermee verwijdert u een virtueel netwerksubnet|
|/virtualNetworks/subnets/join/Action|Lid wordt van een virtueel netwerk|
|/virtualNetworks/subnets/joinViaServiceTunnel/Action|Resource zoals opslagaccount of de SQL-database koppelt aan een subnet Service Tunneling is ingeschakeld.|
|/virtualNetworks/subnets/virtualMachines/Read|Verwijzingen naar alle virtuele machines ophalen in een virtueel netwerksubnet|
|/virtualNetworks/checkIpAddressAvailability/Read|Controleer of IP-adres beschikbaar op de opgegeven virtuele netwerk is|
|/virtualNetworks/virtualMachines/Read|Verwijzingen naar alle virtuele machines ophalen in een virtueel netwerk|
|/expressRouteServiceProviders/Read|Express Route-serviceproviders opgehaald|
|/dnsoperationresults/Read|Resultaten van een DNS-bewerking opgehaald|
|/localnetworkgateways/Read|LocalNetworkGateway opgehaald|
|localnetworkgateways/schrijven|Maken of bijwerken van een bestaande LocalNetworkGateway|
|/localnetworkgateways/DELETE|Hiermee verwijdert u LocalNetworkGateway|
|/trafficManagerProfiles/Read|De profielconfiguratie Traffic Manager-ophalen. Dit omvat DNS-instellingen, instellingen voor verkeersroutering, instellingen voor eindpuntcontrole en de lijst met eindpunten die worden gerouteerd door dit Traffic Manager-profiel.|
|trafficManagerProfiles/schrijven|Een Traffic Manager-profiel maken, of wijzig de configuratie van een bestaand Traffic Manager-profiel. Dit omvat inschakelen of uitschakelen van een profiel en DNS-instellingen, instellingen voor verkeersroutering of instellingen voor eindpuntcontrole wijzigen. Eindpunten die worden gerouteerd door het Traffic Manager-profiel kunnen worden toegevoegd, verwijderd, ingeschakeld of uitgeschakeld.|
|/trafficManagerProfiles/DELETE|Het Traffic Manager-profiel verwijdert. Alle instellingen die zijn gekoppeld aan het Traffic Manager-profiel niet verloren en het profiel kan niet meer worden gebruikt om gegevensverkeer te routeren.|
|/dnsoperationstatuses/Read|Hiermee wordt de status van een DNS-bewerking opgehaald |
|/Operations/Read|Beschikbare bewerkingen ophalen|
|/expressRouteCircuits/Read|Een ExpressRouteCircuit ophalen|
|expressRouteCircuits/schrijven|Maken of bijwerken van een bestaande ExpressRouteCircuit|
|/expressRouteCircuits/DELETE|Een ExpressRouteCircuit verwijderen|
|/expressRouteCircuits/stats/Read|Een ExpressRouteCircuit Stat opgehaald|
|/expressRouteCircuits/peerings/Read|Een ExpressRouteCircuit Peering opgehaald|
|/expressRouteCircuits/peerings/Write|Maken of bijwerken van een bestaande Peering ExpressRouteCircuit|
|/expressRouteCircuits/peerings/DELETE|Hiermee verwijdert u een ExpressRouteCircuit Peering|
|/expressRouteCircuits/peerings/arpTables/Action|Een ExpressRouteCircuit Peering ArpTable opgehaald|
|/expressRouteCircuits/peerings/routeTables/Action|Een ExpressRouteCircuit Peering migratiestatus opgehaald|
|/expressRouteCircuits/peerings /<br>routeTablesSummary/actie|Een overzicht van ExpressRouteCircuit Peering migratiestatus opgehaald|
|/expressRouteCircuits/peerings/stats/Read|Een ExpressRouteCircuit Peering statistieken opgehaald|
|/expressRouteCircuits/authorizations/Read|Een vergunning ExpressRouteCircuit opgehaald|
|/expressRouteCircuits/authorizations/Write|Maken of bijwerken van een bestaande ExpressRouteCircuit-autorisatie|
|/expressRouteCircuits/authorizations/DELETE|Hiermee verwijdert u een ExpressRouteCircuit-autorisatie|
|/Connections/Read|VirtualNetworkGatewayConnection opgehaald|
|verbindingen/schrijven|Maken of bijwerken van een bestaande VirtualNetworkGatewayConnection|
|/Connections/DELETE|Hiermee verwijdert u VirtualNetworkGatewayConnection|
|/Connections/sharedKey/Read|VirtualNetworkGatewayConnection SharedKey opgehaald|
|/Connections/sharedKey/Write|Maken of bijwerken van een bestaande VirtualNetworkGatewayConnection SharedKey|
|/networkSecurityGroups/Read|Een definitie van een netwerk opgehaald|
|networkSecurityGroups/schrijven|Een netwerkbeveiligingsgroep maken of bijwerken van een bestaande netwerkbeveiligingsgroep|
|/networkSecurityGroups/DELETE|Hiermee wordt een netwerkbeveiligingsgroep verwijderd|
|/networkSecurityGroups/join/Action|Lid wordt van een netwerkbeveiligingsgroep|
|/networkSecurityGroups/defaultSecurityRules/Read|Een standaardbeveiligingsregeldefinitie ophalen opgehaald|
|/networkSecurityGroups/securityRules/Read|Een beveiligingsregeldefinitie ophalen opgehaald|
|/networkSecurityGroups/securityRules/Write|Een beveiligingsregel maken of een bestaande beveiligingsregel bijwerken|
|/networkSecurityGroups/securityRules/DELETE|Hiermee verwijdert u een beveiligingsregel|
|/applicationGateways/Read|Een application gateway opgehaald|
|applicationGateways/schrijven|Een toepassingsgateway maken of bijwerken van een application gateway|
|/applicationGateways/DELETE|Een toepassingsgateway verwijderen|
|/applicationGateways/backendhealth/Action|Een back-end toepassingsstatus van de gateway opgehaald|
|/applicationGateways/start/Action|Een application gateway wordt gestart|
|/applicationGateways/Stop/Action|Een application gateway wordt gestopt|
|/applicationGateways/backendAddressPools/join/Action|Een application gateway back-end-adresgroep toevoegen|
|/routeTables/Read|Een routetabeldefinitie opgehaald|
|routeTables/schrijven|Een routetabel maken of een bestaande routetabel bijwerken|
|/routeTables/DELETE|Een routetabeldefinitie verwijderen|
|/routeTables/join/Action|Een routetabel joins|
|/routeTables/routes/Read|De definitie van een route ophalen|
|/routeTables/routes/Write|Een route maken of een bestaande route bijwerken|
|/routeTables/routes/DELETE|Hiermee verwijdert u de definitie van een route|
|/Locations/operationResults/Read|Het bewerkingsresultaat van een asynchrone post- of DELETE-bewerking opgehaald|
|/Locations/checkDnsNameAvailability/Read|Controleert of de DNS-label beschikbaar op de opgegeven locatie is|
|/Locations/usages/Read|Meetgegevens voor softwaregebruik voor resources ophalen|
|/Locations/Operations/Read|Bewerkingsresource ophalen die de status van een asynchrone bewerking vertegenwoordigt|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Hiermee wordt het abonnement voor de NotificationHubs-resourceprovider geregistreerd en wordt het maken van naamruimten en NotificationHubs mogelijk|
|/ CheckNamespaceAvailability/actie|Hiermee wordt gecontroleerd of de resourcenaam voor de naamruimte beschikbaar is binnen de NotificationHub-service.|
|Naamruimten/schrijftijd|Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Tags en status van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt.|
|Naamruimten/leestijd|De lijst met beschrijvingen van resources van naamruimten ophalen|
|Naamruimten, verwijderen|Namespace-bron verwijderen|
|/ Naamruimten/authorizationRules/actie|De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen.|
|/ Naamruimten/CheckNotificationHubAvailability/actie|Hiermee wordt gecontroleerd of de NotificationHub-naam beschikbaar is binnen een naamruimte.|
|Naamruimten/authorizationRules/schrijftijd|Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|Naamruimten/authorizationRules/leestijd|De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen.|
|Naamruimten, authorizationRules/verwijderen|Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd. |
|/ Naamruimten/authorizationRules/listkeys/actie|De verbindingstekenreeks naar de naamruimte ophalen|
|/ Naamruimten/authorizationRules/regenerateKeys/actie|De verificatieregel voor een naamruimte De primaire/secundaire sleutel opnieuw genereren, De sleutel opgeven die opnieuw moet worden gegenereerd|
|Naamruimten/NotificationHubs/schrijftijd|Een Notification Hub maken en bijwerken van de eigenschappen ervan. De eigenschappen zijn hoofdzakelijk PNS-referenties. Autorisatieregels en TTL|
|Naamruimten/NotificationHubs/leestijd|De lijst met beschrijvingen van resources voor Notification Hub ophalen|
|Naamruimten, NotificationHubs/verwijderen|Notification Hub-bron verwijderen|
|/ Naamruimten/NotificationHubs/authorizationRules/actie|De lijst met verificatieregels voor Notification Hub ophalen|
|/ Naamruimten/NotificationHubs/pnsCredentials/actie|Alle Notification Hub PNS referenties ophalen. Dit omvat, WNS, MPNS, APNS, GCM en Baidu-referenties|
|/ Naamruimten/NotificationHubs/debugSend/actie|Een testpushmelding verzenden.|
|Naamruimten/NotificationHubs/metricDefinitions/leestijd|Lijst met Namespace metrische Resource beschrijvingen|
|/Namespaces/NotificationHubs /<br>authorizationRules/schrijven|Notification Hub-autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/NotificationHubs /<br>authorizationRules leestijd|De lijst met verificatieregels voor Notification Hub ophalen|
|/Namespaces/NotificationHubs /<br>authorizationRules/verwijderen|Verificatieregels voor Notification Hub verwijderen|
|/Namespaces/NotificationHubs /<br>listkeys-authorizationRules/actie|De verbindingsreeks naar de Notification Hub ophalen|
|/Namespaces/NotificationHubs /<br>regenerateKeys-authorizationRules/actie|De verificatieregel voor Notification Hub De primaire/secundaire sleutel opnieuw genereren, De sleutel opgeven die opnieuw moet worden gegenereerd|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Een abonnement op een resourceprovider registreren.|
|/linkTargets/Read|Een lijst met bestaande accounts die niet gekoppeld aan een Azure-abonnement zijn. Als deze Azure-abonnement koppeling naar een werkruimte, gebruikt u een klant-id die is geretourneerd door deze bewerking in de eigenschap van de klant-id van de bewerking werkruimte maken.|
|werkruimten/schrijven|Een nieuwe werkruimte of koppelingen naar een bestaande werkruimte wordt gemaakt door de klant-id van de bestaande werkruimte.|
|/Workspaces/Read|Een bestaande werkruimte opgehaald|
|/Workspaces/DELETE|Hiermee verwijdert u een werkruimte. Als u de werkruimte is gekoppeld aan een bestaande werkruimte tijdens het maken worden de werkruimte die deze is gekoppeld aan niet verwijderd.|
|/Workspaces/generateregistrationcertificate/Action|Genereert Registratiecertificaat voor de werkruimte. Dit certificaat wordt gebruikt voor Microsoft System Center Operation Manager verbinden met de werkruimte.|
|/Workspaces/sharedKeys/Action|Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte.|
|/Workspaces/Search/Action|Een zoekopdracht wordt uitgevoerd|
|/Workspaces/Datasources/Read|Gegevensbronnen ophalen onder een werkruimte.|
|/Workspaces/Datasources/Write|Gegevensbronnen onder een werkruimte maken of bijwerken.|
|/Workspaces/Datasources/DELETE|Verwijder de gegevensbronnen onder een werkruimte.|
|/Workspaces/managementGroups/Read|Hiermee haalt de namen en de metagegevens voor System Center Operations Manager-beheergroepen die is verbonden met deze werkruimte.|
|/Workspaces/schema/Read|Hiermee haalt de search-schema voor de werkruimte.  Search schema bevat de blootgestelde velden en hun typen.|
|/Workspaces/usages/Read|Hiermee haalt gebruiksgegevens voor een werkruimte met inbegrip van de hoeveelheid gegevens gelezen door de werkruimte.|
|/Workspaces/intelligencepacks/Read|Geeft een lijst van alle intelligence packs die zichtbaar zijn voor een bepaalde worksapce en ook wordt aangegeven of het pakket is ingeschakeld of uitgeschakeld voor die werkruimte.|
|/Workspaces/intelligencepacks/Enable/Action|Hiermee kunt een intelligence pack voor een bepaalde werkruimte.|
|/Workspaces/intelligencepacks/Disable/Action|Hiermee schakelt u een intelligence pack voor een bepaalde werkruimte.|
|/Workspaces/sharedKeys/Read|Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte.|
|/Workspaces/savedSearches/Read|Een opgeslagen zoekopdracht opgehaald|
|/Workspaces/savedSearches/Write|Hiermee maakt u een opgeslagen zoekopdracht|
|/Workspaces/savedSearches/DELETE|Hiermee verwijdert u een opgeslagen zoekopdracht|
|/Workspaces/storageinsightconfigs/Write|Maakt een nieuwe opslagconfiguratie. Deze configuraties worden gebruikt voor het ophalen van gegevens uit een locatie in een bestaand opslagaccount.|
|/Workspaces/storageinsightconfigs/Read|Hiermee haalt u een configuratie van de opslag.|
|/Workspaces/storageinsightconfigs/DELETE|Hiermee verwijdert u een configuratie van de opslag. Hiermee stopt u Microsoft Operational Insights van lezen van gegevens van het opslagaccount.|
|/Workspaces/configurationScopes/Read|Configuratiebereik ophalen|
|/Workspaces/configurationScopes/Write|Configuratiebereik instellen|
|/Workspaces/configurationScopes/DELETE|Configuratiebereik verwijderen|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Een abonnement op een resourceprovider registreren.|
|oplossingen/schrijven|Nieuwe OMS-oplossing maken|
|/Solutions/Read|Ophalen van OMS oplossing afgesloten|
|/Solutions/DELETE|Verwijderen van bestaande OMS-oplossing|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| Bewerking | Beschrijving |
|---|---|
|/ Kluizen/backupJobsExport/actie|Exporttaken|
|Kluizen/schrijftijd|Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt.|
|Kluizen/leestijd|De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald|
|/ Kluizen/verwijderen|De bewerking verwijderen kluis verwijdert de opgegeven Azure-resource van het type 'kluis'|
|Kluizen/refreshContainers/leestijd|Hiermee vernieuwt u de lijst van de container|
|Kluizen/backupJobsExport/operationResults/leestijd|Retourneert het resultaat van taak exportbewerking.|
|Kluizen/backupOperationResults/leestijd|Hiermee wordt het resultaat van de back-upbewerking voor een Recovery Services-kluis geretourneerd.|
|Kluizen/monitoringAlerts/leestijd|Hiermee haalt de waarschuwingen voor de Recovery services-kluis.|
|/Vaults/monitoringAlerts / {uniqueAlertId} / lezen|Hiermee haalt u de details van de waarschuwing.|
|Kluizen/backupSecurityPIN/leestijd|Beveiliging PINCODE retourneert informatie voor Recovery Services-kluis.|
|/vaults/replicationEvents/Read|Alle gebeurtenissen lezen|
|Kluizen/backupProtectableItems/leestijd|Hiermee wordt een lijst met alle beveiligbare items opgehaald.|
|/vaults/replicationFabrics/Read|Alle Fabrics lezen|
|/vaults/replicationFabrics/Write|Maken of bijwerken van een Fabrics|
|/vaults/replicationFabrics/Remove/Action|Infrastructuur verwijderen|
|/vaults/replicationFabrics/checkConsistency/Action|Hiermee wordt de consistentie van de infrastructuur gecontroleerd|
|/vaults/replicationFabrics/DELETE|Verwijderen van eventuele Fabrics|
|/vaults/replicationFabrics/renewcertificate/Action||
|/vaults/replicationFabrics/deployProcessServerImage/Action|Proces serverinstallatiekopie te implementeren|
|/vaults/replicationFabrics/reassociateGateway/Action|Gateway opnieuw koppelen|
|kluizen/replicationFabrics/replicationRecoveryServicesProviders /<br>Lezen|Een Recovery Services-Providers lezen|
|kluizen/replicationFabrics/replicationRecoveryServicesProviders /<br>verwijderen/actie|Recovery Services-Provider verwijderen|
|kluizen/replicationFabrics/replicationRecoveryServicesProviders /<br>verwijderen|Verwijderen van een Recovery Services-Providers|
|kluizen/replicationFabrics/replicationRecoveryServicesProviders /<br>refreshProvider/actie|Vernieuw de Provider|
|/vaults/replicationFabrics/replicationStorageClassifications/Read|Alle Opslagclassificaties lezen|
|kluizen/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings leestijd|Alle toewijzingen van de classificatie opslag lezen|
|kluizen/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings/schrijven|Maken of bijwerken van alle toewijzingen van de classificatie opslag|
|kluizen/replicationFabrics/replicationStorageClassifications /<br>replicationStorageClassificationMappings/verwijderen|Alle toewijzingen van de classificatie opslag verwijderen|
|/vaults/replicationFabrics/replicationvCenters/Read|Lezen van taken|
|/vaults/replicationFabrics/replicationvCenters/Write|Alle taken maken of bijwerken|
|/vaults/replicationFabrics/replicationvCenters/DELETE|Taken verwijderen|
|/vaults/replicationFabrics/replicationNetworks/Read|Geen netwerken lezen|
|kluizen/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings leestijd|Alle Netwerktoewijzingen lezen|
|kluizen/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings/schrijven|Maken of bijwerken van eventuele Netwerktoewijzingen|
|kluizen/replicationFabrics/replicationNetworks /<br>replicationNetworkMappings/verwijderen|Eventuele Netwerktoewijzingen verwijderen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>Lezen|Beveiliging-Containers gelezen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>discoverProtectableItem/actie|Beveiligbare Item detecteren|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>schrijven|Maken of bijwerken van alle Containers beveiliging|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>verwijderen/actie|De Beveiligingscontainer verwijderen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>switchprotection/actie|Switch Protection Container|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>replicationProtectableItems leestijd|Alle beveiligbare objecten lezen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings leestijd|Alle toewijzingen van de Container beveiliging lezen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/schrijven|Maken of bijwerken van alle toewijzingen van de Container beveiliging|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/verwijderen/actie|Beveiliging Container toewijzing verwijderen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>replicationProtectionContainerMappings/verwijderen|Alle toewijzingen van de Container beveiliging verwijderen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems leestijd|Alle beveiligde Items lezen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/schrijven|Maken of bijwerken van alle beveiligde Items|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/verwijderen|Verwijder alle beveiligde Items|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems/verwijderen/actie|Het beveiligde Item verwijderen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>plannedFailover-replicationProtectedItems/actie|Geplande Failover|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>unplannedFailover-replicationProtectedItems/actie|Failover|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>testFailover-replicationProtectedItems/actie|Failover testen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>testFailoverCleanup-replicationProtectedItems/actie|Het opruimen van testfailover|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>failoverCommit-replicationProtectedItems/actie|Failover doorvoeren|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>replicationProtectedItems, beveiligt/actie|Beveiligde Item opnieuw beveiligen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>updateMobilityService-replicationProtectedItems/actie|Bijwerken van de Mobility-Service|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>repairReplication-replicationProtectedItems/actie|Reparatie replicatie|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>applyRecoveryPoint-replicationProtectedItems/actie|Herstelpunt toepassen|
|kluizen/replicationFabrics/replicationProtectionContainers /<br>recoveryPoints replicationProtectedItems leestijd|Replicatie herstelpunten lezen|
|/vaults/replicationPolicies/Read|Lezen van beleid|
|/vaults/replicationPolicies/Write|Beleid maken of bijwerken|
|/vaults/replicationPolicies/DELETE|Beleid verwijderen|
|/vaults/replicationRecoveryPlans/Read|Alle herstelplannen lezen|
|/vaults/replicationRecoveryPlans/Write|Maken of bijwerken van een herstelplannen|
|/vaults/replicationRecoveryPlans/DELETE|Verwijderen van eventuele herstelplannen|
|/vaults/replicationRecoveryPlans/plannedFailover/Action|Plan voor herstel voor geplande Failover|
|/vaults/replicationRecoveryPlans/unplannedFailover/Action|Plan voor herstel van failover|
|/vaults/replicationRecoveryPlans/testFailover/Action|Test Failover herstelplan|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/Action|Test het opruimen van herstelplan|
|/vaults/replicationRecoveryPlans/failoverCommit/Action|Herstelplan-failover doorvoeren|
|/vaults/replicationRecoveryPlans/reProtect/Action|Plan voor herstel opnieuw beveiligen|
|Kluizen/extendedInformation/leestijd|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|Kluizen/extendedInformation/schrijftijd|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|/ Kluizen/extendedInformation/verwijderen|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|Kluizen/backupManagementMetaData/leestijd|Hiermee worden de metagegevens van back-upbeheer voor een Recovery Services-kluis geretourneerd.|
|Kluizen/backupProtectionContainers/leestijd|Retourneert alle containers die horen bij het abonnement|
|Kluizen/backupFabrics/operationResults/leestijd|Retourneert de status van de bewerking|
|Kluizen/backupFabrics/protectionContainers/leestijd|Retourneert alle geregistreerde containers|
|/ Kluizen/backupFabrics/protectionContainers /<br>operationResults leestijd|Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container.|
|/ Kluizen/backupFabrics/protectionContainers /<br>protectedItems leestijd|Geeft de details van het artikel beveiligd object|
|/ Kluizen/backupFabrics/protectionContainers /<br>protectedItems/schrijven|Een back-up beveiligd Item maken|
|/ Kluizen/backupFabrics/protectionContainers /<br>protectedItems/verwijderen|Hiermee verwijdert u beveiligd Item|
|/ Kluizen/backupFabrics/protectionContainers /<br>back-up-protectedItems/actie|Hiermee wordt een back-up van het beveiligde item gemaakt.|
|/ Kluizen/backupFabrics/protectionContainers /<br>operationResults protectedItems leestijd|Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items.|
|/ Kluizen/backupFabrics/protectionContainers /<br>operationStatus protectedItems leestijd|Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items.|
|/ Kluizen/backupFabrics/protectionContainers /<br>recoveryPoints protectedItems leestijd|Herstelpunten voor beveiligde items ophalen.|
|/ Kluizen/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>/ herstelbewerking|Herstelpunten voor beveiligde items herstellen.|
|/ Kluizen/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>provisionInstantItemRecovery/actie|Inrichten Instant Item herstel voor beveiligde Item|
|/ Kluizen/backupFabrics/protectionContainers /<br>protectedItems/recoveryPoints /<br>revokeInstantItemRecovery/actie|Herstel op directe intrekken voor beveiligde Item|
|Kluizen/gebruik/leestijd|Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd.|
|/vaults/usages/Read|Lezen van het gebruik van een kluis|
|Kluizen/certificaten/schrijftijd|De bewerking Update Resource certificaat updates het referentiecertificaat van de resource/kluis.|
|Kluizen/tokenInfo/leestijd|Retourneert token-informatie voor de Recovery Services-kluis.|
|/vaults/replicationAlertSettings/Read|Alle instellingen voor waarschuwingen lezen|
|/vaults/replicationAlertSettings/Write|Alle waarschuwingsinstellingen maken of bijwerken|
|Kluizen/backupOperations/leestijd|Back-upbewerking retourneert Status voor de Recovery Services-kluis.|
|Kluizen/storageConfig/leestijd|Retourneert opslagconfiguratie voor Recovery Services-kluis.|
|Kluizen/storageConfig/schrijftijd|De opslagconfiguratie updates voor Recovery Services-kluis.|
|Kluizen/backupUsageSummaries/leestijd|Samenvattingen retourneert voor beveiligde Items en beveiligde Servers voor een Recovery Services.|
|Kluizen/backupProtectedItems/leestijd|Hiermee wordt de lijst met alle beveiligde items geretourneerd.|
|Kluizen/backupconfig/vaultconfig/leestijd|Retourneert-configuratie voor Recovery Services-kluis.|
|Kluizen/backupconfig/vaultconfig/schrijftijd|Configuratie van de updates voor Recovery Services-kluis.|
|Kluizen/registeredIdentities/schrijftijd|De bewerking servicecontainer registreren kan worden gebruikt om te registreren van een container met Service voor herstel.|
|Kluizen/registeredIdentities/leestijd|De Containers ophalen bewerking kan worden gebruikt, krijgen de containers die zijn geregistreerd voor een resource.|
|/ Kluizen/registeredIdentities/verwijderen|De Container registratie-bewerking kan worden gebruikt om de registratie van een container te.|
|Kluizen/registeredIdentities/operationResults/leestijd|De bewerking ophalen resulteert de bewerking kan worden gebruikt ophalen de bewerkingsstatus van en het resultaat voor de bewerking asynchroon ingediende|
|/vaults/replicationJobs/Read|Lezen van taken|
|/vaults/replicationJobs/Cancel/Action|Taak annuleren|
|/vaults/replicationJobs/restart/Action|Taak opnieuw starten|
|/vaults/replicationJobs/Resume/Action|Taak hervatten|
|Kluizen/backupPolicies/leestijd|Retourneert alle beleidsregels voor beveiliging|
|Kluizen/backupPolicies/schrijftijd|Hiermee maakt u het beveiligingsbeleid|
|/ Kluizen/backupPolicies/verwijderen|Een beveiligingsbeleid voor verwijderen|
|Kluizen/backupPolicies/operationResults/leestijd|Hiermee worden de resultaten van de beleidsbewerking opgehaald.|
|Kluizen/backupPolicies/operationStatus/leestijd|Status van de beleidsbewerking ophalen.|
|Kluizen/vaultTokens/leestijd|Het Token kluis opnieuw kan worden gebruikt voor bewerkingen van kluis niveau back-end-kluis Token ophalen.|
|Kluizen/monitoringConfigurations/notificationConfiguration/leestijd|Hiermee haalt u de configuratie voor meldingen van Recovery services-kluis.|
|Kluizen/backupJobs/leestijd|Retourneert alle objecten van de taak|
|/ Kluizen/backupJobs / / actie voor annuleren|Annuleer de taak|
|Kluizen/backupJobs/operationResults/leestijd|Hiermee wordt het resultaat van de taakbewerking geretourneerd.|
|/Locations/allocateStamp/Action|AllocateStamp is een interne bewerking die wordt gebruikt door de service|
|/Locations/allocatedStamp/Read|GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service|

## <a name="microsoftrelay"></a>Microsoft.Relay

| Bewerking | Beschrijving |
|---|---|
|/ checkNamespaceAvailability/actie|Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd.|
|registratie-/ actie|Hiermee wordt het abonnement voor de Relay-resourceprovider geregistreerd en wordt het maken van Relay-resources mogelijk|
|naamruimten/schrijven|Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Tags en status van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt.|
|/Namespaces/Read|De lijst met beschrijvingen van resources van naamruimten ophalen|
|/ naamruimten/verwijderen|Namespace-bron verwijderen|
|/Namespaces/authorizationRules/Write|Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/authorizationRules/DELETE|Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd. |
|/Namespaces/authorizationRules/listkeys/Action|De verbindingstekenreeks naar de naamruimte ophalen|
|/Namespaces/HybridConnections/Write|Maken of bijwerken HybridConnection eigenschappen.|
|/Namespaces/HybridConnections/Read|Lijst met beschrijvingen van de Resource HybridConnection ophalen|
|/Namespaces/HybridConnections/DELETE|Bewerking HybridConnection bron verwijderen|
|/Namespaces/HybridConnections/authorizationRules/Write|Autorisatieregels HybridConnection maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/HybridConnections/authorizationRules/DELETE|Bewerking HybridConnection autorisatieregels verwijderen|
|/Namespaces/HybridConnections/authorizationRules/listkeys/Action|De verbindingsreeks naar HybridConnection ophalen|
|/Namespaces/WcfRelays/Write|Maken of bijwerken WcfRelay eigenschappen.|
|/Namespaces/WcfRelays/Read|Lijst met beschrijvingen van de Resource WcfRelay ophalen|
|/Namespaces/WcfRelays/DELETE|Bewerking WcfRelay bron verwijderen|
|/Namespaces/WcfRelays/authorizationRules/Write|Autorisatieregels WcfRelay maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/WcfRelays/authorizationRules/DELETE|Bewerking WcfRelay autorisatieregels verwijderen|
|/Namespaces/WcfRelays/authorizationRules/listkeys/Action|De verbindingsreeks naar WcfRelay ophalen|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| Bewerking | Beschrijving |
|---|---|
|AvailabilityStatuses/leestijd|De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik|
|AvailabilityStatuses/current/leestijd|De beschikbaarheidsstatus ophalen voor de opgegeven resource|

## <a name="microsoftresources"></a>Microsoft.Resources

| Bewerking | Beschrijving |
|---|---|
|/ checkResourceName/actie|Controleer of de resourcenaam geldig is.|
|/providers/Read|De lijst met providers ophalen.|
|/Subscriptions/Read|Hiermee kunt u de lijst met abonnementen ophalen.|
|/Subscriptions/operationresults/Read|Hiermee worden de resultaten van de abonnementsbewerking opgehaald.|
|/Subscriptions/providers/Read|Hiermee kunt u de resourceproviders ophalen of opnemen in een lijst.|
|/Subscriptions/tagNames/Read|Hiermee kunt u abonnementslabels ophalen of opnemen in een lijst.|
|/Subscriptions/tagNames/Write|Hiermee kunt u een abonnementslabel toevoegen.|
|/Subscriptions/tagNames/DELETE|Hiermee kunt u een abonnementslabel verwijderen.|
|/Subscriptions/tagNames/tagValues/Read|Hiermee kunt u abonnementslabelwaarden ophalen of opnemen in een lijst.|
|/Subscriptions/tagNames/tagValues/Write|Hiermee kunt u een abonnementslabelwaarde toevoegen.|
|/Subscriptions/tagNames/tagValues/DELETE|Hiermee kunt een abonnementslabelwaarde verwijderen.|
|/subscriptions/resources/Read|Hiermee kunt u de resources van een abonnement ophalen.|
|/Subscriptions/resourceGroups/Read|Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst.|
|/Subscriptions/resourceGroups/Write|Hiermee kunt u een resourcegroep maken of bijwerken.|
|/Subscriptions/resourceGroups/DELETE|Hiermee kunt u een resourcegroep en de bijhorende resources verwijderen.|
|/Subscriptions/resourceGroups/moveResources/Action|Hiermee kunt u resources van een resourcegroep naar een andere resourcegroep verplaatsen.|
|/Subscriptions/resourceGroups/validateMoveResources/Action|Hiermee wordt de verplaatsing van resources van de ene naar de andere resourcegroep gevalideerd.|
|/Subscriptions/ResourceGroups/resources/Read|Hiermee worden de resources voor de resourcegroep opgehaald.|
|/Subscriptions/ResourceGroups/Deployments/Read|Hiermee kunt u implementaties ophalen of opnemen in een lijst.|
|/Subscriptions/ResourceGroups/Deployments/Write|Hiermee kunt u een implementatie maken of bijwerken.|
|/Subscriptions/ResourceGroups/Deployments/operationstatuses/Read|Hiermee kunt u of implementatie bewerking statussen.|
|/Subscriptions/ResourceGroups/Deployments/Operations/Read|Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst.|
|/Subscriptions/Locations/Read|Hiermee kunt u de lijst met ondersteunde locaties ophalen.|
|/links/Read|Hiermee kunt u resourcekoppelingen ophalen of opnemen in een lijst.|
|koppelingen/schrijven|Hiermee kunt u een resourcekoppeling maken of bijwerken.|
|/links/DELETE|Hiermee kunt u een resourcekoppeling verwijderen.|
|/tenants/Read|Hiermee kunt u de lijst met tenants ophalen.|
|/resources/Read|Hiermee wordt de lijst met resources op basis van filters opgehaald.|
|/Deployments/Read|Hiermee kunt u implementaties ophalen of opnemen in een lijst.|
|implementaties van/schrijven|Hiermee kunt u een implementatie maken of bijwerken.|
|/Deployments/DELETE|Hiermee kunt u een implementatie verwijderen.|
|/Deployments/Cancel/Action|Hiermee kunt u een implementatie annuleren.|
|/Deployments/Validate/Action|Hiermee kunt u een implementatie valideren.|
|/Deployments/Operations/Read|Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| Bewerking | Beschrijving |
|---|---|
|/jobcollections/Read|Taakverzameling ophalen|
|taakverzamelingen/schrijven|Hiermee maakt u een taakverzameling of werkt u er een bij.|
|/jobcollections/DELETE|Hiermee verwijdert u de taakverzameling.|
|/jobcollections/Enable/Action|Kan taakverzameling.|
|/jobcollections/Disable/Action|Taakverzameling wordt uitgeschakeld.|
|/jobcollections/Jobs/Read|Taak opgehaald.|
|/jobcollections/Jobs/Write|Hiermee maakt u een taak of werkt u er een bij.|
|/jobcollections/Jobs/DELETE|Hiermee verwijdert u een taak.|
|/jobcollections/Jobs/Run/Action|De taak wordt uitgevoerd.|
|/jobcollections/Jobs/generateLogicAppDefinition/Action|Hiermee wordt een definitie voor een logische app gegenereerd op basis van een Scheduler-taak.|
|/jobcollections/Jobs/jobhistories/Read|Taakgeschiedenis opgehaald.|

## <a name="microsoftsearch"></a>Microsoft.Search

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Het abonnement voor de resource zoekmachine registreert en wordt het maken van de search-services.|
|/ checkNameAvailability/actie|Controleert de beschikbaarheid van de naam van de service.|
|searchServices/schrijven|Maken of bijwerken van de search-service.|
|/searchServices/Read|Leest de search-service.|
|/searchServices/DELETE|Hiermee verwijdert u de search-service.|
|/searchServices/start/Action|Start de search-service.|
|/searchServices/Stop/Action|De search-service stopt.|
|/searchServices/listAdminKeys/Action|De administratorsleutels leest.|
|/searchServices/regenerateAdminKey/Action|De Administrator-code genereert.|
|/searchServices/createQueryKey/Action|Maakt de querysleutel.|
|/searchServices/queryKey/Read|De querysleutels worden gelezen.|
|/searchServices/queryKey/DELETE|Hiermee verwijdert u de querysleutel.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| Bewerking | Beschrijving |
|---|---|
|/jitNetworkAccessPolicies/Read|Het toegangsbeleid just in time netwerk opgehaald|
|jitNetworkAccessPolicies/schrijven|Hiermee maakt u een nieuw netwerk just-in-time-beleid of een bestaande bijgewerkt|
|/jitNetworkAccessPolicies/initiate/Action|Initieert een netwerktoegangsbeleid just in time|
|/securitySolutionsReferenceData/Read|Referentiegegevens voor de beveiligingsoplossingen opgehaald|
|/securityStatuses/Read|De beveiliging van health-statussen voor Azure-resources opgehaald|
|/webApplicationFirewalls/Read|Het web application firewalls opgehaald|
|webApplicationFirewalls/schrijven|Hiermee maakt u een nieuwe web application firewall of een bestaande bijgewerkt|
|/webApplicationFirewalls/DELETE|Hiermee verwijdert u een web application firewall|
|/securitySolutions/Read|De beveiligingsoplossingen opgehaald|
|securitySolutions/schrijven|Maakt een nieuwe beveiligingsoplossing of een bestaande bijgewerkt|
|/securitySolutions/DELETE|Hiermee verwijdert u een beveiligingsoplossing|
|/Tasks/Read|Alle beschikbare beveiligingsaanbevelingen opgehaald|
|/Tasks/Dismiss/Action|Een beveiligingsaanbeveling negeren|
|/Tasks/Activate/Action|Een beveiligingsaanbeveling activeren|
|/Policies/Read|Het beveiligingsbeleid opgehaald|
|/ beleid/schrijven|Updates van het beveiligingsbeleid|
|/applicationWhitelistings/Read|De toepassing whitelistings opgehaald|
|applicationWhitelistings/schrijven|Hiermee maakt u een nieuwe toepassing whitelisting of een bestaande bijgewerkt|

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

| Bewerking | Beschrijving |
|---|---|
|/ subscriptions/schrijven|Maken of bijwerken van een abonnement|
|gateways/schrijven|Maken of bijwerken van een gateway|
|/gateways/DELETE|Hiermee verwijdert u een gateway|
|/gateways/Read|Een gateway opgehaald|
|/gateways/regenerateprofile/Action|Genereert het gateway-profiel|
|/gateways/upgradetolatest/Action|Upgrades van de gateway naar de nieuwste versie|
|knooppunten/schrijven|maken of bijwerken van een knooppunt|
|/nodes/DELETE|Hiermee verwijdert u een knooppunt|
|/nodes/Read|Een knooppunt opgehaald|
|sessies/schrijven|Maken of bijwerken van een sessie|
|/Sessions/Read|Een sessie opgehaald|
|/Sessions/DELETE|Hiermee verwijdert u een sesssion|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| Bewerking | Beschrijving |
|---|---|
|/ checkNameAvailability/actie|Hiermee wordt de beschikbaarheid van de naamruimte in een bepaald abonnement gecontroleerd.|
|registratie-/ actie|Hiermee wordt het abonnement voor de Service Bus-resourceprovider geregistreerd en wordt het maken van Service Bus-resources mogelijk|
|naamruimten/schrijven|Een Namespace-Resource maken en bijwerken van de eigenschappen ervan. Tags en status van de Namespace zijn de eigenschappen die kunnen worden bijgewerkt.|
|/Namespaces/Read|De lijst met beschrijvingen van resources van naamruimten ophalen|
|/ naamruimten/verwijderen|Namespace-bron verwijderen|
|/Namespaces/metricDefinitions/Read|Lijst met Namespace metrische Resource beschrijvingen|
|/Namespaces/authorizationRules/Write|Een Namespace niveau autorisatieregels maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/authorizationRules/Read|De lijst met beschrijvingen van verificatieregels voor naamruimten ophalen.|
|/Namespaces/authorizationRules/DELETE|Namespace autorisatieregel verwijderen. De standaardregel Namespace autorisatie kan niet worden verwijderd. |
|/Namespaces/authorizationRules/listkeys/Action|De verbindingstekenreeks naar de naamruimte ophalen|
|/Namespaces/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/diagnosticSettings/Read|Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen|
|/Namespaces/diagnosticSettings/Write|Lijst met beschrijvingen van de Resource Namespace diagnostische instellingen|
|/Namespaces/Queues/Write|Maken of wachtrijeigenschappen Update.|
|/Namespaces/Queues/Read|Lijst met beschrijvingen van de wachtrij-Resource|
|/Namespaces/Queues/DELETE|Bewerking bron van de wachtrij verwijderen|
|/Namespaces/Queues/authorizationRules/Write|Autorisatieregels wachtrij maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/Queues/authorizationRules/Read| De lijst met autorisatieregels wachtrij ophalen|
|/Namespaces/Queues/authorizationRules/DELETE|Bewerking autorisatieregels wachtrij verwijderen|
|/Namespaces/Queues/authorizationRules/listkeys/Action|De verbindingsreeks naar wachtrij ophalen|
|/Namespaces/Queues/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/logDefinitions/Read|Lijst met Namespace logboeken Resource beschrijvingen|
|/Namespaces/Topics/Write|Maken of bijwerken onderwerp eigenschappen.|
|/Namespaces/Topics/Read|Lijst met beschrijvingen van de Resource onderwerp|
|/Namespaces/Topics/DELETE|Bewerking onderwerp bron verwijderen|
|/Namespaces/Topics/authorizationRules/Write|Verificatieregels onderwerp maken en bijwerken van de eigenschappen ervan. De toegangsrechten voor het autorisatie-regels, de primaire en secundaire sleutels kunnen worden bijgewerkt.|
|/Namespaces/Topics/authorizationRules/Read| De lijst met verificatieregels onderwerp ophalen|
|/Namespaces/Topics/authorizationRules/DELETE|Bewerking verificatieregels onderwerp verwijderen|
|/Namespaces/Topics/authorizationRules/listkeys/Action|De verbindingsreeks naar onderwerp ophalen|
|/Namespaces/Topics/authorizationRules/regenerateKeys/Action|De primaire of secundaire sleutel voor de resource opnieuw genereren|
|/Namespaces/Topics/Subscriptions/Write|Maken of bijwerken TopicSubscription eigenschappen.|
|/Namespaces/Topics/Subscriptions/Read|Lijst met beschrijvingen van de Resource TopicSubscription ophalen|
|/Namespaces/Topics/Subscriptions/DELETE|Bewerking TopicSubscription bron verwijderen|
|/Namespaces/Topics/Subscriptions/Rules/Write|Maken of de eigenschappen van de regel voor het bijwerken.|
|/Namespaces/Topics/Subscriptions/Rules/Read|Lijst met beschrijvingen van de Resource regel|
|/Namespaces/Topics/Subscriptions/Rules/DELETE|Bewerking bron van de regel verwijderen|

## <a name="microsoftsql"></a>Microsoft.Sql

| Bewerking | Beschrijving |
|---|---|
|/servers/Read|Retourneert een lijst met servers in een resourcegroep voor een abonnement|
|servers/schrijven|Maak een nieuwe server of de eigenschappen van bestaande server in een resourcegroep voor een abonnement wijzigen|
|/servers/DELETE|Verwijderen van een server en alle ingesloten databases en elastische pools|
|/servers/import/Action|Een nieuwe database op de server maken en implementeren van schema en de gegevens uit een DacPac-pakket|
|/servers/upgrade/Action|Nieuwe functionaliteit die beschikbaar is op de nieuwste versie van de server inschakelen en databases edition conversie-kaart|
|/servers/VulnerabilityAssessmentScans/Action|Server-scan voor beoordeling van beveiligingsproblemen uitvoeren|
|/servers/operationResults/Read|Bewerking wordt gebruikt voor het bijhouden van de voortgang van de serverupgrade van lagere versie in hogere|
|/servers/operationResults/DELETE|Upgrade van versie server bezig afbreken|
|/servers/securityAlertPolicies/Read|Ophalen van gegevens van het beleid server threat detectie is geconfigureerd op een bepaalde server|
|/servers/securityAlertPolicies/Write|De server de detectie van dreigingen voor een bepaalde server wijzigen|
|/servers/securityAlertPolicies/operationResults/Read|Resultaten van de detectie van dreigingen beleid Set-bewerking van de server ophalen|
|/servers/Administrators/Read|Administrator-servergegevens ophalen|
|/servers/Administrators/Write|Maken of bijwerken van de serverbeheerder|
|/servers/Administrators/DELETE|Server-beheerder van de server verwijderen|
|/servers/recoverableDatabases/Read|Deze bewerking wordt gebruikt voor herstel na noodgevallen van live-database voor de database herstellen naar laatst bekende goede back-up. Gegevens over de laatste goede back-up worden geretourneerd, maar niet daadwerkelijk herstel van de database.|
|/servers/serviceObjectives/Read|Lijst met serviceniveaudoelstellingen (ook wel bekend als prestatielagen) beschikbaar is op een bepaalde server ophalen|
|/servers/firewallRules/Read|Firewall-regel servergegevens ophalen|
|/servers/firewallRules/Write|Server firewall-regel waarmee u verbinding maken met de server van IP-adresbereik maken of bijwerken|
|/servers/firewallRules/DELETE|Firewall-regel van de server verwijderen|
|/servers/administratorOperationResults/Read|Ophalen van server-beheerder Bewerkingsresultaten|
|/servers/recommendedElasticPools/Read|Aanbevelingen voor pools voor elastische databases kosten verlagen of verbeteren de prestaties op basis van Resourcegebruik historica ophalen|
|/servers/recommendedElasticPools/metrics/Read|Metrische gegevens voor aanbevolen elastische databases voor een bepaalde server ophalen|
|/servers/recommendedElasticPools/databases/Read|Ophalen van databases die moeten worden toegevoegd aan het aanbevolen pools voor elastische databases voor een bepaalde server|
|/servers/elasticPools/Read|Ophalen van gegevens van de pool voor elastische database op een bepaalde server|
|/servers/elasticPools/Write|Maak een nieuwe eigenschappen of wijzigen van bestaande pool voor elastische database|
|/servers/elasticPools/DELETE|Verwijderen van bestaande pool voor elastische database|
|/servers/elasticPools/operationResults/Read|Ophalen van gegevens op een bewerking van de groep van toepassingen opgegeven elastische database|
|/servers/elasticPools/providers/Microsoft.Insights/<br>metricDefinitions leestijd|Retourtypen van metrische gegevens die beschikbaar voor pools voor elastische databases zijn|
|/servers/elasticPools/providers/Microsoft.Insights/<br>diagnosticSettings leestijd|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/servers/elasticPools/providers/Microsoft.Insights/<br>diagnosticSettings/schrijven|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/servers/elasticPools/metrics/Read|Gebruik van metrische gegevens voor elastische database groep resources retourneren|
|/servers/elasticPools/elasticPoolDatabaseActivity/Read|Ophalen van activiteiten en meer informatie over een bepaalde database die deel uitmaakt van een pool voor elastische database|
|/servers/elasticPools/Advisors/Read|Retourneert de lijst met adviseurs die beschikbaar zijn voor de elastische groep|
|/servers/elasticPools/Advisors/Write|Update automatisch-execute status van een advisor op niveau van de elastische groep.|
|/servers/elasticPools/Advisors/recommendedActions/Read|Retourneert de lijst met aanbevolen acties van opgegeven advisor voor de elastische groep|
|/servers/elasticPools/Advisors/recommendedActions/Write|De aanbevolen actie toepassen op de elastische groep|
|/servers/elasticPools/elasticPoolActivity/Read|Ophalen van activiteiten en meer informatie over een bepaalde elastische databasegroep|
|/servers/elasticPools/databases/Read|Ophalen van de lijst en details van de databases die deel van een pool voor elastische database op een bepaalde server uitmaken|
|/servers/auditingPolicies/Read|Ophalen van gegevens van de tabel met standaardtijden server controlebeleid geconfigureerd op een bepaalde server|
|/servers/auditingPolicies/Write|Controle van wijzigingen in de standaard server tabel voor een bepaalde server|
|/servers/disasterRecoveryConfiguration/operationResults/Read|Disaster Recovery configuratie Bewerkingsresultaten ophalen|
|/servers/Advisors/Read|Retourneert de lijst met adviseurs beschikbaar voor de server|
|/servers/Advisors/Write|Updates execute automatisch-status van een advisor op serverniveau.|
|/servers/Advisors/recommendedActions/Read|Retourneert de lijst met aanbevolen acties van opgegeven advisor voor de server|
|/servers/Advisors/recommendedActions/Write|De aanbevolen actie toepassen op de server|
|/servers/usages/Read|DTU-quotum voor server en de huidige DTU consuption geretourneerd door alle databases in de server|
|/servers/elasticPoolEstimates/Read|Retourneert de lijst van de elastische groep schattingen al is gemaakt voor deze server|
|/servers/elasticPoolEstimates/Write|Hiermee maakt u nieuwe elastische pool schatting voor de lijst met databases die zijn opgegeven|
|/servers/auditingSettings/Read|Ophalen van gegevens van de server blob controlebeleid geconfigureerd op een bepaalde server|
|/servers/auditingSettings/Write|De server blob-controle voor een bepaalde server wijzigen|
|/servers/auditingSettings/operationResults/Read|Resultaat van de server blob controle beleid Set-bewerking ophalen|
|/servers/backupLongTermRetentionVaults/Read|Deze bewerking wordt gebruikt voor het ophalen van een back-up langdurig bewaren kluis. Deze retourneert informatie over de kluis geregistreerd met deze server.|
|/servers/backupLongTermRetentionVaults/Write|Een back-up langdurig bewaren kluis registreren|
|/servers/restorableDroppedDatabases/Read|Een lijst met databases die zijn verwijderd op een bepaalde server die binnen het bewaarbeleid ophalen. Deze bewerking wordt een lijst van databases en gekoppelde metagegevens, zoals de datum van verwijdering.|
|/servers/databases/Read|Retourneert een lijst met servers in een resourcegroep voor een abonnement|
|/servers/databases/Write|Maak een nieuwe server of de eigenschappen van bestaande server in een resourcegroep voor een abonnement wijzigen|
|/servers/databases/DELETE|Verwijderen van een server en alle ingesloten databases en elastische pools|
|/servers/databases/export/Action|Een nieuwe database op de server maken en implementeren van schema en de gegevens uit een DacPac-pakket|
|/servers/databases/VulnerabilityAssessmentScans/Action|Database-scan voor beoordeling van beveiligingsproblemen worden uitgevoerd.|
|/servers/databases/pause/Action|Onderbreken van een DataWarehouse-database|
|/servers/databases/Resume/Action|Een DataWarehouse-editie database hervatten|
|/servers/databases/operationResults/Read|Bewerking wordt gebruikt om de voortgang van een langdurige databasebewerking, zoals schaal volgen.|
|/servers/databases/replicationLinks/Read|Geretourneerde gegevens over koppelingen voor databasereplicatie tot stand gebracht voor een bepaalde database|
|/servers/databases/replicationLinks/DELETE|Beëindigen van de replicatierelatie geforceerd en met mogelijk gegevensverlies|
|/servers/databases/replicationLinks/Unlink/Action|Beëindigen van de replicatierelatie geforceerd of na het synchroniseren met de partner|
|/servers/databases/replicationLinks/failover/Action|Failover na het synchroniseren van alle wijzigingen van de primaire waardoor deze database in de replicatierelatie primaire en het aanbrengen van de externe primaire naar een secundair|
|/servers/databases/replicationLinks/forceFailoverAllowDataLoss/Action|Failover onmiddellijk met mogelijk gegevensverlies, waardoor deze database in de replicatierelatie primaire en het aanbrengen van de externe primaire naar een secundair|
|/servers/databases/replicationLinks/updateReplicationMode/Action|Replicatiemodus voor koppeling naar de modus voor synchroon of asynchroon bijwerken|
|/servers/databases/replicationLinks/operationResults/Read|Status van langlopende bewerkingen op de koppelingen voor databasereplicatie ophalen|
|/servers/databases/dataMaskingPolicies/Read|Ophalen van gegevens van het beleid dat op een bepaalde database is geconfigureerd voor gegevensmaskering|
|/servers/databases/dataMaskingPolicies/Write|Beleid voor een bepaalde database gegevensmaskering wijzigen|
|/servers/databases/dataMaskingPolicies/Rules/Read|Ophalen van gegevens van de gegevens die zijn geconfigureerd op een bepaalde database beleidsregel maskeren|
|/servers/databases/dataMaskingPolicies/Rules/Write|Gegevensmaskering beleidsregel voor een bepaalde database wijzigen|
|/servers/databases/securityAlertPolicies/Read|Ophalen van gegevens van de threat beleid geconfigureerd op een bepaalde database|
|/servers/databases/securityAlertPolicies/Write|Het beleid van de detectie van bedreigingen voor een bepaalde database wijzigen|
|/servers/databases/providers/Microsoft.Insights/<br>metricDefinitions leestijd|Retourtypen van metrische gegevens die beschikbaar voor databases zijn|
|/servers/databases/providers/Microsoft.Insights/<br>diagnosticSettings leestijd|Hiermee wordt de diagnostische instelling voor de resource opgehaald|
|/servers/databases/providers/Microsoft.Insights/<br>diagnosticSettings/schrijven|Hiermee wordt de diagnostische instelling voor de resource gemaakt of bijgewerkt|
|/servers/databases/providers/Microsoft.Insights/<br>logDefinitions leestijd|De beschikbare logboeken opgehaald voor databases|
|/servers/databases/topQueries/Read|Retourneert geaggregeerd runtime-statistieken voor de geselecteerde query in de geselecteerde tijdsperiode|
|/servers/databases/topQueries/queryText/Read|Retourneert de Transact-SQL-tekst voor de geselecteerde query-ID|
|/servers/databases/topQueries/statistics/Read|Retourneert geaggregeerd runtime-statistieken voor de geselecteerde query in de geselecteerde tijdsperiode|
|/servers/databases/connectionPolicies/Read|Ophalen van gegevens van het verbindingsbeleid is geconfigureerd op een bepaalde database|
|/servers/databases/connectionPolicies/Write|Verbindingsbeleid voor een bepaalde database wijzigen|
|/servers/databases/metrics/Read|Metrische gegevens voor het gebruik van database resources retourneren|
|/servers/databases/auditRecords/Read|De database blob-controlerecords ophalen|
|/servers/databases/transparentDataEncryption/Read|Status en details van transparent data encryption beveiligingsfunctie voor een bepaalde database ophalen|
|/servers/databases/transparentDataEncryption/Write|In- of uitschakelen van transparante gegevensversleuteling voor een bepaalde database|
|/servers/databases/transparentDataEncryption/operationResults/Read|Status en details van transparent data encryption beveiligingsfunctie voor een bepaalde database ophalen|
|/servers/databases/auditingPolicies/Read|Ophalen van gegevens van de tabel controlebeleid geconfigureerd op een bepaalde database|
|/servers/databases/auditingPolicies/Write|De tabel controlebeleid voor een bepaalde database wijzigen|
|/servers/databases/dataWarehouseQueries/Read|Retourneert het datawarehouse distributie query-gegevens voor geselecteerde query-ID|
|servers/databases/dataWarehouseQueries /<br>dataWarehouseQuerySteps leestijd|Retourneert de gegevens van de gedistribueerde query-stap van de datawarehouse-query voor de ID voor de geselecteerde stap|
|/servers/databases/serviceTierAdvisors/Read|Database schalen omhoog of omlaag wordt voorgesteld geretourneerd op basis van statistieken van de query kan worden uitgevoerd op de prestaties verbeteren of kosten|
|/servers/databases/Advisors/Read|Retourneert de lijst met adviseurs die beschikbaar zijn voor de database|
|/servers/databases/Advisors/Write|Update automatisch-execute status van een advisor op databaseniveau.|
|/servers/databases/Advisors/recommendedActions/Read|Retourneert de lijst met aanbevolen acties van de opgegeven advisor voor de database|
|/servers/databases/Advisors/recommendedActions/Write|De aanbevolen actie toepassen op de database|
|/servers/databases/usages/Read|Maximale databasegrootte die kan worden bereikt en de huidige grootte bezet door gegevens worden geretourneerd|
|/servers/databases/queryStore/Read|Retourneert de huidige waarden van de Query Store-instellingen voor de database|
|/servers/databases/queryStore/Write|Query Store-instelling voor de database-updates|
|/servers/databases/auditingSettings/Read|Ophalen van gegevens van de blob-controlebeleid geconfigureerd op een bepaalde database|
|/servers/databases/auditingSettings/Write|De blob controlebeleid voor een bepaalde database wijzigen|
|/servers/databases/schemas/Tables/recommendedIndexes/Read|Lijst met indexaanbevelingen voor een database ophalen|
|/servers/databases/schemas/Tables/recommendedIndexes/Write|Indexaanbeveling toepassen|
|/servers/databases/schemas/Tables/Columns/Read|Ophalen van lijst met kolommen van een tabel|
|/servers/databases/missingindexes/Read|Retourneren van suggesties over database-indexen maken, wijzigen of verwijderen om te verbeteren de prestaties van query 's|
|/servers/databases/missingindexes/Write|Database-indexaanbeveling in een bepaalde database gebruiken|
|/servers/databases/importExportOperationResults/Read|Details over het importeren van de database- of exportbewerking uit DacPac zich in de storage-account|
|/servers/importExportOperationResults/Read|Retourneert de lijst met details voor databasebewerkingen importeren uit storage-account op een bepaalde server|

## <a name="microsoftstorage"></a>Microsoft.Storage

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Hiermee wordt het abonnement voor de opslagresourceprovider geregistreerd en wordt het maken van opslagaccounts mogelijk.|
|/checknameavailability/Read|Hiermee controleert u of een accountnaam geldig is en niet wordt gebruikt.|
|storageAccounts/schrijven|Hiermee maakt u een opslagaccount met de opgegeven parameters, werkt u de eigenschappen of labels bij of voegt u een aangepast domein toe aan het opgegeven opslagaccount.|
|/storageAccounts/DELETE|Hiermee verwijdert u een bestaand opslagaccount.|
|/storageAccounts/listkeys/Action|Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount.|
|/storageAccounts/regeneratekey/Action|Hiermee genereert u de toegangssleutels voor het opgegeven opslagaccount opnieuw.|
|/storageAccounts/Read|Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount.|
|/storageAccounts/listAccountSas/Action|Retourneert de Account-SAS-token voor het opgegeven opslagaccount.|
|/storageAccounts/listServiceSas/Action|Storage-Service-SAS-Token|
|/storageAccounts/Services/diagnosticSettings/Write|Diagnostische instellingen voor storage-account maken of bijwerken.|
|/skus/Read|Geeft een lijst van de SKU's wordt ondersteund door Microsoft.Storage.|
|/usages/Read|Retourneert de limiet en het huidige gebruik voor resources in het opgegeven abonnement|
|/Operations/Read|Hiermee wordt de status van een asynchrone bewerking gepeild.|
|/Locations/deleteVirtualNetworkOrSubnets/Action|Microsoft.Storage waarschuwt dat virtuele netwerk of subnet wordt verwijderd|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| Bewerking | Beschrijving |
|---|---|
|/managers/clearAlerts/Action|Schakel alle waarschuwingen die zijn gekoppeld aan de Apparaatbeheer.|
|/managers/getActivationKey/Action|Activeringscode voor Apparaatbeheer niet ophalen.|
|/managers/regenerateActivationKey/Action|Activeringscode genereren voor Apparaatbeheer.|
|/managers/regenarateRegistationCertificate/Action|Registratiecertificaat opnieuw genereren voor de managers voor apparaatregistratie.|
|/managers/getEncryptionKey/Action|Download coderingssleutel voor Apparaatbeheer.|
|/managers/Read|Geeft een lijst of de apparaat-Managers opgehaald|
|/managers/DELETE|Hiermee verwijdert u de Managers voor apparaatregistratie|
|managers/schrijven|Maken of bijwerken van de Managers voor apparaatregistratie|
|/managers/configureDevice/Action|Hiermee configureert u een apparaat|
|/managers/listActivationKey/Action|Hiermee haalt u de activeringssleutel van de StorSimple-Apparaatbeheer.|
|/managers/listPublicEncryptionKey/Action|Lijst met codering met openbare sleutels van een StorSimple-Apparaatbeheer.|
|/managers/listPrivateEncryptionKey/Action|Persoonlijke sleutel voor een StorSimple-Apparaatbeheer opgehaald.|
|/managers/provisionCloudAppliance/Action|Maak een nieuw toestel in de cloud.|
|Managers/schrijftijd|Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt.|
|Managers/leestijd|De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald|
|/ Managers/verwijderen|De bewerking verwijderen kluis verwijdert de opgegeven Azure-resource van het type 'kluis'|
|/managers/storageAccountCredentials/Write|Maken of bijwerken van de Opslagaccountreferenties|
|/managers/storageAccountCredentials/Read|Geeft een lijst of de Opslagaccountreferenties opgehaald|
|/managers/storageAccountCredentials/DELETE|Hiermee verwijdert u de Opslagaccountreferenties|
|/managers/storageAccountCredentials/listAccessKey/Action|Toegangstoetsen lijst van Opslagaccountreferenties|
|/managers/accessControlRecords/Read|Geeft een lijst of de Access Control Records opgehaald|
|/managers/accessControlRecords/Write|De Access Control Records maken of bijwerken|
|/managers/accessControlRecords/DELETE|De Access Control Records verwijderd|
|/managers/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/bandwidthSettings/Read|Lijst van de bandbreedte-instellingen (alleen 8000 serie)|
|/managers/bandwidthSettings/Write|Een nieuwe maken of bijwerken van de bandbreedte-instellingen (alleen 8000-serie)|
|/managers/bandwidthSettings/DELETE|Hiermee verwijdert u een bestaande bandbreedte-instellingen (alleen 8000-serie)|
|Managers/extendedInformation/leestijd|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|Managers/extendedInformation/schrijftijd|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|Managers, extendedInformation/verwijderen|Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt|
|/managers/Alerts/Read|Geeft een lijst of de waarschuwingen opgehaald|
|/managers/storageDomains/Read|Geeft een lijst of de opslag domeinen opgehaald|
|/managers/storageDomains/Write|Maken of bijwerken van de opslag-domeinen|
|/managers/storageDomains/DELETE|Hiermee verwijdert u de opslag-domeinen|
|/managers/Devices/scanForUpdates/Action|Zoeken naar updates in een apparaat.|
|/managers/Devices/Download/Action|Download updates voor een apparaat.|
|/managers/Devices/Install/Action|Updates installeren op een apparaat.|
|/managers/Devices/Read|Geeft een lijst of de apparaten opgehaald|
|/managers/Devices/Write|Maken of bijwerken van de apparaten|
|/managers/Devices/DELETE|Hiermee verwijdert u de apparaten|
|/managers/Devices/Deactivate/Action|Een apparaat wordt gedeactiveerd.|
|/managers/Devices/publishSupportPackage/Action|Publiceer ondersteuningspakket van een apparaat voor het oplossen van Microsoft Support.|
|/managers/Devices/failover/Action|Failover van het apparaat.|
|/managers/Devices/sendTestAlertEmail/Action|Waarschuwing testbericht verzenden naar geconfigureerde e-mailontvangers.|
|/managers/Devices/installUpdates/Action|Updates zijn geïnstalleerd op de apparaten|
|/managers/Devices/listFailoverSets/Action|Hiermee stelt u de failover voor een bestaand apparaat een lijst.|
|/managers/Devices/listFailoverTargets/Action|Doelen voor de failover van de apparaten|
|/managers/Devices/publicEncryptionKey/Action|Openbare versleutelingssleutel lijst van Apparaatbeheer|
|managers/apparaten/hardwareComponentGroups /<br>Lezen|Lijst van de Hardware-onderdeelgroepen|
|managers/apparaten/hardwareComponentGroups /<br>changeControllerPowerState/actie|Controller voedingsstatus van hardware-onderdeelgroepen wijzigen|
|/managers/Devices/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/Devices/chapSettings/Write|De Chap-instellingen maken of bijwerken|
|/managers/Devices/chapSettings/Read|Geeft een lijst of ontvangt de Chap-instellingen|
|/managers/Devices/chapSettings/DELETE|Hiermee verwijdert u de Chap-instellingen|
|/managers/Devices/backupScheduleGroups/Read|Geeft een lijst of de back-up schemagroepen opgehaald|
|/managers/Devices/backupScheduleGroups/Write|De back-up schemagroepen maken of bijwerken|
|/managers/Devices/backupScheduleGroups/DELETE|Hiermee verwijdert u de back-upschema-groepen|
|/managers/Devices/updateSummary/Read|Geeft een lijst of een overzicht van de Update wordt opgehaald|
|managers/apparaten/migrationSourceConfigurations /<br>actie/importeren|Importeren van de bron-configuraties voor migratie|
|managers/apparaten/migrationSourceConfigurations /<br>startMigrationEstimate/actie|Een taak voor een schatting van de duur van het migratieproces start.|
|managers/apparaten/migrationSourceConfigurations /<br>startMigration/actie|Begint met de migratie met behulp van de bron-configuraties|
|managers/apparaten/migrationSourceConfigurations /<br>confirmMigration/actie|Bevestigt een succesvolle migratie en het doorvoeren.|
|managers/apparaten/migrationSourceConfigurations /<br>fetchMigrationEstimate/actie|De status van de migratietaak schatting ophalen.|
|managers/apparaten/migrationSourceConfigurations /<br>fetchMigrationStatus/actie|Haal de status van de migratie.|
|managers/apparaten/migrationSourceConfigurations /<br>fetchConfirmMigrationStatus/actie|Haal de status van het bevestigen van de migratie.|
|/managers/Devices/alertSettings/Read|Geeft een lijst of opgehaald van de instellingen voor waarschuwingen|
|/managers/Devices/alertSettings/Write|Maken of bijwerken van de instellingen voor waarschuwingen|
|/managers/Devices/networkSettings/Read|Geeft een lijst of de netwerkinstellingen opgehaald|
|/managers/Devices/networkSettings/Write|Een nieuwe maken of bijwerken van netwerkinstellingen|
|/managers/Devices/Jobs/Read|Geeft een lijst of de taken opgehaald|
|/managers/Devices/Jobs/Cancel/Action|Een actieve taak annuleren|
|/managers/Devices/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/Devices/volumeContainers/Write|Maakt een nieuw of bijgewerkt Volumecontainers (alleen 8000-serie)|
|/managers/Devices/volumeContainers/Read|Lijst van de Volumecontainers (alleen 8000 serie)|
|/managers/Devices/volumeContainers/DELETE|Hiermee verwijdert u een bestaande Volumecontainers (alleen 8000-serie)|
|/managers/Devices/volumeContainers/listEncryptionKeys/Action|De versleutelingssleutels lijst van Volumecontainers|
|/managers/Devices/volumeContainers/rolloverEncryptionKey/Action|De versleutelingssleutels rollover van Volumecontainers|
|/managers/Devices/volumeContainers/metrics/Read|Lijst van de metrische gegevens|
|/managers/Devices/volumeContainers/volumes/Read|Lijst van de Volumes|
|/managers/Devices/volumeContainers/volumes/Write|Een nieuwe maken of bijwerken van Volumes|
|/managers/Devices/volumeContainers/volumes/DELETE|Hiermee verwijdert u een bestaande Volumes|
|/managers/Devices/volumeContainers/volumes/metrics/Read|Lijst van de metrische gegevens|
|/managers/Devices/volumeContainers/volumes/metricsDefinitions/Read|De definities van de metrische gegevens weergeven|
|/managers/Devices/volumeContainers/metricsDefinitions/Read|De definities van de metrische gegevens weergeven|
|/managers/Devices/iscsiservers/Read|Geeft een lijst of opgehaald van de iSCSI-Servers|
|/managers/Devices/iscsiservers/Write|Maken of bijwerken van de iSCSI-Servers|
|/managers/Devices/iscsiservers/DELETE|Hiermee verwijdert u de iSCSI-Servers|
|/managers/Devices/iscsiservers/Backup/Action|Duren back-up van een iSCSI-server.|
|/managers/Devices/iscsiservers/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/Devices/iscsiservers/Disks/Read|Geeft een lijst of de schijven opgehaald|
|/managers/Devices/iscsiservers/Disks/Write|Maken of bijwerken van de schijven|
|/managers/Devices/iscsiservers/Disks/DELETE|Hiermee verwijdert u de schijven|
|/managers/Devices/iscsiservers/Disks/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/Devices/iscsiservers/Disks/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/Devices/iscsiservers/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/Devices/backups/Read|Geeft een lijst of back-upset opgehaald|
|/managers/Devices/backups/DELETE|Hiermee verwijdert u de back-upset|
|/managers/Devices/backups/Restore/Action|Alle volumes terugzetten vanaf een back-upset.|
|/managers/Devices/backups/Elements/Clone/Action|Klonen van een share of het volume met behulp van een back-element.|
|/managers/Devices/backupPolicies/Write|Maakt een nieuw of bijgewerkt beleid van de back-up (alleen 8000-serie)|
|/managers/Devices/backupPolicies/Read|Lijst met de back-up beleidsregels (alleen 8000 serie)|
|/managers/Devices/backupPolicies/DELETE|Hiermee verwijdert u een beleid in een bestaande back-up (alleen 8000-serie)|
|/managers/Devices/backupPolicies/Backup/Action|Een handmatige back-up op aanvraag maken back-up van alle volumes die zijn beveiligd door het beleid te nemen.|
|/managers/Devices/backupPolicies/Schedules/Write|Maakt een nieuw of bijgewerkt schema 's|
|/managers/Devices/backupPolicies/Schedules/Read|Lijst van de planning|
|/managers/Devices/backupPolicies/Schedules/DELETE|Hiermee verwijdert u een bestaande planningen|
|/managers/Devices/securitySettings/update/Action|Werk de beveiligingsinstellingen.|
|/managers/Devices/securitySettings/Read|Lijst van de beveiligingsinstellingen|
|managers/apparaten/securitySettings /<br>syncRemoteManagementCertificate/actie|Het certificaat voor extern beheer voor een apparaat worden gesynchroniseerd.|
|/managers/Devices/securitySettings/Write|Maakt een nieuw of bijgewerkt beveiligingsinstellingen|
|/managers/Devices/fileservers/Read|Geeft een lijst of opgehaald van de bestandsservers|
|/managers/Devices/fileservers/Write|Maken of bijwerken van de bestandsservers|
|/managers/Devices/fileservers/DELETE|Hiermee verwijdert u de bestandsservers|
|/managers/Devices/fileservers/Backup/Action|Duren back-up van een bestandsserver.|
|/managers/Devices/fileservers/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/Devices/fileservers/shares/Write|Maken of bijwerken van de Shares|
|/managers/Devices/fileservers/shares/Read|Geeft een lijst of de Shares opgehaald|
|/managers/Devices/fileservers/shares/DELETE|Hiermee verwijdert u de Shares|
|/managers/Devices/fileservers/shares/metrics/Read|Geeft een lijst of opgehaald van de metrische gegevens|
|/managers/Devices/fileservers/shares/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/Devices/fileservers/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/Devices/timeSettings/Read|Geeft een lijst of de instellingen opgehaald|
|/managers/Devices/timeSettings/Write|Maakt een nieuw of bijgewerkt tijdinstellingen|
|Managers/certificaten/schrijftijd|De bewerking Update Resource certificaat updates het referentiecertificaat van de resource/kluis.|
|/managers/cloudApplianceConfigurations/Read|Lijst met het toestel Cloud ondersteunde configuraties|
|/managers/metricsDefinitions/Read|Geeft een lijst of de definities van de metrische gegevens opgehaald|
|/managers/encryptionSettings/Read|Geeft een lijst of de instellingen voor codering opgehaald|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| Bewerking | Beschrijving |
|---|---|
|/streamingjobs/start/Action|Stream Analytics-taak starten|
|/streamingjobs/Stop/Action|Stream Analytics-taak stoppen|
|/ streamingjobs/lezen|Stream Analytics-taak lezen|
|/ streamingjobs/schrijven|Schrijven van Stream Analytics-taak|
|/ streamingjobs/verwijderen|Verwijderen van de Stream Analytics-taak|
|/streamingjobs/providers/Microsoft.Insights/metricDefinitions/Read|Hiermee wordt de beschikbare metrische gegevens voor streamingjobs|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/Read|Lees de diagnostische instelling.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/Write|Diagnostische instelling schrijven.|
|/streamingjobs/providers/Microsoft.Insights/logDefinitions/Read|De beschikbare logboeken opgehaald voor streamingjobs|
|/streamingjobs/Transformations/Read|Transformatie lezen Stream Analytics-taak|
|/streamingjobs/Transformations/Write|Stream Analytics-taak transformatie schrijven|
|/streamingjobs/Transformations/DELETE|Stream Analytics-taak transformatie verwijderen|
|/streamingjobs/inputs/Read|Invoer lezen Stream Analytics-taak|
|/streamingjobs/inputs/Write|Stream Analytics-taak invoer schrijven|
|/streamingjobs/inputs/DELETE|Stream Analytics-taak invoer verwijderen|
|/streamingjobs/outputs/Read|Lees Stream Analytics-Taakuitvoer|
|/streamingjobs/outputs/Write|Stream Analytics-Taakuitvoer schrijven|
|/streamingjobs/outputs/DELETE|Stream Analytics-Taakuitvoer verwijderen|

## <a name="microsoftsupport"></a>Microsoft.Support

| Bewerking | Beschrijving |
|---|---|
|registratie-/ actie|Hiermee wordt het item geregistreerd bij de ondersteuningsresourceprovider|
|/supportTickets/Read|Hiermee worden gegevens over het ondersteuningsticket opgehaald (inclusief status, ernst, contactgegevens en communicatie) of wordt de lijst met ondersteuningstickets voor de verschillende abonnementen opgehaald.|
|supportTickets/schrijven|Maken of bijwerken van een Ondersteuningsticket. U kunt een Ondersteuningsticket maken voor technische, facturerings, quota's of beheer van abonnementen problemen met betrekking tot. U kunt de ernst, contactgegevens en communicatie voor bestaande ondersteuningstickets bijwerken.|

## <a name="microsoftweb"></a>Microsoft.Web

| Bewerking | Beschrijving |
|---|---|
|/ unregister/actie|Hef de registratie Microsoft.Web resourceprovider voor het abonnement.|
|valideren/actie|Valideren.|
|registratie-/ actie|De registerbronprovider Microsoft.Web voor het abonnement.|
|/ hostingEnvironments/lezen|De eigenschappen van een App-serviceomgeving opgehaald|
|/ hostingEnvironments/schrijven|Een nieuwe App Service-omgeving maken of bijwerken van bestaande|
|/ hostingEnvironments/verwijderen|Een App-serviceomgeving verwijderen|
|/hostingEnvironments/reboot/Action|Start opnieuw op alle machines in een App-serviceomgeving|
|/hostingenvironments/Resume/Action|Hervat de hostomgevingen.|
|/hostingenvironments/Suspend/Action|Onderbreken hostomgevingen.|
|/hostingenvironments/metricdefinitions/Read|Hosting-omgevingen metrische definities ophalen.|
|/hostingEnvironments/workerPools/Read|De eigenschappen van een werknemersgroep in een App-serviceomgeving opgehaald|
|/hostingEnvironments/workerPools/Write|Een nieuwe Worker-groep in een App Service-omgeving maken of bijwerken van een bestaande|
|/hostingenvironments/workerpools/metricdefinitions/Read|Hosting-omgevingen Workerpools metriek definities ophalen.|
|/hostingenvironments/workerpools/metrics/Read|Hosting-omgevingen Workerpools metrische gegevens ophalen.|
|/hostingenvironments/workerpools/skus/Read|Hosting-omgevingen Workerpools SKU's ophalen.|
|/hostingenvironments/workerpools/usages/Read|Hosting-omgevingen Workerpools gebruik ophalen.|
|/hostingenvironments/sites/Read|Hosting-omgevingen Web-Apps worden opgehaald.|
|/hostingenvironments/serverfarms/Read|Hosting-omgevingen App Service-abonnementen worden opgehaald.|
|/hostingenvironments/usages/Read|Hosting-omgevingen gebruik ophalen.|
|/hostingenvironments/capacities/Read|Hosting-omgevingen capaciteiten ophalen.|
|/hostingenvironments/Operations/Read|Hosting-omgevingen bewerkingen ophalen.|
|/hostingEnvironments/multiRolePools/Read|De eigenschappen van een FrontEnd-toepassingen in een App-serviceomgeving opgehaald|
|/hostingEnvironments/multiRolePools/Write|Een nieuwe FrontEnd-toepassingen in een App Service-omgeving maken of bijwerken van een bestaande|
|/hostingenvironments/multirolepools/metricdefinitions/Read|Hosting-omgevingen MultiRole Pools metrische definities ophalen.|
|/hostingenvironments/multirolepools/metrics/Read|Hosting-omgevingen MultiRole Pools metrische gegevens ophalen.|
|/hostingenvironments/multirolepools/skus/Read|Hosting-omgevingen MultiRole Pools SKU's ophalen.|
|/hostingenvironments/multirolepools/usages/Read|Hosting-omgevingen MultiRole Pools gebruik ophalen.|
|/hostingenvironments/Diagnostics/Read|Hosting-omgevingen diagnostische gegevens ophalen.|
|/publishingusers/Read|Publiceren van gebruikers ophalen.|
|publishingusers/schrijven|Bijwerken van de gebruikers te publiceren.|
|/checknameavailability/Read|Controleer of de resourcenaam van de beschikbaar is.|
|/ geoRegions/lezen|De lijst met Geo regions ophalen.|
|/ sites/lezen|De eigenschappen van een Web-App opgehaald|
|/ sites/schrijven|Een nieuwe Web-App maken of bijwerken van een bestaande|
|/ sites/verwijderen|Een bestaande Web-App verwijderen|
|/sites/Backup/Action|Een nieuwe web-app back-up maken|
|/sites/PublishXml/Action|Ophalen van de xml van profiel voor een Web-App publiceren|
|/sites/Publish/Action|Een Web-App publiceren|
|/sites/restart/Action|Een Web-App starten|
|/sites/start/Action|Een Web-App starten|
|/sites/Stop/Action|Een Web-App stoppen|
|/sites/slotsswap/Action|Implementatie Web-appsites wisselen|
|/sites/slotsdiffs/Action|Verschillen in de configuratie tussen web-app en sleuven ophalen|
|/sites/applySlotConfig/Action|Web-app sleuf configuratie van doel-sleuf in de huidige web-app toe te passen|
|/sites/resetSlotConfig/Action|Web-app-configuratie opnieuw instellen|
|/sites/Functions/Action|Functies van Web-Apps.|
|/sites/listsyncfunctiontriggerstatus/Action|Lijst met synchronisatie functie Trigger Status Web-Apps.|
|/sites/networktrace/Action|Netwerk Trace Web-Apps.|
|/sites/NewPassword/Action|NieuwWachtwoord Web-Apps.|
|/sites/Sync/Action|Synchronisatie van Web-Apps.|
|/sites/operationresults/Read|Web-Apps Bewerkingsresultaten krijgt.|
|/sites/webjobs/Read|Web-Apps webtaken worden opgehaald.|
|/ sites/back-up/lezen|Back-up van Web Apps ophalen.|
|/sites/Backup/Write|Back-up van Web-Apps worden bijgewerkt.|
|/sites/metricdefinitions/Read|Web-Apps metriek definities worden opgehaald.|
|/sites/metrics/Read|Web-Apps metrische gegevens worden opgehaald.|
|/sites/continuouswebjobs/DELETE|Web-Apps doorlopende webtaken verwijderen.|
|/sites/continuouswebjobs/Read|Web-Apps doorlopende webtaken worden opgehaald.|
|/sites/continuouswebjobs/start/Action|Web-Apps doorlopende webtaken starten.|
|/sites/continuouswebjobs/Stop/Action|Web-Apps doorlopende webtaken stoppen.|
|/sites/domainownershipidentifiers/Read|Web-Apps domein eigenaar-id's worden opgehaald.|
|/sites/domainownershipidentifiers/Write|Web-Apps domein eigenaar-id's worden bijgewerkt.|
|/sites/premieraddons/DELETE|Web Apps Premier invoegtoepassingen verwijderen.|
|/sites/premieraddons/Read|Web Apps Premier invoegtoepassingen ophalen.|
|/sites/premieraddons/Write|Bijwerken van Web Apps Premier invoegtoepassingen.|
|/sites/triggeredwebjobs/DELETE|Web-Apps Triggered webtaken verwijderen.|
|/sites/triggeredwebjobs/Read|Web-Apps Triggered webtaken worden opgehaald.|
|/sites/triggeredwebjobs/Run/Action|Geactiveerde WebJobs voor Web-Apps worden uitgevoerd.|
|/sites/hostnamebindings/DELETE|Hostnaambindings voor Web-Apps verwijderen.|
|/sites/hostnamebindings/Read|Web-Apps Hostnaambindings ophalen.|
|/sites/hostnamebindings/Write|Web-Apps Hostnaambindings bijwerken.|
|/sites/virtualnetworkconnections/DELETE|Verwijder virtuele netwerkverbindingen van Web Apps.|
|/sites/virtualnetworkconnections/Read|Web-Apps virtuele netwerkverbindingen worden opgehaald.|
|/sites/virtualnetworkconnections/Write|Web-Apps virtuele netwerkverbindingen worden bijgewerkt.|
|/sites/virtualnetworkconnections/gateways/Read|Web-Apps virtuele verbindingen netwerkgateways ophalen.|
|/sites/virtualnetworkconnections/gateways/Write|Web-Apps virtueel netwerk verbindingen Gateways bijwerken.|
|/sites/PublishXml/Read|Web-Apps publiceren XML ophalen.|
|/sites/hybridconnectionrelays/Read|Web-Apps hybride verbinding Relays ophalen.|
|/sites/perfcounters/Read|Prestatiemeteritems voor Web-Apps worden opgehaald.|
|/sites/usages/Read|Het gebruik van Web Apps ophalen.|
|/sites/slots/Write|Een nieuwe Web-Appsite maken of bijwerken van een bestaande|
|/sites/slots/DELETE|Een bestaande Web-Appsite verwijderen|
|/sites/slots/Backup/Action|Nieuwe Web-Appsite back-up maken.|
|/sites/slots/PublishXml/Action|Profiel xml publiceren voor Web-Appsite ophalen|
|/sites/slots/Publish/Action|Een Web-Appsite publiceren|
|/sites/slots/restart/Action|Een Web-Appsite starten|
|/sites/slots/start/Action|Een Web-Appsite starten|
|/sites/slots/Stop/Action|Een Web-Appsite stoppen|
|/sites/slots/slotsswap/Action|Implementatie Web-appsites wisselen|
|/sites/slots/slotsdiffs/Action|Verschillen in de configuratie tussen web-app en sleuven ophalen|
|/sites/slots/applySlotConfig/Action|Web-app siteconfiguratie van doel sleuf naar de huidige sleuf toepassen.|
|/sites/slots/resetSlotConfig/Action|Web-app sleuf configuratie opnieuw instellen|
|/sites/slots/Read|De eigenschappen van een Web-App implementatiesleuf opgehaald|
|/sites/slots/NewPassword/Action|Sleuven NieuwWachtwoord Web-Apps.|
|/sites/slots/Sync/Action|Synchronisatie Web Apps sleuven.|
|/sites/slots/operationresults/Read|Web-Apps sleuven Bewerkingsresultaten krijgt.|
|/sites/slots/webjobs/Read|Web-Apps sleuven webtaken worden opgehaald.|
|/sites/slots/Backup/Write|Bijwerken van Web Apps sleuven back-up.|
|/sites/slots/metricdefinitions/Read|Web-Apps sleuven metrische definities worden opgehaald.|
|/sites/slots/metrics/Read|Web-Apps sleuven metrische gegevens worden opgehaald.|
|/sites/slots/continuouswebjobs/DELETE|Web-Apps sleuven doorlopende webtaken verwijderen.|
|/sites/slots/continuouswebjobs/Read|Web-Apps sleuven doorlopende webtaken worden opgehaald.|
|/sites/slots/continuouswebjobs/start/Action|Web-Apps sleuven doorlopende webtaken starten.|
|/sites/slots/continuouswebjobs/Stop/Action|Web-Apps sleuven doorlopende webtaken stoppen.|
|/sites/slots/premieraddons/DELETE|Web Apps sleuven Premier invoegtoepassingen verwijderen.|
|/sites/slots/premieraddons/Read|Web Apps sleuven Premier invoegtoepassingen ophalen.|
|/sites/slots/premieraddons/Write|Bijwerken van Web Apps sleuven Premier invoegtoepassingen.|
|/sites/slots/triggeredwebjobs/DELETE|Web-Apps sleuven Triggered webtaken verwijderen.|
|/sites/slots/triggeredwebjobs/Read|Web-Apps sleuven Triggered webtaken worden opgehaald.|
|/sites/slots/triggeredwebjobs/Run/Action|Web-Apps sleuven Triggered webtaken worden uitgevoerd.|
|/sites/slots/hostnamebindings/DELETE|Web-Apps sleuven Hostnaambindings verwijderen.|
|/sites/slots/hostnamebindings/Read|Web-Apps sleuven Hostnaambindings ophalen.|
|/sites/slots/hostnamebindings/Write|Web-Apps sleuven Hostnaambindings bijwerken.|
|/sites/slots/phplogging/Read|Web-Apps sleuven Phplogging ophalen.|
|/sites/slots/virtualnetworkconnections/DELETE|Web-Apps sleuven virtuele netwerkverbindingen verwijderen.|
|/sites/slots/virtualnetworkconnections/Read|Web-Apps sleuven virtuele netwerkverbindingen worden opgehaald.|
|/sites/slots/virtualnetworkconnections/Write|Web-Apps sleuven virtuele netwerkverbindingen worden bijgewerkt.|
|/sites/slots/virtualnetworkconnections/gateways/Write|Web-Apps sleuven virtueel netwerk verbindingen Gateways bijwerken.|
|/sites/slots/usages/Read|Het gebruik van Web Apps sleuven ophalen.|
|/sites/slots/hybridconnection/DELETE|Sleuven hybride verbinding voor Web-Apps verwijderen.|
|/sites/slots/hybridconnection/Read|Hybride verbinding voor Web-Apps sleuven ophalen.|
|/sites/slots/hybridconnection/Write|Hybride verbinding voor Web-Apps sleuven bijwerken.|
|/sites/slots/config/Read|Web-Appsite van configuratie-instellingen ophalen|
|/sites/slots/config/List/Action|Web-Appsite van gevoelige beveiligingsinstellingen, zoals het publiceren van referenties, app-instellingen en verbindingsreeksen weergeven|
|/sites/slots/config/Write|Web-Appsite van configuratie-instellingen bijwerken|
|/sites/slots/config/DELETE|Webconfiguratie Apps sleuven verwijderen.|
|/sites/slots/Instances/Read|Web-Apps sleuven instanties worden opgehaald.|
|/sites/slots/Instances/processes/Read|Web-Apps sleuven exemplaren processen worden opgehaald.|
|/sites/slots/Instances/Deployments/Read|Implementaties van Web Apps sleuven instanties worden opgehaald.|
|/sites/slots/sourcecontrols/Read|Configuratie-instellingen voor Web-Appsite van broncodebeheer ophalen|
|/sites/slots/sourcecontrols/Write|Instellingen voor Web-Appsite van bronbeheer configuratie bijwerken|
|/sites/slots/sourcecontrols/DELETE|Instellingen voor Web-Appsite van bronbeheer configuratie verwijderen|
|/sites/slots/Restore/Read|Web-Apps sleuven terugzetten ophalen.|
|/sites/slots/analyzecustomhostname/Read|Ophalen van Web Apps sleuven analyseren aangepaste hostnaam.|
|/sites/slots/backups/Read|De eigenschappen van een web-appsites back-up|
|/sites/slots/backups/List/Action|Lijst met Web-Apps sleuven back-ups.|
|/sites/slots/backups/Restore/Action|Web-Apps sleuven back-ups herstellen.|
|/sites/slots/Deployments/DELETE|Web-Apps sleuven implementaties verwijderd.|
|/sites/slots/Deployments/Read|Web-Apps sleuven implementaties ophalen.|
|/sites/slots/Deployments/Write|Update-implementaties van Web Apps sleuven.|
|/sites/slots/Deployments/log/Read|Web-Apps sleuven implementaties logboek ophalen.|
|/sites/hybridconnection/DELETE|Web-Apps hybride verbinding verwijderen.|
|/sites/hybridconnection/Read|Verbinding voor Web-Apps hybride ophalen.|
|/sites/hybridconnection/Write|Web-Apps hybride verbinding bijwerken.|
|/sites/recommendationhistory/Read|Web-Apps aanbeveling geschiedenis ophalen.|
|/sites/Recommendations/Read|Haal de lijst met aanbevelingen voor web-app.|
|/sites/Recommendations/Disable/Action|Web-Apps aanbevelingen uitschakelen.|
|/sites/config/Read|Ophalen van configuratie-instellingen voor Web-App|
|/sites/config/List/Action|Web-App gevoelige beveiligingsinstellingen, zoals het publiceren van referenties, app-instellingen en verbindingsreeksen weergeven|
|/sites/config/Write|Configuratie-instellingen voor Web-App bijwerken|
|/sites/config/DELETE|Webconfiguratie Apps verwijderen.|
|/sites/Instances/Read|Web-Apps instanties worden opgehaald.|
|/sites/Instances/processes/DELETE|Web-Apps exemplaren processen verwijderen.|
|/sites/Instances/processes/Read|Web-Apps exemplaren processen worden opgehaald.|
|/sites/Instances/Deployments/Read|Implementaties van Web Apps instanties worden opgehaald.|
|/sites/sourcecontrols/Read|Broncodebeheer voor Web-App-configuratie-instellingen ophalen|
|/sites/sourcecontrols/Write|Instellingen voor Web-App bronbeheer configuratie bijwerken|
|/sites/sourcecontrols/DELETE|Instellingen voor Web-App bronbeheer configuratie verwijderen|
|/sites/Restore/Read|Terugzetten van de Web-Apps worden opgehaald.|
|/sites/analyzecustomhostname/Read|Analyseer de aangepaste hostnaam.|
|/sites/backups/Read|De eigenschappen van back-up van een web-app|
|/sites/backups/List/Action|Lijst met Web-Apps back-ups.|
|/sites/backups/Restore/Action|Terugzetten van back-ups van Web-Apps.|
|/sites/Snapshots/Read|Momentopnamen van de Web-Apps ophalen.|
|/sites/Functions/DELETE|Functies voor Web-Apps verwijderen.|
|/sites/Functions/listsecrets/Action|Lijst met geheimen Web Apps functies.|
|/sites/Functions/Read|Functies voor Web-Apps worden opgehaald.|
|/sites/Functions/Write|Bijwerken van Web Apps functies.|
|/sites/Deployments/DELETE|Implementaties van Web-Apps verwijderd.|
|/sites/Deployments/Read|Implementaties van Web-Apps worden opgehaald.|
|/sites/Deployments/Write|Update-implementaties van Web-Apps.|
|/sites/Deployments/log/Read|Web-Apps implementaties logboek ophalen.|
|/sites/Diagnostics/Read|Web-Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/workerprocessrecycle/Read|Recyclen van Web Apps Diagnostics Worker proces worden opgehaald.|
|/sites/Diagnostics/workeravailability/Read|Web-Apps Diagnostics Workeravailability ophalen.|
|/sites/Diagnostics/runtimeavailability/Read|Runtime-beschikbaarheid van Web-Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/cpuanalysis/Read|Web-Apps Diagnostics Cpuanalysis ophalen.|
|/sites/Diagnostics/servicehealth/Read|Servicestatus van Web Apps diagnostische gegevens worden opgehaald.|
|/sites/Diagnostics/frebanalysis/Read|Web-Apps Diagnostics FREB analyse worden opgehaald.|
|/availablestacks/Read|Beschikbare Stacks ophalen.|
|/isusernameavailable/Read|Controleer of de gebruikersnaam beschikbaar is.|
|/Microsoft.Web/apiManagementAccounts/<br>API's leestijd|Haal de lijst van API's.|
|/Microsoft.Web/apiManagementAccounts/<br>API's / schrijven|Een nieuwe Api toevoegen of bijwerken van bestaande.|
|/Microsoft.Web/apiManagementAccounts/<br>API's en verwijderen|Verwijder een bestaande Api.|
|/Microsoft.Web/apiManagementAccounts/<br>verbindingen API's leestijd|De lijst met verbindingen opgehaald.|
|/Microsoft.Web/apiManagementAccounts/<br>verbindingen API's schrijftijd|Sla een nieuwe verbinding of een bestaande bijgewerkt.|
|/Microsoft.Web/apiManagementAccounts/<br>API's, verbindingen, verwijderen|Verwijder een bestaande verbinding.|
|/Microsoft.Web/apiManagementAccounts/<br>API's verbindingen/connectionAcls/leestijd|ConnectionAcls lezen|
|/Microsoft.Web/apiManagementAccounts/<br>API's / verbindingen/connectionAcls/schrijven|Toevoegen of bijwerken van ConnectionAcl|
|/Microsoft.Web/apiManagementAccounts/<br>API's, verbindingen, connectionAcls/verwijderen|ConnectionAcl verwijderen|
|/Microsoft.Web/apiManagementAccounts/<br>connectionAcls API's leestijd|Lees ConnectionAcls voor Api|
|/Microsoft.Web/apiManagementAccounts/<br>apiAcls API's leestijd|ConnectionAcls lezen|
|/Microsoft.Web/apiManagementAccounts/<br>apiAcls API's schrijftijd|Maken of bijwerken van de Api-ACL 's|
|/Microsoft.Web/apiManagementAccounts/<br>API's, apiAcls, verwijderen|Api-ACL's verwijderen|
|/ serverfarms/lezen|Opvragen van de eigenschappen voor een App Service-Plan|
|/ serverfarms/schrijven|Maak een nieuwe App Service-abonnement of een bestaande bijgewerkt|
|/ serverfarms/verwijderen|Verwijder een bestaande App Service-Plan|
|/serverfarms/restartSites/Action|Start opnieuw op alle Web-Apps in een App Service-Plan|
|/serverfarms/operationresults/Read|App Service-abonnementen Bewerkingsresultaten krijgt.|
|/serverfarms/Capabilities/Read|Mogelijkheden van App Service-abonnementen ophalen.|
|/serverfarms/metricdefinitions/Read|App Service-abonnementen metrische definities worden opgehaald.|
|/serverfarms/metrics/Read|App Service-abonnementen metrische gegevens worden opgehaald.|
|/serverfarms/hybridconnectionplanlimits/Read|Ophalen van App Service-abonnementen hybride verbindingslimieten-Plan.|
|/serverfarms/virtualnetworkconnections/Read|App Service-abonnementen virtuele netwerkverbindingen worden opgehaald.|
|/serverfarms/virtualnetworkconnections/routes/DELETE|App Service-abonnementen virtueel netwerk verbindingen Routes verwijderen.|
|/serverfarms/virtualnetworkconnections/routes/Read|App Service-abonnementen virtueel netwerk verbindingen Routes worden opgehaald.|
|/serverfarms/virtualnetworkconnections/routes/Write|App Service-abonnementen virtueel netwerk verbindingen Routes bijwerken.|
|/serverfarms/virtualnetworkconnections/gateways/Write|App Service-abonnementen virtueel netwerk verbindingen Gateways bijwerken.|
|/serverfarms/firstpartyapps/Settings/DELETE|App Service-abonnementen eerste partijen Apps instellingen verwijderd.|
|/serverfarms/firstpartyapps/Settings/Read|App Service-abonnementen eerste partijen Apps-instellingen ophalen.|
|/serverfarms/firstpartyapps/Settings/Write|App Service-abonnementen eerste partijen Apps instellingen worden bijgewerkt.|
|/serverfarms/sites/Read|Web-Apps van App Service-abonnementen ophalen.|
|/serverfarms/workers/reboot/Action|Start opnieuw op App Service-abonnementen werknemers.|
|/serverfarms/hybridconnectionrelays/Read|App Service-abonnementen hybride verbinding Relays ophalen.|
|/serverfarms/skus/Read|App Service-abonnementen SKU's worden opgehaald.|
|/serverfarms/usages/Read|Het gebruik van App Service-abonnementen ophalen.|
|/serverfarms/hybridconnectionnamespaces/relays/sites/Read|App Service-plannen voor hybride verbinding naamruimten Relays WebApps ophalen.|
|/ishostnameavailable/Read|Controleer of de hostnaam beschikbaar is.|
|/ connectionGateways/lezen|De lijst van Gateways verbinding ophalen.|
|/ connectionGateways/schrijven|Maken of bijwerken van een Gateway-verbinding.|
|/ connectionGateways/verwijderen|Hiermee verwijdert u een Gateway verbinding.|
|/connectionGateways/join/Action|Een Gateway verbinding koppelt.|
|/classicmobileservices/Read|Klassieke mobiele Services krijgen.|
|/skus/Read|SKU's worden opgehaald.|
|certificaten/lezen|Haal de lijst van certificaten.|
|certificaten/schrijven|Voeg een nieuw certificaat of een bestaande bijgewerkt.|
|certificaten/verwijderen|Een bestaand certificaat verwijderen.|
|/Operations/Read|Bewerkingen ophalen.|
|/ aanbevelingen/lezen|De lijst met aanbevelingen voor abonnementen ophalen.|
|/ishostingenvironmentnameavailable/Read|Get als de naam van de Hosting-omgeving beschikbaar is.|
|/ apiManagementAccounts/lezen|De lijst met ApiManagementAccounts ophalen.|
|/ apiManagementAccounts/schrijven|Een nieuwe ApiManagementAccount toevoegen of een bestaande bijgewerkt|
|/ apiManagementAccounts/verwijderen|Een bestaande ApiManagementAccount verwijderen|
|/apiManagementAccounts/connectionAcls/Read|De lijst met ACL's verbinding ophalen.|
|/apiManagementAccounts/apiAcls/Read|ConnectionAcls lezen|
|/ verbindingen/lezen|De lijst met verbindingen opgehaald.|
|/ verbindingen/schrijven|Maken of bijwerken van een verbinding.|
|/ verbindingen/verwijderen|Hiermee verwijdert u een verbinding.|
|/Connections/join/Action|Lid van een verbinding.|
|/Connections/confirmconsentcode/Action|Controleer de verbindingen toestemming Code.|
|/Connections/listconsentlinks/Action|Lijst met toestemming koppelingen voor verbindingen.|
|/deploymentlocations/Read|Implementatie locaties ophalen.|
|/sourcecontrols/Read|Besturingselementen voor gegevensbronnen worden opgehaald.|
|sourcecontrols/schrijven|Besturingselementen voor gegevensbronnen bijwerken.|
|/managedhostingenvironments/Read|Ophalen van beheerde hostomgevingen.|
|/managedhostingenvironments/sites/Read|Ophalen van beheerde die als host fungeert voor de Web-Apps omgevingen.|
|/managedhostingenvironments/serverfarms/Read|Ophalen van beheerde omgevingen App Service-abonnementen die als host fungeert.|
|/Locations/managedapis/Read|Locaties beheerde API's worden opgehaald.|
|/Locations/apioperations/Read|Locaties API-bewerkingen ophalen.|
|/Locations/connectiongatewayinstallations/Read|Locaties verbinding Gateway installaties worden opgehaald.|
|/ listSitesAssignedToHostName/lezen|Namen van sites die zijn toegewezen aan de hostnaam ophalen.|

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [maakt u een aangepaste rol](role-based-access-control-custom-roles.md).

- Controleer de [ingebouwde RBAC-rollen](role-based-access-built-in-roles.md).

- Informatie over het beheren van toegangstoewijzingen [door gebruiker](role-based-access-control-manage-assignments.md) of [door resource](role-based-access-control-configure.md) 
