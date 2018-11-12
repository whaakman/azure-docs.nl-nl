---
title: Stream het Azure-activiteitenlogboek naar Eventhubs
description: Meer informatie over het streamen van de Azure-activiteitenlogboek naar Event Hubs.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: johnkem
ms.component: activitylog
ms.openlocfilehash: 41965c4a498cfa4c1f0dfefaf7615e43083eb94c
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51011830"
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Stream het Azure-activiteitenlogboek naar Eventhubs
U kunt streamen de [Azure Activity Log](monitoring-overview-activity-logs.md) in bijna realtime voor elke toepassing door een van beide:

* Met behulp van de ingebouwde **exporteren** optie in de portal
* Inschakelen van de Azure Service Bus-regel-ID in een logboekprofiel via de Azure PowerShell-cmdlets of Azure CLI

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>U kunt doen met het activiteitenlogboek en Event Hubs
Hier vindt u de streaming-mogelijkheden kunt gebruiken voor het activiteitenlogboek op twee manieren:

* **Stream naar de logboeken en telemetrie systemen van derden**: gedurende een periode, Azure Event Hubs streamen wordt het mechanisme voor het doorgeven van het activiteitenlogboek is opgenomen in de siem's van derden en meld u analytics-oplossingen.
* **Een aangepaste Telemetrie-en logboekregistratie platform**: als u al beschikken over een platform op maat gemaakte telemetrie of na te denken over het bouwen van een zijn, het zeer schaalbare publish-subscribe aard van Event Hubs kunt u het activiteitenlogboek flexibel opnemen. Zie voor meer informatie, [Dan Rosanova van video over het gebruik van Event Hubs in een wereldwijde schaal telemetrie-platform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-the-activity-log"></a>Streaming van het activiteitenlogboek inschakelen
U kunt het inschakelen van het activiteitenlogboek streamen via programmacode of via de portal. In beide gevallen die u kiest een Event Hubs-naamruimte en een gedeeld toegangsbeleid voor die naamruimte. Een event hub met de naam insights-logs-operationallogs wordt gemaakt in die naamruimte als de eerste nieuwe activiteitenlogboekgebeurtenis plaatsvindt. 

Als u een Event Hubs-naamruimte hebt, moet u eerst een te maken. Als u gestreamd eerder activiteitenlogboek gebeurtenissen naar deze Event Hubs-naamruimte, wordt de event hub die eerder is gemaakt worden hergebruikt. 

Het beleid voor gedeelde toegang definieert de machtigingen die het mechanisme voor streaming is. Vandaag de dag streamen naar Event Hubs vereist **beheren**, **verzenden**, en **luisteren** machtigingen. U kunt maken of wijzigen van beleid voor gedeelde toegang voor de Event Hubs-naamruimte in Azure portal onder de **configureren** tabblad voor uw Event Hubs-naamruimte. 

Voor het bijwerken van het activiteitenlogboek logboekprofiel om op te nemen streaming, moet de gebruiker die een wijziging aan te brengen de machtiging ListKey hebben op deze Event Hubs-autorisatieregel. De Event Hubs-naamruimte is niet nodig om zich in hetzelfde abonnement bevinden als het abonnement dat wordt dat Logboeken verzendt, zolang de gebruiker die de instelling configureert de juiste RBAC heeft toegang tot beide abonnementen en beide abonnementen in de dezelfde AAD-tenant zijn.

### <a name="via-the-azure-portal"></a>Via de Azure-portal
1. Blader naar de **activiteitenlogboek** sectie met behulp van de **alle services** zoeken aan de linkerkant van de portal.
   
   ![Activiteitenlogboek selecteren in de lijst met services in de portal](./media/monitoring-stream-activity-logs-event-hubs/activity-logs-portal-navigate-v2.png)
2. Selecteer de **exporteren naar Event Hub** knop aan de bovenkant van het logboek.
   
   ![De knop exporteren in de portal](./media/monitoring-stream-activity-logs-event-hubs/activity-logs-portal-export-v2.png)

   Houd er rekening mee dat de filterinstellingen die u toegepast tijdens het bekijken van het activiteitenlogboek in de vorige weergave geen invloed op uw instellingen voor exporteren hebben. Dit zijn alleen voor het filteren van wat u ziet tijdens het bladeren door het activiteitenlogboek is opgenomen in de portal.
3. Selecteer in de sectie die wordt weergegeven, **alle regio's**. Selecteer specifieke regio's niet.
   
   ![Sectie exporteren](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)

   > [!WARNING]  
   > Als u iets anders dan selecteert **alle regio's**, u zult missen belangrijke gebeurtenissen die u verwacht te ontvangen. Het activiteitenlogboek is een logboek met algemene (niet-regionaal) zodat de meeste gebeurtenissen niet in een regio die is gekoppeld aan deze hoeft. 
   >

4. Klik op de **Azure Event Hubs** optie en selecteert u een event hubs-naamruimte naar welke logboeken moet worden verzonden, klik vervolgens op **OK**.
5. Selecteer **opslaan** deze instellingen op te slaan. De instellingen worden onmiddellijk toegepast op uw abonnement.
6. Als u meerdere abonnementen hebt, herhaalt u deze actie en alle gegevens verzenden naar de dezelfde event hub.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-cmdlets
Als een logboekprofiel al bestaat, moet u eerst de bestaande logboekprofiel verwijderen en maak vervolgens een nieuwe logboekprofiel.

1. Gebruik `Get-AzureRmLogProfile` om te identificeren als een logboekprofiel bestaat.  Als een logboekprofiel bestaat, gaat u naar de *naam* eigenschap.
2. Gebruik `Remove-AzureRmLogProfile` te verwijderen van het logboekprofiel met de waarde van de *naam* eigenschap.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzureRmLogProfile -Name "default"
    ```
3. Gebruik `Add-AzureRmLogProfile` om een nieuwe logboekprofiel te maken:

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzureRmLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your event hub belongs to>"
   $eventHubNamespace = "<event hub namespace>"

   # Build the service bus rule Id from the settings above
   $serviceBusRuleId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.EventHub/namespaces/$eventHubNamespace/authorizationrules/RootManageSharedAccessKey"

   Add-AzureRmLogProfile -Name $logProfileName -Location $locations -ServiceBusRuleId $serviceBusRuleId
   ```

### <a name="via-azure-cli"></a>Via Azure CLI
Als een logboekprofiel al bestaat, moet u eerst de bestaande logboekprofiel verwijderen en maak vervolgens een nieuwe logboekprofiel.

1. Gebruik `az monitor log-profiles list` om te identificeren als een logboekprofiel bestaat.
2. Gebruik `az monitor log-profiles delete --name "<log profile name>` te verwijderen van het logboekprofiel met de waarde van de *naam* eigenschap.
3. Gebruik `az monitor log-profiles create` om een nieuwe logboekprofiel te maken:

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

## <a name="consume-the-log-data-from-event-hubs"></a>De logboekgegevens van Event Hubs gebruiken
Het schema voor het activiteitenlogboek is beschikbaar in [abonnement activiteiten met de Azure-activiteitenlogboek](monitoring-overview-activity-logs.md). Elke gebeurtenis in een matrix met JSON-blobs met de naam is *records*.

## <a name="next-steps"></a>Volgende stappen
* [Het activiteitenlogboek naar een opslagaccount archiveren](monitoring-archive-activity-log.md)
* [Lees het overzicht van de Azure-activiteitenlogboek](monitoring-overview-activity-logs.md)
* [Instellen van een waarschuwing op basis van een activiteitenlogboek-gebeurtenis](monitor-alerts-unified-log-webhook.md)

