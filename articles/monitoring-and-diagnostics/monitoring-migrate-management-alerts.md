---
title: Waarschuwingen van Azure op gebeurtenissen Management migreren naar activiteit logboek waarschuwingen | Microsoft Docs
description: Waarschuwingen op gebeurtenissen management worden op 1 oktober verwijderd. Voorbereiden door te migreren bestaande waarschuwingen.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: johnkem
ms.openlocfilehash: 08a457029d3721f5c38dbcd2d2aab7d09a241d8f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-azure-alerts-on-management-events-to-activity-log-alerts"></a>Waarschuwingen van Azure op gebeurtenissen management migreren naar activiteitenlogboek van waarschuwingen


> [!WARNING]
> Waarschuwingen op gebeurtenissen management worden uitgeschakeld op of na oktober 1. Gebruik de volgende instructies uit om te begrijpen als u deze waarschuwingen hebben en deze als dit het geval migreert.
>
> 

## <a name="what-is-changing"></a>Wat wordt gewijzigd

Monitor voor Azure (voorheen Azure Insights) aangeboden een mogelijkheid voor het maken van een waarschuwing geactiveerd op gebeurtenissen en meldingen naar de URL of e-mailadressen van een webhook gegenereerd. U hebt gemaakt een van deze waarschuwingen van de volgende manieren:
* In de Azure-portal voor bepaalde resourcetypen onder bewaking -> waarschuwingen-waarschuwing toevoegen, waarbij 'Waarschuwen' is ingesteld op "Gebeurtenissen" >
* Door de cmdlet Add-AzureRmLogAlertRule PowerShell
* Met behulp van rechtstreeks [de waarschuwing REST-API](http://docs.microsoft.com/rest/api/monitor/alertrules) met odata.type = 'ManagementEventRuleCondition' en dataSource.odata.type = "RuleManagementEventDataSource"
 
De volgende PowerShell-script retourneert een lijst met alle waarschuwingen op gebeurtenissen die u in uw abonnement, evenals de voorwaarden op elke waarschuwing hebt.

```powershell
Login-AzureRmAccount
$alerts = $null
foreach ($rg in Get-AzureRmResourceGroup ) {
  $alerts += Get-AzureRmAlertRule -ResourceGroup $rg.ResourceGroupName
}
foreach ($alert in $alerts) {
  if($alert.Properties.Condition.DataSource.GetType().Name.Equals("RuleManagementEventDataSource")) {
    "Alert Name: " + $alert.Name
    "Alert Resource ID: " + $alert.Id
    "Alert conditions:"
    $alert.Properties.Condition.DataSource
    "---------------------------------"
  }
} 
```

Als er geen waarschuwingen voor van Beheergebeurtenissen, wordt een reeks waarschuwingsberichten zoals deze uitvoer in de bovenstaande PowerShell-cmdlet:

`WARNING: The output of this cmdlet will be flattened, i.e. elimination of the properties field, in a future release to improve the user experience.`

Deze waarschuwingsberichten kunnen worden genegeerd. Als u waarschuwingen op gebeurtenissen management hebt, wordt de uitvoer van deze PowerShell-cmdlet ziet er als volgt:

```
Alert Name: webhookEvent1
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/webhookEvent1
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : microsoft.web/sites/start/action
ResourceGroupName    : 
ResourceProviderName : 
Status               : succeeded
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Web/sites/samplealertapp

---------------------------------
Alert Name: someclilogalert
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/someclilogalert
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : Start
ResourceGroupName    : 
ResourceProviderName : 
Status               : 
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Compute/virtualMachines/Seaofclouds

---------------------------------
```

Elke waarschuwing wordt gescheiden door een gestreepte lijn en details opnemen in de bron-ID van de waarschuwing en de specifieke regel die wordt bewaakt.

Deze functionaliteit is overgegaan naar [waarschuwingen van Azure controleren activiteit logboek](monitoring-activity-log-alerts.md). Deze nieuwe waarschuwingen kunnen u een voorwaarde instellen op activiteitenlogboek van gebeurtenissen en een melding ontvangen wanneer een nieuwe gebeurtenis overeenkomt met de voorwaarde. Ze bieden ook verschillende verbeteringen van waarschuwingen op gebeurtenissen management:
* U kunt uw groep geadresseerden voor meldingen ('acties') hergebruiken over veel waarschuwingen met [actiegroepen](monitoring-action-groups.md), waardoor de complexiteit van het wijzigen van wie een waarschuwing moet ontvangen.
* U kunt een melding ontvangen rechtstreeks op uw telefoon met SMS met groepen in te grijpen.
* U kunt [activiteit logboek waarschuwingen maken met Resource Manager-sjablonen](monitoring-create-activity-log-alerts-with-resource-manager-template.md).
* U kunt voorwaarden maken met de grotere flexibiliteit en complexiteit om te voldoen aan uw specifieke behoeften.
* Meldingen worden sneller afgeleverd.
 
## <a name="how-to-migrate"></a>Het migreren
 
Voor het maken van een nieuwe activiteit logboek waarschuwing, kunt u:
* Ga als volgt [onze handleiding voor het maken van een waarschuwing in de Azure portal](monitoring-activity-log-alerts.md)
* Meer informatie over hoe [een waarschuwing met een Resource Manager-sjabloon maken](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
 
Waarschuwingen op het van Beheergebeurtenissen die u eerder hebt gemaakt wordt niet automatisch worden gemigreerd naar activiteit logboek waarschuwingen. U moet het voorgaande PowerShell-script gebruiken voor het weergeven van de waarschuwingen op gebeurtenissen voor dat u momenteel hebt geconfigureerd en deze handmatig opnieuw als de activiteit logboek waarschuwingen maken. Dit moet worden uitgevoerd vóór oktober 1, waarna waarschuwingen op gebeurtenissen management niet langer zichtbaar zijn in uw Azure-abonnement. Andere typen waarschuwingen van Azure, met inbegrip van metrische waarschuwingen van Azure controleren, Application Insights-waarschuwingen en Log Analytics waarschuwingen worden hierdoor niet beïnvloed door deze wijziging. Als u vragen hebt, kunt u boeken in de opmerkingen hieronder.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Activity Log](monitoring-overview-activity-logs.md)
* Configureer [activiteit logboek waarschuwingen per Azure-portal](monitoring-activity-log-alerts.md)
* Configureer [activiteit logboek waarschuwingen via Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Controleer de [activiteit logboek waarschuwing webhook schema](monitoring-activity-log-alerts-webhook.md)
* Meer informatie over [servicemeldingen](monitoring-service-notifications.md)
* Meer informatie over [actiegroepen](monitoring-action-groups.md)
