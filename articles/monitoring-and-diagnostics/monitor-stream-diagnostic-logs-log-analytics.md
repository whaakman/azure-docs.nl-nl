---
title: Stream Azure diagnostische logboeken met Log Analytics | Microsoft Docs
description: Informatie over het Azure logboeken met diagnostische gegevens naar een werkruimte voor logboekanalyse stream.
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
ms.date: 11/01/2017
ms.author: johnkem
ms.openlocfilehash: 9440bd7f872914887c1f6e50f08a3c273536fcf8
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Stream Azure diagnostische logboeken met Log Analytics
**[Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)**  kan worden gestreamd in bijna realtime met Azure Log Analytics via de portal, PowerShell-cmdlets of Azure CLI.

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>Wat u kunt doen met diagnostische gegevens geregistreerd in Log Analytics

Azure Log Analytics is een flexibele logboek zoeken en analytics hulpprogramma waarmee u inzicht te verwerven in de onbewerkte logboekgegevens gegenereerd op basis van de Azure-resources. Er zijn enkele mogelijkheden:

* **Logboek zoeken** -schrijven geavanceerde query's via uw logboekgegevens, vergelijken Logboeken uit diverse bronnen, en zelfs genereren grafieken die kunnen worden vastgemaakt aan uw Azure-dashboard.
* **Waarschuwingen** -detecteren wanneer een of meer gebeurtenissen overeenkomen met een bepaalde query en worden een melding met een e-mailadres of webhook-aanroep.
* **Oplossingen** -gebruik van vooraf samengestelde weergaven en dashboards die u onmiddellijk inzicht in uw logboekgegevens geven.
* **Geavanceerde analyses** - toepassing van machine learning en overeenkomende algoritmen om u te identificeren mogelijke problemen die door uw logboeken getoond patroon.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Streaming van diagnostische logboeken met logboekanalyse inschakelen
U kunt inschakelen, streamen van diagnostische logboeken programmatisch via de portal of met behulp van de [Monitor REST-API's van Azure](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). In beide gevallen moet u een diagnostische instelling maken in die u opgeeft een werkruimte voor logboekanalyse en de logboek-categorieën en metrische gegevens die u wilt verzenden naar deze werkruimte. Een diagnose **logboek categorie** is een type-log dat een resource kan bieden.

De werkruimte voor logboekanalyse heeft geen zich in hetzelfde abonnement als de bron logboeken verzenden zolang de gebruiker die de instelling configureert juiste RBAC toegang tot beide abonnementen heeft.

## <a name="stream-diagnostic-logs-using-the-portal"></a>Diagnostische logboeken met behulp van de portal
1. In de portal, gaat u naar Azure Monitor en klikt u op **diagnostische instellingen**

    ![Sectie van de Monitor Azure bewaking](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. Optioneel de lijst filteren op resourcegroep of brontype en klik vervolgens op de bron die u wilt geen diagnostische instellen.

3. Als er geen instellingen bestaan op de bron voor hebt u geselecteerd, u wordt gevraagd om een instelling te maken. Klik op "Diagnostische gegevens inschakelen."

   ![Diagnostische instelling - geen bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   Als er bestaande instellingen op de bron, ziet u een lijst met instellingen die al zijn geconfigureerd op deze resource. Klik op 'Diagnostische instelling toevoegen'.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. Geef een naam van de instelling en schakel het selectievakje voor **verzenden met logboekanalyse**, selecteer vervolgens een werkruimte voor logboekanalyse.
   
   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)

4. Klik op **Opslaan**.

De nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze bron na enkele ogenblikken en logboeken met diagnostische gegevens worden gestreamd naar deze werkruimte zodra er nieuwe gebeurtenisgegevens wordt gegenereerd. Houd er rekening mee dat er mogelijk tot vijftien minuten tussen wanneer een gebeurtenis wordt gegenereerd en wanneer deze wordt weergegeven in de logboekanalyse.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets
Om in te schakelen streaming via de [Azure PowerShell-Cmdlets](insights-powershell-samples.md), kunt u de `Set-AzureRmDiagnosticSetting` cmdlet met de volgende parameters:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Houd er rekening mee dat de eigenschap workspaceID duurt de volledige Azure-resource-ID van de werkruimte niet uit de werkruimte-ID/sleutel die wordt weergegeven in de Log Analytics-portal voordat.

### <a name="via-azure-cli"></a>Via Azure CLI
Om in te schakelen streaming via de [Azure CLI](insights-cli-samples.md), kunt u de `insights diagnostic set` opdracht als volgt:

```azurecli
azure insights diagnostic set --resourceId <resourceID> --workspaceId <workspace resource ID> --categories <list of categories> --enabled true
```

Houd er rekening mee dat de eigenschap workspaceId duurt de volledige Azure-resource-ID van de werkruimte niet uit de werkruimte-ID/sleutel die wordt weergegeven in de Log Analytics-portal voordat.

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Hoe ik de gegevens opvragen in Log Analytics?

U kunt de logboeken met diagnostische gegevens als onderdeel van het beheersysteem voor logboek onder de tabel AzureDiagnostics query in de blade logboek zoeken in de portal of Advanced Analytics ervaring als onderdeel van logboekanalyse. Er zijn ook [verschillende oplossingen voor Azure-resources](../log-analytics/log-analytics-add-solutions.md) u kunt installeren als u onmiddellijk inzicht in de logboekgegevens die u verzendt naar logboekanalyse.


## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)
