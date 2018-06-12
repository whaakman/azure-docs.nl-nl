---
title: Stream Azure diagnostische logboeken met Log Analytics
description: Informatie over het Azure logboeken met diagnostische gegevens naar een werkruimte voor logboekanalyse stream.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: 634cecb247686afd8c5c749d6e28b301d7e07c4f
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263504"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Stream Azure diagnostische logboeken met Log Analytics

**[Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)**  in bijna realtime met Azure Log Analytics via de portal, PowerShell-cmdlets of Azure CLI 2.0 kan worden gestreamd.

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>Wat u kunt doen met diagnostische gegevens geregistreerd in Log Analytics

Azure Log Analytics is een flexibele logboek zoeken en analytics hulpprogramma waarmee u inzicht te verwerven in de onbewerkte logboekgegevens gegenereerd op basis van de Azure-resources. Er zijn enkele mogelijkheden:

* **Logboek zoeken** -schrijven geavanceerde query's via uw logboekgegevens, vergelijken Logboeken uit diverse bronnen, en zelfs genereren grafieken die kunnen worden vastgemaakt aan uw Azure-dashboard.
* **Waarschuwingen** -detecteren wanneer een of meer gebeurtenissen overeenkomen met een bepaalde query en worden een melding met een e-mailadres of webhook-aanroep.
* **Oplossingen** -gebruik van vooraf samengestelde weergaven en dashboards die u onmiddellijk inzicht in uw logboekgegevens geven.
* **Geavanceerde analyses** - toepassing van machine learning en overeenkomende algoritmen om u te identificeren mogelijke problemen die door uw logboeken getoond patroon.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Streaming van diagnostische logboeken met logboekanalyse inschakelen

U kunt inschakelen, streamen van diagnostische logboeken programmatisch via de portal of met behulp van de [Monitor REST-API's van Azure](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). In beide gevallen moet u een diagnostische instelling maken in die u opgeeft een werkruimte voor logboekanalyse en de logboek-categorieën en metrische gegevens die u wilt verzenden naar deze werkruimte. Een diagnose **logboek categorie** is een type-log dat een resource kan bieden.

De werkruimte voor logboekanalyse heeft geen zich in hetzelfde abonnement als de bron logboeken verzenden zolang de gebruiker die de instelling configureert juiste RBAC toegang tot beide abonnementen heeft.

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Een voorbeeld*: de metriek 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Maar wanneer de metriek wordt geëxporteerd via diagnostische instellingen, geeft de metriek alle binnenkomende berichten in alle wachtrijen in de Event Hub aan.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Diagnostische logboeken met behulp van de portal
1. In de portal, gaat u naar Azure Monitor en klikt u op **diagnostische instellingen**

    ![Sectie van de Monitor Azure bewaking](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-blade.png)

2. Optioneel de lijst filteren op resourcegroep of brontype en klik vervolgens op de bron die u wilt geen diagnostische instellen.

3. Als er geen instellingen bestaan op de bron voor hebt u geselecteerd, u wordt gevraagd om een instelling te maken. Klik op "Diagnostische gegevens inschakelen."

   ![Diagnostische instelling - geen bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-none.png)

   Als er bestaande instellingen op de bron, ziet u een lijst met instellingen die al zijn geconfigureerd op deze resource. Klik op 'Diagnostische instelling toevoegen'.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-multiple.png)

3. Geef een naam van de instelling en schakel het selectievakje voor **verzenden met logboekanalyse**, selecteer vervolgens een werkruimte voor logboekanalyse.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-configure.png)

4. Klik op **Opslaan**.

De nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze bron na enkele ogenblikken en logboeken met diagnostische gegevens worden gestreamd naar deze werkruimte zodra er nieuwe gebeurtenisgegevens wordt gegenereerd. Houd er rekening mee dat er mogelijk tot vijftien minuten tussen wanneer een gebeurtenis wordt gegenereerd en wanneer deze wordt weergegeven in de logboekanalyse.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets
Om in te schakelen streaming via de [Azure PowerShell-Cmdlets](insights-powershell-samples.md), kunt u de `Set-AzureRmDiagnosticSetting` cmdlet met de volgende parameters:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Houd er rekening mee dat de eigenschap workspaceID duurt de volledige Azure-resource-ID van de werkruimte niet uit de werkruimte-ID/sleutel die wordt weergegeven in de Log Analytics-portal voordat.

### <a name="via-azure-cli-20"></a>Via Azure CLI 2.0

Om in te schakelen streaming via de [Azure CLI 2.0](insights-cli-samples.md), kunt u de [az monitor diagnose-instellingen maken](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) opdracht.

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

U kunt extra categorieën toevoegen aan de diagnostische logboeken door woordenlijsten toe te voegen aan de JSON-matrix doorgegeven als de `--logs` parameter.

De `--resource-group` -argument is alleen vereist als `--workspace` is niet een object-ID.

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Hoe ik de gegevens opvragen in Log Analytics?

U kunt de logboeken met diagnostische gegevens als onderdeel van het beheersysteem voor logboek onder de tabel AzureDiagnostics query in de blade logboek zoeken in de portal of Advanced Analytics ervaring als onderdeel van logboekanalyse. Er zijn ook [verschillende oplossingen voor Azure-resources](../log-analytics/log-analytics-add-solutions.md) u kunt installeren als u onmiddellijk inzicht in de logboekgegevens die u verzendt naar logboekanalyse.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)
