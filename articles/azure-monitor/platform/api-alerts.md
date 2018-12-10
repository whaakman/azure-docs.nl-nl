---
title: Met behulp van Log Analytics Alert REST API
description: De Log Analytics Alert REST API kunt u waarschuwingen in Log Analytics die deel uitmaakt van Log Analytics maken en beheren.  Dit artikel bevat informatie over de API en enkele voorbeelden voor het uitvoeren van verschillende bewerkingen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/10/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 0176cc5688f7210d5e444b094b360bb1e7df1e7c
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53136403"
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>Maken en beheren van regels voor waarschuwingen in Log Analytics met REST-API
De Log Analytics Alert REST API kunt u waarschuwingen in Log Analytics maken en beheren.  Dit artikel bevat informatie over de API en enkele voorbeelden voor het uitvoeren van verschillende bewerkingen.

De Log Analytics Search REST-API is RESTful en zijn toegankelijk via de Azure Resource Manager REST API. In dit document vindt u voorbeelden waarin de API is toegankelijk vanuit een PowerShell-opdrachtregel met [ARMClient](https://github.com/projectkudu/ARMClient), een open-source-opdrachtregelprogramma dat vereenvoudigt het aanroepen van de Azure Resource Manager-API. Het gebruik van ARMClient en PowerShell is een van de vele opties voor toegang tot de Log Analytics Search-API. Met deze hulpprogramma's, kunt u gebruikmaken van de RESTful API van Azure Resource Manager om aanroepen naar Log Analytics-werkruimten en zoeken in opdrachten in deze uitvoeren. De API uitvoer zoekresultaten aan u in JSON-indeling, zodat u kunt de lijst met zoekresultaten via een programma op veel verschillende manieren gebruiken.

## <a name="prerequisites"></a>Vereisten
Waarschuwingen kunnen op dit moment alleen worden gemaakt met een opgeslagen zoekopdracht in Log Analytics.  U kunt verwijzen naar de [Log Search REST-API](../../azure-monitor/log-query/log-query-overview.md) voor meer informatie.

## <a name="schedules"></a>Planningen
Een opgeslagen zoekopdracht kan een of meer schema's hebben. Het schema wordt gedefinieerd hoe vaak de zoekopdracht wordt uitgevoerd en het tijdsinterval op waarover de criteria wordt geïdentificeerd.
Schema's hebben de eigenschappen in de volgende tabel.

| Eigenschap | Description |
|:--- |:--- |
| Interval |Hoe vaak de zoekopdracht wordt uitgevoerd. Gemeten in minuten. |
| QueryTimeSpan |Het tijdsinterval op waarover de criteria wordt geëvalueerd. Moet gelijk zijn aan of groter zijn dan het Interval. Gemeten in minuten. |
| Versie |De API-versie die wordt gebruikt.  Op dit moment moet dit altijd worden ingesteld op 1. |

Bijvoorbeeld, kunt u een gebeurtenisquery met een Interval van 15 minuten en een periode van 30 minuten. In dit geval wordt de query wordt uitgevoerd om de 15 minuten, en een waarschuwing wordt geactiveerd als de criteria verder worden omgezet naar de waarde true via een reeks van 30 minuten.

### <a name="retrieving-schedules"></a>Bij het ophalen van schema 's
De Get-methode gebruiken om op te halen van alle schema's voor een opgeslagen zoekactie.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Gebruik de Get-methode met de ID van een schema om op te halen van een bepaalde planning voor een opgeslagen zoekactie.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Hieronder volgt een voorbeeldantwoord voor een schema.

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a>Het maken van een planning
Gebruik de Put-methode met de ID van een uniek schema een nieuw schema maken.  Houd er rekening mee dat twee schema's kunnen niet dezelfde ID, zelfs als ze gekoppeld aan verschillende zijn opgeslagen zoekopdrachten.  Wanneer u een planning in de Log Analytics-console maakt, wordt een GUID gemaakt voor de schema-ID.

> [!NOTE]
> De naam voor alle opgeslagen zoekacties, schema's en acties die zijn gemaakt met de Log Analytics-API moet in kleine letters.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Een schema bewerken
Gebruik de Put-methode met de ID van een bestaande planning voor de dezelfde opgeslagen zoekopdracht om te wijzigen die planning.  De hoofdtekst van de aanvraag moet de etag van het schema bevatten.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Schema's verwijderen
Gebruik de Delete-methode met de ID van een planning om te verwijderen van een schema.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Acties
Een planning kan meerdere acties hebben. Een actie kan definiëren een of meer processen om uit te voeren, zoals een e-mail wordt verzonden of een runbook starten, of het kan een drempelwaarde waarmee wordt bepaald wanneer de resultaten van een zoekopdracht voldoen aan bepaalde criteria definiëren.  Sommige acties worden beide definiëren, zodat de processen worden uitgevoerd wanneer de drempelwaarde wordt voldaan.

Alle acties hebben de eigenschappen in de volgende tabel.  Verschillende typen waarschuwingen hebben verschillende extra eigenschappen, worden hieronder beschreven.

| Eigenschap | Description |
|:--- |:--- |
| Type |Het type van de actie.  De mogelijke waarden zijn momenteel waarschuwings- en Webhook. |
| Name |Weergavenaam voor de waarschuwing. |
| Versie |De API-versie die wordt gebruikt.  Op dit moment moet dit altijd worden ingesteld op 1. |

### <a name="retrieving-actions"></a>Bij het ophalen van acties

> [!NOTE]
> Vanaf 14 mei 2018 worden wordt alle waarschuwingen in een openbare Azure-cloud-exemplaar van Log Analytics-werkruimte automatisch uitgebreid naar Azure. Een gebruiker kunt vrijwillig uitbreiding van waarschuwingen naar Azure initiëren vóór 14 mei 2018. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure Log Analytics](../../monitoring-and-diagnostics/monitoring-alerts-extend.md). Voor gebruikers die waarschuwingen naar Azure uitbreiden, worden de acties nu beheerd in Azure-Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actie groep API](https://docs.microsoft.com/rest/api/monitor/actiongroups).

De Get-methode gebruiken om op te halen van alle acties voor een schema.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Gebruik de Get-methode met de actie-ID ophalen van een bepaalde actie voor een schema.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Het maken of bewerken van acties
De Put-methode gebruiken met een actie-ID die uniek is voor de planning te maken van een nieuwe actie.  Wanneer u een actie in de Log Analytics-console maakt, wordt een GUID wordt voor de actie-ID.

> [!NOTE]
> De naam voor alle opgeslagen zoekacties, schema's en acties die zijn gemaakt met de Log Analytics-API moet in kleine letters.

Gebruik de Put-methode met een bestaande actie-ID voor de dezelfde opgeslagen zoekopdracht om te wijzigen die planning.  De hoofdtekst van de aanvraag moet de etag van het schema bevatten.

De indeling van de aanvraag voor het maken van een nieuwe actie verschilt per actietype, zodat deze voorbeelden u in de volgende secties vindt.

### <a name="deleting-actions"></a>Acties verwijderen

> [!NOTE]
> Vanaf 14 mei 2018 worden wordt alle waarschuwingen in een openbare Azure-cloud-exemplaar van Log Analytics-werkruimte automatisch uitgebreid naar Azure. Een gebruiker kunt vrijwillig uitbreiding van waarschuwingen naar Azure initiëren vóór 14 mei 2018. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure Log Analytics](../../monitoring-and-diagnostics/monitoring-alerts-extend.md). Voor gebruikers die waarschuwingen naar Azure uitbreiden, worden de acties nu beheerd in Azure-Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actie groep API](https://docs.microsoft.com/rest/api/monitor/actiongroups).

Gebruik de Delete-methode met de actie-ID om te verwijderen van een actie.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Waarschuwingsacties
Een planning moet slechts één actie bij waarschuwing hebben.  Waarschuwingsacties zijn een of meer van de secties in de volgende tabel.  Elk wordt nader besproken die hieronder beschreven.

| Sectie | Description | Gebruik |
|:--- |:--- |:--- |
| Drempelwaarde |Criteria voor wanneer de actie wordt uitgevoerd.| Vereist voor elke waarschuwing voordat of nadat ze zijn uitgebreid naar Azure. |
| Severity |Het label dat wordt gebruikt om te classificeren waarschuwing wanneer ze worden geactiveerd.| Vereist voor elke waarschuwing voordat of nadat ze zijn uitgebreid naar Azure. |
| Onderdrukken |Optie voor het stoppen van meldingen van de waarschuwing. | Optioneel voor elke waarschuwing voordat of nadat ze zijn uitgebreid naar Azure. |
| Actiegroepen |Id's van Azure ActionGroup waar de acties die vereist zijn opgegeven, zoals - e-mailberichten, SMSs, gesprekken, Webhooks, Automation-Runbooks, ITSM-Connectors, enzovoort.| Vereist wanneer waarschuwingen worden uitgebreid naar Azure|
| Acties aanpassen|De standaarduitvoer voor select acties op basis van ActionGroup wijzigen| Optioneel voor elke waarschuwing kan worden gebruikt nadat er waarschuwingen zijn uitgebreid naar Azure. |
| EmailNotification |E-mail verzenden naar meerdere ontvangers. | Niet vereist als de waarschuwingen worden uitgebreid naar Azure|
| Herstel |Een runbook te starten in Azure Automation om te proberen geïdentificeerde probleem te corrigeren. |Niet vereist als de waarschuwingen worden uitgebreid naar Azure|
| Webhookacties | Gegevens van waarschuwingen, pushen naar gewenste service als JSON |Niet vereist als de waarschuwingen worden uitgebreid naar Azure|

> [!NOTE]
> Vanaf 14 mei 2018 worden wordt alle waarschuwingen in een openbare Azure-cloud-exemplaar van Log Analytics-werkruimte automatisch uitgebreid naar Azure. Een gebruiker kunt vrijwillig uitbreiding van waarschuwingen naar Azure initiëren vóór 14 mei 2018. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure Log Analytics](../../monitoring-and-diagnostics/monitoring-alerts-extend.md).

