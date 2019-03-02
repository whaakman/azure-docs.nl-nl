---
title: Opgeslagen zoekopdrachten in oplossingen voor | Microsoft Docs
description: Beheeroplossingen bevatten meestal opgeslagen zoekopdrachten in Log Analytics voor het analyseren van gegevens die zijn verzameld door de oplossing. Ze kunnen ook waarschuwingen om de hoogte van de gebruiker te definiëren of automatisch een actie uitvoeren in reactie op een belangrijk probleem. In dit artikel wordt beschreven hoe u Log Analytics opgeslagen zoekopdrachten in Resource Manager-sjabloon, zodat ze kunnen worden opgenomen in oplossingen voor het beheer te definiëren.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2019
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dd158dac9ea50d0b98b6e2d37412c602ea54f7e8
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57246644"
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-management-solution-preview"></a>Toevoegen van Log Analytics opgeslagen zoekopdrachten en waarschuwingen in management-oplossing (Preview)

> [!IMPORTANT]
> De details voor het maken van een waarschuwing met behulp van Resource Manager-sjabloon zijn van de datum nu die [Log Analytics-waarschuwingen zijn uitgebreid naar Azure Monitor](../platform/alerts-extend.md). Zie voor meer informatie over het maken van een waarschuwing met Resource Manager-sjabloon [waarschuwingen beheren met behulp van Azure-Resourcesjabloon](../platform/alerts-log.md#managing-log-alerts-using-azure-resource-template).

> [!NOTE]
> Dit is een voorlopige documentatie voor het maken van oplossingen die zich momenteel in preview. Er is geen schema die hieronder worden beschreven kan worden gewijzigd.

[Beheeroplossingen](solutions.md) omvatten [opgeslagen zoekopdrachten](../../azure-monitor/log-query/log-query-overview.md) in Log Analytics voor het analyseren van gegevens die zijn verzameld door de oplossing. Ze kunnen ook definiëren [waarschuwingen](../../azure-monitor/platform/alerts-overview.md) aan de gebruiker waarschuwen of automatisch een actie uitvoeren in reactie op een belangrijk probleem. In dit artikel wordt beschreven hoe u voor het definiëren van Log Analytics opgeslagen zoekopdrachten en waarschuwingen in een [Resource Manager-sjabloon](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md) , zodat ze kunnen worden opgenomen [beheeroplossingen](solutions-creating.md).

> [!NOTE]
> De voorbeelden in dit artikel Gebruik parameters en variabelen die zijn vereist of gemeenschappelijke beheeroplossingen en wordt beschreven in [ontwerpen en bouwen van een oplossing in Azure](solutions-creating.md)

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u bent al bekend bent met het [maken van een oplossing voor](solutions-creating.md) en de structuur van een [Resource Manager-sjabloon](../../azure-resource-manager/resource-group-authoring-templates.md) en de bestandsnaam van de oplossing.


## <a name="log-analytics-workspace"></a>Log Analytics-werkruimte
Alle resources in Log Analytics zijn opgenomen in een [werkruimte](../../azure-monitor/platform/manage-access.md). Zoals beschreven in [Log Analytics-werkruimte en het Automation-account](solutions.md#log-analytics-workspace-and-automation-account), de werkruimte niet is opgenomen in de oplossing voor beheer, maar moet bestaan voordat de oplossing is geïnstalleerd. Als deze niet beschikbaar is, mislukt de installatie van de oplossing.

De naam van de werkruimte is de naam van elke Log Analytics-resource. Dit doet u in de oplossing met de **werkruimte** parameter zoals in het volgende voorbeeld van een resource SavedSearch.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Log Analytics-API-versie
Alle Log Analytics-resources in een Resource Manager-sjabloon gedefinieerd hebben een eigenschap **apiVersion** die de versie van de API moet worden gebruikt door de resource definieert.

De volgende tabel bevat de API-versie voor de resource die in dit voorbeeld worden gebruikt.

| Resourcetype | API-versie | Query’s uitvoeren |
|:---|:---|:---|
| savedSearches | 2017-03-15-preview | Event &#124; where EventLevelName == "Error"  |


## <a name="saved-searches"></a>Opgeslagen zoekopdrachten
Opnemen [opgeslagen zoekopdrachten](../../azure-monitor/log-query/log-query-overview.md) in een oplossing waarmee gebruikers query uitvoeren op gegevens die door uw oplossing worden verzameld. Opgeslagen zoekopdrachten worden weergegeven onder **opgeslagen zoekopdrachten** in Azure portal. Een opgeslagen zoekopdracht is ook vereist voor elke waarschuwing.

[Log Analytics opgeslagen zoekopdracht](../../azure-monitor/log-query/log-query-overview.md) resources zijn een type `Microsoft.OperationalInsights/workspaces/savedSearches` en hebben de volgende structuur. Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters.

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }

Elke eigenschap van een opgeslagen zoekopdracht wordt in de volgende tabel beschreven.

| Eigenschap | Description |
|:--- |:--- |
| category | De categorie voor de opgeslagen zoekopdracht.  Alle opgeslagen zoekopdrachten in dezelfde oplossing delen vaak één categorie, zodat ze samen worden gegroepeerd in de console. |
| displayname | De naam om weer te geven voor de opgeslagen zoekopdracht in de portal. |
| query | De query wilt uitvoeren. |

> [!NOTE]
> Mogelijk moet u het escape-tekens gebruiken in de query bevat tekens die kunnen worden geïnterpreteerd als JSON. Bijvoorbeeld, als uw query is **AzureActivity | OperationName:"Microsoft.Compute/virtualMachines/write"**, ze moet worden geschreven in het oplossingsbestand als **AzureActivity | OperationName: /\"Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Waarschuwingen
[Waarschuwingen voor Azure](../../azure-monitor/platform/alerts-unified-log.md) zijn gemaakt door Azure waarschuwingsregels die opgegeven logboeken-query's met regelmatige tussenpozen uitvoeren. Als de resultaten van de query aan opgegeven criteria voldoen, wordt een waarschuwingsrecord gemaakt en een of meer acties worden uitgevoerd met behulp van [actiegroepen](../../azure-monitor/platform/action-groups.md).

> [!NOTE]
> Vanaf 14 mei 2018 worden alle waarschuwingen in een openbare Azure-cloud-exemplaar van Log Analytics-werkruimte is begonnen om uit te breiden naar Azure. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure](../../azure-monitor/platform/alerts-extend.md). Voor gebruikers die waarschuwingen naar Azure uitbreiden, worden de acties nu beheerd in Azure-Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actiegroep - Azure Resource Manager-sjabloon](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).
Waarschuwingsregels in een oplossing voor bestaan uit de volgende drie verschillende bronnen.

