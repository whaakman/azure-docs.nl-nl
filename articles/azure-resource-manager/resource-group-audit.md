---
title: Azure activiteitenlogboeken om te controleren bronnen weergeven | Microsoft Docs
description: Gebruik de activiteitenlogboeken gebruikersacties controleren en fouten. Toont PowerShell voor Azure Portal, Azure CLI en REST.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: tomfitz
ms.openlocfilehash: 9f90bc80c146c6c2da04aacbc110f7d389c0baa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a>Activiteitenlogboeken bekijken om te controleren van de acties op resources
Via activiteitenlogboeken, kunt u bepalen:

* welke bewerkingen zijn uitgevoerd op de resources in uw abonnement
* wie heeft de bewerking gestart (Hoewel de bewerkingen die zijn gestart door een back-endservice niet retourneren als de aanroeper van een gebruiker)
* Wanneer de bewerking is opgetreden
* De status van de bewerking
* De bewerking van de waarden van andere eigenschappen die u kunnen helpen onderzoek

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

U kunt informatie ophalen uit de activiteitenlogboeken van de via de portal, PowerShell, Azure CLI, inzicht REST API of [Insights .NET-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="portal"></a>Portal
1. De om activiteitenlogboeken te raadplegen via de portal, selecteer **Monitor**.
   
    ![Selecteer activiteitenlogboeken](./media/resource-group-audit/select-monitor.png)

   Of selecteer automatisch filter het logboek voor een bepaalde bron of de resourcegroep: **activiteitenlogboek** van die resourceblade. U ziet dat het activiteitenlogboek automatisch wordt gefilterd op de geselecteerde bron.
   
    ![filteren op resource](./media/resource-group-audit/filtered-by-resource.png)
2. In de **activiteitenlogboek** blade ziet u een overzicht van recente bewerkingen.
   
    ![acties weergeven](./media/resource-group-audit/audit-summary.png)
3. Als u wilt beperken het aantal bewerkingen die worden weergegeven, selecteer andere voorwaarden. Bijvoorbeeld de volgende afbeelding toont de **Timespan** en **gebeurtenis wordt gestart door** velden gewijzigd om de acties die door een bepaalde gebruiker of toepassing voor de afgelopen maand weer te geven. Selecteer **toepassen** de resultaten van uw query wilt weergeven.
   
    ![Stel filteropties](./media/resource-group-audit/set-filter.png)

4. Als u de query opnieuw uitvoert wilt, selecteert u **opslaan** en geef een naam op voor de query.
   
    ![query opslaan](./media/resource-group-audit/save-query.png)
5. Om snel een query uitvoert, kunt u een van de ingebouwde query's, zoals mislukte implementatie.

    ![query selecteren](./media/resource-group-audit/select-quick-query.png)

   De geselecteerde query stelt automatisch de vereiste filterwaarden.

    ![implementatie-fouten weergeven](./media/resource-group-audit/view-failed-deployment.png)   

6. Selecteer een van de bewerkingen voor een overzicht van de gebeurtenis.

    ![de bewerking weergeven](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell
1. Uitvoeren voor het logboekvermeldingen ophalen, de **Get-AzureRmLog** opdracht. U opgeven extra parameters om te filteren op de lijst met items. Als u een begin- en -tijd niet opgeeft, worden vermeldingen voor het afgelopen uur geretourneerd. Als u bijvoorbeeld voor het ophalen van de bewerkingen voor een resourcegroep in het afgelopen uur uitgevoerd:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    Het volgende voorbeeld ziet hoe u het activiteitenlogboek onderzoek bewerkingen die zijn uitgevoerd tijdens een opgegeven periode. De begin- en einddatums zijn opgegeven in een datumnotatie.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Of u kunt de datumfuncties gebruiken om op te geven het datumbereik, zoals de afgelopen 14 dagen.
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. Afhankelijk van de begintijd die u opgeeft, kunnen de eerdere opdrachten een lange lijst met bewerkingen voor de resourcegroep geretourneerd. U kunt de resultaten voor wat u zoekt dankzij de zoekcriteria filteren. Als u probeert te onderzoeken hoe een web-app is gestopt, kunt u bijvoorbeeld de volgende opdracht uitvoeren:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    Die in dit voorbeeld ziet u dat een stopactie is uitgevoerd door someone@contoso.com. 

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

3. U kunt de acties die door een bepaalde gebruiker, zelfs voor een resourcegroep die niet langer bestaat opzoeken.

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. U kunt filteren op mislukte bewerkingen.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. U kunt zich richten op één fout door te kijken het statusbericht voor dat item.
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    Die wordt geretourneerd:
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a>Azure CLI
* Voor het ophalen van logboekvermeldingen die u uitvoert het **azure-groep logboek weergeven** opdracht.

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a>REST API
De REST-bewerkingen voor het werken met het activiteitenlogboek deel uitmaken van de [Insights REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx). Zie voor het ophalen van de activiteit logboekgebeurtenissen [lijst van de management-gebeurtenissen in een abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Volgende stappen
* Azure activiteitenlogboeken kunnen worden gebruikt met Power BI om meer inzicht over de acties in uw abonnement te krijgen. Zie [weergeven en analyseren van Azure activiteitenlogboeken in Power BI en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Zie voor meer informatie over het instellen van beveiligingsbeleid [toegangsbeheer op basis van rollen in Azure](../active-directory/role-based-access-control-configure.md).
* Zie voor meer informatie over de opdrachten voor het weergeven van implementatiebewerkingen, [implementatiebewerkingen weergeven](resource-manager-deployment-operations.md).
* Zie voor informatie over het voorkomen van een resource voor alle gebruikers zijn verwijderd, [resources met Azure Resource Manager vergrendelen](resource-group-lock-resources.md).

