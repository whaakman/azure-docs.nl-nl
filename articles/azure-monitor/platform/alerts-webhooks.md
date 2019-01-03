---
title: Hebt u een klassieke waarschuwing voor metrische gegevens op de hoogte stellen een niet-Azure-systeem met behulp van een webhook
description: Leer hoe u Azure metrische waarschuwingen naar andere, niet-Azure-systemen worden omgeleid.
author: snehithm
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/03/2017
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: 2df24cfe217a9955a3abb08d042836821b083946
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53580634"
---
# <a name="have-a-classic-metric-alert-notify-a-non-azure-system-using-a-webhook"></a>Hebt u een klassieke waarschuwing voor metrische gegevens op de hoogte stellen een niet-Azure-systeem met behulp van een webhook
U kunt webhooks gebruiken voor het routeren van een Azure waarschuwingsmelding met andere systemen voor na verwerking of aangepaste acties. U kunt een webhook gebruiken op een waarschuwing aan het wordt doorgestuurd naar services die de SMS-berichten, om aan te melden bugs, om de hoogte van een team via chatten of berichtenservices, of voor verschillende andere acties te verzenden. 

In dit artikel wordt beschreven hoe u een webhook instellen op een Azure waarschuwing voor metrische gegevens. Ook ziet u hoe de nettolading voor de HTTP POST naar een webhook eruitziet. Zie voor meer informatie over de installatie en het schema voor een Azure-activiteit (waarschuwing bij gebeurtenissen), een waarschuwing [een webhook aanroepen op een waarschuwing voor een Azure-activiteitenlogboek](alerts-log-webhook.md).

Azure-waarschuwingen Gebruik HTTP POST voor het verzenden van de inhoud van de waarschuwing in JSON-indeling naar een webhook-URI die u opgeeft bij het maken van de waarschuwing. Het schema is gedefinieerd verderop in dit artikel. De URI moet een geldige HTTP of HTTPS-eindpunt. Azure wordt één vermelding per aanvraag wanneer een waarschuwing wordt geactiveerd.

## <a name="configure-webhooks-via-the-azure-portal"></a>Configureren van webhooks via Azure portal
Toevoegen of bijwerken van de webhook URI, in de [Azure-portal](https://portal.azure.com/), gaat u naar **Create/Update waarschuwingen**.

![Een deelvenster waarschuwingsregel toevoegen](./media/alerts-webhooks/Alertwebhook.png)

U kunt ook een waarschuwing te posten naar de URI van een webhook met behulp van [Azure PowerShell-cmdlets](../../azure-monitor/platform/powershell-quickstart-samples.md#create-metric-alerts), een [platformoverschrijdende CLI](../../azure-monitor/platform/cli-samples.md#work-with-alerts), of [Azure Monitor REST API's](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticate-the-webhook"></a>Verifiëren van de webhook
De webhook kan verifiëren met behulp van verificatie op basis van tokens. De webhook URI wordt opgeslagen met een token-ID. Bijvoorbeeld: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>De nettolading van schema
De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle waarschuwingen op basis van metrische gegevens:

```JSON
{
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
```


| Veld | Verplicht | Vaste set waarden | Opmerkingen |
|:--- |:--- |:--- |:--- |
| status |J |Geactiveerd, is opgelost |De status van de waarschuwing op basis van de voorwaarden die u instelt. |
| Context |J | |De context van de waarschuwing. |
| tijdstempel |J | |De tijd waarop de waarschuwing is geactiveerd. |
| id |J | |Elke waarschuwingsregel heeft een unieke ID. |
| naam |J | |De naam van de waarschuwing. |
| description |J | |Een beschrijving van de waarschuwing. |
| conditionType |J |Metrische gegevens, gebeurtenis |Twee typen waarschuwingen worden ondersteund: metrische gegevens en gebeurtenissen. Metrische waarschuwingen zijn gebaseerd op een metrische gegevens. Waarschuwingen zijn gebaseerd op een gebeurtenis in het activiteitenlogboek. Gebruik deze waarde om te controleren of de waarschuwing is gebaseerd op een metrische waarde of op een gebeurtenis. |
| voorwaarde |J | |De specifieke velden om te controleren op basis van de **conditionType** waarde. |
| MetricName |Voor metrische waarschuwingen | |De naam van de metrische gegevens die definieert wat de regel controleert. |
| metricUnit |Voor metrische waarschuwingen |Bytes, BytesPerSecond, aantal, CountPerSecond, procent, seconden |De eenheid die is toegestaan in de metrische gegevens. Zie [toegestane waarden](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Voor metrische waarschuwingen | |De werkelijke waarde van de metrische gegevens die de waarschuwing heeft veroorzaakt. |
| Drempelwaarde |Voor metrische waarschuwingen | |De drempelwaarde waarmee de waarschuwing is geactiveerd. |
| Venstergrootte |Voor metrische waarschuwingen | |De periode die wordt gebruikt voor het bewaken van waarschuwingsactiviteit op basis van de drempelwaarde. De waarde moet tussen 5 minuten en 1 dag. De waarde moet in ISO 8601-notatie voor de duur. |
| timeAggregation |Voor metrische waarschuwingen |Gemiddelde, laatste, Maximum, Minimum, None, totaal |Hoe moeten de gegevens die worden verzameld worden gecombineerd na verloop van tijd. De standaardwaarde is de gemiddelde. Zie [toegestane waarden](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| Operator |Voor metrische waarschuwingen | |De operator die wordt gebruikt voor de huidige metrische gegevens vergelijken met de ingestelde drempelwaarde. |
| subscriptionId |J | |De Azure-abonnement-ID. |
| resourceGroupName |J | |De naam van de resourcegroep voor de betreffende resource. |
| resourceName |J | |De resourcenaam van de betreffende resource. |
| ResourceType |J | |Het brontype van de betreffende resource. |
| resourceId |J | |De resource-ID van de betreffende resource. |
| resourceRegion |J | |De regio of de locatie van de betreffende resource. |
| portalLink |J | |Een directe koppeling naar de pagina overzicht van portal-resource. |
| properties |N |Optioneel |Een set van sleutel/waarde-paren met informatie over de gebeurtenis. Bijvoorbeeld `Dictionary<String, String>`. Het eigenschappenveld is optioneel. In een aangepaste UI of de werkstroom voor logische Apps, kunnen gebruikers de sleutel/waarde-paren die kunnen worden doorgegeven via de nettolading van de invoeren. Een alternatieve manier om door te geven van aangepaste eigenschappen terug naar de webhook is via de webhook URI zelf (zoals queryparameters). |

> [!NOTE]
> U kunt instellen dat de **eigenschappen** veld alleen met behulp van [Azure Monitor REST API's](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over Azure-waarschuwingen en webhooks in de video [Azure integreert waarschuwingen met PagerDuty](https://go.microsoft.com/fwlink/?LinkId=627080).
* Meer informatie over het [Azure Automation-scripts (runbooks) uit te voeren op Azure-waarschuwingen](https://go.microsoft.com/fwlink/?LinkId=627081).
* Meer informatie over het [een logische app gebruiken voor het verzenden van een SMS-bericht via Twilio vanuit een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).
* Meer informatie over het [gebruik van een logische app een Slack-bericht verzenden vanuit een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).
* Meer informatie over het [gebruik van een logische app een bericht verzenden naar een Azure-wachtrij vanuit een Azure-waarschuwing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).
