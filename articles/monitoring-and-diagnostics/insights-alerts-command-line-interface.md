---
title: De platformoverschrijdende Azure CLI gebruiken om te maken van klassieke waarschuwingen voor Azure-services | Microsoft Docs
description: E-mailberichten of meldingen activeert, of URL's van websites (webhooks) of automation aanroepen wanneer de door u opgegeven voorwaarden wordt voldaan.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/24/2016
ms.author: robb
ms.component: alerts
ms.openlocfilehash: 8112b868bc8d2ca2a9478d38ee702d8b3350d48e
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287095"
---
# <a name="use-the-cross-platform-azure-cli-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>De platformoverschrijdende Azure CLI gebruiken klassieke metrische om waarschuwingen te maken in de Azure-Monitor voor Azure-services 

> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>
> [!NOTE]
> In dit artikel wordt beschreven hoe oudere klassieke metrische waarschuwingen maken. Azure ondersteunt van de Monitor nu [nieuwere, metrische waarschuwingen voor een betere](monitoring-near-real-time-metric-alerts.md). Deze waarschuwingen kunnen meerdere metrische gegevens controleren en waarschuwen voor dimensionale metrische gegevens mogelijk maken. Azure CLI-ondersteuning voor nieuwere metrische waarschuwingen is binnenkort beschikbaar.
>
>

Dit artikel laat zien hoe u waarschuwingen van Azure classic metrische instelt met behulp van de platformoverschrijdende opdrachtregelinterface (Azure CLI).

> [!NOTE]
> Monitor voor Azure is de nieuwe naam voor wat 'Azure Insights' is aangeroepen tot 25 September 2016. Echter, de naamruimten en dus de opdrachten die hier worden beschreven nog steeds het woord 'insights'.

U kunt een waarschuwing op basis van de metrische gegevens voor uw Azure-services of op basis van gebeurtenissen die in Azure plaatsvinden ontvangen.

* **Metrische waarden**: de waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven waarde overschrijdt de drempelwaarde die u in beide richtingen toewijst. Dat wil zeggen, deze beide wordt geactiveerd wanneer de voorwaarde voor het eerst wordt voldaan en wanneer die voorwaarde niet langer wordt voldaan.    

* **Activiteit logboekgebeurtenissen**: een waarschuwing kunt activeren voor *elke* gebeurtenis of wanneer bepaalde gebeurtenissen optreden. Zie voor meer informatie over activiteitenlogboeken, [activiteit logboek waarschuwingen (klassiek) maken](monitoring-activity-log-alerts.md). 

U kunt het volgende te doen als er wordt een klassieke metrische-waarschuwing configureren:

* E-mailmeldingen verzenden naar de servicebeheerder en medebeheerders. 
* Verzenden van e-mailbericht naar e-mailadressen die u opgeeft.
* Een webhook aanroepen.
* Uitvoering van een Azure-runbook (alleen van de Azure-portal op dit moment) starten.

U kunt configureren en informatie over klassieke metrische waarschuwingsregels ophalen met behulp van de volgende: 

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Azure-CLI](insights-alerts-command-line-interface.md)
* [Monitor voor Azure REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

U kunt ook hulp voor opdrachten met de opdracht met **-help** aan het einde. Hieronder volgt een voorbeeld: 

```console
 azure insights alerts -help
 azure insights alerts actions email create -help
 ```

## <a name="create-alert-rules-by-using-azure-cli"></a>Waarschuwingsregels maken met Azure CLI
1. Nadat u de vereiste onderdelen hebt geïnstalleerd, moet u zich aanmelden bij Azure. Zie [Azure Monitor CLI voorbeelden](insights-cli-samples.md) voor de opdrachten die u nodig hebt om te beginnen. Deze opdrachten te verkrijgen aangemeld, ziet u welke abonnement u en voorbereiding op Azure Monitor opdrachten uit te voeren.

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. U kunt bestaande regels voor een resourcegroep gebruiken de volgende indeling: 

   **inzicht van Azure waarschuwingen regel lijst** *[opties] &lt;resourceGroup&gt;*

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. Als u wilt een regel maakt, moet u eerst hebt van belangrijke stukjes informatie.
    * De **resource-ID** voor de resource die u wilt een waarschuwing voor het instellen.
    * De **metrische definities** die beschikbaar zijn voor die bron.

     Een manier om op te halen van de resource-ID is de Azure portal gebruiken. Ervan uitgaande dat de resource al gemaakt is, selecteert u deze in de portal. Klik in de volgende blade in de **instellingen** sectie **eigenschappen**. **De RESOURCE-ID** is een veld in de volgende blade. 
     
     U kunt ook de resource-ID opvragen met behulp van [Azure Resource Explorer](https://resources.azure.com/).

     Hier volgt een voorbeeld van de resource-ID voor een web-app:

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Als u een lijst van de beschikbare metrische gegevens en de eenheden voor de metrische gegevens in het vorige voorbeeld van de resource, gebruikt u de volgende opdracht in de Azure CLI:  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     *PT1M* wordt de granulatie van de meting beschikbaar (in intervallen van 1 minuut). U hebt verschillende metrische opties wanneer u verschillende granulaties gebruikt.
     
4. Als u wilt een waarschuwingsregel op basis van een metriek maakt, moet u een opdracht gebruiken in de volgende indeling:

    **inzicht van Azure waarschuwingen metrische regelset** *[opties] &lt;ruleName&gt; &lt;locatie&gt; &lt;resourceGroup&gt; &lt;venstergrootte&gt; &lt;operator&gt; &lt;drempelwaarde&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*

    Het volgende voorbeeld heeft een waarschuwing van een website resource ingesteld. De waarschuwing triggers wanneer het consistent verkeer ontvangt voor 5 minuten en opnieuw wanneer er geen verkeer ontvangen gedurende vijf minuten.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. Voor het maken van een webhook of een e-mailbericht verzenden wanneer een klassieke metrische waarschuwing wordt geactiveerd, moet u eerst de e-mailadres of webhook maken. Vervolgens maakt u de regel onmiddellijk daarna. U kunt webhooks of e-mailberichten niet koppelen aan regels die al zijn gemaakt.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. U kunt controleren of uw waarschuwingen correct zijn gemaakt door te kijken op een afzonderlijke regel.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. Om te verwijderen regels, gebruikt u een opdracht in de volgende indeling:

    **Waarschuwingen regel verwijderen Insights** [opties] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Deze opdrachten verwijdert u de regels die eerder in dit artikel zijn gemaakt.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>Volgende stappen
* [Een overzicht van Azure monitoring](monitoring-overview.md), met inbegrip van de typen gegevens u kunt verzamelen en controleren.
* Meer informatie over [configureren van webhooks in waarschuwingen](insights-webhooks-alerts.md).
* Meer informatie over [waarschuwingen configureren op activiteit logboekgebeurtenissen](monitoring-activity-log-alerts.md).
* Meer informatie over [Azure Automation-runbooks](../automation/automation-starting-a-runbook.md).
* Ophalen van een [overzicht van het verzamelen van diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) voor het verzamelen van gedetailleerde hoge frequentie metrische gegevens voor uw service.
* Ophalen van een [overzicht van metrische gegevens verzameling](insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
