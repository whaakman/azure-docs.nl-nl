---
title: Begrijpen hoe metriek werk waarschuwingen in Azure Monitor.
description: Bekijk een overzicht van wat u kunt doen met metrische waarschuwingen en hoe deze werken in Azure Monitor.
author: snehithm
ms.author: snmuvva
ms.date: 9/18/2018
ms.topic: conceptual
ms.service: azure-monitor
ms.subservice: alerts
ms.openlocfilehash: 40cf37c08705384fb664402c3d40efa229cbbcdf
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54422793"
---
# <a name="understand-how-metric-alerts-work-in-azure-monitor"></a>Begrijpen hoe metriek werk waarschuwingen in Azure Monitor

Metrische waarschuwingen in Azure Monitor hierop worden gebaseerd multi-dimensionale metrische gegevens. Deze metrische gegevens mogelijk platform metrische gegevens, [aangepaste metrische gegevens](../../azure-monitor/platform/metrics-custom-overview.md), [populaire logboeken van Log Analytics wordt geconverteerd naar metrische gegevens](../../azure-monitor/platform/alerts-metric-logs.md), standaard metrische gegevens van Application Insights. Metrische waarschuwingen evalueren met regelmatige intervallen om te controleren of voorwaarden op een of meer metriek tijdreeksen ' True ' zijn en een melding wanneer de evaluaties wordt voldaan. Metrische waarschuwingen zijn stateful, dat wil zeggen, ze alleen verzenden van meldingen wanneer de status verandert.

## <a name="how-do-metric-alerts-work"></a>De werking van metrische waarschuwingen

U kunt een waarschuwingsregel voor metrische gegevens definiëren door een doelbron om te worden bewaakt, metrische naam en de voorwaarde (een operator en een drempelwaarde) en een actiegroep wordt geactiveerd wanneer de waarschuwingsregel wordt geactiveerd op te geven.
Stel dat u hebt een eenvoudige waarschuwingsregel voor metrische gegevens als volgt gemaakt:

- Doelresource (de Azure resource die u wilt bewaken): myVM
- Meetwaarde: Percentage CPU
- Tijdverzameling (statistiek die wordt uitgevoerd via onbewerkte metrische waarden. Ondersteunde tijd aggregaties zijn Min, Max, Gem, totaal): Gemiddeld
- Periode (het uiterlijk back venster via welk metrische gegeven waarden worden gecontroleerd):      Gedurende de laatste 5 minuten
- Frequentie (de frequentie waarmee de waarschuwing voor metrische gegevens controleert als de voorwaarden wordt voldaan): 1 min
- Operator:     Greater Than
- Drempelwaarde:      70

Vanaf het moment dat de waarschuwingsregel wordt gemaakt, de monitor wordt uitgevoerd elke 1 minuut en gecontroleerd metrische waarden voor de laatste 5 minuten en als het gemiddelde van deze waarden is groter dan 70 wordt gecontroleerd. Als aan de voorwaarde is voldaan dat wil zeggen, de gemiddelde CPU-Percentage voor de laatste vijf minuten hoger is dan 70, een melding geactiveerd door de waarschuwingsregel wordt geactiveerd. Als u een e-mailbericht of een web-hook-actie in de actiegroep die zijn gekoppeld aan de waarschuwingsregel hebt geconfigureerd, ontvangt u een melding geactiveerd op beide.

Dit specifieke exemplaar van de waarschuwingsregel starten kan ook worden weergegeven in de Azure portal in de blade alle waarschuwingen.

Bijvoorbeeld, het gebruik van 'myVM' worden boven de drempelwaarde blijft in de volgende controles, de waarschuwingsregel worden niet opnieuw gestart totdat de voorwaarde is opgelost.

Nadat u later, als het gebruik van "myVM" weer omlaag op normaal duikt, dat wil zeggen, gaat u dan de opgegeven drempelwaarde. De waarschuwingsregel bewaakt de voorwaarde voor twee keer, voor het verzenden van een melding opgelost. De waarschuwingsregel verzendt een bericht opgelost/gedeactiveerd als voorwaarde voor de waarschuwing is niet voldaan voor drie opeenvolgende punten om ruis in het geval van voorwaarden op en neer te verminderen.

Als de opgelost melding wordt verzonden via e-mail of webhooks, de status van de waarschuwing instantie (Monitorstatus genoemd) in Azure portal is ook ingesteld op opgelost.

## <a name="monitoring-at-scale-using-metric-alerts-in-azure-monitor"></a>Bewaking op schaal met behulp van metrische waarschuwingen in Azure Monitor

### <a name="using-dimensions"></a>Met behulp van dimensies

Metrische waarschuwingen in Azure Monitor bieden ook ondersteuning voor het bewaken van meerdere combinaties van dimensiewaarden met één regel. We gaan begrijpen waarom u combinaties van meerdere dimensies met behulp van een voorbeeld kunt gebruiken.

Stel dat u een App Service-plan hebt voor uw website. U wilt bewaken van CPU-gebruik op meerdere exemplaren uitvoeren van uw website/app. U kunt dit doen met behulp van een waarschuwingsregel voor metrische gegevens als volgt

- Doelresource: myAppServicePlan
- Meetwaarde: Percentage CPU
- Dimensies
  - Exemplaar InstanceName1, InstanceName2 =
- Tijdverzameling: Gemiddeld
- Periode: Gedurende de laatste 5 minuten
- Frequentie van: 1 min
- Operator: GreaterThan
- Drempelwaarde: 70

Net als voordat deze regel controleert als de gemiddelde CPU-gebruik gedurende de laatste vijf minuten hoger is dan 70%. U kunt echter met dezelfde regel twee exemplaren met uw website controleren. Elk exemplaar wordt afzonderlijk ophalen bewaakt en ontvangt u meldingen afzonderlijk.

