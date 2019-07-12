---
title: Stream Azure diagnostische logboeken naar Log Analytics-werkruimte in Azure Monitor
description: Meer informatie over het streamen van diagnostische logboeken naar Log Analytics-werkruimte in Azure Monitor in Azure.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: e8e6276a38f06b5c6ebb24c89f3733b9fd7220f7
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612830"
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

Na enkele ogenblikken wordt de nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze resource en logboeken met diagnostische gegevens worden gestreamd naar deze werkruimte zodra de gegevens van een nieuwe gebeurtenis wordt gegenereerd. Mogelijk zijn er maximaal 15 minuten tussen wanneer een gebeurtenis wordt verzonden en wanneer deze wordt weergegeven in Log Analytics.

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

### <a name="examples"></a>Voorbeelden

```Kusto
// Resources that collect diagnostic logs into this Log Analytics workspace, using Diagnostic Settings
AzureDiagnostics
| distinct _ResourceId
```
```Kusto
// Resource providers collecting diagnostic logs into this Log Analytics worksapce, with log volume per category
AzureDiagnostics
| summarize count() by ResourceProvider, Category
```
```Kusto
// Resource types collecting diagnostic logs into this Log Analytics workspace, with number of resources onboarded
AzureDiagnostics
| summarize ResourcesOnboarded=dcount(_ResourceId) by ResourceType
```
```Kusto
// Operations logged by specific resource provider, in this example - KeyVault
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.KEYVAULT"
| distinct OperationName
```

## <a name="azure-diagnostics-vs-resource-specific"></a>Azure Diagnostics vs Resourcespecifieke  
Zodra een Log Analytics-doel in een configuratie van de Azure Diagnostics is ingeschakeld, zijn er twee verschillende manieren waarop gegevens worden weergegeven in uw werkruimte:  
- **Azure Diagnostics** -dit is de oudere methode vandaag nog wordt gebruikt door de meerderheid van de Azure-services. In deze modus kunnen alle de gegevens uit een diagnostische instelling waarnaar wordt verwezen naar een opgegeven werkruimte verschijnen de _AzureDiagnostics_ tabel. 
<br><br>Omdat veel bronnen gegevens naar dezelfde tabel verzenden (_AzureDiagnostics_), het schema van deze tabel is de Super set de schema's van alle verschillende gegevenstypen die worden verzameld. Bijvoorbeeld, als u diagnostische instellingen voor het verzamelen van de volgende gegevenstypen hebt gemaakt, alle verstuurd naar dezelfde werkruimte:
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

