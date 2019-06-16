---
title: Azure-activiteitenlogboeken voor het bewaken van resources weergeven | Microsoft Docs
description: Gebruik de activiteitenlogboeken streamen naar acties van de gebruiker controleren en fouten. Toont de Azure portal PowerShell of Azure CLI en REST.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: tomfitz
ms.openlocfilehash: 7ff45be4eea5c6e8ab83093847164ede0e94579a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65606663"
---
# <a name="view-activity-logs-to-monitor-actions-on-resources"></a>Activiteitenlogboeken bekijken voor het bewaken van acties op resources

Met activiteitenlogboeken kunt u het volgende bepalen:

* welke bewerkingen zijn uitgevoerd voor de resources in uw abonnement
* wie de bewerking is gestart
* Wanneer de bewerking is
* De status van de bewerking
* De bewerking van de waarden van andere eigenschappen die u kunnen helpen bij onderzoek

Het activiteitenlogboek staan alle schrijfbewerkingen (PUT, POST, DELETE) voor uw resources. Deze bevat geen leesbewerkingen (GET). Zie voor een lijst van acties van de resource, [Resourceprovider van Azure Resource Manager-bewerkingen](../role-based-access-control/resource-provider-operations.md). U kunt de activiteitenlogboeken gebruiken om fouten te vinden bij foutoplossing of om te controleren hoe een gebruiker in uw organisatie een resource heeft gewijzigd.

Activiteitenlogboeken worden gedurende 90 dagen bewaard. U kunt een query voor een bereik van datums, zolang de begindatum is niet meer dan 90 dagen in het verleden.

