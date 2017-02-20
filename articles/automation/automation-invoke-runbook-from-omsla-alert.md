---
title: Een Azure Automation-runbook aanroepen vanuit een Log Analytics-waarschuwing | Microsoft Docs
description: Dit artikel bevat een overzicht van hoe u een Automation-runbook aanroept vanuit een Microsoft OMS Log Analytics-waarschuwing.
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/31/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 4ce5ad30d79e92a11231313fe13dd42b94fc2aa4
ms.openlocfilehash: 50969591267ca74e5c4d4aa5c1efe5b673498309

---

# <a name="calling-an-azure-automation-runbook-from-an-oms-log-analytics-alert"></a>Een Azure Automation-runbook aanroepen vanuit een Log Analytics-waarschuwing

Wanneer er in Log Analytics een waarschuwing is geconfigureerd om een waarschuwingsrecord te maken als resultaten overeenkomen met bepaalde criteria (zoals een langdurige piek in CPU-gebruik of als een bepaald toepassingsproces dat essentieel is voor de functionaliteit van een bedrijfstoepassing, mislukt en een overeenkomende gebeurtenis naar het Windows-gebeurtenislogboek schrijft), kan deze waarschuwing automatisch een Automation-runbook uitvoeren in een poging om het probleem automatisch op te lossen.  

Tijdens het configureren van de waarschuwing hebt u twee opties voor het aanroepen van een runbook.  Met name:

1. Een webhook gebruiken.
   * Dit is de enige beschikbare optie als uw OMS-werkruimte niet is gekoppeld aan een Automation-account.
   * Als u al een Automation-account hebt gekoppeld aan een OMS-werkruimte, is deze optie ook nog steeds beschikbaar.  

2. Een runbook direct selecteren.
   * Deze optie is alleen beschikbaar wanneer de OMS-werkruimte is gekoppeld aan een Automation-account.  

## <a name="calling-a-runbook-using-a-webhook"></a>Een runbook aanroepen met behulp van een webhook

