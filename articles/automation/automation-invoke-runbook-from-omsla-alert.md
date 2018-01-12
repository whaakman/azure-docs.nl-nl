---
title: Een Azure Automation-runbook aanroepen vanuit een Log Analytics-waarschuwing | Microsoft Docs
description: Dit artikel bevat een overzicht van hoe u een Automation-runbook aanroept vanuit een Log Analytics-waarschuwing in Operations Management Suite.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: magoedte
ms.openlocfilehash: a10be867965eef9746a0f4cc9b14c4fc429f6e35
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="call-an-azure-automation-runbook-from-a-log-analytics-alert"></a>Een Azure Automation-runbook aanroepen vanuit een Log Analytics-waarschuwing

U kunt een waarschuwing configureren in Azure Log Analytics voor het maken van een waarschuwingsrecord wanneer resultaten aan uw criteria voldoen. Deze waarschuwing kan vervolgens automatisch een Azure Automation-runbook uitvoeren in een poging het probleem automatisch op te lossen. 

Zo kan een waarschuwing bijvoorbeeld een langdurige piek in het processorgebruik aangeven. Of dat een toepassingsproces dat essentieel is voor de functionaliteit van een zakelijke toepassing, is mislukt. Een runbook kan vervolgens een overeenkomende gebeurtenis naar het Windows-gebeurtenislogboek schrijven.  

Er zijn twee opties voor het aanroepen van een runbook in de waarschuwingsconfiguratie:

* Een webhook gebruiken.
   * Dit is de enige beschikbare optie als uw Operations Management Suite-werkruimte niet is gekoppeld aan een Automation-account.
   * Ook als u wel al een Automation-account hebt gekoppeld aan een Operations Management Suite-werkruimte, blijft deze optie beschikbaar.  

* Een runbook direct selecteren.
   * Deze optie is alleen beschikbaar wanneer de Operations Management Suite-werkruimte is gekoppeld aan een Automation-account.

## <a name="calling-a-runbook-by-using-a-webhook"></a>Een runbook aanroepen met behulp van een webhook