- **Opgeslagen zoekopdracht.** Hiermee definieert u de zoeken in logboeken die wordt uitgevoerd. Meerdere regels voor waarschuwingen kan een enkele opgeslagen zoekopdracht kunnen delen.
- **Planning.** Bepaalt hoe vaak de zoeken in Logboeken wordt uitgevoerd. De waarschuwingsregel is slechts één schema.
- **Actie bij waarschuwing.** De waarschuwingsregel is één actie groep of actie bron (verouderde) met een type van **waarschuwing** die de details van de waarschuwing, zoals de criteria voor wanneer een waarschuwingsrecord gemaakt en de ernst van de waarschuwing definieert. [Actiegroep](../../azure-monitor/platform/action-groups.md) resource kan ook een lijst van geconfigureerde acties moet worden uitgevoerd wanneer de waarschuwing wordt geactiveerd - zoals telefoongesprek, SMS, e-mail, webhook, ITSM-hulpprogramma, automation-runbook, logische app, enzovoort.

De actie-resource (verouderde) definieert een e-mail en runbook-reactie (optioneel).
- **Webhookactie (verouderd).** Als de waarschuwingsregel een webhook aanroept, wordt hiervoor een actie-resource met een type **Webhook**.

Opgeslagen zoekopdracht resources hierboven worden beschreven. De andere resources worden hieronder beschreven.

### <a name="schedule-resource"></a>Schema-resource

Een opgeslagen zoekopdracht kan een of meer planningen met elke planning voor een afzonderlijke waarschuwingsregel hebben. Het schema wordt gedefinieerd hoe vaak de zoekopdracht wordt uitgevoerd en het tijdsinterval op waarover de gegevens worden opgehaald. Schema-resources zijn een type `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` en hebben de volgende structuur. Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters.

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }
De eigenschappen voor schema-resources worden in de volgende tabel beschreven.
| De naam van element | Vereist | Description |
|:--|:--|:--|
| ingeschakeld       | Ja | Hiermee geeft u op of de waarschuwing is ingeschakeld wanneer deze wordt gemaakt. |
| interval      | Ja | Hoe vaak de query wordt uitgevoerd in minuten. |
| queryTimeSpan | Ja | De lengte van de tijd in minuten op voor het evalueren van de resultaten. |
De schema-resource moet afhankelijk van de opgeslagen zoekopdracht zodat deze voordat u de planning gemaakt.
> [!NOTE]
> Schemanaam moet uniek zijn in een bepaalde werkruimte. twee schema's kunnen niet dezelfde ID hebben, zelfs als ze gekoppeld aan verschillende opgeslagen zoekopdrachten zijn. Naam voor alle opgeslagen zoekacties, schema's en acties die zijn gemaakt met de Log Analytics-API moet ook in kleine letters.

