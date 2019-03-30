---
title: Stream Azure diagnostische logboeken naar Log Analytics-werkruimte in Azure Monitor
description: Meer informatie over het streamen van diagnostische logboeken naar Log Analytics-werkruimte in Azure Monitor in Azure.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 33d8f2e7c65a786d1ecb389574fe186efb6fb705
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630791"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics-workspace-in-azure-monitor"></a>Stream Azure diagnostische logboeken naar Log Analytics-werkruimte in Azure Monitor

**[Diagnostische logboeken in Azure](diagnostic-logs-overview.md)**  kan worden gestreamd in bijna realtime bij een Log Analytics-werkruimte in Azure Monitor via de portal, PowerShell-cmdlets of Azure CLI.

## <a name="what-you-can-do-with-diagnostics-logs-in-a-log-analytics-workspace"></a>Wat u kunt doen met diagnostische logboeken in Log Analytics-werkruimte

Azure Monitor biedt een flexibel log query- en analysemogelijkheden hulpprogramma waarmee u meer inzicht krijgen in de onbewerkte logboekgegevens gegenereerd op basis van Azure-resources. Sommige mogelijkheden zijn onder andere:

* **Query voor** -schrijven geavanceerde query's via uw logboekgegevens, correleren Logboeken uit verschillende bronnen en genereren grafieken die kunnen worden vastgemaakt aan uw Azure-dashboard.
* **Waarschuwingen** -detecteren wanneer een of meer gebeurtenissen komen overeen met een bepaalde query en een melding te ontvangen met een e-mailadres of webhook-aanroep met behulp van Azure Monitor-waarschuwingen.
* **Geavanceerde analyse** : machine learning toepassen en dezelfde algoritmen voor het identificeren van mogelijke problemen aan het licht komt door uw logboeken patroon.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics-workspace"></a>Streaming van diagnostische logboeken naar Log Analytics-werkruimte inschakelen

U kunt inschakelen via een programma, via de portal van diagnostische logboeken streamen of met behulp van de [Azure Monitor REST API's](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). In beide gevallen maakt u een diagnostische instelling in die u geeft een Log Analytics-werkruimte en de logboekcategorieën en metrische gegevens die u verzenden wilt in voor de desbetreffende werkruimte. Een diagnose **logboekcategorie** is een type logboek dat een resource kan bieden.

De Log Analytics-werkruimte heeft geen zich in hetzelfde abonnement als de resource dat Logboeken verzendt, zolang de gebruiker die de instelling configureert de juiste RBAC-toegang voor beide abonnementen heeft.

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Bijvoorbeeld*: De metriek 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Stream diagnostische logboeken met behulp van de portal
1. In de portal, gaat u naar Azure Monitor en klikt u op **diagnostische instellingen** in de **instellingen** menu.


2. (Optioneel) de lijst met door de resourcegroep of resourcetype filteren en klik vervolgens op de resource waarvoor u wilt een diagnostische instelling instellen.

3. Als er geen instellingen zijn op de resource hebt u geselecteerd, wordt u gevraagd om een instelling te maken. Klik op "Diagnostische gegevens inschakelen."

   ![Diagnostische instelling - er zijn geen bestaande instellingen toevoegen](media/diagnostic-logs-stream-log-store/diagnostic-settings-none.png)

   Als er bestaande instellingen op de resource, ziet u een lijst met instellingen die al zijn geconfigureerd voor deze resource. Klik op 'Diagnostische instelling toevoegen'.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/diagnostic-logs-stream-log-store/diagnostic-settings-multiple.png)

3. Geef een naam van uw instelling en schakel het selectievakje voor **verzenden naar Log Analytics**, selecteer vervolgens een Log Analytics-werkruimte.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/diagnostic-logs-stream-log-store/diagnostic-settings-configure.png)

4. Klik op **Opslaan**.

Na enkele ogenblikken wordt de nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze resource en logboeken met diagnostische gegevens worden gestreamd naar deze werkruimte zodra de gegevens van een nieuwe gebeurtenis wordt gegenereerd. Houd er rekening mee dat er maximaal vijftien minuten tussen wanneer een gebeurtenis wordt verzonden en wanneer deze wordt weergegeven in Log Analytics kan zijn.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Om in te schakelen streaming via de [Azure PowerShell-Cmdlets](../../azure-monitor/platform/powershell-quickstart-samples.md), kunt u de `Set-AzDiagnosticSetting` cmdlet met de volgende parameters:

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Houd er rekening mee dat de werkruimte-id-eigenschap duurt de volledige Azure-resource-ID van de werkruimte, niet de werkruimte-ID/sleutel wordt weergegeven in de Log Analytics-portal voordat.

### <a name="via-azure-cli"></a>Via Azure CLI

Om in te schakelen streaming via de [Azure CLI](../../azure-monitor/platform/cli-samples.md), kunt u de [az monitor diagnostic-settings maken](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) opdracht.

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

