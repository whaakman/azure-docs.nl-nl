---
title: Configureren van webhooks op waarschuwingen van Azure metrische | Microsoft Docs
description: Waarschuwingen van Azure met andere niet-Azure-systemen worden omgeleid.
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
ms.openlocfilehash: 06ec1263046f7878871de628b6a0ac25682b2f83
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Configureren van een webhook op een Azure metrische waarschuwing
Webhooks kunt u een Azure waarschuwingsmeldingen versturen met andere systemen voor na verwerking of aangepaste acties. U kunt een webhook gebruiken op een waarschuwing aan het wordt doorgestuurd naar de services die de SMS-bericht verzenden, meld bugs, een team via chat/berichtenservices melden of komen een aantal andere acties. Dit artikel wordt beschreven hoe u een webhook instelt voor een Azure metrische waarschuwing en hoe de nettolading voor de HTTP POST naar een webhook eruit ziet. Voor informatie over de installatie en het schema voor een Azure Activity Log-signaal (waarschuwing van gebeurtenissen) [deze pagina vindt u in plaats daarvan](insights-auditlog-to-webhook-email.md).

Waarschuwingen van Azure HTTP POST de inhoud van de waarschuwing in JSON-indeling, schema hieronder gedefinieerd voor een webhook URI die u opgeeft bij het maken van de waarschuwing. Deze URI moet een geldige HTTP of HTTPS-eindpunt. Azure boekt één vermelding per aanvraag wanneer een waarschuwing is geactiveerd.

## <a name="configuring-webhooks-via-the-portal"></a>Configureren van webhooks via de portal.
U kunt toevoegen of bijwerken van de webhook URI in het scherm Create/Update waarschuwingen in de [portal](https://portal.azure.com/).

![Een waarschuwingsregel toevoegen](./media/insights-webhooks-alerts/Alertwebhook.png)

U kunt ook een waarschuwing moet worden gepost, een webhook URI met de [Azure PowerShell-Cmdlets](insights-powershell-samples.md#create-metric-alerts), [platformoverschrijdende CLI](insights-cli-samples.md#work-with-alerts), of [REST-API van Azure-Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Verifiëren van de webhook
De webhook kunt verifiëren met behulp van verificatie op basis van tokens. De webhook URI wordt opgeslagen met een token ID, bijv. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>De nettolading van schema
De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle waarschuwingen op basis van metrische gegevens.

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


| Veld | Verplicht | Vaste Set van waarden | Opmerkingen |
|:--- |:--- |:--- |:--- |
| status |J |'Geactiveerd', 'Opgelost' |Status van de waarschuwing die is gebaseerd op de voorwaarden die u hebt ingesteld. |
| Context |J | |De context van de waarschuwing. |
| tijdstempel |J | |De tijd waarop de waarschuwing is geactiveerd. |
| id |J | |Elke waarschuwingsregel heeft een unieke id. |
| naam |J | |De naam van de waarschuwing. |
| description |J | |Beschrijving van de waarschuwing. |
| conditionType |J |'Metrische', 'Gebeurtenis' |Twee typen waarschuwingen worden ondersteund. Op basis van een metrische voorwaarde en de andere op basis van een gebeurtenis in het gebeurtenissenlogboek. Gebruik deze waarde om te controleren of de waarschuwing is gebaseerd op metriek of gebeurtenis. |
| voorwaarde |J | |De specifieke velden om te controleren voor op basis van de conditionType. |
| metricName |voor metrische waarschuwingen | |De naam van de metrische gegevens die definieert wat de regel wordt bewaakt. |
| metricUnit |voor metrische waarschuwingen |"Bytes", "BytesPerSecond", "Count", "CountPerSecond", "Percent", "Seconds" |De eenheid die is toegestaan in de metrische gegevens. [Toegestane waarden worden hier vermeld](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |voor metrische waarschuwingen | |De werkelijke waarde van de metrische gegevens die de waarschuwing heeft veroorzaakt. |
| Drempelwaarde |voor metrische waarschuwingen | |De drempelwaarde waarbij de waarschuwing wordt geactiveerd. |
| windowSize |voor metrische waarschuwingen | |De periode die wordt gebruikt voor het bewaken van de waarschuwing activiteit op basis van de drempelwaarde. Moet tussen 5 minuten en 1 dag. ISO 8601-duurnotatie. |
| timeAggregation |voor metrische waarschuwingen |'Gemiddeld', 'Laatste', 'Maximum', 'Minimale', 'None', 'Totaal' |Hoe de gegevens die worden verzameld moeten worden gecombineerd gedurende een bepaalde periode. De standaardwaarde is de gemiddelde. [Toegestane waarden worden hier vermeld](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| operator |voor metrische waarschuwingen | |De operator die wordt gebruikt voor de huidige metrische gegevens vergelijken met de ingestelde drempelwaarde. |
| subscriptionId |J | |Azure-abonnement-ID. |
| resourceGroupName |J | |De naam van de resourcegroep voor de betrokken resource. |
| resourceName |J | |De naam van de bron van de betrokken resource. |
| resourceType |J | |Het brontype van de betrokken resource. |
| resourceId |J | |Bron-ID van de betrokken resource. |
| resourceRegion |J | |Regio of de locatie van de betrokken resource. |
| portalLink |J | |Directe koppeling naar de overzichtspagina portal-resource. |
| properties |N |Optioneel |Een set `<Key, Value>` paren (dat wil zeggen `Dictionary<String, String>`) die bevat details over de gebeurtenis. De van eigenschappenveld is optioneel. In een aangepaste UI of de logische app op basis van een werkstroom, kunnen gebruikers die kunnen worden doorgegeven via de nettolading van de sleutel en-waarden invoeren. De alternatieve manier om door te geven van aangepaste eigenschappen terug naar de webhook is via de webhook uri zelf (als queryparameters) |

> [!NOTE]
> Het eigenschappenveld kan alleen worden ingesteld met behulp van de [REST-API van Azure-Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over waarschuwingen van Azure en webhooks in de video [Azure waarschuwingen integreren met PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
* [Azure Automation-scripts (Runbooks) op waarschuwingen van Azure uitvoeren](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Logische Apps gebruiken voor het verzenden van een SMS-bericht via Twilio door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
* [Logische App gebruiken voor het verzenden van een toegestane bericht door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
* [Logische App gebruiken voor het verzenden van een bericht naar een Azure-wachtrij door een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
