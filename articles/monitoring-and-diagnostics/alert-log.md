---
title: Maken, weergeven en beheren met behulp van Azure Monitor een waarschuwing log
description: Gebruik de Azure-Monitor maken, weergeven en beheren van waarschuwingsregels in Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: c2f018915d5e27d4cf16447e403fcb2e1b3f55a6
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52969512"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Maken, weergeven en beheren van waarschuwingen met behulp van Azure Monitor  

## <a name="overview"></a>Overzicht
Dit artikel leest u hoe het instellen van waarschuwingen met behulp van de interface van de waarschuwingen in Azure portal. Er is een definitie van een waarschuwingsregel in drie delen:
- Doel: Specifieke Azure-resource, die moet worden bewaakt
- De criteria: Bepaalde voorwaarde of logische dat wanneer in signaal zien, moet actie activeren
- Actie: Specifieke aanroep verzonden naar een ontvanger van een melding - e-mail, SMS, webhook, enzovoort.

De term **Logboekwaarschuwingen** om te beschrijven van waarschuwingen met waar signaal aangepaste query op basis van [Log Analytics](../azure-monitor/learn/tutorial-viewdata.md) of [Application Insights](../application-insights/app-insights-analytics.md). Meer informatie over functies, -terminologie en typen van [Logboekwaarschuwingen - overzicht](monitor-alerts-unified-log.md).

> [!NOTE]
> Populaire logboekgegevens van [Azure Log Analytics](../azure-monitor/learn/tutorial-viewdata.md) is nu ook beschikbaar op de metrische platform in Azure Monitor. Voor de detailweergave [metrische waarschuwingen voor logboeken](monitoring-metric-alerts-logs.md)

## <a name="managing-log-alerts-from-the-azure-portal"></a>Waarschuwingen beheren vanuit Azure portal

Gedetailleerde volgende is een stapsgewijze handleiding voor het gebruik van waarschuwingen met behulp van de interface van de Azure portal.