Met een webhook kunt u een bepaald runbook in Azure Automation starten via een afzonderlijke HTTP-aanvraag. Voordat u de [Log Analytics-waarschuwing](../log-analytics/log-analytics-alerts.md#alert-rules) configureert om het runbook aan te roepen met behulp van een webhook als waarschuwingsactie, moet u een [webhook maken](automation-webhooks.md#creating-a-webhook) voor het runbook dat met deze methode wordt aangeroepen. Vergeet niet om de webhook-URL te registreren, zodat u ernaar kunt verwijzen tijdens het configureren van de waarschuwingsregel.   

## <a name="calling-a-runbook-directly"></a>Een runbook direct aanroepen

U kunt het OMS Automation and Control-pakket installeren en configureren in uw Operations Management Suite-werkruimte. Tijdens de configuratie van de optie voor runbookacties voor de waarschuwing kunt u alle runbooks in de vervolgkeuzelijst **Een runbook selecteren** bekijken en het runbook selecteren dat u wilt uitvoeren als reactie op de waarschuwing. Het geselecteerde runbook kan worden uitgevoerd in een Azure-werkruimte of op een Hybrid Runbook Worker. 

Nadat u de waarschuwing hebt gemaakt met de runbookoptie, wordt er een webhook voor het runbook gemaakt. U kunt de webhook zien als u naar het Automation-account gaat en het deelvenster Webhook van het geselecteerde runbook opent. 

Als u de waarschuwing verwijdert, wordt de webhook niet verwijderd. Dit is geen probleem. De webhook wordt alleen een zwevend item dat u uiteindelijk handmatig moet verwijderen om uw Automation-account geordend te houden.  

## <a name="characteristics-of-a-runbook"></a>Kenmerken van een runbook

Beide methoden voor het aanroepen van het runbook vanuit de Log Analytics-waarschuwing hebben kenmerken. Het is belangrijk dat u hier inzicht in hebt voordat u de waarschuwingsregels gaat configureren. 

De waarschuwingsgegevens staan in JSON-indeling in één eigenschap, genaamd **SearchResult**. Deze indeling is voor runbook- en webhook-acties met een standaardpayload. Voor webhookacties met aangepaste payloads, inclusief **IncludeSearchResults:True** in **RequestBody**, is de eigenschap **SearchResults**.

U moet een runbook-invoerparameter hebben met de naam **WebhookData** en van het type **Object**. Deze kan verplicht of optioneel zijn. Met deze invoerparameter geeft de waarschuwing de zoekresultaten door aan het runbook.

```powershell
param  
    (  
    [Parameter (Mandatory=$true)]  
    [object] $WebhookData  
    )
```
U moet ook code hebben om **WebhookData** te converteren naar een PowerShell-object.

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
```

**$SearchResult** is een matrix met objecten. Elk object bevat de velden met waarden uit één zoekresultaat.


## <a name="example-walkthrough"></a>Voorbeeldscenario

Het volgende voorbeeld van een grafisch runbook laat zien hoe dit proces werkt. Er wordt een Windows-service gestart.

![Grafisch runbook voor het starten van een Windows-service](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)

Het runbook heeft één invoerparameter van het type **Object**, die **WebhookData** wordt genoemd. Deze bevat de webhookgegevens die zijn doorgegeven door de waarschuwing die **SearchResult** bevat.

![Invoerparameters voor runbook](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)

In dit voorbeeld hebben we twee aangepaste velden gemaakt in Log Analytics: **SvcDisplayName_CF** en **SvcState_CF**. Deze velden extraheren de weergavenaam van de service en de status van de service (ofwel 'wordt uitgevoerd' of 'is gestopt') van de gebeurtenis die naar het logboek voor systeemgebeurtenissen wordt geschreven. Vervolgens is een waarschuwingsregel met de volgende zoekquery gemaakt, zodat kan worden gedetecteerd wanneer de afdrukspoolerservice op het Windows-systeem is gestopt:

`Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"` 

Dit kan elke relevante service zijn. Voor dit voorbeeld is gebruikgemaakt van een bestaande service die deel uitmaakt van het Windows-besturingssysteem. De actie bij waarschuwing is geconfigureerd om het runbook uit dit voorbeeld uit te voeren en om te worden uitgevoerd op Hybrid Runbook Worker, dat is ingeschakeld op het doelsysteem.   

Met de runbookcodeactiviteit **Get Service Name from LA** wordt de tekenreeks in JSON-indeling geconverteerd naar een objecttype en gefilterd op het item **SvcDisplayName_CF**. De weergavenaam van de Windows-service wordt geëxtraheerd en deze waarde wordt doorgegeven aan de volgende activiteit, waarbij wordt gecontroleerd of de service is gestopt voordat wordt geprobeerd deze opnieuw te starten. **SvcDisplayName_CF** is een [aangepast](../log-analytics/log-analytics-custom-fields.md) veld dat in Log Analytics is gemaakt voor dit voorbeeld.

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
$SearchResult.SvcDisplayName_CF  
```

Wanneer de service stopt, wordt er met de waarschuwingsregel in Log Analytics een overeenkomst gedetecteerd en wordt het runbook geactiveerd. Daarbij wordt de context van de waarschuwing naar het runbook verzonden. Het runbook probeert te verifiëren of de service is gestopt. Als dit het geval is, probeert het runbook de service opnieuw te starten en te controleren of deze correct is gestart. Vervolgens geeft het de resultaten weer.     

Als u uw Automation-account niet hebt gekoppeld aan uw Operations Management Suite-werkruimte, kunt u de waarschuwingsregel configureren met een webhookactie. Door de webhookactie wordt het runbook geactiveerd. Ook wordt het runbook geconfigureerd voor het converteren van de tekenreeks in JSON-indeling en gefilterd op **SearchResult** om de eerder vermelde instructies te volgen.    

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [nieuwe querytaal van Log Analytics](../log-analytics/log-analytics-log-search-upgrade.md), is de webhookpayload gewijzigd. Details van de indeling zijn te vinden in de [Azure Log Analytics REST-API](https://aka.ms/loganalyticsapiresponse).

## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie over waarschuwingen in Log Analytics en het maken van een waarschuwing raadpleegt u [Alerts in Log Analytics](../log-analytics/log-analytics-alerts.md) (Waarschuwingen in Log Analytics).

* Als u wilt weten hoe u runbooks activeert met een webhook, raadpleegt u [Azure Automation-webhooks](automation-webhooks.md).
