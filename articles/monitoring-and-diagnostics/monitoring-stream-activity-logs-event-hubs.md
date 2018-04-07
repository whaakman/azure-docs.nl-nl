---
title: De Azure Activity Log naar Event Hubs Stream | Microsoft Docs
description: Informatie over het streamen van de Azure Activity Log naar Event Hubs.
author: johnkemnetz
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2018
ms.author: johnkem
ms.openlocfilehash: 8a599558fc35ca2bf48ce2a5f11ec4978bf10277
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Stream de Azure Activity Log naar Event Hubs
U kunt streamen de [Azure Activity Log](monitoring-overview-activity-logs.md) in bijna realtime voor elke toepassing door een van beide:

* Met de ingebouwde **exporteren** optie in de portal
* Inschakelen van de Azure Service Bus-regel-ID in een logboek profiel via de Azure PowerShell-cmdlets of Azure CLI

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Wat u kunt doen met de activiteitenlogboek en Event Hubs
Hier zijn twee manieren kunt u de streaming mogelijkheid voor het logboek:

* **Stream met de logboekregistratie en telemetrie systemen van derden**: gedurende een periode, streaming van Azure Event Hubs, raken de methode om uw activiteitenlogboek doorsluizen naar siem's van derden en meld u analytics-oplossingen.
* **Maken van een aangepaste Telemetrie en logboekregistratie platform**: als u al hebt een op maat gemaakte telemetrie-platform of nadenken over het bouwen van een zijn, zeer schaalbare voor publiceren / abonneren aard van Event Hubs kunt u het activiteitenlogboek flexibel opnemen. Zie voor meer informatie [Dan Rosanova van video over het gebruik van Event Hubs in een platform wereldwijde schaal telemetrie](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-the-activity-log"></a>Streaming van het activiteitenlogboek activeren
U kunt het inschakelen van het activiteitenlogboek streaming via programmacode of via de portal. In beide gevallen moet u kiest een Event Hubs-naamruimte en een gedeeld toegangsbeleid voor die naamruimte. Een event hub met de naam insights-logboeken-operationallogs wordt gemaakt in die naamruimte als de eerste nieuwe activiteitenlogboek gebeurtenis plaatsvindt. 

Als u een Event Hubs-naamruimte hebt, moet u eerst een maken. Als u eerder activiteitenlogboek van gebeurtenissen naar deze naamruimte Event Hubs gestreamd, wordt opnieuw gebruikt de event hub die eerder is gemaakt. 

Het beleid voor gedeelde toegang definieert de machtigingen van het mechanisme voor streaming. Vandaag de dag streaming naar Event Hubs vereist **beheren**, **verzenden**, en **luisteren** machtigingen. U kunt maken of wijzigen van beleid voor gedeelde toegang voor de naamruimte Event Hubs in de Azure portal onder de **configureren** tabblad voor de naamruimte van uw Event Hubs. 

Voor het bijwerken van het profiel van het logboek activiteitenlogboek om op te nemen streaming moet de gebruiker die de wijziging aan te brengen de machtiging ListKey hebben op deze Event Hubs-autorisatieregel. De naamruimte van Event Hubs heeft geen zich in hetzelfde abonnement als het abonnement dat u zich aanmeldt, wordt tekensetcodering, zolang de gebruiker die de instelling configureert juiste RBAC toegang tot beide abonnementen heeft.

### <a name="via-the-azure-portal"></a>Via de Azure-portal
1. Blader naar de **activiteitenlogboek** sectie met behulp van de **alle services** zoeken aan de linkerkant van de portal.
   
   ![Activiteitenlogboek selecteren in de lijst met services in de portal](./media/monitoring-stream-activity-logs-event-hubs/activity.png)
2. Selecteer de **exporteren** knop aan de bovenkant van het logboek.
   
   ![Knop exporteren in de portal](./media/monitoring-stream-activity-logs-event-hubs/export.png)

   Houd er rekening mee u toegepast waren tijdens het bekijken van het activiteitenlogboek in de vorige weergave instellingen voor het filter hebben geen invloed op uw instellingen exporteren. Dit zijn alleen voor het filteren van wat u tijdens het bladeren door uw activiteitenlogboek in de portal ziet.
3. Selecteer in de sectie **alle regio's**. Selecteer specifieke regio's niet.
   
   ![Sectie exporteren](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)

   > [!WARNING]  
   > Als u iets anders dan selecteert **alle regio's**, mist u belangrijke gebeurtenissen die u verwacht te ontvangen. Het activiteitenlogboek is een logboek met algemene (niet-regionale), de meeste gebeurtenissen hoeft dus niet een regio die is gekoppeld. 
   >

4. Selecteer **opslaan** deze instellingen op te slaan. De instellingen zijn direct toegepast op uw abonnement.
5. Als u meerdere abonnementen hebt, herhaalt u deze actie en alle gegevens verzenden naar de dezelfde event hub.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-cmdlets
Als een profiel voor een logboek al bestaat, moet u eerst het bestaande profiel voor het logboek verwijderen en vervolgens een nieuw logboekbestand-profiel maken.

1. Gebruik `Get-AzureRmLogProfile` om te bepalen of er een logboek-profiel bestaat.  Als u een profiel voor een logboek bestaat, zoekt u naar de *naam* eigenschap.
2. Gebruik `Remove-AzureRmLogProfile` verwijderen van het logboek-profiel met behulp van de waarde van de *naam* eigenschap.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzureRmLogProfile -Name "default"
    ```
3. Gebruik `Add-AzureRmLogProfile` voor het maken van een nieuw logboekbestand-profiel:

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzureRmLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your event hub belongs to>"
   $eventHubNamespace = "<event hub namespace>"

   # Build the service bus rule Id from the settings above
   $serviceBusRuleId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.EventHub/namespaces/$eventHubNamespaceName/authorizationrules/RootManageSharedAccessKey"

   Add-AzureRmLogProfile -Name $logProfileName -Location $locations -ServiceBusRuleId $serviceBusRuleId
   ```

### <a name="via-azure-cli"></a>Via Azure CLI
Als een profiel voor een logboek al bestaat, moet u eerst het bestaande profiel voor het logboek verwijderen en vervolgens een nieuw logboekbestand-profiel maken.

1. Gebruik `az monitor log-profiles list` om te bepalen of er een logboek-profiel bestaat.
2. Gebruik `az monitor log-profiles delete --name "<log profile name>` verwijderen van het logboek-profiel met behulp van de waarde van de *naam* eigenschap.
3. Gebruik `az monitor log-profiles create` voor het maken van een nieuw logboekbestand-profiel:

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

## <a name="consume-the-log-data-from-event-hubs"></a>De logboekgegevens van Event Hubs gebruiken
Het schema voor het logboek is beschikbaar in [bewaken abonnement activiteit met de Azure Activity Log](monitoring-overview-activity-logs.md). Elke gebeurtenis wordt in een matrix met JSON-blobs aangeroepen *records*.

## <a name="next-steps"></a>Volgende stappen
* [Het activiteitenlogboek naar een opslagaccount archiveren](monitoring-archive-activity-log.md)
* [Lees het overzicht van de Azure Activity Log](monitoring-overview-activity-logs.md)
* [Een waarschuwing op basis van een gebeurtenis activiteitenlogboek instellen](insights-auditlog-to-webhook-email.md)

