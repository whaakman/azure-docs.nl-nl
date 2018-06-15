---
title: Opgeslagen zoekopdrachten en waarschuwingen in oplossingen | Microsoft Docs
description: Opgeslagen zoekopdrachten omvatten beheeroplossingen in Log Analytics om gegevens verzameld door de oplossing te analyseren.  Ze kunnen ook waarschuwingen om de gebruiker te waarschuwen definiëren of automatisch actie ondernemen als reactie op een kritiek probleem.  In dit artikel wordt beschreven hoe logboekanalyse opgeslagen zoekopdrachten en waarschuwingen in Resource Manager-sjabloon, zodat ze kunnen worden opgenomen in beheeroplossingen definiëren.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/16/2018
ms.author: bwren, vinagara
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b16c88b5ec45dec7bf0fe40da24e817ae325a3e
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
ms.locfileid: "33887914"
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-management-solution-preview"></a>Log Analytics toe te voegen opgeslagen zoekopdrachten en waarschuwingen met beheersysteem (Preview)

> [!NOTE]
> Dit is voorlopige documentatie voor het maken van oplossingen die zich momenteel in preview. De hieronder beschreven schema kan worden gewijzigd.   


[Oplossingen voor](monitoring-solutions.md) omvatten meestal [opgeslagen zoekopdrachten](../log-analytics/log-analytics-log-searches.md) in Log Analytics om gegevens verzameld door de oplossing te analyseren.  Ze kunnen ook definiëren [waarschuwingen](../log-analytics/log-analytics-alerts.md) aan de gebruiker waarschuwen of automatisch actie ondernemen als reactie op een kritiek probleem.  In dit artikel wordt beschreven hoe u voor het definiëren van logboekanalyse opgeslagen zoekopdrachten en waarschuwingen in een [Resource Management-sjabloon](../resource-manager-template-walkthrough.md) zodat ze kunnen worden opgenomen [beheeroplossingen](monitoring-solutions-creating.md).

> [!NOTE]
> De voorbeelden in dit artikel gebruiken parameters en variabelen die zijn vereist of gemeenschappelijke voor beheeroplossingen en wordt beschreven in [ontwerp en build een beheeroplossing in Azure](monitoring-solutions-creating.md)  

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al bekend met het bent [maken van een beheeroplossing](monitoring-solutions-creating.md) en de structuur van een [Resource Manager-sjabloon](../resource-group-authoring-templates.md) en oplossingsbestand.