#### <a name="thresholds"></a>Drempelwaarden
Een actie bij waarschuwing mag slechts één drempelwaarde hebben.  Als de resultaten van een opgeslagen zoekopdracht overeenkomen met de drempelwaarde in een actie die is gekoppeld aan die zoekopdracht, worden alle andere processen in die actie uitgevoerd.  Een actie kan ook alleen een drempelwaarde bevatten, zodat deze kan worden gebruikt met de acties die van andere typen die geen drempelwaarden bevatten.

Drempelwaarden hebben de eigenschappen in de volgende tabel.

| Eigenschap | Description |
|:--- |:--- |
| Operator |Operator voor de voor drempelwaardevergelijking. <br> gt = groter dan <br> lt = minder dan |
| Waarde |De waarde voor de drempelwaarde. |

Bijvoorbeeld, kunt u een gebeurtenisquery met een Interval van 15 minuten, een periode van 30 minuten en een drempelwaarde van meer dan 10. In dit geval wordt de query wordt uitgevoerd om de 15 minuten, en een waarschuwing wordt geactiveerd als het 10 gebeurtenissen die zijn gemaakt via een reeks van 30 minuten geretourneerd.

Hieronder volgt een voorbeeldantwoord voor een actie met een drempelwaarde overschrijdt.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