### <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Een waarschuwingsregel maken met de Azure portal
1. In de [portal](https://portal.azure.com/), selecteer **Monitor** en kies onder de sectie MONITOR - **waarschuwingen**.  
    ![Controle](media/alert-log/AlertsPreviewMenu.png)

1. Selecteer de **nieuwe waarschuwingsregel** knop een nieuwe waarschuwing maken in Azure.
    ![Waarschuwing toevoegen](media/alert-log/AlertsPreviewOption.png)

1. De sectie waarschuwingen maken wordt weergegeven met de drie onderdelen die bestaan uit: *waarschuwingsvoorwaarde definiëren*, *Waarschuwingsdetails definiëren*, en *actiegroep definiëren*.

    ![Regel maken](media/alert-log/AlertsPreviewAdd.png)

1.  De waarschuwingsvoorwaarde definiëren met behulp van de **Resource selecteren** koppeling en het doel op te geven door het selecteren van een resource. Filteren op het kiezen van de _abonnement_, _resourcetype_, en vereist _Resource_. 

    >[!NOTE]

    > Voor het maken van een logboek waarschuwing - Controleer of de **log** signaal is beschikbaar voor de geselecteerde resource voordat u doorgaat.
    ![Resource selecteren](media/alert-log/Alert-SelectResourceLog.png)

 
1. *Waarschuwingen voor activiteitenlogboeken*: Zorg ervoor dat **resourcetype** is een analytics-bron, zoals *Log Analytics* of *Application Insights* en aan te geven als **Log** , klikt u vervolgens één keer juiste **resource** is gekozen, klikt u op *gedaan*. Vervolgens gebruikt de **criteria toevoegen** om de lijst weergeven van signaal opties die beschikbaar zijn voor de resource en uit de lijst signaal **zoeken in Logboeken aangepaste** optie voor de gekozen monitor-service, zoals melden *Log Analytics* of *Application Insights*.

   ![Selecteer een resource - aangepaste zoekopdrachten in Logboeken](media/alert-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Waarschuwingen lijsten kunnen importeren analytics-query als signaaltype - **logboek (opgeslagen Query)**, zoals weergegeven in bovenstaande afbeelding. Gebruikers kunnen verbeteren van de query in Analytics en deze vervolgens opslaan voor toekomstig gebruik in waarschuwingen: meer informatie over het gebruik van opgeslagen query die beschikbaar zijn op [met zoeken in Logboeken in log analytics](../azure-monitor/log-query/log-query-overview.md) of [gedeelde query in application insights Analytics](../azure-monitor/log-query/log-query-overview.md). 

1.  *Waarschuwingen voor activiteitenlogboeken*: eenmaal hebt geselecteerd, query voor waarschuwingen kan worden vermeld in de **zoekquery** veld; als de query-syntaxis is onjuist fout wordt in het veld in het rood weergegeven. Als de querysyntaxis juist - ter referentie wordt historische gegevens van de opgegeven query weergegeven als een grafiek met de optie voor het aanpassen van het tijdvenster van afgelopen zes uur voor de afgelopen week.

 ![Waarschuwingsregel configureren](media/alert-log/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Historische gegevensvisualisatie kan alleen worden weergegeven als de queryresultaten details van de tijd hebt. Als uw query in waarden van de specifieke kolom- of samengevatte gegevens resulteert is hetzelfde als een enkelvoudige diagram weergegeven.

    >  Het type van de meting van metrische gegevens van waarschuwingen met Application insights, kunt u opgeven welke specifieke variabele als u wilt de gegevens groeperen met behulp van de **cumulatieve op** optie; als de hieronder weergegeven:

    ![statistische optie](media/alert-log/aggregate-on.png)

1.  *Waarschuwingen voor activiteitenlogboeken*: met de visualisatie aanwezig is, **Alert Logic** kunnen worden geselecteerd in de weergegeven opties van de voorwaarde, aggregatie en ten slotte drempelwaarde. Ten slotte opgeven in de logica, de tijd om te beoordelen voor de opgegeven voorwaarde, met behulp van **periode** optie. Samen met hoe vaak waarschuwing moet worden uitgevoerd door het selecteren van **frequentie**.
Voor **Logboekwaarschuwingen** waarschuwingen kunnen worden gebaseerd op:
   - *Aantal Records*: een waarschuwing gemaakt als het aantal records dat wordt geretourneerd door de query is groter dan of kleiner is dan de waarde die is opgegeven.
   - *Meting van metrische gegevens*: een waarschuwing gemaakt als elke *statistische waarde* overschrijdt de drempelwaarde die is opgegeven in de resultaten en is *gegroepeerd op* gekozen waarde. Het aantal schendingen voor een waarschuwing is het aantal keren dat die de drempelwaarde is overschreden in de geselecteerde tijdsperiode. Totaal aantal schendingen voor elke combinatie van schendingen kunt u opgeven via de resultatenset of achtereenvolgende schendingen om te vereisen dat de schendingen in opeenvolgende steekproeven plaatsvinden moeten. Meer informatie over [waarschuwingen en het bijhorende type](monitor-alerts-unified-log.md).


1. Als de tweede stap definieert u een naam voor de waarschuwing in de **naam waarschuwingsregel** veld samen met een **beschrijving** met gedetailleerde informatie over specificaties voor de waarschuwing en **ernst** waarde uit de opties opgegeven. Deze gegevens opnieuw worden gebruikt in alle e-mails met waarschuwingen, meldingen of push uitgevoerd door Azure Monitor. Bovendien gebruiker kan kiezen bij het maken van waarschuwingsregel onmiddellijk geactiveerd door het omschakelen van op de juiste wijze **regel inschakelen bij het maken van** optie.

    Voor **Logboekwaarschuwingen** alleen aanvullende functionaliteit is beschikbaar in de details van waarschuwing:

    - **Waarschuwingen onderdrukken**: wanneer u onderdrukking voor de waarschuwingsregel inschakelen, acties voor de regel zijn uitgeschakeld voor een gedefinieerde tijdsduur na het maken van een nieuwe waarschuwing. De regel wordt nog uitgevoerd en waarschuwing records gemaakt, mits de criteria wordt voldaan. Zodat u tijd kunt dit probleem oplossen zonder dat dubbele acties worden uitgevoerd.

        ![Waarschuwingen onderdrukken voor Logboekwaarschuwingen](media/alert-log/AlertsPreviewSuppress.png)

        > [!TIP]
        > Geef een onderdrukken waarschuwing die groter is dan de frequentie van de waarschuwing om ervoor te zorgen meldingen zonder overlap worden gestopt

1. Als de derde en laatste stap, geef eventuele **actiegroep** moet voor de waarschuwingsregel wordt geactiveerd wanneer waarschuwingen voorwaarde wordt voldaan. U kunt kiezen eventuele bestaande actiegroep met waarschuwing of een nieuwe actiegroep maken. Geselecteerd volgens actiegroep, wanneer de waarschuwing is wordt Azure trigger: email(s) verzenden, SMS(s) verzenden, aanroepen van SMS-bericht(en), herstellen met behulp van Azure-Runbooks, pushen naar uw ITSM-hulpprogramma, enzovoort. Meer informatie over [actiegroepen](monitoring-action-groups.md).

    > [!NOTE]
    > Raadpleeg de [Servicelimieten van Azure-abonnement](../azure-subscription-service-limits.md) voor beperkingen met betrekking tot de Runbook-nettoladingen geactiveerd voor logboekwaarschuwingen via Azure actiegroepen

    Voor **Logboekwaarschuwingen** aanvullende functionaliteit is beschikbaar voor de standaardacties onderdrukken:

    - **E-mailmelding**: onderdrukt *e-mailonderwerp* in het e-mailbericht verzonden via actiegroep; als een of meer e-mailacties in de groep met deze actie bestaat. U kunt de hoofdtekst van het e-mailbericht niet wijzigen en dit veld is **niet** voor e-mailadres.
    - **Aangepaste Json-nettolading opnemen**: heeft voorrang op de JSON die wordt gebruikt door actiegroepen; van de webhook als een of meer webhookacties aanwezig zijn in de groep met deze actie. Gebruiker kan de indeling van JSON moet worden gebruikt voor alle webhooks die zijn geconfigureerd in de bijbehorende actie groep; opgeven Zie voor meer informatie over de opmaak van de webhook [webhookactie voor Logboekwaarschuwingen](monitor-alerts-unified-log-webhook.md). Webhook-optie weergeven wordt om te controleren of de indeling met behulp van de voorbeeld-JSON-gegevens geboden.

        ![Actie onderdrukkingen voor Logboekwaarschuwingen](media/alert-log/AlertsPreviewOverrideLog.png)


1. Als alle velden geldig zijn en met groene maatstreepjes de **maken van waarschuwingsregel** knop kan worden geklikt en wordt er een waarschuwing gemaakt in Azure Monitor - waarschuwingen. Alle waarschuwingen kunnen worden weergegeven van de Dashboard-waarschuwingen.

    ![Het maken van regels](media/alert-log/AlertsPreviewCreate.png)

    Binnen een paar minuten, wordt de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

Gebruikers kunnen ook hun analytics-query in voltooid [logboeken Analytics-pagina in Azure portal](../azure-monitor/log-query/portals.md#log-analytics-page
) en pusht u deze om te maken van een waarschuwing via de knop 'Waarschuwing instellen' - en volg de instructies in stap 6 en hoger in de bovenstaande zelfstudie.

 ![Log Analytics - waarschuwing instellen](media/alert-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>Weergeven en beheren van waarschuwingen in Azure portal

1. In de [portal](https://portal.azure.com/), selecteer **Monitor** en kies onder de sectie MONITOR - **waarschuwingen**.  

1. De **waarschuwingen Dashboard** wordt weergegeven - waarin alle Azure-waarschuwingen (inclusief logboekwaarschuwingen) worden weergegeven in een enkel bord, met inbegrip van elke instantie van wanneer de waarschuwingsregel voor uw logboek is gestart. Zie voor meer informatie, [Waarschuwingsbeheer](https://aka.ms/managealertinstances).
    > [!NOTE]
    > Waarschuwingsregels bestaan uit aangepaste query's gebaseerde logica die door gebruikers en wordt daarmee zonder een opgelost. Dit telkens wanneer de voorwaarden die zijn opgegeven in de waarschuwingsregel wordt voldaan, deze wordt geactiveerd. 


1. Selecteer de **regels beheren** knop op de bovenste balk, om te navigeren naar de beheersectie van regel - waar alle waarschuwingsregels gemaakt worden weergegeven, met inbegrip van waarschuwingen die zijn uitgeschakeld.
    ![ regels voor waarschuwingen beheren](media/alert-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>Beheren van waarschuwingen met behulp van Azure Resource-sjabloon
Momenteel log waarschuwingen kunnen worden gemaakt met behulp van twee verschillende Resource-sjablonen, gebaseerd op welke analytics-platform in de waarschuwing wordt baseren (dat wil zeggen) Log Analytics of Application Insights.

Daarom de onderstaande sectie vindt u informatie over het gebruik van Resource-sjabloon voor Logboekwaarschuwingen voor elk platform voor streaminganalyse.

### <a name="azure-resource-template-for-log-analytics"></a>Azure-Resourcesjabloon voor Log Analytics
Waarschuwingen voor logboeken voor Log Analytics worden gemaakt door waarschuwingsregels die een opgeslagen zoekopdracht op een vast interval uitvoert. Als de resultaten van de query overeenkomen met criteria opgegeven, wordt een waarschuwingsrecord gemaakt en een of meer acties worden uitgevoerd. 

Resource-sjabloon voor Log analytics opgeslagen zoekopdracht en Log analytics-waarschuwingen zijn beschikbaar in Log Analytics-sectie van documentatie. Zie voor meer informatie, [toe te voegen met Log Analytics opgeslagen zoekopdrachten en waarschuwingen](../azure-monitor/insights/solutions-resources-searches-alerts.md); die bevat illustratieve voorbeelden, evenals de schemadetails van het.

### <a name="azure-resource-template-for-application-insights"></a>Azure-Resourcesjabloon voor Application Insights
Waarschuwing voor Application Insights-resources heeft een type `Microsoft.Insights/scheduledQueryRules/`. Zie voor meer informatie over dit brontype [Azure Monitor - gepland Query regels API-verwijzing](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/).

Hieronder volgt de structuur voor [queryregels gepland maken](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) op basis van resource-sjabloon met de voorbeeld-gegevensset als variabelen.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0", 
    "parameters": {      
    },   
    "variables": {
    "alertLocation": "southcentralus",
    "alertName": "samplelogalert",
    "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
    "alertDesription": "Sample log search alert",
    "alertStatus": "true",
    "alertSource":{
        "Query":"requests",
        "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
        "Type":"ResultCount"
         },
     "alertSchedule":{
         "Frequency": 15,
         "Time": 60
         },
     "alertActions":{
         "SeverityLevel": "4"
         },
      "alertTrigger":{
        "Operator":"GreaterThan",
        "Threshold":"1"
         },
       "actionGrp":{
        "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
        "Subject": "Customized Email Header",
        "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"           
         }
  },
  "resources":[ {
    "name":"[variables('alertName')]",
    "type":"Microsoft.Insights/scheduledQueryRules",
    "apiVersion": "2018-04-16",
    "location": "[variables('alertLocation')]",
    "tags":{"[variables('alertTag')]": "Resource"},
    "properties":{
       "description": "[variables('alertDesription')]",
       "enabled": "[variables('alertStatus')]",
       "source": {
           "query": "[variables('alertSource').Query]",
           "dataSourceId": "[variables('alertSource').SourceId]",
           "queryType":"[variables('alertSource').Type]"
       },
      "schedule":{
           "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
           "timeWindowInMinutes": "[variables('alertSchedule').Time]"    
       },
      "action":{
           "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
           "severity":"[variables('alertActions').SeverityLevel]",
           "aznsAction":{
               "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
               "emailSubject":"[variables('actionGrp').Subject]",
               "customWebhookPayload":"[variables('actionGrp').Webhook]"
           },
       "trigger":{
               "thresholdOperator":"[variables('alertTrigger').Operator]",
               "threshold":"[variables('alertTrigger').Threshold]"
           }
       }
     }
   }
 ]
}
```
> [!IMPORTANT]
> Label-veld met verborgen-koppeling naar de doelresource is verplicht in het gebruik van [queryregels gepland ](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) sjabloon voor API-oproep of een resource. 

Het bovenstaande voorbeeld-json als (bijvoorbeeld) sampleScheduledQueryRule.json ten behoeve van dit scenario kunnen worden opgeslagen en kunnen worden geïmplementeerd met [Azure Resource Manager in Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).


## <a name="managing-log-alerts-using-powershell-cli-or-api"></a>Waarschuwingen met behulp van PowerShell, CLI of API beheren
Momenteel log waarschuwingen kunnen worden gemaakt met behulp van twee verschillende compatibele API's voor resourcebeheer, gebaseerd op welke analytics-platform in de waarschuwing wordt baseren (dat wil zeggen) Log Analytics of Application Insights.

Daarom de onderstaande sectie vindt u informatie over het gebruik van de API via Powershell of CLI voor Logboekwaarschuwingen voor elk platform voor streaminganalyse.

### <a name="powershell-cli-or-api-for-log-analytics"></a>PowerShell, CLI of API voor Log Analytics
De Log Analytics Alert REST-API is RESTful en zijn toegankelijk via de Azure Resource Manager REST API. De API kan dus worden benaderd vanaf een PowerShell-opdrachtregel en wordt de uitvoer zoekresultaten aan u in JSON-indeling, zodat u kunt de resultaten een programmatische manier op veel verschillende manieren gebruiken.

Meer informatie over [maken en beheren van regels voor waarschuwingen in Log Analytics met REST-API](../azure-monitor/platform/api-alerts.md), inclusief voorbeelden van toegang tot de API vanuit Powershell.

### <a name="powershell-cli-or-api-for-application-insights"></a>PowerShell, CLI of API voor Application Insights
[Azure Monitor - geplande queryregels API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) is een REST-API en volledig compatibel met Azure Resource Manager REST API. Daarom kan deze worden gebruikt via Powershell met Resource Manager-cmdlet, evenals Azure CLI.

Hieronder gebruik via Azure Resource Manager PowerShell-cmdlet voor het voorbeeld hierboven Resourcesjabloon (sampleScheduledQueryRule.json) wordt weergegeven de [Resource sjabloonsectie](#azure-resource-template-for-application-insights) :
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Hieronder gebruik via Azure Resource Manager-opdracht in de Azure CLI voor het voorbeeld hierboven Resourcesjabloon (sampleScheduledQueryRule.json) wordt weergegeven de [Resource sjabloonsectie](#azure-resource-template-for-application-insights) :

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Op de goede werking 201 te maken van waarschuwingsregel nieuwe status wordt geretourneerd of 200 wordt geretourneerd als een bestaande waarschuwingsregel is gewijzigd.


  
## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure-waarschuwingen](monitor-alerts-unified-log.md)
* Inzicht in [webhookacties voor logboekwaarschuwingen](monitor-alerts-unified-log-webhook.md)
* Meer informatie over [Application Insights](../application-insights/app-insights-analytics.md)
* Meer informatie over [Log Analytics](../azure-monitor/log-query/log-query-overview.md). 