## <a name="log-analytics-workspace"></a>Log Analytics-werkruimte
Alle resources in logboekanalyse zijn opgenomen in een [werkruimte](../log-analytics/log-analytics-manage-access.md).  Zoals beschreven in [werkruimte voor logboekanalyse en Automation-account](monitoring-solutions.md#log-analytics-workspace-and-automation-account), de werkruimte niet is opgenomen in de oplossing voor beheer, maar moet bestaan voordat de oplossing is geïnstalleerd.  Als deze niet beschikbaar is, mislukt de installatie van de oplossing.

De naam van de werkruimte wordt op de naam van elke resource logboekanalyse.  Dit doet u in de oplossing met de **werkruimte** parameter zoals in het volgende voorbeeld van een resource SavedSearch.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Log Analytics-API-versie
Alle logboekanalyse resources die zijn gedefinieerd in het Resource Manager-sjabloon hebben een eigenschap **apiVersion** waarmee wordt gedefinieerd met de versie van de API van de bron moet gebruiken.  Deze versie is verschillend voor resources die gebruikmaken van de [legacy en de bijgewerkte querytaal](../log-analytics/log-analytics-log-search-upgrade.md).  

 De volgende tabel geeft de Log Analytics-API-versies voor opgeslagen zoekopdrachten in eerdere versies en bijgewerkte werkruimten: 

| Werkruimteversie | API-versie | Query’s uitvoeren |
|:---|:---|:---|
| V1 (verouderd)   | 2015-11-01-preview | Verouderde indeling.<br> Bijvoorbeeld: Typ = gebeurtenis EventLevelName fout =  |
| v2 (upgrade) | 2015-11-01-preview | Verouderde indeling.  Geconverteerd naar bijgewerkte indeling op installeren.<br> Bijvoorbeeld: Typ = gebeurtenis EventLevelName fout =<br>Geconverteerd naar: gebeurtenis &#124; waar EventLevelName == "Error"  |
| v2 (upgrade) | 2017-03-03-preview | De indeling van de upgrade. <br>Voorbeeld: Gebeurtenis &#124; waar EventLevelName == "Error"  |



## <a name="saved-searches"></a>Opgeslagen zoekopdrachten
Omvatten [opgeslagen zoekopdrachten](../log-analytics/log-analytics-log-searches.md) in een oplossing waarmee gebruikers kunnen gegevens opvragen die is verzameld door uw oplossing.  Opgeslagen zoekopdrachten worden weergegeven onder **Favorieten** in de OMS-portal en **opgeslagen zoekacties** in de Azure portal.  Een opgeslagen zoekopdracht is ook vereist voor elke waarschuwing.   

[Log Analytics opgeslagen zoekopdracht](../log-analytics/log-analytics-log-searches.md) resources zijn een type `Microsoft.OperationalInsights/workspaces/savedSearches` en hebben de volgende structuur.  Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters. 

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

| Eigenschap | Beschrijving |
|:--- |:--- |
| category | De categorie voor de opgeslagen zoekopdracht.  Opgeslagen zoekopdrachten in dezelfde oplossing wordt vaak één categorie delen, zodat ze samen worden gegroepeerd in de console. |
| weergavenaam | De naam moet worden weergegeven voor de opgeslagen zoekopdracht in de portal. |
| query | De query wilt uitvoeren. |

> [!NOTE]
> U moet wellicht escape-tekens gebruiken in de query bevat tekens die kunnen worden geïnterpreteerd als JSON.  Bijvoorbeeld, als de query is **Type: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write '**, deze worden geschreven in het oplossingsbestand als **Type: AzureActivity OperationName:\" Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Waarschuwingen
[Meld u Analytics waarschuwingen](../log-analytics/log-analytics-alerts.md) zijn gemaakt door regels voor waarschuwingen die een opgeslagen zoekopdracht op een vast interval uitvoert.  Als de resultaten van de query overeenkomen met criteria opgegeven, wordt een waarschuwing record gemaakt en een of meer acties worden uitgevoerd.  

> [!NOTE]
> Alle waarschuwingen in een werkruimte, dat is vanaf 14 mei 2018, wordt automatisch gestart om uit te breiden naar Azure. Een gebruiker kunt uitbreiden waarschuwingen naar Azure vrijwillig voordat 14 mei 2018 initiëren. Zie voor meer informatie [waarschuwingen uitbreiden naar Azure van OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Acties zijn voor gebruikers die waarschuwingen naar Azure uitbreiden, nu beheerd met Azure Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actie groep - Azure Resource Manager-sjabloon](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).

Waarschuwingsregels in een beheersysteem bestaan uit de volgende drie verschillende bronnen.

- **Opgeslagen zoekopdracht.**  Hiermee definieert u het logboek zoeken die wordt uitgevoerd.  Meerdere regels voor waarschuwingen kunnen een enkele opgeslagen zoekopdracht delen.
- **Plannen.**  Hiermee definieert u hoe vaak de logboek-zoekopdracht wordt uitgevoerd.  Elke waarschuwingsregel heeft slechts één schema.
- **Actie van de waarschuwing.**  Elke waarschuwingsregel heeft een actie groepsbron of actie resource (verouderde) met een type van **waarschuwing** die de details van de waarschuwing, zoals de criteria voor wanneer een record voor een waarschuwing wordt gemaakt en de ernst van de waarschuwing definieert. [Actiegroep](../monitoring-and-diagnostics/monitoring-action-groups.md) bron kan ook een lijst met geconfigureerde acties die worden uitgevoerd wanneer de waarschuwing wordt geactiveerd - zoals telefoongesprek, SMS, e-mail, webhook, ITSM hulpprogramma, automation-runbook, logische app, enzovoort.
 
De actie-resource (verouderde) definieert eventueel een e-mail en runbook-antwoord.
- **Webhook actie (verouderd).**  Als de waarschuwingsregel een webhook aanroepen, wordt er een actie van extra resource met een type vereist **Webhook**.    

Opgeslagen zoekopdracht resources hierboven zijn beschreven.  De andere resources worden hieronder beschreven.


### <a name="schedule-resource"></a>Schema-resource

Een opgeslagen zoekopdracht kan een of meer schema's met het schema voor een afzonderlijke waarschuwingsregel hebben. Het schema wordt gedefinieerd hoe vaak de zoekopdracht is uitgevoerd en de tijdsinterval waarover de gegevens worden opgehaald.  Planning resources zijn een type `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` en hebben de volgende structuur. Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters. 


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



De eigenschappen voor resources plannen worden in de volgende tabel beschreven.

| Elementnaam | Vereist | Beschrijving |
|:--|:--|:--|
| ingeschakeld       | Ja | Hiermee geeft u op of de waarschuwing is ingeschakeld wanneer deze wordt gemaakt. |
| interval      | Ja | Hoe vaak de query wordt uitgevoerd in minuten. |
| queryTimeSpan | Ja | De lengte van de tijd in minuten gedurende welke resultaten evalueren. |

De schema-bron moet afhankelijk van de opgeslagen zoekopdracht zodat deze voordat u de planning gemaakt.

> [!NOTE]
> De schemanaam van het moet uniek zijn in een bepaalde werkruimte; twee planningen kunnen niet dezelfde ID hebben, zelfs als ze gekoppeld aan verschillende opgeslagen zoekopdrachten zijn. Naam voor alle opgeslagen zoekopdrachten, schema's en acties die zijn gemaakt met de Log Analytics-API moet ook in kleine letters.


### <a name="actions"></a>Acties
Een planning kan meerdere acties hebben. Een actie kan definiëren een of meer processen uit te voeren zoals het verzenden van een e-mail of een runbook starten, of het kan een drempelwaarde die bepaalt wanneer de resultaten van een zoekopdracht voldoen aan bepaalde criteria definiëren.  Sommige acties definiëren beide zodat de processen worden uitgevoerd wanneer de drempelwaarde wordt voldaan.

Acties kunnen worden gedefinieerd met behulp van de [actiegroep] of actie bron.

> [!NOTE]
> Alle waarschuwingen in een werkruimte, dat is vanaf 14 mei 2018, wordt automatisch gestart om uit te breiden naar Azure. Een gebruiker kunt uitbreiden waarschuwingen naar Azure vrijwillig voordat 14 mei 2018 initiëren. Zie voor meer informatie [waarschuwingen uitbreiden naar Azure van OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Acties zijn voor gebruikers die waarschuwingen naar Azure uitbreiden, nu beheerd met Azure Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actie groep - Azure Resource Manager-sjabloon](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).


Er zijn twee soorten actie resource die is opgegeven door de **Type** eigenschap.  Een planning vereist één **waarschuwing** actie waarin de details van de waarschuwingsregel en welke acties worden uitgevoerd wanneer een waarschuwing wordt gemaakt. Actie resources zijn een type `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.  

Waarschuwing acties hebben de volgende structuur.  Dit omvat de algemene variabelen en parameters zodat u kunt kopiëren en plak dit codefragment in uw oplossingsbestand en wijzig de namen van parameters. 


```
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

De eigenschappen voor de actie waarschuwing resources worden in de volgende tabellen beschreven.

| Elementnaam | Vereist | Beschrijving |
|:--|:--|:--|
| Type | Ja | Type van de actie.  Dit is **waarschuwing** voor meldingen. |
| Naam | Ja | Weergavenaam voor de waarschuwing.  Dit is de naam die wordt weergegeven in de console voor de waarschuwingsregel. |
| Beschrijving | Nee | Optionele beschrijving van de waarschuwing. |
| Ernst | Ja | Ernst van de waarschuwing record uit de volgende waarden:<br><br> **Kritieke**<br>**Waarschuwing**<br>**Informatief**


#### <a name="threshold"></a>Drempelwaarde
Deze sectie is vereist.  Hiermee definieert u de eigenschappen voor de drempelwaarde voor waarschuwingen.

| Elementnaam | Vereist | Beschrijving |
|:--|:--|:--|
| Operator | Ja | De operator voor de vergelijking van de volgende waarden:<br><br>**gt = groter is dan<br>lt = kleiner dan** |
| Waarde | Ja | De waarde om de resultaten te vergelijken. |

##### <a name="metricstrigger"></a>MetricsTrigger
Deze sectie is optioneel.  Op te nemen voor een waarschuwing metrische meting.

> [!NOTE]
> Metrische maateenheids waarschuwingen zijn momenteel in de openbare preview. 

| Elementnaam | Vereist | Beschrijving |
|:--|:--|:--|
| TriggerCondition | Ja | Hiermee bepaalt u of de drempelwaarde voor het totale aantal schendingen of opeenvolgende schendingen van de volgende waarden:<br><br>**Totaal aantal<br>opeenvolgende** |
| Operator | Ja | De operator voor de vergelijking van de volgende waarden:<br><br>**gt = groter is dan<br>lt = kleiner dan** |
| Waarde | Ja | Nummer van de tijdstippen die de criteria voor het activeren van de waarschuwing moeten worden voldaan. |


#### <a name="throttling"></a>Beperking
Deze sectie is optioneel.  Deze sectie bevatten als u onderdrukken van waarschuwingen van dezelfde regel voor een bepaalde hoeveelheid tijd wilt nadat een waarschuwing is gemaakt.

| Elementnaam | Vereist | Beschrijving |
|:--|:--|:--|
| DurationInMinutes | Ja als u beperking van element opgenomen | Aantal minuten waarschuwingen onderdrukt nadat een van de dezelfde waarschuwingsregel is gemaakt. |


#### <a name="azure-action-group"></a>Azure actiegroep
Alle waarschuwingen in Azure, gebruik actie groep als het standaardmechanisme voor het verwerken van acties. U kunt uw acties één keer opgeven en vervolgens de actiegroep meerdere waarschuwingen - koppelen in Azure met actie-groep. Zonder de noodzaak om te declareren dezelfde acties herhaaldelijk steeds opnieuw. Actie beheergroepen ondersteunen meerdere acties - zoals e-mail, SMS, telefoongesprek, ITSM verbinding, Automation-Runbook, Webhook URI en meer. 

Een planning hebt voor gebruikers die hun waarschuwingen hebt uitgebreid naar Azure - nu actie groepsgegevens doorgegeven samen met de drempelwaarde, te kunnen maken van een waarschuwing. E-details, Webhook-URL's, Runbook-automatisering details en andere acties moeten worden gedefinieerd aan de kant van een actie groep eerst maken voordat een waarschuwing; een kunt maken [groep acties van de Azure-Monitor](../monitoring-and-diagnostics/monitoring-action-groups.md) in de Portal of gebruik [actie groep - Resource sjabloon](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).

| Elementnaam | Vereist | Beschrijving |
|:--|:--|:--|
| AzNsNotification | Ja | De resource-ID van de actiegroep Azure moet worden gekoppeld aan de waarschuwing voor de nodige maatregelen nemen wanneer waarschuwing criteria wordt voldaan. |
| CustomEmailSubject | Nee | Aangepaste onderwerpregel van het e-mail verzonden naar alle adressen in de groep van de bijbehorende actie is opgegeven. |
| CustomWebhookPayload | Nee | Aangepaste nettolading worden verzonden naar alle webhook eindpunten zijn gedefinieerd in de bijbehorende actiegroep. De indeling is afhankelijk van de webhook verwacht en moet een geldige geserialiseerde JSON. |


#### <a name="actions-for-oms-legacy"></a>Acties voor OMS (verouderd)

Elke planning heeft een **waarschuwing** in te grijpen.  Hiermee definieert u de details van de waarschuwing en eventueel acties melding en herstel.  Een melding verzendt een e-mailbericht naar een of meer adressen.  Een herstelbewerking wordt gestart van een runbook in Azure Automation om te proberen het gedetecteerde probleem oplossen.

> [!NOTE]
> Alle waarschuwingen in een werkruimte, dat is vanaf 14 mei 2018, wordt automatisch gestart om uit te breiden naar Azure. Een gebruiker kunt uitbreiden waarschuwingen naar Azure vrijwillig voordat 14 mei 2018 initiëren. Zie voor meer informatie [waarschuwingen uitbreiden naar Azure van OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Acties zijn voor gebruikers die waarschuwingen naar Azure uitbreiden, nu beheerd met Azure Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actie groep - Azure Resource Manager-sjabloon](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).

##### <a name="emailnotification"></a>EmailNotification
 Deze sectie is optioneel als u de waarschuwing e-mail te verzenden naar een of meer geadresseerden wilt opnemen.

| Elementnaam | Vereist | Beschrijving |
|:--|:--|:--|
| Geadresseerden | Ja | Door komma's gescheiden lijst met e-mailadressen melding verzenden wanneer een waarschuwing wordt gemaakt, zoals in het volgende voorbeeld.<br><br>**[ "recipient1@contoso.com", "recipient2@contoso.com" ]** |
| Onderwerp | Ja | Onderwerpregel van het e-mailbericht. |
| Bijlage | Nee | Bijlagen worden momenteel niet ondersteund.  Als dit element is opgenomen, worden deze **geen**. |


##### <a name="remediation"></a>Herstel
Deze sectie is optioneel als u een runbook te starten in reactie op de waarschuwing wilt opnemen. |

| Elementnaam | Vereist | Beschrijving |
|:--|:--|:--|
| RunbookName | Ja | De naam van het runbook te starten. |
| WebhookUri | Ja | De URI van de webhook voor het runbook. |
| Vervaldatum | Nee | Datum en tijd waarop het herstel is verlopen. |

##### <a name="webhook-actions"></a>Webhookacties

Een proces starten webhookacties door het aanroepen van een URL en het eventueel geven een nettolading worden verzonden. Ze zijn vergelijkbaar met herstelacties maar ze zijn bedoeld voor webhooks die van processen dan Azure Automation-runbooks gebruikmaken mogelijk. Ze bieden ook de aanvullende mogelijkheid bieden een nettolading moet worden geleverd aan het externe proces.

Als de waarschuwing wordt een webhook aanroepen, dan deze een actie-resource met een type moet **Webhook** naast de **waarschuwing** actie resource.  

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

De eigenschappen voor Webhook actie bronnen worden in de volgende tabellen beschreven.

| Elementnaam | Vereist | Beschrijving |
|:--|:--|:--|
| type | Ja | Type van de actie.  Dit is **Webhook** voor webhookacties. |
| naam | Ja | Weergavenaam voor de actie.  Dit wordt niet weergegeven in de console. |
| wehookUri | Ja | De URI voor de webhook. |
| CustomPayload | Nee | Aangepaste nettolading worden verzonden naar de webhook. De indeling is afhankelijk van wat de webhook wordt verwacht. |


## <a name="sample"></a>Voorbeeld

Hier volgt een voorbeeld van een oplossing die met de volgende bronnen:

- Opgeslagen zoekopdracht
- Planning
- Actiegroep

In dit voorbeeld worden [standaardoplossing parameters]( monitoring-solutions-solution-file.md#parameters) variabelen die meestal in een oplossing in plaats van hardcoderen waarden in de resourcedefinities gebruikt wordt.

```
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
    
          "LogAnalyticsApiVersion": "2015-03-20",
    
          "MySearch": {
            "displayName": "Error records by hour",
            "query": "Type=MyRecord_CL | measure avg(Rating_d) by Instance_s interval 60minutes",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "Critical",
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
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
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
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
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
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
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
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
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
```
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
* [Weergaven toevoegen](monitoring-solutions-resources-views.md) bij uw systeem.
* [Automation-runbooks en andere resources toevoegen](monitoring-solutions-resources-automation.md) bij uw systeem.