De Put-methode gebruiken met een unieke actie-ID te maken van een nieuwe drempelwaarde-actie voor een schema.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Gebruik de Put-methode met de ID van een bestaande actie om een actie van de drempelwaarde voor een schema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="severity"></a>Severity
Log Analytics kunt u uw waarschuwingen te classificeren in categorieën, waarmee eenvoudiger beheer en sorteren. De ernst van de waarschuwing die gedefinieerd is: informatief, waarschuwing en kritiek. Deze worden toegewezen aan de ernst van genormaliseerde schaal van Azure-waarschuwingen als:

|Ernstniveau van log Analytics  |Ernstniveau van waarschuwingen van Azure  |
|---------|---------|
|kritiek |Sev 0|
|waarschuwing |Sev 1|
|Informatief | Sev 2|

Hieronder volgt een voorbeeldantwoord voor een actie met alleen een drempelwaarde en ernst. 

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Severity": "critical",
        "Version": 1    }

Gebruik de Put-methode met een unieke actie-ID om een nieuwe actie voor een schema met ernst.  

    $thresholdWithSevJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdWithSevJson

Gebruik de Put-methode met de ID van een bestaande actie om een actie van de ernst voor een schema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $thresholdWithSevJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdWithSevJson

#### <a name="suppress"></a>Onderdrukken
Log Analytics op basis van query waarschuwingen geactiveerd telkens als drempelwaarde is bereikt of overschreden. Op basis van de logica in de query de impliciete, dit kan leiden tot ophalen voor een reeks intervallen geactiveerde waarschuwing en kan daarom meldingen ook verstuurd voortdurend. Om te voorkomen dat dergelijke scenario, kunt een gebruiker onderdrukken optie Log Analytics dat moet worden gewacht op een bepaald hoeveelheid tijd voordat de tweede keer voor de waarschuwingsregel wordt geactiveerd door melding instellen. Dus als onderdrukken is ingesteld voor 30 minuten. waarschuwing wordt vervolgens de eerste keer wordt gestart en verzenden van meldingen geconfigureerd. Maar wacht 30 minuten voordat de melding voor de waarschuwingsregel wordt opnieuw gebruikt. Waarschuwingsregel wordt nog uitgevoerd in de tussenperiode - alleen melding door Log Analytics is onderdrukt voor de opgegeven periode, ongeacht hoe vaak de waarschuwingsregel geactiveerd in deze periode.

