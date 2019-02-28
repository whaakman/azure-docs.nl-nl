---
title: Activiteitenlogboeken bekijken voor RBAC voor Azure-resources | Microsoft Docs
description: Weergave activiteitenlogboeken voor op rollen gebaseerd toegangsbeheer (RBAC) wijzigingen voor Azure-resources voor de afgelopen 90 dagen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5a06b7f66742ad7e97e4ae2ab57aabf15f21faf3
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56957725"
---
# <a name="view-activity-logs-for-rbac-changes-to-azure-resources"></a>Activiteitenlogboeken bekijken voor RBAC-wijzigingen in de Azure-resources

Soms moet u informatie over toegang op rollen gebaseerd beheer (RBAC) wijzigingen in de Azure-resources, zoals voor controle of het oplossen van problemen. Telkens wanneer iemand anders wijzigingen in roltoewijzingen of roldefinities binnen uw abonnementen aanbrengt, de wijzigingen geregistreerd [Azure Activity Log](../azure-monitor/platform/activity-logs-overview.md). U kunt de activiteitenlogboeken als u wilt zien van alle RBAC-wijzigingen voor de afgelopen 90 dagen kunt bekijken.

## <a name="operations-that-are-logged"></a>Bewerkingen die worden geregistreerd

Hier volgen de RBAC-gerelateerde bewerkingen die zijn vastgelegd in het activiteitenlogboek:

- Roltoewijzing maken
- Roltoewijzing verwijderen
- Een aangepaste roldefinitie maken of bijwerken
- De aangepaste roldefinitie verwijderen

## <a name="azure-portal"></a>Azure Portal

De eenvoudigste manier om te beginnen is om de activiteitenlogboeken met de Azure-portal weer te geven. De volgende Schermafbeelding toont een voorbeeld van een activiteitenlogboek waarin een filter is toegepast om roltoewijzing en rol definitie bewerkingen weer te geven. Dit omvat ook een koppeling voor het downloaden van de logboeken als een CSV-bestand.

![Activiteitenlogboeken via de portal - schermafbeelding](./media/change-history-report/activity-log-portal.png)

Het activiteitenlogboek in de portal heeft verschillende filters. Hier volgen de RBAC-gerelateerde filters:

|Filteren  |Value  |
|---------|---------|
|Gebeurteniscategorie     | <ul><li>Administratief</li></ul>         |
|Bewerking     | <ul><li>Roltoewijzing maken</li> <li>Roltoewijzing verwijderen</li> <li>Een aangepaste roldefinitie maken of bijwerken</li> <li>De aangepaste roldefinitie verwijderen</li></ul>      |


Zie voor meer informatie over activiteitenlogboeken [gebeurtenissen weergeven in het activiteitenlogboek](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json).

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Als u wilt weergeven van activiteitenlogboeken met Azure PowerShell, gebruikt u de [Get-AzLog](/powershell/module/Az.Monitor/Get-AzLog) opdracht.

Met deze opdracht worden de wijzigingen aan toewijzingen van alle rollen in een abonnement voor de afgelopen zeven dagen:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

Met deze opdracht worden alle rollen de definitie van wijzigingen in een resourcegroep voor de afgelopen zeven dagen:

```azurepowershell
Get-AzLog -ResourceGroupName pharma-sales-projectforecast -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

Met deze opdracht geeft een lijst van alle roltoewijzing en wijzigingen in het definitie rol in een abonnement voor de afgelopen zeven dagen en de resultaten in een lijst weergegeven:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
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

Als u wilt weergeven van activiteitenlogboeken met de Azure CLI, gebruikt u de [az monitor activiteitenlogboek lijst](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) opdracht.

Met deze opdracht worden de activiteitenlogboeken in een resourcegroep gemaakt sinds de begintijd:

```azurecli
az monitor activity-log list --resource-group pharma-sales-projectforecast --start-time 2018-04-20T00:00:00Z
```

Met deze opdracht worden de activiteitenlogboeken voor de autorisatie-resourceprovider sinds de begintijd:

```azurecli
az monitor activity-log list --resource-provider "Microsoft.Authorization" --start-time 2018-04-20T00:00:00Z
```

## <a name="azure-monitor-logs"></a>Logboeken in Azure Monitor

[Logboeken in Azure Monitor](../log-analytics/log-analytics-overview.md) is een ander hulpprogramma, kunt u het verzamelen en analyseren van RBAC-wijzigingen voor al uw Azure-resources. Logboeken in Azure Monitor biedt de volgende voordelen:

- Schrijven van complexe query's en logica
- Integreren met waarschuwingen, Power BI en andere hulpprogramma 's
- Gegevens worden opgeslagen voor langere bewaarperioden
- Met andere logboeken zoals beveiliging, de virtuele machine en de aangepaste kruisverwijzing

Hier volgen de basisstappen om te beginnen:

1. [Een Log Analytics-werkruimte maken](../azure-monitor/learn/quick-create-workspace.md).

1. [De oplossing Activity Log Analytics configureren](../azure-monitor/platform/collect-activity-logs.md#configuration) voor uw werkruimte.

1. [Bekijk de activiteitenlogboeken](../azure-monitor/platform/collect-activity-logs.md#using-the-solution). Een snelle manier om te navigeren naar de pagina overzicht voor analyse van activiteit is om op de **Log Analytics** optie.

   ![Log Analytics-optie in de portal](./media/change-history-report/azure-log-analytics-option.png)

1. (Optioneel) gebruik van de [zoeken in logboeken](../log-analytics/log-analytics-log-search.md) pagina of het [portal Advanced Analytics](../azure-monitor/log-query/get-started-portal.md) query's uitvoeren en bekijk de logboeken. Zie voor meer informatie over deze twee opties [pagina voor zoeken in Logboeken of de portal Advanced Analytics](../azure-monitor/log-query/portals.md).

Hier volgt een query waarmee nieuwe roltoewijzingen ingedeeld door de doel-resourceprovider geretourneerd:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments/write" and ActivityStatus == "Succeeded"
| parse ResourceId with * "/providers/" TargetResourceAuthProvider "/" *
| summarize count(), makeset(Caller) by TargetResourceAuthProvider
```

Hier volgt een query die wordt geretourneerd van wijzigingen aan toewijzingen van rol in een diagram weergegeven:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments"
| summarize count() by bin(TimeGenerated, 1d), OperationName
| render timechart
```

![Activiteitenlogboeken via de portal Advanced Analytics - schermafbeelding](./media/change-history-report/azure-log-analytics.png)

## <a name="next-steps"></a>Volgende stappen
* [Gebeurtenissen in het activiteitenlogboek bekijken](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Abonnement-activiteit controleren met de Azure-activiteitenlogboek](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
