---
title: Configureren van webhooks op waarschuwingen van Azure metrische | Microsoft Docs
description: Informatie over het omleiden van waarschuwingen van Azure naar andere, niet-Azure-systemen.
author: johnkemnetz
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 8b3ae540-1d19-4f3d-a635-376042f8a5bb
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: johnkem
ms.openlocfilehash: 049803e7701c68559103d9b1fa5dfacf820d0548
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Configureren van een webhook op een Azure metrische waarschuwing
U kunt webhooks gebruiken voor het routeren van een Azure waarschuwingsmeldingen met andere systemen voor na verwerking of aangepaste acties. U kunt een webhook gebruiken op een waarschuwing aan het wordt doorgestuurd naar de services die de SMS-berichten aan te melden bugs, om de hoogte van een team via chat of messaging-services, of voor verschillende andere acties te verzenden. 

Dit artikel wordt beschreven hoe u een webhook instelt voor een Azure metrische waarschuwing. Ook ziet u hoe de nettolading voor de HTTP POST naar een webhook eruit ziet. Zie voor informatie over de installatie en het schema voor een Azure-activiteit logboek waarschuwing (waarschuwing van gebeurtenissen) [een webhook niet aanroepen voor een Azure-activiteit logboek waarschuwing](insights-auditlog-to-webhook-email.md).

Waarschuwingen van Azure gebruiken HTTP POST naar de inhoud van de waarschuwing in JSON-indeling naar een webhook URI die u opgeeft tijdens het maken van de waarschuwing te verzenden. Het schema is gedefinieerd verderop in dit artikel. De URI moet een geldige HTTP of HTTPS-eindpunt. Azure boekt één vermelding per aanvraag wanneer een waarschuwing is geactiveerd.

## <a name="configure-webhooks-via-the-azure-portal"></a>Configureren van webhooks via de Azure-portal
Toevoegen of bijwerken van de webhook URI, in de [Azure-portal](https://portal.azure.com/), gaat u naar **Create/Update waarschuwingen**.

![Een waarschuwingsregel deelvenster toevoegen](./media/insights-webhooks-alerts/Alertwebhook.png)

U kunt ook een waarschuwing moet worden gepost, met behulp van een webhook URI configureren [Azure PowerShell-cmdlets](insights-powershell-samples.md#create-metric-alerts), een [platformoverschrijdende CLI](insights-cli-samples.md#work-with-alerts), of [Monitor REST-API's van Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticate-the-webhook"></a>De webhook verifiëren
De webhook kunt verifiëren met behulp van verificatie op basis van tokens. De webhook URI wordt opgeslagen met een token ID. Bijvoorbeeld: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>De nettolading van schema
De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle waarschuwingen op basis van metrische gegevens:

```JSON
{
    "WebhookName": "Alert1515515157799",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                "metricName": "Requests",
                "metricUnit": "Count",
                "metricValue": "10",
                "threshold": "10",
                "windowSize": "15",
                "timeAggregation": "Average",
                "operator": "GreaterThanOrEqual"
            },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
        },
        "properties": {
            "key1": "value1",
            "key2": "value2"
        }
    }
}
```


| Veld | Verplicht | Vaste set van waarden | Opmerkingen |
|:--- |:--- |:--- |:--- |
| status |J |Geactiveerd, opgelost |De status van de waarschuwing op basis van de voorwaarden die u ingesteld. |
| Context |J | |De context van de waarschuwing. |
| tijdstempel |J | |De tijd waarop de waarschuwing is geactiveerd. |
| id |J | |Elke waarschuwingsregel heeft een unieke ID. |
| naam |J | |De naam van de waarschuwing. |
| description |J | |Een beschrijving van de waarschuwing. |
| conditionType |J |Metriek, gebeurtenis |Twee typen waarschuwingen worden ondersteund: metrische gegevens en gebeurtenissen. Metrische waarschuwingen zijn gebaseerd op een metrische voorwaarde. Waarschuwingen zijn gebaseerd op een gebeurtenis in het gebeurtenissenlogboek. Gebruik deze waarde om te controleren of de waarschuwing is gebaseerd op een waarde of op een gebeurtenis. |
| voorwaarde |J | |De specifieke velden om te controleren op basis van de **conditionType** waarde. |
| metricName |Voor metrische waarschuwingen | |De naam van de metrische gegevens die definieert wat de regel wordt bewaakt. |
| metricUnit |Voor metrische waarschuwingen |Bytes, BytesPerSecond, Count, CountPerSecond, procent, seconden |De eenheid die is toegestaan in de metrische gegevens. Zie [toegestane waarden](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Voor metrische waarschuwingen | |De werkelijke waarde van de metrische gegevens die de waarschuwing heeft veroorzaakt. |
| Drempelwaarde |Voor metrische waarschuwingen | |De drempelwaarde waarbij de waarschuwing wordt geactiveerd. |
| windowSize |Voor metrische waarschuwingen | |De periode die wordt gebruikt voor het bewaken van de waarschuwing activiteit op basis van de drempelwaarde. De waarde moet tussen 5 minuten en 1 dag. De waarde moet in de ISO 8601-notatie voor de duur. |
| timeAggregation |Voor metrische waarschuwingen |Gemiddelde, laatste, Maximum, Minimum, None, totaal |Hoe de gegevens die worden verzameld moeten worden gecombineerd gedurende een bepaalde periode. De standaardwaarde is de gemiddelde. Zie [toegestane waarden](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| operator |Voor metrische waarschuwingen | |De operator die wordt gebruikt voor de huidige metrische gegevens vergelijken met de ingestelde drempelwaarde. |
| subscriptionId |J | |De Azure-abonnement-ID. |
| resourceGroupName |J | |De naam van de resourcegroep voor de betreffende resource. |
| resourceName |J | |De resourcenaam van de betreffende resource. |
| resourceType |J | |Het brontype van de betreffende resource. |
| resourceId |J | |De resource-ID van de betreffende resource. |
| resourceRegion |J | |De regio of de locatie van de betreffende resource. |
| portalLink |J | |Een directe koppeling naar de overzichtspagina portal-resource. |
| properties |N |Optioneel |Een set van sleutel-waardeparen die informatie over de gebeurtenis heeft. Bijvoorbeeld `Dictionary<String, String>`. De van eigenschappenveld is optioneel. Gebruikers kunnen sleutel/waarde-paren die kunnen worden doorgegeven via de nettolading van de invoeren in een aangepaste UI of de logic app op basis van een werkstroom. Een andere manier om door te geven van aangepaste eigenschappen terug naar de webhook is via de webhook URI zelf (als queryparameters). |

> [!NOTE]
> U kunt instellen de **eigenschappen** veld alleen met behulp van [Monitor REST-API's van Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over waarschuwingen van Azure en webhooks in de video [waarschuwingen Azure integreren met PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080).
* Meer informatie over hoe [Azure Automation-scripts (runbooks) uitvoeren op Azure waarschuwingen](http://go.microsoft.com/fwlink/?LinkId=627081).
* Meer informatie over hoe [een logische app gebruiken voor het verzenden van een SMS-bericht via Twilio vanuit een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).
* Meer informatie over hoe [gebruik van een logische app een toegestane bericht verzenden vanuit een Azure waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).
* Meer informatie over hoe [gebruik van een logische app een bericht verzenden naar een Azure-wachtrij door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).