Met een webhook kunt u een bepaald runbook in Azure Automation starten via een afzonderlijke HTTP-aanvraag.  Voordat u de [Log Analytics-waarschuwing](../log-analytics/log-analytics-alerts.md#creating-an-alert-rule) configureert om het runbook aan te roepen met behulp van een webhook als waarschuwingsactie, moet u een webhook voor het runbook maken dat met deze methode wordt aangeroepen.  Bekijk en volg de stappen in het artikel [Create a webhook](automation-webhooks.md#creating-a-webhook) (Een webhook maken) en vergeet niet om de webhook-URL te registreren, zodat u ernaar kunt verwijzen tijdens het configureren van de waarschuwingsregel.   

## <a name="calling-a-runbook-directly"></a>Een runbook direct aanroepen

Als u Automation & Control in uw OMS-werkruimte hebt geïnstalleerd en geconfigureerd, kunt u tijdens de configuratie van de opties voor runbookacties alle runbooks in de vervolgkeuzelijst **Een runbook selecteren** bekijken en het runbook selecteren dat u wilt uitvoeren als reactie op de waarschuwing.  Het geselecteerde runbook kan worden uitgevoerd in een werkruimte in de Azure-cloud of op een Hybrid Runbook Worker.  Wanneer de waarschuwing is gemaakt met de runbookoptie, wordt er een webhook voor het runbook gemaakt.  U kunt de webhook zien als u naar het Automation-account gaat en naar de blade Webhook van het geselecteerde runbook navigeert.  Als u de waarschuwing verwijdert, wordt de webhook niet verwijderd. De gebruiker kan de webhook wel handmatig verwijderen.  Het is geen probleem als de webhook niet wordt verwijderd. Het is alleen een zwevend item dat uiteindelijk moet worden verwijderd om een georganiseerd Automation-account te behouden.  

## <a name="characteristics-of-a-runbook-for-both-options"></a>Kenmerken van een runbook (voor beide opties)

Beide methoden voor het aanroepen van het runbook vanuit de Log Analytics-waarschuwing hebben eigen gedragskenmerken. Het is belangrijk dat u deze begrijpt voordat u de waarschuwingsregels gaat configureren.  

* U moet een runbook-invoerparameter hebben met de naam **WebhookData** en van het type **Object**.  Deze kan verplicht of optioneel zijn.  Met deze invoerparameter geeft de waarschuwing de zoekresultaten door aan het runbook.

        param  
         (  
          [Parameter (Mandatory=$true)]  
          [object] $WebhookData  
         )
  
*  U moet code hebben om de WebhookData te converteren naar een PowerShell-object.

    `$SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value`

    *$SearchResults* is een matrix met objecten; elk object bevat de velden met waarden uit één zoekresultaat

### <a name="webhookdata-inconsistencies-between-the-webhook-option-and-runbook-option"></a>WebhookData-inconsistenties tussen de webhook-optie en runbook-optie 

* Wanneer u een waarschuwing configureert om een webhook aan te roepen, voert u een webhook-URL in die u voor een runbook hebt gemaakt en klikt u op de knop **Webhook testen**.  De resulterende WebhookData die naar het runbook worden verzonden, bevat geen *.SearchResult* of *.SearchResults*.

*  Als u deze waarschuwing opslaat, bevat de WebhookData die naar het runbook wordt verzonden, *.SearchResult* wanneer de waarschuwing de webhook activeert en aanroept.
* Als u een waarschuwing maakt en configureert om een runbook aan te roepen (waardoor ook een webhook wordt gemaakt), wordt WebhookData met *.SearchResults* naar het runbook verzonden wanneer de waarschuwing wordt geactiveerd.

In bovenstaand codevoorbeeld moet u dus *.SearchResult* ophalen als de waarschuwing een webhook aanroept en *.SearchResults* als de waarschuwing een runbook rechtstreeks aanroept.

## <a name="example-walkthrough"></a>Voorbeeldscenario 

U ziet hoe dit werkt in het volgende grafische voorbeeldrunbook waarmee een Windows-service wordt gestart.<br><br> ![Grafisch runbook dat Windows-service start](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)<br>

Het runbook heeft één invoerparameter van het type **Object**. De parameter heet **WebhookData** en bevat de webhookgegevens die zijn doorgegeven door de waarschuwing die *.SearchResults* bevat.<br><br> ![Invoerparameters voor runbook](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)<br>

Voor dit voorbeeld zijn in Log Analytics twee aangepaste velden gemaakt: *SvcDisplayName_CF* en *SvcState_CF*. Deze dienen om de weergavenaam en de status van de service (Wordt uitgevoerd of Gestopt) op te halen uit de gebeurtenis die naar het logboek voor systeemgebeurtenissen is geschreven.  Vervolgens is een waarschuwingsregel met de volgende query gemaakt: `Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"` zodat kan worden gedetecteerd wanneer de afdrukspoolerservice op het Windows-systeem is gestopt.  Dit kan ook een andere service zijn, maar voor dit voorbeeld is gebruikgemaakt van een bestaande service die deel uitmaakt van het Windows-besturingssysteem.  De actie bij waarschuwing is geconfigureerd om het runbook uit dit voorbeeld uit te voeren en om te worden uitgevoerd op Hybrid Runbook Worker, dat is ingeschakeld op de doelsystemen.   

De activiteit **Get Service Name from LA** in de runbookcode converteert de tekenreeks in JSON-indeling naar een objecttype en filtert op het item *SvcDisplayName_CF* om de weergavenaam van de Windows-service te verkrijgen. Deze wordt doorgegeven aan de volgende activiteit, die controleert of de service is gestopt voordat wordt geprobeerd de service opnieuw te starten.  *SvcDisplayName_CF* is een [aangepast](../log-analytics/log-analytics-custom-fields.md) veld dat in Log Analytics is gemaakt voor dit voorbeeld.

    $SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value
    $SearchResults.SvcDisplayName_CF  

Wanneer de service stopt, wordt er door de waarschuwingsregel in Log Analytics een overeenkomst gedetecteerd en het runbook geactiveerd. Daarbij wordt de context van de waarschuwing naar het runbook verzonden. Het runbook onderneemt actie om te controleren of de service daadwerkelijk is gestopt. Als dat het geval is, wordt geprobeerd de service opnieuw te starten, wordt gecontroleerd of de service is gestart en worden de resultaten uitgevoerd.     

Als u uw Automation-account niet hebt gekoppeld aan uw OMS-werkruimte, configureert u de waarschuwingsregel met een webhookactie die het runbook activeert, en configureert u het runbook om de tekenreeks in JSON-indeling te converteren en om te filteren op *.SearchResult* volgens de eerder vermelde richtlijnen.    

## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie over waarschuwingen in Log Analytics en het maken van een waarschuwing raadpleegt u [Alerts in Log Analytics](../log-analytics/log-analytics-alerts.md) (Waarschuwingen in Log Analytics).

* Als u wilt weten hoe u runbooks activeert met een webhook, raadpleegt u [Azure Automation-webhooks](automation-webhooks.md).



<!--HONumber=Feb17_HO2-->