Onderdrukken van eigenschap van Log Analytics waarschuwingsregel is opgegeven met behulp van de *beperking* waarde en de onderdrukking periode via *DurationInMinutes* waarde.

Volgende is een voorbeeldantwoord voor een actie met alleen een drempelwaarde, ernst en de eigenschap onderdrukken

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Throttling": {
          "DurationInMinutes": 30
        },
        "Severity": "critical",
        "Version": 1    }

Gebruik de Put-methode met een unieke actie-ID om een nieuwe actie voor een schema met ernst.  

    $AlertSuppressJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Throttling': { 'DurationInMinutes': 30 },'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myalert?api-version=2015-03-20 $AlertSuppressJson

Gebruik de Put-methode met de ID van een bestaande actie om een actie van de ernst voor een schema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $AlertSuppressJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Throttling': { 'DurationInMinutes': 30 },'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myalert?api-version=2015-03-20 $AlertSuppressJson

#### <a name="action-groups"></a>Actiegroepen
Alle waarschuwingen in Azure, gebruik actiegroep als het standaardmechanisme voor het verwerken van acties. Met de actiegroep, kunt u uw acties één keer opgeven en koppel vervolgens de actie die u wilt meerdere waarschuwingen - binnen Azure. Zonder de noodzaak om te declareren dezelfde acties herhaaldelijk telkens opnieuw. Actiegroepen ondersteuning voor meerdere acties - inclusief e-mail, SMS, Spraakoproep, ITSM-verbinding, Automation-Runbook, Webhook URI en meer. 

Voor gebruikers die hun waarschuwingen hebt uitgebreid naar Azure, hebt een planning nu actiegroep informatie doorgegeven, samen met de drempelwaarde, kunnen een waarschuwing wilt maken. Details van de e-mail, Webhook-URL's, Runbook-automatisering details en andere acties moeten worden gedefinieerd in naast een actiegroep eerst voordat het maken van een waarschuwing; een kunt maken [actiegroep van Azure Monitor](../../monitoring-and-diagnostics/monitoring-action-groups.md) in de Portal of gebruik [actie groep API](https://docs.microsoft.com/rest/api/monitor/actiongroups).

Koppeling van de actiegroep toevoegen aan een waarschuwing, geef de unieke ID voor Azure Resource Manager van de actiegroep in het definitie van de waarschuwing. Hieronder vindt u een voorbeeld van afbeelding:

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a descriptio here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ]
        },
        "Severity": "critical",
        "Version": 1
      },

De Put-methode gebruiken met een unieke actie-ID te koppelen van bestaande actiegroep voor een schema.  Hier volgt een voorbeeld van afbeelding van het gebruik van.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Gebruik de Put-methode met de ID van een bestaande actie om een actiegroep die is gekoppeld voor een schema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

#### <a name="customize-actions"></a>Acties aanpassen
Door standaardacties, volgt u de standaardsjabloon en de indeling voor meldingen. Maar gebruiker bepaalde acties kunt aanpassen, zelfs als ze worden beheerd door Actiegroepen. Op dit moment is aanpassing mogelijk dat het onderwerp van E-mail en Webhook-nettolading.

##### <a name="customize-e-mail-subject-for-action-group"></a>E-mailonderwerp voor actiegroep aanpassen
Het e-mailonderwerp voor waarschuwingen is standaard: waarschuwingsmelding <AlertName> voor <WorkspaceName>. Maar dit kan worden aangepast, zodat u kunt specifieke woorden of tags - kunt u eenvoudig gebruikmaken van filterregels in uw postvak in. De informatie aanpassen per e-mail verzenden-header moeten verzenden, samen met ActionGroup-details conform onderstaand voorbeeld.

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a descriptio here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ],
          "CustomEmailSubject": "Azure Alert fired"
        },
        "Severity": "critical",
        "Version": 1
      },

