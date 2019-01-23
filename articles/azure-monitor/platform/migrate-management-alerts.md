---
title: Azure-waarschuwingen op gebeurtenissen voor migreren naar waarschuwingen voor activiteitenlogboek
description: Waarschuwingen over gebeurtenissen worden verwijderd op 1 oktober. Voorbereiden op migreren bestaande waarschuwingen.
author: johnkemnetz
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/14/2017
ms.author: johnkem
ms.subservice: alerts
ms.openlocfilehash: 55d0269aaa330f928a9d037eec6a3445825a5ed3
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470338"
---
# <a name="migrate-azure-alerts-on-management-events-to-activity-log-alerts"></a>Azure-waarschuwingen op gebeurtenissen voor migreren naar waarschuwingen voor activiteitenlogboek


> [!WARNING]
> Waarschuwingen over gebeurtenissen worden uitgeschakeld op of na 1 oktober. Gebruik de onderstaande instructies om te begrijpen als u deze waarschuwingen hebben en migreren van de accounts als dit het geval is.
>
> 

## <a name="what-is-changing"></a>Wat wordt gewijzigd

Azure Monitor (voorheen Azure Insights) aangeboden een mogelijkheid om te maken van een waarschuwing geactiveerd korting op gebeurtenissen en meldingen naar een webhook-URL of e-mailadressen die worden gegenereerd. U hebt gemaakt een van deze waarschuwingen een van de volgende manieren:
* In de Azure-portal voor bepaalde resourcetypen onder bewaking -> waarschuwingen-waarschuwing toevoegen, waarbij 'Waarschuwen' is ingesteld op "Events" >
* Door de cmdlet Add-AzureRmLogAlertRule PowerShell
* Met behulp van rechtstreeks [de waarschuwing REST-API](https://docs.microsoft.com/rest/api/monitor/alertrules) met odata.type = "ManagementEventRuleCondition" en dataSource.odata.type = "RuleManagementEventDataSource"
 
De volgende PowerShell-script retourneert een lijst van alle waarschuwingen voor gebeurtenissen die u in uw abonnement, evenals de voorwaarden op elke waarschuwing hebt.

```powershell
Connect-AzureRmAccount
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

Hebt u geen waarschuwingen voor gebeurtenissen, wordt een reeks waarschuwingsberichten zoals deze in de bovenstaande PowerShell-cmdlet uitvoeren:

`WARNING: The output of this cmdlet will be flattened, i.e. elimination of the properties field, in a future release to improve the user experience.`

Deze waarschuwingen kunnen worden genegeerd. Als u waarschuwingen op gebeurtenissen voor hebt, wordt de uitvoer van deze PowerShell-cmdlet ziet er als volgt:

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

Elke waarschuwing wordt gescheiden door een gestreepte lijn en details omvatten de resource-ID van de waarschuwing en de specifieke regel die worden bewaakt.

Deze functionaliteit is overgegaan naar [waarschuwingen voor activiteitenlogboeken Azure Monitor](../../azure-monitor/platform/activity-log-alerts.md). Deze nieuwe waarschuwingen kunnen u een voorwaarde instellen op gebeurtenissen in activiteitenlogboeken en een melding ontvangen wanneer een nieuwe gebeurtenis overeenkomt met de voorwaarde. Ze bieden ook verschillende verbeteringen van waarschuwingen over gebeurtenissen:
* U kunt uw groep geadresseerden voor meldingen ('acties') opnieuw gebruiken voor veel waarschuwingen via [actiegroepen](../../azure-monitor/platform/action-groups.md), waardoor de complexiteit van het wijzigen van wie een waarschuwing moet ontvangen.
* U kunt een melding ontvangen rechtstreeks op uw telefoon met SMS met Actiegroepen.
* U kunt [waarschuwingen voor activiteitenlogboeken maken met Resource Manager-sjablonen](../../azure-monitor/platform/alerts-activity-log.md).
* U kunt voorwaarden maken met grotere flexibiliteit en complexiteit om te voldoen aan uw specifieke behoeften.
* Meldingen worden sneller geleverd.
 
## <a name="how-to-migrate"></a>Migreren
 
Voor het maken van een nieuwe activiteit Log waarschuwing, kunt u:
* Ga als volgt [onze handleiding voor het maken van een waarschuwing in Azure portal](../../azure-monitor/platform/activity-log-alerts.md)
* Meer informatie over het [een waarschuwing met behulp van Resource Manager-sjabloon maken](../../azure-monitor/platform/alerts-activity-log.md)
 
Waarschuwingen over gebeurtenissen voor dat u eerder hebt gemaakt wordt niet automatisch worden gemigreerd naar waarschuwingen voor activiteitenlogboeken. U moet de voorgaande PowerShell-script gebruiken om de waarschuwingen op gebeurtenissen voor dat u momenteel hebt geconfigureerd en deze handmatig opnieuw als waarschuwingen voor activiteitenlogboeken maken weergeven. Dit moet worden uitgevoerd vóór 1 oktober, waarna waarschuwingen over gebeurtenissen niet langer zichtbaar is in uw Azure-abonnement. Andere typen waarschuwingen van Azure, met inbegrip van Azure Monitor metrische waarschuwingen, Application Insights-waarschuwingen en Log Analytics-waarschuwingen worden niet beïnvloed door deze wijziging. Als u vragen hebt, kunt u boeken in de opmerkingen hieronder.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [activiteitenlogboek](../../azure-monitor/platform/activity-logs-overview.md)
* Configureer [waarschuwingen voor activiteitenlogboeken via Azure portal](../../azure-monitor/platform/activity-log-alerts.md)
* Configureer [waarschuwingen voor activiteitenlogboeken via Resource Manager](../../azure-monitor/platform/alerts-activity-log.md)
* Controleer de [activiteit log waarschuwing webhook-schema](../../azure-monitor/platform/activity-log-alerts-webhook.md)
* Meer informatie over [servicemeldingen](../../azure-monitor/platform/service-notifications.md)
* Meer informatie over [actiegroepen](../../azure-monitor/platform/action-groups.md)

