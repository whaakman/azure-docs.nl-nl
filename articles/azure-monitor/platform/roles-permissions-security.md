---
title: Aan de slag met rollen, machtigingen en beveiliging met Azure Monitor
description: Informatie over het gebruik van Azure Monitor ingebouwde rollen en machtigingen voor toegang tot het bewaken van resources te beperken.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: 310a349aad4e6626033ca2f378e7c1b0ffa96560
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53388847"
---
# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Aan de slag met rollen, machtigingen en beveiliging met Azure Monitor
Veel teams moeten strikt regelen de toegang tot gegevens en instellingen controleren. Bijvoorbeeld, als u de teamleden die uitsluitend over het bewaken van (ondersteuningstechnici, devops-technici) werken hebt of als u een provider van beheerde services gebruikt, kunt u ze om toegang te verlenen tot alleen bewakingsgegevens tijdens het beperken van de mogelijkheid om te maken, wijzigen, of resources verwijderen. In dit artikel laat zien hoe snel een ingebouwde bewaking RBAC-rol van toepassing op een gebruiker in Azure of bouw uw eigen aangepaste rol voor een gebruiker bent en beperkte machtigingen voor bewaking. Hierin worden vervolgens beveiligingsoverwegingen voor uw resources met betrekking tot Azure Monitor en het beperken van toegang tot de gegevens die ze bevatten.

## <a name="built-in-monitoring-roles"></a>Ingebouwde bewaking rollen
Azure Monitor ingebouwde rollen zijn ontworpen om te beperken van toegang tot resources in een abonnement terwijl nog steeds die verantwoordelijk zijn voor het bewaken van de infrastructuur voor het downloaden en configureren van de gegevens die ze nodig hebben. Azure Monitor biedt twee out-of-the-box-rollen: Een Monitoring Reader en een controle Inzender.

### <a name="monitoring-reader"></a>Lezer voor bewaking
Mensen de rol Lezer bewaking toe kunt alle controlegegevens weergeven in een abonnement, maar kan niet alle resource wijzigen of bewerken instellingen met betrekking tot het bewaken van resources. Deze rol is geschikt voor gebruikers in een organisatie, zoals ondersteuning of operations engineers, die nodig kunnen hebben:

* Monitoring dashboards weergeven in de portal en hun eigen persoonlijke monitoring dashboards maken.
* Waarschuwingsregels is gedefinieerd in weergeven [Azure-waarschuwingen](../../azure-monitor/platform/alerts-overview.md)
* Query voor het gebruik van metrische gegevens over de [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell-cmdlets](../../monitoring-and-diagnostics/insights-powershell-samples.md), of [platformoverschrijdende CLI](../../azure-monitor/platform/cli-samples.md).
* Query uitvoeren op het activiteitenlogboek via de portal, Azure Monitor REST API, PowerShell-cmdlets of platformoverschrijdende CLI.
* Weergave de [diagnostische instellingen](../../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) voor een resource.
* Weergave de [logboekprofiel](../../monitoring-and-diagnostics/monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) voor een abonnement.
* Bekijk de instellingen voor automatisch schalen.
* Waarschuwingsactiviteit weergeven en -instellingen.
* Toegang tot Application Insights-gegevens en gegevens weergeven in Analytics AI.
* Zoeken naar gegevens in de werkruimte Log Analytics met inbegrip van gegevens over gebruik voor de werkruimte.
* Log Analytics-beheergroepen weergeven.
* De Log Analytics-zoekschema ophalen.
* Lijst met Log Analytics intelligencepacks.
* Haal en Log Analytics opgeslagen zoekopdrachten uitvoeren.
* De opslagconfiguratie voor logboekanalyse ophalen.