- **Resource-specifieke** -In deze modus kunnen afzonderlijke tabellen in de geselecteerde werkruimte per elke categorie worden geselecteerd in de configuratie van diagnostische instellingen zijn gemaakt. Deze nieuwere methode maakt het veel gemakkelijker te vinden precies u wilt zoeken via expliciete scheiding van taken: een tabel voor elke categorie. Daarnaast biedt deze blade voordelen in de ondersteuning voor dynamische typen. U kunt al deze modus voor select typen Azure-resources, bijvoorbeeld zien [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics) of [Intune](https://docs.microsoft.com/intune/review-logs-using-azure-monitor) Logboeken. We verwachten uiteindelijk moet elk gegevenstype te migreren naar de Resource-specifieke modus. 

    In het bovenstaande voorbeeld is zou dit leiden tot drie tabellen worden gemaakt: 
    - Tabel _AuditLogs_ als volgt:

        | ResourceProvider | Categorie | A | B | C |
        | -- | -- | -- | -- | -- |
        | Microsoft.Resource1 | AuditLogs | x1 | y1 | z1 |
        | Microsoft.Resource1 | AuditLogs | x5 | y5 | z5 |
        | ... |

    - Tabel _foutenlogboeken_ als volgt:  

        | ResourceProvider | Categorie | D | E | F |
        | -- | -- | -- | -- | -- | 
        | Microsoft.Resource2 | ErrorLogs | q1 | w1 | e1 |
        | Microsoft.Resource2 | ErrorLogs | q2 | w2 | e2 |
        | ... |

    - Tabel _DataFlowLogs_ als volgt:  

        | ResourceProvider | Categorie | G | H | I |
        | -- | -- | -- | -- | -- | 
        | Microsoft.Resource3 | DataFlowLogs | j1 | k1 | l1|
        | Microsoft.Resource3 | DataFlowLogs | j3 | k3 | l3|
        | ... |

    Andere voordelen van het gebruik van de Resource-specifieke modus zijn prestaties zijn verbeterd voor zowel opnamelatentie en uitvoertijden van query, betere zichtbaarheid van schema's en de structuur ervan, de mogelijkheid het RBAC-rechten op een specifieke tabel, en meer.

### <a name="selecting-azure-diagnostic-vs-resource-specific-mode"></a>Diagnostische Azure vs Resource-specifieke modus selecteren
Voor de meeste Azure-resources, hebt u geen keuze of u wilt gebruiken de Azure Diagnostics of Resource-specifieke modus; de gegevens doorgestuurd automatisch via de methode die de resource heeft geselecteerd om te gebruiken. Raadpleeg de documentatie van de resource die u hebt ingeschakeld om gegevens te verzenden naar Log Analytics voor meer informatie waarop modus wordt gebruikt. 

Zoals vermeld in de vorige sectie, is het doel van Azure Monitor hebben alle services in Azure-gebruik de Resource-specifieke modus. Deze overgang en zorg ervoor dat er geen gegevens verloren als onderdeel van het, sommige Azure-services als onboarding naar Log Analytics u over een selectie van de modus beschikt:  
   ![Diagnostische instellingen modus selector](media/diagnostic-logs-stream-log-store/diagnostic-settings-mode-selector.png)

We **sterk** raadzaam om, om te voorkomen dat mogelijk moeilijk migraties omlaag onderweg, een zojuist maken gebruik van de diagnostische instellingen voor de Resource gericht modus.  

Bestaande diagnostische instellingen, eenmaal is ingeschakeld door een bepaalde Azure-Resource, kunt u zich met terugwerkende kracht overschakelen van de Azure Diagnostics naar de Resource-specifieke modus. Uw gegevens eerder opgenomen blijven beschikbaar in de _AzureDiagnostics_ tabel totdat deze leeftijden uit zoals geconfigureerd in uw instelling bewaren in de werkruimte, maar geen nieuwe gegevens wordt verzonden naar de specifieke tabel. Dit betekent dat voor alle query's die moeten zowel de oude gegevens omvatten en nieuwe (totdat de oude gegevens leeftijden volledig uit), een [union](https://docs.microsoft.com/azure/kusto/query/unionoperator) operator in uw query's moeten de twee gegevenssets combineren.

Bekijk voor nieuws over nieuwe Azure ondersteunende Logboeken in de modus Resource-specifieke services op de [Azure Updates](https://azure.microsoft.com/updates/) blog!

### <a name="known-limitation-column-limit-in-azurediagnostics"></a>Bekende beperking: kolomlimiet in AzureDiagnostics
Er is een expliciete limiet van een opgegeven Azure Log-tabel niet met meer dan 500 kolommen. Wanneer is bereikt, wordt op het moment van opname met gegevens met een andere kolom buiten de eerste 500 rijen verwijderd. De tabel AzureDiagnostics is in het bijzonder gevoelig beïnvloed als deze limiet. Dit gebeurt meestal omdat een grote verscheidenheid aan gegevensbronnen worden verzonden naar dezelfde werkruimte, of verschillende uitgebreide gegevensbronnen worden verzonden naar dezelfde werkruimte. 

#### <a name="azure-data-factory"></a>Azure Data Factory  
Azure Data Factory, is vanwege een zeer uitgebreide set zich aanmeldt, een resource die bekend is met name beïnvloed door deze limiet. In het bijzonder voor diagnostische instellingen geconfigureerd voordat u de Resource-specifieke is modus ingeschakeld of expliciet kiezen voor het gebruik van de Resource-specifieke-modus van de reverse-compatibiliteit:  
- *Parameters van de gebruiker gedefinieerd voor elke activiteit in de pijplijn*: Er is een nieuwe kolom die is gemaakt voor elke parameter van de gebruiker een unieke naam op basis van een activiteit. 
- *Invoer en uitvoer*: deze variëren van activiteit-naar-activiteit en het genereren van een groot aantal kolommen vanwege hun uitgebreide aard. 
 
Als met de bredere tijdelijke oplossing voorstellen die hieronder, het verdient aanbeveling voor het migreren van uw logboeken voor het gebruik van de Resource-specifieke modus zo snel mogelijk. Als u niet onmiddellijk mogelijk, is een tussentijdse alternatief ADF logboeken isoleren in hun eigen werkruimte te minimaliseren, de kans dat deze logboeken die invloed hebben op andere typen logboeken die worden verzameld in uw werkruimten. 
 
#### <a name="workarounds"></a>Tijdelijke oplossingen
Korte termijn, tot alle Azure-services niet zijn ingeschakeld in de modus Resource-specifieke voor alle services die nog ondersteuning biedt voor de Resource-specifieke modus, is het aanbevolen voor het scheiden van uitgebreide gegevenstypen die door deze services zijn gepubliceerd in afzonderlijke werkruimten om te beperken de mogelijkheid om de limiet hebt bereikt.  
 
Langere termijn, de Azure Diagnostics is over op alle Azure-services ondersteunt de modus Resource-specifieke. Wordt aangeraden de overstap naar deze modus zo snel mogelijk te verminderen van de mogelijkheden die worden beïnvloed door deze beperking 500 kolom.  


## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure-logboeken met diagnostische gegevens](diagnostic-logs-overview.md)

