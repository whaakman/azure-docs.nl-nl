---
title: Azure-activiteitenlogboeken voor het bewaken van resources weergeven | Microsoft Docs
description: Gebruik de activiteitenlogboeken streamen naar acties van de gebruiker controleren en fouten. Toont de Azure Portal PowerShell, Azure CLI en REST.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: 09f7fba2b8ae3b3ccc8710ffe9302d02d311c74c
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51514329"
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a>Activiteitenlogboeken bekijken om te controleren van acties op resources

Met activiteitenlogboeken kunt u het volgende bepalen:

* welke bewerkingen zijn uitgevoerd voor de resources in uw abonnement
* wie de bewerking heeft gestart (Hoewel bewerkingen, gestart door een back-endservice een gebruiker als de aanroeper niet retourneren)
* Wanneer de bewerking is
* De status van de bewerking
* De bewerking van de waarden van andere eigenschappen die u kunnen helpen bij onderzoek

Het activiteitenlogboek staan alle schrijfbewerkingen (PUT, POST, DELETE) uitgevoerd op uw resources. Deze omvatten geen leesbewerkingen (GET). Zie voor een lijst van acties van de resource, [Resourceprovider van Azure Resource Manager-bewerkingen](../role-based-access-control/resource-provider-operations.md). U kunt de auditlogboeken te vinden van een fout opgetreden bij het oplossen van of om te controleren hoe een gebruiker in uw organisatie een resource heeft gewijzigd.

Activiteitenlogboeken worden gedurende 90 dagen bewaard. U kunt een query uitvoeren voor een willekeurig bereik van datums, zolang de begindatum niet verder dan 90 dagen in het verleden is.

U kunt gegevens ophalen uit de activiteitenlogboeken via de portal, PowerShell, Azure CLI, Insights REST-API of [Insights .NET-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="portal"></a>Portal

1. Als u wilt weergeven in de activiteitenlogboeken via de portal, selecteert u **Monitor**.

    ![Selecteer de activiteitenlogboeken](./media/resource-group-audit/select-monitor.png)

   Of u kunt automatisch filteren van het activiteitenlogboek voor een bepaalde resource of resourcegroep selecteren **activiteitenlogboek**. U ziet dat het activiteitenlogboek automatisch wordt gefilterd op de geselecteerde resource.

    ![filteren op bron](./media/resource-group-audit/filtered-by-resource.png)
2. In de **activiteitenlogboek**, ziet u een overzicht van recente bewerkingen.

    ![acties weergeven](./media/resource-group-audit/audit-summary.png)
3. Als u wilt beperken het aantal bewerkingen weergegeven, selecteert u verschillende voorwaarden. Bijvoorbeeld, de volgende afbeelding toont de **Timespan** en **gebeurtenis gestart door** velden gewijzigd om de acties die door een bepaalde gebruiker of toepassing voor de afgelopen maand weer te geven. Selecteer **toepassen** om de resultaten van uw query weer te geven.

    ![filter instellen](./media/resource-group-audit/set-filter.png)

4. Als u de query later opnieuw uitvoeren wilt, selecteert u **opslaan** en geef een naam op voor de query.

    ![Query opslaan](./media/resource-group-audit/save-query.png)
5. Als u wilt snel een query uitvoert, kunt u een van de ingebouwde query's, zoals mislukte implementaties selecteren.

    ![query selecteren](./media/resource-group-audit/select-quick-query.png)

   De geselecteerde query stelt automatisch de vereiste filterwaarden.

    ![fouten bij de implementatie van de weergave](./media/resource-group-audit/view-failed-deployment.png)

6. Selecteer een van de bewerkingen voor een overzicht van de gebeurtenis.

    ![weergave-bewerking](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell

1. Om op te halen logboekvermeldingen, voer de **Get-AzureRmLog** opdracht. U opgeven aanvullende parameters voor het filteren van de lijst met vermeldingen. Als u een begin- en -tijd niet opgeeft, worden vermeldingen voor het afgelopen uur geretourneerd. Bijvoorbeeld, om op te halen van de bewerkingen voor een resourcegroep in het afgelopen uur worden uitgevoerd:

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```

    Het volgende voorbeeld ziet het gebruik van het activiteitenlogboek voor onderzoek-bewerkingen die tijdens een opgegeven periode. De begin- en einddatums zijn opgegeven in een datumnotatie.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Of u datumfuncties kunt gebruiken om op te geven van het datumbereik, zoals de afgelopen 14 dagen.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. Afhankelijk van de begintijd die u opgeeft, kunnen de vorige opdrachten retourneren een lange lijst bewerkingen voor de resourcegroep. U kunt de resultaten voor wat u zoekt door zoekcriteria filteren. Als u onderzoeken hoe een web-app is gestopt wilt, kunt u bijvoorbeeld de volgende opdracht uitvoeren:

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    In dit voorbeeld laat zien dat een actie bij stoppen is uitgevoerd door someone@contoso.com.

  ```powershell
  Authorization     :
  Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Action    : Microsoft.Web/sites/stop/action
  Role      : Subscription Admin
  Condition :
  Caller            : someone@contoso.com
  CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
  EventSource       : Administrative
  EventTimestamp    : 8/28/2015 4:08:18 PM
  OperationName     : Microsoft.Web/sites/stop/action
  ResourceGroupName : ExampleGroup
  ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Status            : Succeeded
  SubscriptionId    : xxxxx
  SubStatus         : OK
  ```

3. U kunt de acties die door een bepaalde gebruiker, zelfs voor een resourcegroep die niet meer bestaat opzoeken.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. U kunt filteren op mislukte bewerkingen.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. U kunt zich richten op één fout door te kijken naar het statusbericht voor die vermelding.

  ```azurepowershell-interactive
  ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
  ```

    Dat geeft als resultaat:

        code           message
        ----           -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP.

## <a name="azure-cli"></a>Azure-CLI

Om op te halen logboekvermeldingen, voer de [az monitor activiteitenlogboek lijst](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) opdracht.

  ```azurecli
  az monitor activity-log list --resource-group <group name>
  ```

## <a name="rest-api"></a>REST-API

De REST-bewerkingen voor het werken met het activiteitenlogboek maken deel uit van de [Insights REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx). Als u wilt ophalen van gebeurtenissen in het activiteitenlogboek, Zie [lijst van de gebeurtenissen in een abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Volgende stappen

* Activiteitenlogboeken van Azure kunnen worden gebruikt met Power BI om meer inzichten over de acties in uw abonnement te verkrijgen. Zie [weergeven en analyseren van Azure-activiteitenlogboeken in Power BI en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Zie voor meer informatie over het instellen van beveiligingsbeleid [Azure Role-based Access Control](../role-based-access-control/role-assignments-portal.md).
* Zie voor meer informatie over de opdrachten voor het weergeven van implementatiebewerkingen, [implementatiebewerkingen bekijken](resource-manager-deployment-operations.md).
* Als u wilt weten hoe om te voorkomen dat verwijderingen op een resource voor alle gebruikers, Zie [resources met Azure Resource Manager vergrendelen](resource-group-lock-resources.md).
* Zie voor de lijst met bewerkingen weergegeven die beschikbaar zijn voor elke Microsoft Azure Resource Manager-provider [Resourceprovider van Azure Resource Manager-bewerkingen](../role-based-access-control/resource-provider-operations.md)