De Put-methode gebruiken met een unieke actie-ID al bestaande actiegroep koppelen aan een schema aanpassen.  Hier volgt een voorbeeld van afbeelding van het gebruik van.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'], 'CustomEmailSubject': 'Azure Alert fired'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Gebruik de Put-methode met de ID van een bestaande actie om een actiegroep die is gekoppeld voor een schema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']}, 'CustomEmailSubject': 'Azure Alert fired' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

##### <a name="customize-webhook-payload-for-action-group"></a>Webhookpayload aanpassen voor de actiegroep
De webhook is verzonden via actiegroep voor log analytics heeft standaard een vaste structuur. Maar een de JSON-nettolading kunt aanpassen met behulp van specifieke variabelen die worden ondersteund, om te voldoen aan de vereisten van de webhook-eindpunt. Zie voor meer informatie, [webhookactie voor waarschuwingsregels](../../monitoring-and-diagnostics/monitor-alerts-unified-log-webhook.md). 

De details van de webhook aanpassen hoeven in te verzenden, samen met details van ActionGroup en worden toegepast op alle Webhook URI opgegeven binnen de actiegroep; conform onderstaand voorbeeld.

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a descriptio here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ],
          "CustomWebhookPayload": "{\"field1\":\"value1\",\"field2\":\"value2\"}",
          "CustomEmailSubject": "Azure Alert fired"
        },
        "Severity": "critical",
        "Version": 1
      },

De Put-methode gebruiken met een unieke actie-ID al bestaande actiegroep koppelen aan een schema aanpassen.  Hier volgt een voorbeeld van afbeelding van het gebruik van.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'], 'CustomEmailSubject': 'Azure Alert fired','CustomWebhookPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Gebruik de Put-methode met de ID van een bestaande actie om een actiegroep die is gekoppeld voor een schema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']}, 'CustomEmailSubject': 'Azure Alert fired','CustomWebhookPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

#### <a name="email-notification"></a>E-mailmelding
E-mailmeldingen verzenden e-mail naar een of meer ontvangers.  Ze bevatten de eigenschappen in de volgende tabel.