## <a name="how-do-i-query-the-data-from-a-log-analytics-workspace"></a>Hoe ik de gegevens vanuit een Log Analytics-werkruimte op te vragen?

U kunt Logboeken met diagnostische gegevens in de blade Logboeken in de portal voor Azure Monitor opvragen als onderdeel van de oplossing Log Management onder de tabel AzureDiagnostics. Er zijn ook [verschillende bewakingsoplossingen voor Azure-resources](../../azure-monitor/insights/solutions.md) u om op te halen onmiddellijk inzicht in de logboekgegevens die u wilt verzenden naar Azure Monitor kunt installeren.

### <a name="known-limitation-column-limit-in-azurediagnostics"></a>Bekende beperking: kolomlimiet in AzureDiagnostics
Omdat veel resources verzenden gegevenstypen worden verzonden naar dezelfde tabel (_AzureDiagnostics_), het schema van deze tabel is de Super set de schema's van alle verschillende gegevenstypen die worden verzameld. Bijvoorbeeld, als u diagnostische instellingen voor het verzamelen van de volgende gegevenstypen hebt gemaakt, alle verstuurd naar dezelfde werkruimte:
- Controlelogboeken van bron 1 (met een schema dat bestaat uit kolommen A, B en C)  
- Foutenlogboeken van Resource-2 (met een schema dat bestaat uit kolommen D, E en F)  
- Stroomlogboeken van de gegevens van de Resource-3 (met een schema dat bestaat uit de kolommen G, H en ik)  
 
De tabel AzureDiagnostics eruit als volgt met wat voorbeeldgegevens:  
 
| ResourceProvider | Categorie | A | B | C | D | E | F | G | H | I |
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| Microsoft.Resource1 | AuditLogs | x1 | y1 | z1 |
| Microsoft.Resource2 | ErrorLogs | | | | q1 | w1 | e1 |
| Microsoft.Resource3 | DataFlowLogs | | | | | | | j1 | k1 | l1|
| Microsoft.Resource2 | ErrorLogs | | | | q2 | w2 | e2 |
| Microsoft.Resource3 | DataFlowLogs | | | | | | | j3 | k3 | l3|
| Microsoft.Resource1 | AuditLogs | x5 | y5 | z5 |
| ... |
 
Er is een expliciete limiet van een opgegeven Azure Log-tabel niet met meer dan 500 kolommen. Wanneer is bereikt, wordt op het moment van opname met gegevens met een andere kolom buiten de eerste 500 rijen verwijderd. De tabel AzureDiagnostics is in het bijzonder gevoelig beïnvloed als deze limiet. Dit gebeurt meestal omdat een grote verscheidenheid aan gegevensbronnen worden verzonden naar dezelfde werkruimte, of verschillende zeer uitgebreide gegevensbronnen worden verzonden naar dezelfde werkruimte. 
 
#### <a name="azure-data-factory"></a>Azure Data Factory  
Azure Data Factory, vanwege een zeer uitgebreide set zich aanmeldt, is een resource die bekend is met name beïnvloed door deze limiet. In het bijzonder:  
- *Parameters van de gebruiker gedefinieerd voor elke activiteit in de pijplijn*: Er is een nieuwe kolom die is gemaakt voor elke parameter van de gebruiker een unieke naam op basis van een activiteit. 
- *Invoer en uitvoer*: deze variëren van activiteit-naar-activiteit en genereren van een grote hoeveelheid kolommen vanwege hun uitgebreide aard. 
 
Als met de bredere tijdelijke oplossing voorstellen die hieronder, het verdient aanbeveling om te isoleren van ADF Logboeken in hun eigen werkruimte te minimaliseren, de kans dat deze logboeken die invloed hebben op andere typen logboeken die worden verzameld in uw werkruimten. We verwachten hebt gecureerd logboeken voor Azure Data Factory beschikbaar door mid April 2019.
 
#### <a name="workarounds"></a>Tijdelijke oplossingen
Korte termijn, totdat de limiet van 500-kolom is gedefinieerd, is het aanbevolen om te typen uitgebreide gegevens verdelen in afzonderlijke werkruimten te verminderen van de mogelijkheid om de limiet hebt bereikt.
 
Langere termijn Azure Diagnostics wordt worden verplaatst van een uniforme, sparse-schema in afzonderlijke tabellen per elk gegevenstype; in combinatie met ondersteuning voor dynamische typen, verbetert dit aanzienlijk de bruikbaarheid van gegevens die afkomstig zijn in Logboeken van Azure via de Azure Diagnostics-mechanisme. U kunt al dit zien voor select typen Azure-resources, bijvoorbeeld [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics) of [Intune](https://docs.microsoft.com/intune/review-logs-using-azure-monitor) Logboeken. Raadpleeg voor nieuws over nieuwe resourcetypen in Azure ondersteunen deze gecureerde logboeken op de [Azure Updates](https://azure.microsoft.com/updates/) blog!


## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure-logboeken met diagnostische gegevens](diagnostic-logs-overview.md)

