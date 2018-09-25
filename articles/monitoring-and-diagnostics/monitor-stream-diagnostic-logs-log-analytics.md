---
title: Diagnostische logboeken naar Log Analytics in Azure Stream
description: Meer informatie over het streamen van diagnostische logboeken naar Log Analytics-werkruimte in Azure.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: c419a3c44a38f72d56f2b7b362c62e683fc20c7f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993014"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Diagnostische logboeken naar Log Analytics in Azure Stream

**[Diagnostische logboeken in Azure](monitoring-overview-of-diagnostic-logs.md)**  kan worden gestreamd in bijna realtime met Azure Log Analytics met behulp van de portal, PowerShell-cmdlets of Azure CLI.

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>Wat u kunt doen met diagnostische logboeken in Log Analytics

Azure Log Analytics is een flexibele log search en analyse hulpprogramma waarmee u meer inzicht krijgen in de onbewerkte logboekgegevens gegenereerd op basis van Azure-resources. Sommige mogelijkheden zijn onder andere:

* **Zoeken in logboeken** -schrijven geavanceerde query's via uw logboekgegevens, correleren Logboeken uit diverse bronnen, en zelfs genereren grafieken die kunnen worden vastgemaakt aan uw Azure-dashboard.
* **Waarschuwingen** -detecteren wanneer een of meer gebeurtenissen komen overeen met een bepaalde query en een melding te met een e-mailadres of webhook-aanroep ontvangen.
* **Oplossingen** -gebruik van vooraf gemaakte weergaven en dashboards die u onmiddellijk inzicht in uw logboekgegevens geven.
* **Geavanceerde analyse** : machine learning toepassen en dezelfde algoritmen voor het identificeren van mogelijke problemen aan het licht komt door uw logboeken patroon.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Streaming van diagnostische logboeken naar Log Analytics inschakelen

U kunt inschakelen via een programma, via de portal van diagnostische logboeken streamen of met behulp van de [Azure Monitor REST API's](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). In beide gevallen maakt u een diagnostische instelling in die u geeft een Log Analytics-werkruimte en de logboekcategorieën en metrische gegevens die u verzenden wilt in voor de desbetreffende werkruimte. Een diagnose **logboekcategorie** is een type logboek dat een resource kan bieden.

De Log Analytics-werkruimte heeft geen zich in hetzelfde abonnement als de resource dat Logboeken verzendt, zolang de gebruiker die de instelling configureert de juiste RBAC-toegang voor beide abonnementen heeft.

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Een voorbeeld*: de meetwaarde 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Stream diagnostische logboeken met behulp van de portal
1. In de portal, gaat u naar Azure Monitor en klikt u op **diagnostische instellingen**

    ![Sectie van Azure Monitor bewaking](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-blade.png)

2. (Optioneel) de lijst met door de resourcegroep of resourcetype filteren en klik vervolgens op de resource waarvoor u wilt een diagnostische instelling instellen.

3. Als er geen instellingen zijn op de resource hebt u geselecteerd, wordt u gevraagd om een instelling te maken. Klik op "Diagnostische gegevens inschakelen."

   ![Diagnostische instelling - er zijn geen bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-none.png)

   Als er bestaande instellingen op de resource, ziet u een lijst met instellingen die al zijn geconfigureerd voor deze resource. Klik op 'Diagnostische instelling toevoegen'.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-multiple.png)

3. Geef een naam van uw instelling en schakel het selectievakje voor **verzenden naar Log Analytics**, selecteer vervolgens een Log Analytics-werkruimte.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-configure.png)

4. Klik op **Opslaan**.

Na enkele ogenblikken wordt de nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze resource en logboeken met diagnostische gegevens worden gestreamd naar deze werkruimte zodra de gegevens van een nieuwe gebeurtenis wordt gegenereerd. Houd er rekening mee dat er maximaal vijftien minuten tussen wanneer een gebeurtenis wordt verzonden en wanneer deze wordt weergegeven in Log Analytics kan zijn.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets
Om in te schakelen streaming via de [Azure PowerShell-Cmdlets](insights-powershell-samples.md), kunt u de `Set-AzureRmDiagnosticSetting` cmdlet met de volgende parameters:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Houd er rekening mee dat de werkruimte-id-eigenschap duurt de volledige Azure-resource-ID van de werkruimte, niet de werkruimte-ID/sleutel wordt weergegeven in de Log Analytics-portal voordat.

### <a name="via-azure-cli"></a>Via Azure CLI

Om in te schakelen streaming via de [Azure CLI](insights-cli-samples.md), kunt u de [az monitor diagnostic-settings maken](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) opdracht.

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

U kunt extra categorieën toevoegen aan de diagnostische logboeken van woordenlijsten toe te voegen aan de JSON-matrix die is doorgegeven als de `--logs` parameter.

De `--resource-group` argument is alleen vereist als `--workspace` is niet een object-ID.

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Hoe ik de gegevens in Log Analytics op te vragen?

U kunt de logboeken met diagnostische gegevens als onderdeel van de oplossing Log Management onder de tabel AzureDiagnostics opvragen in de blade zoeken in Logboeken in de portal of Advanced Analytics-ervaring als onderdeel van Log Analytics. Er zijn ook [verschillende oplossingen voor Azure-resources](../log-analytics/log-analytics-add-solutions.md) u kunt installeren als u onmiddellijk inzicht in de logboekgegevens die u wilt verzenden naar Log Analytics.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure-logboeken met diagnostische gegevens](monitoring-overview-of-diagnostic-logs.md)