### <a name="actions"></a>Acties
Een planning kan meerdere acties hebben. Een actie kan definiëren een of meer processen om uit te voeren, zoals een e-mail wordt verzonden of een runbook starten, of het kan een drempelwaarde waarmee wordt bepaald wanneer de resultaten van een zoekopdracht voldoen aan bepaalde criteria definiëren. Sommige acties worden beide definiëren, zodat de processen worden uitgevoerd wanneer de drempelwaarde wordt voldaan.
Acties kunnen worden gedefinieerd met behulp van [actiegroep] of actie bron.
> [!NOTE]
> Vanaf 14 mei 2018 worden is alle waarschuwingen in een openbare Azure-cloud-exemplaar van Log Analytics-werkruimte automatisch uitgebreid naar Azure. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure](../../azure-monitor/platform/alerts-extend.md). Voor gebruikers die waarschuwingen naar Azure uitbreiden, worden de acties nu beheerd in Azure-Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actiegroep - Azure Resource Manager-sjabloon](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).
Er zijn twee soorten actie resource die is opgegeven door de **Type** eigenschap. Een planning vereist een **waarschuwing** actie, waarin de details van de waarschuwingsregel en welke acties worden uitgevoerd wanneer een waarschuwing wordt gemaakt. Actie resources zijn een type `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.

Waarschuwingsacties hebben de volgende structuur. Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters.

```json
{
    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
    "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
    ],
    "properties": {
        "etag": "*",
        "type": "Alert",
        "name": "[variables('Alert').Name]",
        "description": "[variables('Alert').Description]",
        "severity": "[variables('Alert').Severity]",
        "threshold": {
            "operator": "[variables('Alert').Threshold.Operator]",
            "value": "[variables('Alert').Threshold.Value]",
            "metricsTrigger": {
                "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                "operator": "[variables('Alert').Trigger.Operator]",
                "value": "[variables('Alert').Trigger.Value]"
            },
        },
        "AzNsNotification": {
            "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
            "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]",
            "CustomWebhookPayload": "[variables('MyAlert').AzNsNotification.CustomWebhookPayload]"
        }
    }
}
```

De eigenschappen voor actie bij waarschuwing resources worden in de volgende tabellen beschreven.

| De naam van element | Vereist | Description |
|:--|:--|:--|
| Type | Ja | Het type van de actie.  Dit is **waarschuwing** voor waarschuwingsacties. |
| Name | Ja | Weergavenaam voor de waarschuwing.  Dit is de naam die wordt weergegeven in de console voor de waarschuwingsregel. |
| Description | Nee | Optionele beschrijving van de waarschuwing. |
| Severity | Ja | Ernst van de waarschuwing record uit de volgende waarden:<br><br> **critical**<br>**Waarschuwing**<br>**Informatief**


#### <a name="threshold"></a>Drempelwaarde
In deze sectie is vereist. Hiermee worden de eigenschappen van drempelwaarde voor waarschuwingen gedefinieerd.

| De naam van element | Vereist | Description |
|:--|:--|:--|
| Operator | Ja | Operator voor de vergelijking van de volgende waarden:<br><br>**gt = groter is dan<br>lt = minder dan** |
| Value | Ja | De waarde om de resultaten te vergelijken. |

##### <a name="metricstrigger"></a>MetricsTrigger
In deze sectie is optioneel. Op te nemen voor een waarschuwing voor een meting van metrische gegevens.

> [!NOTE]
> Waarschuwingen van de meting van metrische gegevens zijn momenteel in openbare preview.

| De naam van element | Vereist | Description |
|:--|:--|:--|
| TriggerCondition | Ja | Geeft aan of de drempelwaarde voor het totale aantal schendingen of achtereenvolgende schendingen van de volgende waarden:<br><br>**Totaal aantal<br>opeenvolgende** |
| Operator | Ja | Operator voor de vergelijking van de volgende waarden:<br><br>**gt = groter is dan<br>lt = minder dan** |
| Value | Ja | Het aantal keren dat de criteria voor het activeren van de waarschuwing moeten worden voldaan. |


#### <a name="throttling"></a>Beperking
In deze sectie is optioneel. Deze sectie bevatten als u onderdrukken van waarschuwingen van de dezelfde regel voor een bepaalde hoeveelheid tijd wilt nadat een waarschuwing is gemaakt.

| De naam van element | Vereist | Description |
|:--|:--|:--|
| DurationInMinutes | Ja als beperking element opgenomen | Het aantal minuten dat waarschuwingen onderdrukken nadat een van de dezelfde waarschuwingsregel is gemaakt. |

#### <a name="azure-action-group"></a>Azure actiegroep
Alle waarschuwingen in Azure, gebruik actiegroep als het standaardmechanisme voor het verwerken van acties. Met de actiegroep, kunt u uw acties één keer opgeven en koppel vervolgens de actie die u wilt meerdere waarschuwingen - binnen Azure. Zonder de noodzaak om te declareren dezelfde acties herhaaldelijk telkens opnieuw. Actiegroepen ondersteuning voor meerdere acties - inclusief e-mail, SMS, Spraakoproep, ITSM-verbinding, Automation-Runbook, Webhook URI en meer.

Voor gebruikers die hun waarschuwingen hebt uitgebreid naar Azure, hebt een planning nu actiegroep informatie doorgegeven, samen met de drempelwaarde, kunnen een waarschuwing wilt maken. Details van de e-mail, Webhook-URL's, Runbook-automatisering details en andere acties moeten worden gedefinieerd in naast een actiegroep eerst voordat het maken van een waarschuwing; een kunt maken [actiegroep van Azure Monitor](../../azure-monitor/platform/action-groups.md) in de Portal of gebruik [actiegroep - Resource-sjabloon](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

| De naam van element | Vereist | Description |
|:--|:--|:--|
| AzNsNotification | Ja | De resource-ID van de actiegroep Azure moet worden gekoppeld aan de waarschuwing voor de nodige maatregelen nemen wanneer waarschuwingscriteria wordt voldaan. |
| CustomEmailSubject | Nee | Aangepaste onderwerpregel van het e-mailbericht verzonden naar alle adressen die zijn opgegeven in actiegroep gekoppeld. |
| CustomWebhookPayload | Nee | Aangepaste nettolading wordt verzonden naar alle webhook eindpunten zijn gedefinieerd in de bijbehorende actiegroep. De indeling, is afhankelijk van wat de webhook wordt verwacht en moet een geldige geserialiseerde JSON. |

#### <a name="actions-for-oms-legacy"></a>Acties voor OMS (verouderd)

Elke planning heeft een **waarschuwing** actie. Hiermee definieert u de details van de waarschuwing en (optioneel) de melding en herstel. Een melding verzendt een e-mailbericht naar een of meer adressen. Een herstel wordt een runbook in Azure Automation om te proberen het gedetecteerde probleem herstellen gestart.

> [!NOTE]
> Vanaf 14 mei 2018 worden is alle waarschuwingen in een openbare Azure-cloud-exemplaar van Log Analytics-werkruimte automatisch uitgebreid naar Azure. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure](../../azure-monitor/platform/alerts-extend.md). Voor gebruikers die waarschuwingen naar Azure uitbreiden, worden de acties nu beheerd in Azure-Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actiegroep - Azure Resource Manager-sjabloon](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

##### <a name="emailnotification"></a>EmailNotification
 Deze sectie is optioneel als u de melding om e-mail te verzenden naar een of meer ontvangers wilt opnemen.

| De naam van element | Vereist | Description |
|:--|:--|:--|
| Geadresseerden | Ja | Door komma's gescheiden lijst met e-mailadressen te sturen wanneer een waarschuwing wordt gemaakt, zoals in het volgende voorbeeld.<br><br>**[ "recipient1@contoso.com", "recipient2@contoso.com" ]** |
| Onderwerp | Ja | Onderwerpregel van het e-mailbericht. |
| Bijlage | Nee | Bijlagen worden momenteel niet ondersteund. Als dit element opgenomen is, moeten deze **geen**. |

##### <a name="remediation"></a>Herstel
Deze sectie is optioneel als u een runbook te starten in reactie op de waarschuwing wilt opnemen. 

| De naam van element | Vereist | Description |
|:--|:--|:--|
| RunbookName | Ja | De naam van het runbook te starten. |
| WebhookUri | Ja | De URI van de webhook voor het runbook. |
| Vervaldatum | Nee | Datum en tijd waarop het herstel is verlopen. |

##### <a name="webhook-actions"></a>Webhookacties

Webhookacties start een proces doordat aanroepen van een URL en eventueel een nettolading wordt verzonden. Ze zijn vergelijkbaar met herstelacties tenzij ze zijn bedoeld voor webhooks die processen dan Azure Automation-runbooks kunnen aanroepen. Ze bieden ook de aanvullende mogelijkheid van het leveren van een payload moet worden geleverd aan het extern proces.

Als de waarschuwing een webhook wordt aangeroepen, dan deze een actie-resource met een type moet **Webhook** naast de **waarschuwing** actie resource.

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }
De eigenschappen voor Webhook actie resources worden in de volgende tabellen beschreven.
| De naam van element | Vereist | Description |
|:--|:--|:--|
| type | Ja | Het type van de actie. Dit is **Webhook** voor webhookacties. |
| naam | Ja | De weergavenaam voor de actie. Dit wordt niet weergegeven in de console. |
| webhookUri | Ja | URI voor de webhook. |
| customPayload | Nee | Aangepaste nettolading wordt verzonden naar de webhook. De indeling is afhankelijk van wat de webhook wordt verwacht. |

## <a name="sample"></a>Voorbeeld

Hieronder volgt een voorbeeld van een oplossing met de volgende bronnen:

- Opgeslagen zoekopdracht
- Planning
- Actiegroep

In het voorbeeld wordt [standaardoplossing parameters]( solutions-solution-file.md#parameters) variabelen die vaak worden gebruikt in een oplossing in plaats van hardcoderen waarden in de resourcedefinities.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "Description": "Name of Log Analytics workspace"
            }
        },
        "workspaceregionId": {
            "type": "string",
            "metadata": {
                "Description": "Region of Log Analytics workspace"
            }
        },
        "actiongroup": {
            "type": "string",
            "metadata": {
                "Description": "List of action groups for alert actions separated by semicolon"
            }
        }
    },
    "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
        "LogAnalyticsApiVersion-Search": "2017-03-15-preview",
        "LogAnalyticsApiVersion-Solution": "2015-11-01-preview",
        "MySearch": {
            "displayName": "Error records by hour",
            "query": "MyRecord_CL | summarize AggregatedValue = avg(Rating_d) by Instance_s, bin(TimeGenerated, 60m)",
            "category": "Samples",
            "name": "Samples-Count of data"
        },
        "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert. Fires when 3 error records found over hour interval.",
            "Severity": "critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
                "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
                "Interval": 15,
                "TimeSpan": 60
            },
            "MetricsTrigger": {
                "TriggerCondition": "Consecutive",
                "Operator": "gt",
                "Value": 3
            },
            "ThrottleMinutes": 60,
            "AzNsNotification": {
                "GroupIds": [
                    "[parameters('actiongroup')]"
                ],
                "CustomEmailSubject": "Sample alert"
            }
        }
    },
    "resources": [
        {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Solution')]",
            "dependsOn": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
            ],
            "properties": {
                "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
                "referencedResources": [
                ],
                "containedResources": [
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
                ]
            },
            "plan": {
                "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
                "Version": "[variables('SolutionVersion')]",
                "product": "[variables('ProductName')]",
                "publisher": "[variables('SolutionPublisher')]",
                "promotionCode": ""
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
                "etag": "*",
                "query": "[variables('MySearch').query]",
                "displayName": "[variables('MySearch').displayName]",
                "category": "[variables('MySearch').category]"
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
                "etag": "*",
                "interval": "[variables('MyAlert').Schedule.Interval]",
                "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
                "enabled": true
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
                "etag": "*",
                "Type": "Alert",
                "Name": "[variables('MyAlert').DisplayName]",
                "Description": "[variables('MyAlert').Description]",
                "Severity": "[variables('MyAlert').Severity]",
                "Threshold": {
                    "Operator": "[variables('MyAlert').ThresholdOperator]",
                    "Value": "[variables('MyAlert').ThresholdValue]",
                    "MetricsTrigger": {
                        "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                        "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                        "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                    }
                },
                "Throttling": {
                    "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
                },
                "AzNsNotification": {
                    "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
                    "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]"
                }
            }
        }
    ]
}
```

De volgende parameter-bestand bevat voorbeelden van waarden voor deze oplossing.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspacename": {
            "value": "myWorkspace"
        },
        "accountName": {
            "value": "myAccount"
        },
        "workspaceregionId": {
            "value": "East US"
        },
        "regionId": {
            "value": "East US 2"
        },
        "pricingTier": {
            "value": "Free"
        },
        "actiongroup": {
            "value": "/subscriptions/3b540246-808d-4331-99aa-917b808a9166/resourcegroups/myTestGroup/providers/microsoft.insights/actiongroups/sample"
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
* [Weergaven toevoegen](solutions-resources-views.md) aan uw oplossing.
* [Automation-runbooks en andere resources toevoegen](solutions-resources-automation.md) aan uw oplossing.