U kunt gegevens ophalen uit de activiteitenlogboeken via de portal, PowerShell, Azure CLI, Insights REST-API of [Insights .NET-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="azure-portal"></a>Azure Portal

1. Als u wilt weergeven in de activiteitenlogboeken via de portal, selecteert u **Monitor**.

    ![Selecteer monitor](./media/resource-group-audit/select-monitor.png)

1. Selecteer **activiteitenlogboek**.

    ![Activiteitenlogboek selecteren](./media/resource-group-audit/select-activity-log.png)

1. U ziet een overzicht van recente bewerkingen. Een set filters wordt toegepast op de bewerkingen. U ziet dat de informatie over het overzicht bevat die de actie gestart en wanneer dat is gebeurd.

    ![Overzicht van recente bewerkingen weergeven](./media/resource-group-audit/audit-summary.png)

1. Als u wilt snel uitvoeren van een vooraf gedefinieerde set filters, **snelle inzichten**.

    ![Selecteer snelle inzichten](./media/resource-group-audit/select-quick-insights.png)

1. Selecteer een van de opties. Selecteer bijvoorbeeld **mislukte implementaties** om te zien van fouten van implementaties.

    ![Selecteer mislukte implementaties](./media/resource-group-audit/select-failed-deployments.png)

1. U ziet dat de filters is gewijzigd in focus op fouten bij de implementatie in de afgelopen 24 uur. Alleen de bewerkingen die overeenkomen met de filters worden weergegeven.

    ![Weergavefilters](./media/resource-group-audit/view-filters.png)

1. Als u wilt zich richten op specifieke bewerkingen, de filters wijzigen of nieuwe labels toe te passen. De volgende afbeelding ziet u bijvoorbeeld een nieuwe waarde voor de **Timespan** en **resourcetype** is ingesteld op storage-accounts. 

    ![filter instellen](./media/resource-group-audit/set-filter.png)

1. Als u de query later opnieuw uitvoeren wilt, selecteert u **pincode huidige filters**.

    ![Pincode-filters](./media/resource-group-audit/pin-filters.png)

1. Geef een naam op het filter.

    ![De naam van filters](./media/resource-group-audit/name-filters.png)

1. Het filter is beschikbaar in het dashboard.

    ![Filter weergeven in dashboard](./media/resource-group-audit/show-dashboard.png)

1. U kunt wijzigingen aan een resource weergeven vanuit de portal. Ga terug naar de standaardwaarde in Monitor weergeven, en selecteer een bewerking die betrokken zijn een resource te wijzigen.

    ![Bewerking selecteren](./media/resource-group-audit/select-operation.png)

1. Selecteer **wijzigingsoverzicht (Preview)** en kies een van de beschikbare bewerkingen.

    ![Selecteer wijzigingsoverzicht](./media/resource-group-audit/select-change-history.png)

1. De wijzigingen in de bron worden weergegeven.

    ![Wijzigingen weergeven](./media/resource-group-audit/show-changes.png)

Zie voor meer informatie over de wijzigingsgeschiedenis, [wijzigingen van resources ophalen](../governance/resource-graph/how-to/get-resource-changes.md).

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Om op te halen logboekvermeldingen, voer de **Get-AzLog** opdracht. U opgeven aanvullende parameters voor het filteren van de lijst met vermeldingen. Als u een begin- en -tijd niet opgeeft, worden vermeldingen voor de afgelopen zeven dagen worden geretourneerd.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup
```

Het volgende voorbeeld ziet het gebruik van het activiteitenlogboek voor onderzoek-bewerkingen die tijdens een opgegeven periode. De begin- en einddatums zijn opgegeven in een datumnotatie.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime 2019-05-05T06:00 -EndTime 2019-05-09T06:00
```

Of u datumfuncties kunt gebruiken om op te geven van het datumbereik, zoals de afgelopen 14 dagen.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
```

U kunt de acties die door een bepaalde gebruiker opzoeken.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
```

U kunt filteren op mislukte bewerkingen.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -Status Failed
```

U kunt zich richten op één fout door te kijken naar het statusbericht voor die vermelding.

```azurepowershell-interactive
(Get-AzLog -ResourceGroup ExampleGroup -Status Failed).Properties.Content.statusMessage | ConvertFrom-Json
```

U kunt specifieke waarden om te beperken van de gegevens die wordt geretourneerd.

```azurepowershell-interactive
Get-AzLog -ResourceGroupName ExampleGroup | Format-table EventTimeStamp, Caller, @{n='Operation'; e={$_.OperationName.value}}, @{n='Status'; e={$_.Status.value}}, @{n='SubStatus'; e={$_.SubStatus.LocalizedValue}}
```

Afhankelijk van de begintijd die u opgeeft, kunnen de vorige opdrachten retourneren een lange lijst bewerkingen voor de resourcegroep. U kunt de resultaten voor wat u zoekt door zoekcriteria filteren. U kunt bijvoorbeeld filteren op het type bewerking.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup | Where-Object {$_.OperationName.value -eq "Microsoft.Resources/deployments/write"}
```

U kunt Resource Graph gebruiken om te zien van de wijzigingsgeschiedenis voor een resource. Zie voor meer informatie, [wijzigingen van resources ophalen](../governance/resource-graph/how-to/get-resource-changes.md).

## <a name="azure-cli"></a>Azure-CLI

Om op te halen logboekvermeldingen, voer de [az monitor activiteitenlogboek lijst](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) opdracht met een offset om aan te geven van de tijdsduur.

```azurecli-interactive
az monitor activity-log list --resource-group ExampleGroup --offset 7d
```

Het volgende voorbeeld ziet het gebruik van het activiteitenlogboek voor onderzoek-bewerkingen die tijdens een opgegeven periode. De begin- en einddatums zijn opgegeven in een datumnotatie.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --start-time 2019-05-01 --end-time 2019-05-15
```

U kunt de acties die door een bepaalde gebruiker, zelfs voor een resourcegroep die niet meer bestaat opzoeken.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --caller someone@contoso.com --offset 5d
```

U kunt filteren op mislukte bewerkingen.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --status Failed --offset 1d
```

U kunt zich richten op één fout door te kijken naar het statusbericht voor die vermelding.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --status Failed --offset 1d --query [].properties.statusMessage
```

U kunt specifieke waarden om te beperken van de gegevens die wordt geretourneerd.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --offset 1d --query '[].{Operation: operationName.value, Status: status.value, SubStatus: subStatus.localizedValue}'
```

Afhankelijk van de begintijd die u opgeeft, kunnen de vorige opdrachten retourneren een lange lijst bewerkingen voor de resourcegroep. U kunt de resultaten voor wat u zoekt door zoekcriteria filteren. U kunt bijvoorbeeld filteren op het type bewerking.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --offset 1d --query "[?operationName.value=='Microsoft.Storage/storageAccounts/write']"
```

U kunt Resource Graph gebruiken om te zien van de wijzigingsgeschiedenis voor een resource. Zie voor meer informatie, [wijzigingen van resources ophalen](../governance/resource-graph/how-to/get-resource-changes.md).

## <a name="rest-api"></a>REST-API

De REST-bewerkingen voor het werken met het activiteitenlogboek maken deel uit van de [Insights REST-API](/rest/api/monitor/). Als u wilt ophalen van gebeurtenissen in het activiteitenlogboek, Zie [lijst van de gebeurtenissen in een abonnement](/rest/api/monitor/activitylogs).

## <a name="next-steps"></a>Volgende stappen

* Activiteitenlogboeken van Azure kunnen worden gebruikt met Power BI om meer inzichten over de acties in uw abonnement te verkrijgen. Zie [weergeven en analyseren van Azure-activiteitenlogboeken in Power BI en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Zie voor meer informatie over het instellen van beveiligingsbeleid [Azure Role-based Access Control](../role-based-access-control/role-assignments-portal.md).
* Zie voor meer informatie over de opdrachten voor het weergeven van implementatiebewerkingen, [implementatiebewerkingen bekijken](resource-manager-deployment-operations.md).
* Als u wilt weten hoe om te voorkomen dat verwijderingen op een resource voor alle gebruikers, Zie [resources met Azure Resource Manager vergrendelen](resource-group-lock-resources.md).
* Zie voor de lijst met bewerkingen weergegeven die beschikbaar zijn voor elke Microsoft Azure Resource Manager-provider [Resourceprovider van Azure Resource Manager-bewerkingen](../role-based-access-control/resource-provider-operations.md)
