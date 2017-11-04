---
title: Aan de slag met rollen, machtigingen en -beveiliging met Azure Monitor | Microsoft Docs
description: Informatie over het gebruik van de ingebouwde rollen en machtigingen van de Monitor van het Azure toegang tot de bewaking van de resources te beperken.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2686e53b-72f0-4312-bcd3-3dc1b4a9b912
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: johnkem
ms.openlocfilehash: f8767073bb7a6723088bb2727346d23ec8872cd1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Aan de slag met rollen, machtigingen en -beveiliging met Azure-Monitor
Veel teams moeten strikt reguleren toegang tot gegevens en instellingen controleren. Als er teamleden die werken alleen op de bewaking (ondersteuningsmedewerkers, devops engineers) of als u een provider van beheerde services gebruikt, u kunt ze om toegang te verlenen tot alleen bewakingsgegevens terwijl de beperkingen voor de mogelijkheid om te maken, wijzigen of verwijderen van resources. In dit artikel laat zien hoe snel een ingebouwde bewaking RBAC-rol toepassen op een gebruiker in Azure of uw eigen aangepaste rol voor een gebruiker aan wie beperkte machtigingen voor controle moet bouwen. Vervolgens wordt de beveiligingsoverwegingen voor uw Azure-Monitor-gerelateerde resources en hoe u toegang tot de gegevens die ze bevatten alleen besproken.

## <a name="built-in-monitoring-roles"></a>Ingebouwde bewaking rollen
De ingebouwde rollen Azure Monitor zijn ontworpen om te beperken van toegang tot bronnen in een abonnement terwijl nog steeds degenen die verantwoordelijk zijn voor het bewaken van de infrastructuur voor het verkrijgen en configureert u de gegevens die ze nodig hebben. Azure biedt twee out-of-the-box-rollen: een bewaking lezer en Inzender bewaking.

### <a name="monitoring-reader"></a>Bewaking van de lezer
Personen die de rol Lezer bewaking toe kunt alle bewakingsgegevens weergeven in een abonnement, maar kan niet wijzigen van een resource of bewerken eventuele instellingen met betrekking tot de bewaking van resources. Deze rol is geschikt voor gebruikers in een organisatie, zoals ondersteuning of bewerkingen engineers die nodig kunnen hebben:

* Monitoring dashboards weergeven in de portal en maken van hun eigen persoonlijke dashboards voor bewaking.
* Query voor het gebruik van metrische gegevens de [REST-API van Azure-Monitor](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell-cmdlets](insights-powershell-samples.md), of [platformoverschrijdende CLI](insights-cli-samples.md).
* Query uitvoeren op het activiteitenlogboek met behulp van de portal, Azure Monitor REST-API, PowerShell-cmdlets of platformoverschrijdende CLI.
* Weergave de [diagnostische instellingen](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) voor een resource.
* Weergave de [Meld profiel](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) voor een abonnement.
* Weergave-instellingen voor automatisch schalen.
* Waarschuwing activiteit weergeven en instellingen.
* Toegang tot gegevens van Application Insights en gegevens weergeven in AI Analytics.
* Zoeken naar gegevens in de werkruimte logboekanalyse met inbegrip van gegevens over het gebruik van de werkruimte.
* Log Analytics-beheergroepen weergeven.
* Het schema van de zoekopdracht logboekanalyse ophalen.
* Log Analytics intelligence packs weergeven.
* Haal en Log Analytics opgeslagen zoekopdrachten uitvoeren.
* De opslagconfiguratie voor logboekanalyse ophalen.

