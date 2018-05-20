---
title: Activiteitenlogboeken RBAC wijzigingen weergeven in Azure | Microsoft Docs
description: Activiteitenlogboeken voor op rollen gebaseerde toegang besturingselement wordt gewijzigd voor de afgelopen negentig dagen weergeven.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2017
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e48ea2293c186bbc337f9d70464df374d64b5e61
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="view-activity-logs-for-role-based-access-control-changes"></a>Activiteitenlogboeken bekijken voor op rollen gebaseerde toegang besturingselement wordt gewijzigd

Elk gewenst moment iemand wijzigingen in roldefinities of roltoewijzingen binnen uw abonnementen aanbrengt, de wijzigingen worden geregistreerd [Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) in de beheercategorie. U kunt de activiteitenlogboeken wilt zien welke wijzigingen voor de toegangsbeheer (RBAC) toegang op basis van rollen voor de afgelopen negentig dagen weergeven.

## <a name="operations-that-are-logged"></a>Bewerkingen die worden geregistreerd

Hier volgen de RBAC-gerelateerde bewerkingen die worden vastgelegd in logboek:

- Een aangepaste roldefinitie maken of bijwerken
- De aangepaste roldefinitie verwijderen
- Roltoewijzing maken
- Roltoewijzing verwijderen

## <a name="azure-portal"></a>Azure Portal

De eenvoudigste manier om te beginnen is om de activiteitenlogboeken met de Azure-portal weer te geven. De volgende Schermafbeelding toont een voorbeeld van een gebeurtenislogboek die een filter is toegepast om weer te geven de **Administrative** categorie samen met de roldefinitie en rol toewijzingsbewerkingen. Dit omvat ook een koppeling om te downloaden van de logboeken als een CSV-bestand.

![Activiteitenlogboeken via de portal - schermafbeelding](./media/change-history-report/activity-log-portal.png)

Zie voor meer informatie [gebeurtenissen weergeven in activiteitenlogboek](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json).

## <a name="azure-powershell"></a>Azure PowerShell

Om activiteitenlogboeken te raadplegen met Azure PowerShell, gebruikt u de [Get-AzureRmLog](/powershell/module/azurerm.insights/get-azurermlog) opdracht.

Met deze opdracht worden de wijzigingen aan toewijzingen van alle rollen in een abonnement voor de afgelopen zeven dagen:

```azurepowershell
Get-AzureRmLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

Met deze opdracht worden alle wijzigingen in de rol definities in een resourcegroep voor de afgelopen zeven dagen:

```azurepowershell
Get-AzureRmLog -ResourceGroupName pharma-sales-projectforecast -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

Met deze opdracht worden alle toewijzingen van rollen en functie definitiewijzigingen in een abonnement voor de afgelopen zeven dagen en de resultaten in een lijst weergegeven:

```azurepowershell
Get-AzureRmLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
```

```Example
Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:07 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          statusCode     : Created
                          serviceRequestId: 11111111-1111-1111-1111-111111111111

Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:05 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          requestbody    : {"Id":"22222222-2222-2222-2222-222222222222","Properties":{"PrincipalId":"33333333-3333-3333-3333-333333333333","RoleDefinitionId":"/subscriptions/00000000-0000-0000-0000-000000000000/providers
                          /Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","Scope":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast"}}

```

## <a name="azure-cli"></a>Azure-CLI

Om activiteitenlogboeken te raadplegen met de Azure CLI, gebruikt u de [az monitor activiteitenlogboek lijst](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) opdracht.

Met deze opdracht worden de activiteitenlogboeken in een resourcegroep sinds de begintijd:

```azurecli
az monitor activity-log list --resource-group pharma-sales-projectforecast --start-time 2018-04-20T00:00:00Z
```

Met deze opdracht worden de activiteitenlogboeken van de voor autorisatie resourceprovider omdat de begintijd:

```azurecli
az monitor activity-log list --resource-provider "Microsoft.Authorization" --start-time 2018-04-20T00:00:00Z
```

## <a name="azure-log-analytics"></a>Azure Log Analytics

[Azure Log Analytics](../log-analytics/log-analytics-overview.md) is een ander hulpmiddel dat u gebruiken kunt voor het verzamelen en analyseren van op rollen gebaseerde toegang besturingselement wordt gewijzigd voor alle Azure-resources. Log Analytics biedt de volgende voordelen:

- Schrijven van complexe query's en logica
- Integreren met waarschuwingen, Power BI en andere hulpprogramma 's
- Gegevens opslaan voor langere bewaartermijn
- Met andere logboeken zoals beveiliging, de virtuele machine en aangepaste kruisverwijzingen

Hier volgen de basisstappen om te beginnen:

1. [Maken van een werkruimte voor logboekanalyse](../log-analytics/log-analytics-quick-create-workspace.md).

1. [De activiteit Log Analytics-oplossing configureren](../log-analytics/log-analytics-activity.md#configuration) voor uw werkruimte.

1. [Bekijk de activiteitenlogboeken](../log-analytics/log-analytics-activity.md#using-the-solution). Een snelle manier om te navigeren naar de pagina overzicht van de activiteit Log Analytics is te klikken op de **logboekanalyse** optie.

   ![Log Analytics optie in de portal](./media/change-history-report/azure-log-analytics-option.png)

1. Optioneel gebruik van de [logboek zoeken](../log-analytics/log-analytics-log-search.md) pagina of de [Advanced Analytics-portal](https://docs.loganalytics.io/docs/Learn) doorzoeken en bekijk de logboeken. Zie voor meer informatie over deze twee opties [zoekpagina logboek of de portal Advanced Analytics](../log-analytics/log-analytics-log-search-portals.md).

Hier volgt een query waarmee nieuwe roltoewijzingen onderverdeeld op basis van de doel-resourceprovider geretourneerd:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments/write" and ActivityStatus == "Succeeded"
| parse ResourceId with * "/providers/" TargetResourceAuthProvider "/" *
| summarize count(), makeset(Caller) by TargetResourceAuthProvider
```

Hier volgt een query die als resultaat wijzigingen aan toewijzingen van rollen in een grafiek weergegeven geeft:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments"
| summarize count() by bin(TimeGenerated, 1d), OperationName
| render timechart
```

![Activiteitenlogboeken via de portal Advanced Analytics - schermafbeelding](./media/change-history-report/azure-log-analytics.png)

## <a name="next-steps"></a>Volgende stappen
* [Gebeurtenissen in het activiteitenlogboek bekijken](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Monitor abonnement activiteit met de Azure Activity Log](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