Stel, u hebt een web-app die zeer grote vraag te zien en moet u meer instanties toevoegen. De bovenstaande regel controleert nog steeds slechts twee exemplaren. U kunt echter een regel als volgt maken.

- Doelresource: myAppServicePlan
- Meetwaarde: Percentage CPU
- Dimensies
  - Instantie = *
- Tijdverzameling: Gemiddeld
- Periode: Gedurende de laatste 5 minuten
- Frequentie van: 1 min
- Operator: GreaterThan
- Drempelwaarde: 70

Deze regel worden alle waarden voor het exemplaar van Internet Explorer automatisch gecontroleerd u kunt uw exemplaren bewaken deze direct hoeft te wijzigen van de waarschuwingsregel voor metrische gegevens opnieuw.

### <a name="monitoring-multiple-resources-using-metric-alerts"></a>Bewaking van meerdere resources met behulp van metrische waarschuwingen

Als u uit de vorige sectie hebt gezien, is het mogelijk om een enkele waarschuwingsregel voor metrische gegevens die elke dimensiecombinatie van afzonderlijke (zoals controleert) een metrische tijdreeks). Eerder waren u echter nog steeds beperkt tot één resource tegelijk uitvoeren. Azure Monitor biedt ook ondersteuning voor het bewaken van meerdere resources met een waarschuwingsregel voor metrische gegevens. Deze functie is momenteel preview en alleen ondersteund op virtuele machines. Een één waarschuwing voor metrische gegevens kan ook resources in een Azure-regio te bewaken.

U kunt het bereik van de bewaking met een één waarschuwing voor metrische gegevens op drie manieren opgeven:

- Als een lijst met virtuele machines in een Azure-regio binnen een abonnement
- alle virtuele machines (in een Azure-regio) in een of meer resourcegroepen in een abonnement
- alle virtuele machines (in een Azure-regio) in één abonnement

Het maken van regels voor metrische waarschuwingen die meerdere resources bewaken wordt momenteel niet ondersteund via Azure portal. U kunt deze regels via maken [Azure Resource Manager-sjablonen](../../azure-monitor/platform/alerts-metric-create-templates.md#resource-manager-template-for-metric-alert-that-monitors-multiple-resources). U ontvangt afzonderlijke meldingen voor elke virtuele machine. 

## <a name="typical-latency"></a>Normale latentie

Voor metrische waarschuwingen, doorgaans u gewaarschuwd in minder dan vijf minuten als u de waarschuwingsregel frequentie moet 1 minuut. In geval van zware belasting voor melding systemen ziet u mogelijk een langere latentie.

## <a name="supported-resource-types-for-metric-alerts"></a>Ondersteunde resourcetypen voor de metrische waarschuwingen

U vindt de volledige lijst met ondersteunde resourcetypen in deze [artikel](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported).

Als u bent al gebruikmaken van de klassieke metrische waarschuwingen en informatie als metrische waarschuwingen ondersteuning bieden voor de alle de resourcetypen die willen u, de volgende tabel bevat de resource gegevenstypen ondersteund door klassieke metrische waarschuwingen en als ze worden ondersteund door metrische waarschuwingen vandaag of niet.

|Resource dat wordt ondersteund door klassieke metrische waarschuwingen | Ondersteund door metrische waarschuwingen |
|-------------------------------------------------|----------------------------|
| Microsoft.ApiManagement/service | Ja |
| Microsoft.Batch/batchAccounts| Ja|
|Microsoft.Cache/redis| Ja
|Microsoft.ClassicCompute/virtualMachines | Nee |
|Microsoft.ClassicCompute/domainNames/slots/roles | Nee|
|Microsoft.CognitiveServices/accounts | Nee |
|Microsoft.Compute/virtualMachines | Ja|
|Microsoft.Compute/virtualMachineScaleSets| Ja|
|Microsoft.ClassicStorage/storageAccounts| Nee |
|Microsoft.DataFactory/datafactories | Ja|
|Microsoft.DBforMySQL/servers| Ja|
|Microsoft.DBforPostgreSQL/servers| Ja|
|Microsoft.Devices/IotHubs | Nee|
|Microsoft.DocumentDB/databaseAccounts| Nee|
|Microsoft.EventHub/namespaces | Ja|
|Microsoft.Logic/workflows | Ja|
|Microsoft.Network/loadBalancers |Ja|
|Microsoft.Network/publicIPAddresses| Ja|
|Microsoft.Network/applicationGateways| Ja|
|Microsoft.Network/expressRouteCircuits| Ja|
|Microsoft.Network/trafficManagerProfiles | Ja|
|Microsoft.Search/searchServices | Nee|
|Microsoft.ServiceBus/namespaces| Nee|
|Microsoft.Storage/storageAccounts | Ja|
|Microsoft.StreamAnalytics/streamingjobs| Ja|
|Microsoft.TimeSeriesInsights/environments | Ja|
|Microsoft. Web/serverfarms | Ja |
|Microsoft. Websites (met uitzondering van de functies) | Ja|
|Microsoft. Web/hostingEnvironments/multiRolePools | Nee|
|Microsoft. Web/hostingEnvironments/workerPools| Nee
|Microsoft.SQL/Servers | Nee|

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over het maken, weergeven en beheren van metrische waarschuwingen in Azure](alerts-metric.md)
- [Informatie over het implementeren van metrische waarschuwingen met behulp van Azure Resource Manager-sjablonen](../../azure-monitor/platform/alerts-metric-create-templates.md)
- [Meer informatie over actiegroepen](action-groups.md)