> [!NOTE]
> Deze rol biedt niet alleen toegang geven tot logboekgegevens die is gestreamd naar een event hub of opgeslagen in een opslagaccount. [Zie hieronder](#security-considerations-for-monitoring-data) voor informatie over het configureren van toegang tot deze bronnen.
> 
> 

### <a name="monitoring-contributor"></a>Inzender bewaking
Personen die de rol van Inzender bewaking toegewezen alle bewakingsgegevens kunt weergeven in een abonnement en maak of controle-instellingen wijzigen, maar niet alle andere resources wijzigen. Deze rol is een hoofdverzameling van de rol Lezer bewaking en geschikt is voor leden van de bewaking team een organisatie of de beheerde service-providers die naast de bovenstaande, machtigingen en ook moet kunnen:

* Monitoring dashboards publiceren als een gedeelde dashboard.
* Stel [diagnostische instellingen](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) voor een resource.*
* Stel de [Meld profiel](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) voor een subscription.*
* Waarschuwing activiteit en -instellingen instellen.
* Maak webtests Application Insights en onderdelen.
* Werkruimte voor logboekanalyse lijst gedeelde sleutels.
* In- of uitschakelen van logboekanalyse intelligence packs.
* Maken en verwijderen en Log Analytics opgeslagen zoekopdrachten uitvoeren.
* Maken en verwijderen van de opslagconfiguratie voor logboekanalyse.

* gebruiker moet ook afzonderlijk worden gemachtigd ListKeys voor de doelbron (storage-account of event hub naamruimte) een profiel logboek of de diagnostische instelling in te stellen.

> [!NOTE]
> Deze rol biedt niet alleen toegang geven tot logboekgegevens die is gestreamd naar een event hub of opgeslagen in een opslagaccount. [Zie hieronder](#security-considerations-for-monitoring-data) voor informatie over het configureren van toegang tot deze bronnen.
> 
> 

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Bewaking van aangepaste RBAC-rollen en machtigingen
Als u de bovenstaande ingebouwde rollen niet voldoen aan de exacte behoeften van uw team, kunt u [maakt u een aangepaste RBAC-rol](../active-directory/role-based-access-control-custom-roles.md) met meer gedetailleerde machtigingen. Hieronder vindt u de algemene Azure Monitor RBAC-bewerkingen met de bijbehorende beschrijvingen.

| Bewerking | Beschrijving |
| --- | --- |
| Microsoft.Insights/ActionGroups/[Read, schrijven, verwijderen] |Lezen/schrijven/verwijderingsactie groepen. |
| Microsoft.Insights/ActivityLogAlerts/[Read, schrijven, verwijderen] |Activiteit voor lezen, schrijven, verwijderen logboek waarschuwingen. |
| Microsoft.Insights/AlertRules/[Read, schrijven, verwijderen] |Lezen/schrijven/verwijderen waarschuwingsregels (metrische waarschuwingen). |
| Microsoft.Insights/AlertRules/Incidents/Read |Lijst met incidenten (geschiedenis van de waarschuwingsregel wordt geactiveerd) voor regels voor waarschuwingen. Dit geldt alleen voor de portal. |
| Microsoft.Insights/AutoscaleSettings/[Read, schrijven, verwijderen] |Instellingen voor automatisch schalen die lezen/schrijven/verwijderen. |
| Microsoft.Insights/DiagnosticSettings/[Read, schrijven, verwijderen] |Diagnostische instellingen voor lezen, schrijven, verwijderen. |
| Microsoft.Insights/EventCategories/Read |Alle categorieën mogelijk in het activiteitenlogboek inventariseren. Gebruikt door de Azure-Portal. |
| Microsoft.Insights/eventtypes/digestevents/Read |Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. |
| Microsoft.Insights/eventtypes/values/Read |Activiteitenlogboek gebeurtenissen (management gebeurtenissen) in een abonnement weergeven. Deze machtiging is van toepassing op de portal zowel programmatische toegang tot het activiteitenlogboek. |
| Microsoft.Insights/ExtendedDiagnosticSettings/[Read, schrijven, verwijderen] | Lezen/schrijven/verwijderen van diagnostische instellingen voor network stroom Logboeken. |
| Microsoft.Insights/LogDefinitions/Read |Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. |
| Microsoft.Insights/LogProfiles/[Read, schrijven, verwijderen] |Lezen/schrijven/verwijderen logboek profielen (streaming activiteitenlogboek bij event hub- of opslag). |
| Microsoft.Insights/MetricAlerts/[Read, schrijven, verwijderen] |Lezen/schrijven/verwijderen bijna realtime metrische waarschuwingen (openbare preview). |
| Microsoft.Insights/MetricDefinitions/Read |Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen. |
| Microsoft.Insights/Metrics/Read |Metrische gegevens voor een resource lezen. |
| Microsoft.Insights/Register/Action |Registreer de Monitor voor Azure resourceprovider. |


> [!NOTE]
> Toegang tot waarschuwingen, diagnostische instellingen en metrische gegevens voor een bron is vereist dat de gebruiker leestoegang tot de resourcetype en bereik van de bron heeft. Een diagnostische instelling of logboek-profiel dat met een opslagaccount of stromen naar event hubs-archieven maken ('schrijven') is ook ListKeys machtiging hebben voor de doelresource van de gebruiker vereist.
> 
> 

Bijvoorbeeld, met behulp van de bovenstaande tabel kunt u een aangepaste RBAC-rol voor een 'activiteit logboekweergave' als volgt:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Beveiligingsoverwegingen voor het bewaken van gegevens
Bewakingsgegevens, met name de logboekbestanden: vertrouwelijke gegevens, zoals IP-adressen of namen van gebruikers kan bevatten. Het bewaken van gegevens van Azure wordt geleverd in drie basic formulieren:

1. Het activiteitenlogboek waarin alle besturingselement vlak acties op uw Azure-abonnement.
2. Diagnostische logboeken die verzonden door een resource van Logboeken zijn.
3. Metrieken worden gegenereerd door de bronnen.

Alle drie van de volgende gegevenstypen worden opgeslagen in een opslagaccount of worden gestreamd naar Event Hub, die beide algemeen Azure-resources zijn. Omdat dit algemene bronnen, is maken, verwijderen en toegang hebben tot deze een bewerking met bevoegdheid doorgaans gereserveerd voor een beheerder. Het is raadzaam om gebruik te maken van de volgende procedures voor bewaking-gerelateerde bronnen om te voorkomen dat misbruik:

* Een enkele, speciaal opslagaccount gebruiken voor het bewaken van gegevens. Als u bewakingsgegevens opdelen in meerdere opslagaccounts, nooit delen informatie over het gebruik van een opslagaccount tussen bewaking en niet-bewaking gegevens, zoals dit mogelijk per ongeluk geven degenen die hoeft alleen toegang tot bewakingsgegevens (bv. een derde partij SIEM) toegang tot de niet-bewaking gegevens.
* Een enkele, toegewezen Service Bus- of Event Hub-naamruimte via alle diagnostische instellingen gebruiken om dezelfde reden als hierboven.
* Beperken van toegang tot de bewaking-gerelateerde storage-accounts of event hubs doordat ze op een afzonderlijke resourcegroep en [gebruik scope](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) op uw bewaking rollen te beperken tot alleen die resourcegroep.
* Nooit de machtiging ListKeys voor storage-accounts of event hubs op abonnementsbereik wanneer een gebruiker alleen toegang tot bewakingsgegevens nodig. In plaats daarvan deze machtigingen geven voor de gebruiker op een resource of resourcegroep (als u een speciale bewaking resourcegroep hebt) bereik.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Beperken van toegang tot de bewaking-gerelateerde storage-accounts
Wanneer een gebruiker of toepassing moet toegang tot bewakingsgegevens in een opslagaccount, moet u [genereren van een Account-SAS](https://msdn.microsoft.com/library/azure/mt584140.aspx) op het opslagaccount waarin bewakingsgegevens serviceniveau alleen-lezen toegang tot blob-opslag. In PowerShell dit als volgt uitzien:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

U kunt vervolgens het token geeft aan de entiteit dat moet worden gelezen uit de opslag die account en deze kunt lijst en uit alle blobs in dit opslagaccount gelezen.

U kunt ook als u beheren met deze machtiging met RBAC wilt, kunt u die entiteit Microsoft.Storage/storageAccounts/listkeys/action toestemming geven op dat bepaalde storage-account. Dit is nodig voor gebruikers die worden kunt instellen van een diagnostische instelling of meld u profiel moeten te archiveren naar een opslagaccount. U kunt bijvoorbeeld de volgende aangepaste RBAC-rol voor een gebruiker of toepassing die alleen moet worden gelezen vanuit een opslagaccount maken:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [!WARNING]
> De machtiging ListKeys kan de gebruiker voor een lijst met sleutels voor de primaire en secundaire opslagaccount. Deze sleutels verlenen de gebruiker alle ondertekende machtigingen (lezen, schrijven, BLOB's maken, verwijderen van blobs, etc.) voor alle ondertekend services (blob, queue, table, bestand) in die storage-account. U wordt aangeraden met een Account-SAS hierboven wordt beschreven indien mogelijk.
> 
> 

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Beperken van toegang tot de bewaking-gerelateerde event hubs
Een vergelijkbaar patroon met event hubs kan worden uitgevoerd, maar u moet eerst een speciale Listen autorisatieregel maken. Als u toegang verlenen tot een toepassing die alleen nodig heeft om te luisteren naar bewaking-gerelateerde event hubs wilt, het volgende doen:

1. Een gedeeld toegangsbeleid maken op de event hub die zijn gemaakt voor streaming bewakingsgegevens met alleen Listen claims. Dit kan worden gedaan in de portal. Bijvoorbeeld, u mogelijk deze aanroepen 'monitoringReadOnly'. Indien mogelijk wilt u deze sleutel rechtstreeks naar de verbruiker geven en de volgende stap overslaan.
2. Als de consument kunnen moet ophalen van de belangrijkste ad-hoc, verleent u de gebruiker de actie ListKeys voor deze event hub. Dit is ook nodig zijn voor gebruikers die moeten worden kunt instellen van een diagnostische instelling of meld u profiel naar een stream naar event hubs. U kunt bijvoorbeeld een regel RBAC maken:
   
   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over RBAC en machtigingen in Resource Manager](../active-directory/role-based-access-control-what-is.md)
* [Lees het overzicht van de bewaking in Azure](monitoring-overview.md)