> [!NOTE]
> Vanaf 14 mei 2018 worden wordt alle waarschuwingen in een openbare Azure-cloud-exemplaar van Log Analytics-werkruimte automatisch uitgebreid naar Azure. Een gebruiker kunt vrijwillig uitbreiding van waarschuwingen naar Azure initiëren vóór 14 mei 2018. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure Log Analytics](../../monitoring-and-diagnostics/monitoring-alerts-extend.md). Voor gebruikers die waarschuwingen naar Azure uitbreiden, worden de acties zoals e-mailmelding nu beheerd in Azure-Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actie groep API](https://docs.microsoft.com/rest/api/monitor/actiongroups).
   

| Eigenschap | Description |
|:--- |:--- |
| Geadresseerden |Lijst met e-mailadressen. |
| Onderwerp |Het onderwerp van het e-mailbericht. |
| Bijlage |Bijlagen worden momenteel niet ondersteund, dus dit is altijd een waarde van 'None'. |

Hieronder volgt een voorbeeldantwoord voor een e-mailactie voor melding met een drempelwaarde.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

De Put-methode gebruiken met een unieke actie-ID te maken van een nieuw e-mailactie voor een schema.  Het volgende voorbeeld wordt een e-mailmelding met een drempelwaarde, zodat het e-mailbericht wordt verzonden wanneer de drempelwaarde, de resultaten van de opgeslagen zoekopdracht overschrijden.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

Gebruik de Put-methode met de ID van een bestaande actie om een e-mailactie voor een schema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a>Herstelacties
Herstel een runbook te starten in Azure Automation waarmee wordt geprobeerd om het probleem geïdentificeerd door de waarschuwing te verhelpen.  U moet maken van een webhook voor het runbook uit een herstelactie en geeft u de URI in de eigenschap WebhookUri.  Wanneer u deze actie met behulp van Azure portal maakt, wordt automatisch een nieuwe webhook gemaakt voor het runbook.

> [!NOTE]
> Vanaf 14 mei 2018 worden wordt alle waarschuwingen in een openbare Azure-cloud-exemplaar van Log Analytics-werkruimte automatisch uitgebreid naar Azure. Een gebruiker kunt vrijwillig uitbreiding van waarschuwingen naar Azure initiëren vóór 14 mei 2018. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure Log Analytics](../../monitoring-and-diagnostics/monitoring-alerts-extend.md). Voor gebruikers die waarschuwingen naar Azure uitbreiden, worden de acties, zoals herstel met behulp van runbook nu beheerd in Azure-Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actie groep API](https://docs.microsoft.com/rest/api/monitor/actiongroups).

De eigenschappen bevatten herstelbewerkingen in de volgende tabel.

| Eigenschap | Description |
|:--- |:--- |
| RunbookName |De naam van het runbook. Dit moet overeenkomen met een gepubliceerd runbook in het automation-account dat is geconfigureerd in de oplossing voor automatisering in uw Log Analytics-werkruimte. |
| WebhookUri |De URI van de webhook. |
| Vervaldatum |De vervaldatum en -tijd van de webhook.  Als de webhook niet beschikt over een verlopen, kan dit een geldige datum in de toekomst zijn. |

Hieronder volgt een voorbeeldantwoord voor een herstelactie met een drempelwaarde.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

De Put-methode gebruiken met een unieke actie-ID te maken van een nieuwe herstelactie voor een schema.  Het volgende voorbeeld wordt een herstel met een drempelwaarde, zodat het runbook wordt gestart wanneer de drempelwaarde, de resultaten van de opgeslagen zoekopdracht overschrijden.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Gebruik de Put-methode met de ID van een bestaande actie om een herstelactie voor een schema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Voorbeeld
Hieronder volgt een compleet voorbeeld om te maken van een nieuw e-mailmelding.  Hiermee maakt u een nieuw schema samen met een actie met een drempelwaarde en e-mailbericht.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"
    
    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson
    
    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

#### <a name="webhook-actions"></a>Webhookacties
Webhookacties start een proces doordat aanroepen van een URL en eventueel een nettolading wordt verzonden.  Ze zijn vergelijkbaar met herstelacties tenzij ze zijn bedoeld voor webhooks die processen dan Azure Automation-runbooks kunnen aanroepen.  Ze bieden ook de aanvullende mogelijkheid van het leveren van een payload moet worden geleverd aan het extern proces.

> [!NOTE]
> Vanaf 14 mei 2018 worden wordt alle waarschuwingen in een openbare Azure-cloud-exemplaar van Log Analytics-werkruimte automatisch uitgebreid naar Azure. Een gebruiker kunt vrijwillig uitbreiding van waarschuwingen naar Azure initiëren vóór 14 mei 2018. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure Log Analytics](../../monitoring-and-diagnostics/monitoring-alerts-extend.md). Voor gebruikers die waarschuwingen naar Azure uitbreiden, worden acties zoals Webhook nu beheerd in Azure-Actiegroepen. Wanneer een werkruimte en de waarschuwingen worden uitgebreid naar Azure, kunt u ophalen of acties toevoegen met behulp van de [actie groep API](https://docs.microsoft.com/rest/api/monitor/actiongroups).


Webhookacties hoeven niet een drempelwaarde, maar in plaats daarvan moeten worden toegevoegd aan een schema dat een actie bij waarschuwing met een drempelwaarde is.  

Hieronder volgt een voorbeeldantwoord voor webhookactie en een bijbehorende actie bij waarschuwing met een drempelwaarde.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

##### <a name="create-or-edit-a-webhook-action"></a>Maken of bewerken van een webhookactie
De Put-methode gebruiken met een unieke actie-ID te maken van een nieuwe webhookactie voor een schema.  Het volgende voorbeeld wordt een webhookactie en een actie bij waarschuwing met een drempelwaarde zodat de webhook wordt geactiveerd wanneer de drempelwaarde, de resultaten van de opgeslagen zoekopdracht overschrijden.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Gebruik de Put-methode met de ID van een bestaande actie om een webhookactie voor een schema.  De hoofdtekst van de aanvraag moet de etag van de actie bevatten.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction


## <a name="next-steps"></a>Volgende stappen
* Gebruik de [REST-API voor het uitvoeren van zoekopdrachten in logboeken](../../azure-monitor/log-query/log-query-overview.md) in Log Analytics.
* Meer informatie over [waarschuwingen voor activiteitenlogboeken in azure-waarschuwingen](../../monitoring-and-diagnostics/monitor-alerts-unified-log.md)