> [!NOTE]
> Deze rol heeft geen leestoegang geven tot logboekgegevens die zijn gestreamd naar een event hub of die zijn opgeslagen in een storage-account. [Zie hieronder](#security-considerations-for-monitoring-data) voor informatie over het configureren van toegang tot deze bronnen.
> 
> 

### <a name="monitoring-contributor"></a>Bijdrager voor bewaking
Mensen de rol Inzender bewaking toegewezen kan alle controlegegevens weergeven in een abonnement en maak of controle-instellingen wijzigen, maar alle andere resources kunnen niet worden gewijzigd. Deze rol is een superset van de rol van lezer bewaking en geschikt is voor leden van de controle-team van een organisatie of de providers van beheerde services die naast de bovenstaande, machtigingen en ook moet kunnen:

* Monitoring dashboards als een gedeeld dashboard publiceren.
* Stel [diagnostische instellingen](../../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) voor een resource.*
* Stel de [logboekprofiel](../../monitoring-and-diagnostics/monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) voor een wijzigen.*
* Activiteit van de regels voor waarschuwingen en-instellingen via [Azure-waarschuwingen](../../azure-monitor/platform/alerts-overview.md).
* Application Insights-webtests en -onderdelen maken.
* Lijst met Log Analytics-werkruimte gedeelde sleutels.
* In- of uitschakelen van Log Analytics intelligencepacks.
* Maken en verwijderen en Log Analytics opgeslagen zoekopdrachten uitvoeren.
* Maak en verwijder de opslagconfiguratie voor logboekanalyse.

* de gebruiker moet ook afzonderlijk worden verleend ListKeys machtiging voor de doelresource (storage-account of event hub-naamruimte) een logboekprofiel of de diagnostische instelling in te stellen.

> [!NOTE]
> Deze rol heeft geen leestoegang geven tot logboekgegevens die zijn gestreamd naar een event hub of die zijn opgeslagen in een storage-account. [Zie hieronder](#security-considerations-for-monitoring-data) voor informatie over het configureren van toegang tot deze bronnen.
> 
> 

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Machtigingen voor controle en aangepaste RBAC-rollen
Als de bovenstaande ingebouwde rollen niet voldoen aan de exacte behoeften van uw team, kunt u [maakt u een aangepaste RBAC-rol](../../role-based-access-control/custom-roles.md) met meer gedetailleerde machtigingen. Hieronder vindt u de algemene Azure Monitor RBAC-bewerkingen met de bijbehorende beschrijvingen.

| Bewerking | Description |
| --- | --- |
| Microsoft.Insights/ActionGroups/[Read, schrijven, verwijderen] |Actiegroepen lezen, schrijven en verwijderen. |
| Microsoft.Insights/ActivityLogAlerts/[Read, schrijven, verwijderen] |Waarschuwingen voor activiteitenlogboeken lezen, schrijven en verwijderen. |
| Microsoft.Insights/AlertRules/[Read, schrijven, verwijderen] |Waarschuwingsregels (van klassieke waarschuwingen) lezen, schrijven en verwijderen. |
| Microsoft.Insights/AlertRules/Incidents/Read |Lijst met incidenten (geschiedenis van de waarschuwingsregel wordt geactiveerd) voor waarschuwingsregels. Dit is alleen van toepassing op de portal. |
| Microsoft.Insights/AutoscaleSettings/[Read, schrijven, verwijderen] |Instellingen voor automatisch schalen lezen, schrijven en verwijderen. |
| Microsoft.Insights/DiagnosticSettings/[Read, schrijven, verwijderen] |Diagnostische instellingen lezen, schrijven en verwijderen. |
| Microsoft.Insights/EventCategories/Read |Het inventariseren van alle categorieën mogelijk in het activiteitenlogboek. Gebruikt door de Azure-portal. |
| Microsoft.Insights/eventtypes/digestevents/Read |Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. |
| Microsoft.Insights/eventtypes/values/Read |Lijst met gebeurtenissen in activiteitenlogboeken (van Beheergebeurtenissen) in een abonnement. Deze machtiging is van toepassing op zowel programmatische als portal toegang tot het activiteitenlogboek. |
| Microsoft.Insights/ExtendedDiagnosticSettings/[Read, schrijven, verwijderen] | Diagnostische instellingen lezen, schrijven en verwijderen voor netwerk-stroomlogboeken. |
| Microsoft.Insights/LogDefinitions/Read |Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. |
| Microsoft.Insights/LogProfiles/[Read, schrijven, verwijderen] |Lezen/schrijven/verwijderen logboekprofielen (activiteitenlogboek streamen naar event hub of storage-account). |
| Microsoft.Insights/MetricAlerts/[Read, schrijven, verwijderen] |Lezen/schrijven/verwijderen bijna realtime metrische waarschuwingen |
| Microsoft.Insights/MetricDefinitions/Read |Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen. |
| Microsoft.Insights/Metrics/Read |De metrische gegevens voor een resource lezen. |
| Microsoft.Insights/Register/Action |Registreer de resourceprovider van Azure Monitor. |
| Microsoft.Insights/ScheduledQueryRules/[Read, schrijven, verwijderen] |Lezen/schrijven/verwijderen logboekwaarschuwingen in Azure Monitor. |



> [!NOTE]
> Toegang tot waarschuwingen, instellingen voor diagnostische en metrische gegevens voor een resource vereist dat de gebruiker leestoegang tot de resourcetype en bereik van die resource heeft. Het maken van ('write') moet een diagnostische instelling of log-profiel dat aan een opslagaccount of stromen naar eventhubs-archieven de gebruiker ook ListKeys machtiging hebben voor de doelresource.
> 
> 

Bijvoorbeeld, met behulp van de bovenstaande tabel die u kunt een aangepaste RBAC-rol maken voor een 'activiteit Log Reader' als volgt:

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
Door gegevens te controleren, met name de logboekbestanden, vertrouwelijke gegevens, zoals IP-adressen of namen van de gebruikers kan bevatten. Bewakingsgegevens van Azure is beschikbaar in drie eenvoudige vormen:

1. Het activiteitenlogboek die hierin alle controlelaag acties in uw Azure-abonnement worden.
2. Diagnostische logboeken, de logboeken door een resource zijn.
3. Metrische gegevens die worden gegenereerd door resources.

Alle drie van de volgende gegevenstypen kunnen worden opgeslagen in een opslagaccount of gestreamd naar Event Hub, die beide algemene Azure-resources zijn. Omdat dit algemene resources zijn, is het maken, verwijderen en ze toegang tot een bevoegde bewerking die is gereserveerd voor een beheerder. Het is raadzaam om gebruik te maken van de volgende procedures voor bewaking met betrekking tot resources om misbruik te voorkomen:

* Een enkele, specifieke storage-account gebruiken voor het bewaken van gegevens. Als u nodig hebt voor het scheiden van gegevens in meerdere opslagaccounts, gebruik van een opslagaccount tussen bewaking nooit delen en niet-bewaking gegevens, zoals dit mogelijk per ongeluk geven de personen die alleen toegang tot bewakingsgegevens (bijvoorbeeld een SIEM van derden) toegang tot niet-bewaking gegevens.
* Gebruik een enkele, specifieke Service Bus of Event Hub-naamruimte voor alle diagnostische instellingen voor dezelfde reden als hierboven.
* Beperk de toegang tot de bewaking met betrekking tot storage-accounts of eventhubs doordat ze op een afzonderlijke resourcegroep en [bereik gebruiken](../../role-based-access-control/overview.md#scope) op uw bewaking rollen om te beperken tot alleen die resourcegroep.
* Nooit de machtiging ListKeys voor storage-accounts of eventhubs binnen het abonnementsbereik als een gebruiker alleen toegang moet tot gegevens te controleren. (Als u een speciale resourcegroep waarin u bewaking hebt) in plaats daarvan deze machtigingen geven aan de gebruiker op een resource of resourcegroep bereik.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Beperken van toegang tot de bewaking met betrekking tot storage-accounts
Wanneer een gebruiker of toepassing moet toegang tot gegevens in een storage-account te controleren, moet u [genereren van een Account-SAS](https://msdn.microsoft.com/library/azure/mt584140.aspx) op het opslagaccount waarin gegevens met service level alleen-lezen toegang tot blob-opslag. In PowerShell, dit als volgt uitzien:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

U kunt vervolgens het token naar de entiteit geven dat moet leesrechten voor deze opslag-account en het kunt weergeven en uit alle blobs in dat opslagaccount lezen.

U kunt ook als u nodig hebt voor het beheren van deze machtiging met RBAC, kunt u die entiteit de Microsoft.Storage/storageAccounts/listkeys/action machtiging verlenen voor die bepaalde storage-account. Dit is nodig voor gebruikers die nodig kunnen een diagnostische instelling instellen of logboekprofiel te archiveren naar een opslagaccount. U kunt bijvoorbeeld de volgende aangepaste RBAC-rol voor een gebruiker of toepassing die moet alleen lezen uit een opslagaccount maken:

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
> De machtiging ListKeys kan de gebruiker om de primaire en secundaire opslagaccountsleutels weer te geven. Deze sleutels Verleen de gebruiker alle ondertekende machtigingen (lezen, schrijven, blobs te maken, verwijderen van blobs, enz.) voor alle ondertekende services (blob, wachtrij, tabel, -bestand) dat opslagaccount. We raden u aan met behulp van een Account-SAS die hierboven worden beschreven, indien mogelijk.
> 
> 

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Beperken van toegang tot de bewaking met betrekking tot eventhubs
Een vergelijkbaar patroon kan worden gevolgd met eventhubs, maar u moet eerst een toegewezen Listen-autorisatieregel. Als u wilt verlenen, toegang tot een toepassing die alleen nodig heeft om te luisteren naar bewaking-gerelateerde eventhubs, het volgende doen:

1. Maak een beleid voor gedeelde toegang op de event hub die zijn gemaakt voor het streamen van gegevens met alleen Listen-claims. Dit kan worden gedaan in de portal. Bijvoorbeeld, kunt u deze aanroepen "monitoringReadOnly." U wilt, indien mogelijk, geven deze sleutel rechtstreeks naar de gebruiker en de volgende stap overslaan.
2. Als de consument kunnen om op te halen van de belangrijkste ad-hoc moet, verleent u de gebruiker de actie ListKeys voor die event hub. Dit is ook nodig zijn voor gebruikers die willen kunnen een diagnostische instelling instellen of logboekprofiel te streamen naar eventhubs. U kunt bijvoorbeeld een regel voor RBAC maken:
   
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

## <a name="monitoring-within-a-secured-virtual-network"></a>Bewaking in een beveiligde Virtueelnetwerk

Azure Monitor moet toegang hebben tot uw Azure-resources voor de services die u inschakelt. Als u bewaken van uw Azure-resources wilt terwijl nog steeds beveiligen tegen toegang tot Internet, kunt u de volgende instellingen inschakelen.

### <a name="secured-storage-accounts"></a>Beveiligde Storage-Accounts 

Door gegevens te controleren is vaak geschreven naar een opslagaccount. Het is raadzaam om ervoor te zorgen dat de gegevens die zijn gekopieerd naar een Opslagaccount kan niet worden geopend door onbevoegde gebruikers. Voor extra beveiliging kunt u toegang tot het netwerk toe te staan dat uw geautoriseerde resources en de vertrouwde Microsoft-services toegang tot een opslagaccount vergrendelen door het beperken van een storage-account voor het gebruik van 'geselecteerde netwerken'.
![Dialoogvenster van de instellingen voor Azure Storage](./media/roles-permissions-security/secured-storage-example.png) Azure Monitor worden beschouwd als een van deze 'vertrouwde Microsoft-services' als u toestaat dat vertrouwde Microsoft-services voor toegang tot uw beveiligde opslag, Azure monitor hebben toegang tot uw beveiligde Storage-Account; inschakelen Diagnostische logboeken van Azure Monitor, activiteitenlogboek en metrische gegevens schrijven naar uw Storage-Account in deze beveiligde omstandigheden. Hiermee schakelt u ook Log Analytics om te lezen logboeken van beveiligde opslag.   


Zie voor meer informatie, [Network security en Azure Storage](../../storage/common/storage-network-security.md)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over RBAC en machtigingen in Resource Manager](../../role-based-access-control/overview.md)
* [Lees het overzicht van de bewaking in Azure](../../azure-monitor/overview.md)

