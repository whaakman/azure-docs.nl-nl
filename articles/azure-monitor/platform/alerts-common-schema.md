---
title: Algemene waarschuwing schema voor Azure monitor-waarschuwingen
description: Informatie over de algemene waarschuwing schema, waarom u gebruiken moet en hoe u deze inschakelen
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: 8f8dcff0b72ea92e835c0702113a9cb6a7678e86
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851727"
---
# <a name="common-alert-schema"></a>Algemene waarschuwing schema

Dit artikel wordt beschreven wat het algemene schema van de waarschuwing is, de voordelen van het gebruik van deze en het inschakelen ervan.

## <a name="what-is-the-common-alert-schema"></a>Wat is het algemene schema van de waarschuwing?

Het algemene schema van de waarschuwing standaardiseert vandaag nog de ervaring voor waarschuwingen in Azure. De drie typen waarschuwingen in Azure vandaag (metrische gegevens, logboeken en het activiteitenlogboek) hebben in het verleden had hun eigen e-mailsjablonen, webhook-schema's, enzovoort. U kunt nu waarschuwingen met een consistent schema met het algemene schema van de waarschuwing ontvangen.

Beschrijving van elke waarschuwing instantie **de resource die betrokken** en **de oorzaak van de waarschuwing**, en deze instanties worden beschreven in het algemene schema in de volgende secties:
* **Essentials**: Een set **velden gestandaardiseerd**, gebruikt voor alle Waarschuwingstypen beschrijven **welke resource** de waarschuwing is ingeschakeld, samen met aanvullende algemene waarschuwing metagegevens (bijvoorbeeld ernst of beschrijving). 
* **Ontvang een waarschuwing Context**: Een set velden waarin de **ertoe leiden dat van de waarschuwing**, met velden die verschillen **op basis van het Waarschuwingstype**. Een waarschuwing voor metrische gegevens hoeft bijvoorbeeld velden, zoals de naam van de meetwaarde en de metrische waarde in de context van de waarschuwing, terwijl er een waarschuwing voor activiteitenlogboek informatie over de gebeurtenis die de waarschuwing heeft gegenereerd. 

De integratie van typische scenario's die we luister naar klanten hebben betrekking op de routering van de waarschuwing exemplaar naar de betrokken team op basis van bepaalde pivot (bijvoorbeeld resourcegroep), waarna de verantwoordelijk team begint met werken op het. Met het algemene waarschuwing schema, kunt u hebben gestandaardiseerd routeringlogica voor Waarschuwingstypen door gebruik te maken van de essentiële velden verlaten van de velden in context, zoals is bedoeld voor de betrokken teams voor verder onderzoek.

Dit betekent dat u kunt mogelijk minder integraties, waardoor het proces van het beheer en onderhoud ervan een _veel_ eenvoudiger taak. Bovendien toekomstige alert payload enrichments (bijvoorbeeld, aanpassing, diagnostische verrijking, enzovoort) wordt alleen het oppervlak van in het algemene schema.

## <a name="what-enhancements-does-the-common-alert-schema-bring"></a>Welke verbeteringen zijn brengen in het algemene schema van de waarschuwing?

Het algemene schema van de waarschuwing wordt voornamelijk zelf manifest in uw meldingen van waarschuwingen. Hieronder vindt u de uitbreidingen die wordt weergegeven:

| Bewerking | Verbeteringen|
|:---|:---|
| Sms | Een consistente SMS-sjabloon voor alle typen waarschuwingen. |
| Email | Een consistente en gedetailleerde e-mailsjabloon, zodat u eenvoudig problemen in een oogopslag vaststellen. Ingesloten deep-koppelingen naar de waarschuwing exemplaar op de portal en de betreffende resource zorgen ervoor dat u snel kunt gaan in het herstelproces van. |
| Webhook/Logic App/Azure-functie | Een consistente JSON-structuur voor alle typen waarschuwingen, waarmee u eenvoudig integraties over de verschillende typen waarschuwingen. |

Het nieuwe schema wordt ook een rijkere ervaring voor waarschuwingen verbruik in zowel de Azure-portal en Azure mobile app inschakelen in de nabije toekomst. 

[Meer informatie over de schemadefinities voor Webhooks/Logic Apps/Azure Functions.](https://aka.ms/commonAlertSchemaDefinitions)

> [!NOTE]
> De volgende acties bieden geen ondersteuning voor het algemene waarschuwing schema: ITSM-Connector, Automation-Runbook.

## <a name="how-do-i-enable-the-common-alert-schema"></a>Hoe kan ik het algemene schema van de waarschuwing inschakelen?

U kunt ervoor kiezen of opt-out aan het algemene schema Waarschuwing via actiegroepen op zowel de portal en de REST-API. De wisselknop om over te schakelen naar het nieuwe schema bestaat op het actieniveau van een. U moet bijvoorbeeld afzonderlijk aanmelden voor een e-mailactie en een webhookactie.

> [!NOTE]
> 1. De volgende waarschuwingstypen ondersteuning voor het algemene schema standaard (geen opt-in vereist):
>     * Waarschuwingen voor anomaliedetectie
> 1. De volgende waarschuwingstypen ondersteuning momenteel geen voor het algemene schema:
>     * Status van de service-waarschuwingen
>     * Activiteitenlogboek - beveiligingswaarschuwingen
>     * Waarschuwingen die zijn gegenereerd door [Azure Monitor voor virtuele machines](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)

### <a name="through-the-azure-portal"></a>Via de Azure-portal

![Algemene waarschuwing schema opt-in](media/alerts-common-schema/portal-opt-in.png)

1. Open een bestaande of een nieuwe actie in een actiegroep. 
1. Selecteer 'Ja' voor de wisselknop om in te schakelen van het algemene schema van de waarschuwing, zoals wordt weergegeven.

### <a name="through-the-action-groups-rest-api"></a>Via de actiegroepen REST-API

U kunt ook de [actie groepen API](https://docs.microsoft.com/rest/api/monitor/actiongroups) te schrijven voor het algemene schema van de waarschuwing. Tijdens het maken van de [maken of bijwerken](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) REST API-aanroep kunt u instellen de vlag 'useCommonAlertSchema' op 'true' (opt-in) of 'onwaar' (voor opt-out) voor het gebruik van de volgende acties - webhook-e-mailadres/logic app/Azure-functie.

Bijvoorbeeld, de volgende aanvraagtekst aangebracht in de [maken of bijwerken](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) REST-API wordt het volgende doen:

* Het algemene waarschuwing schema voor de e-mailactie 'John Doe's e-mail' inschakelen
* Het algemene waarschuwing schema voor de e-mailactie 'E-mail van Jane Smith' uitschakelen
* Het algemene waarschuwing schema voor de webhookactie "Voorbeeld webhook" inschakelen

```json
{
  "properties": {
    "groupShortName": "sample",
    "enabled": true,
    "emailReceivers": [
      {
        "name": "John Doe's email",
        "emailAddress": "johndoe@email.com",
        "useCommonAlertSchema": true
      },
      {
        "name": "Jane Smith's email",
        "emailAddress": "janesmith@email.com",
        "useCommonAlertSchema": false
      }
    ],
    "smsReceivers": [
      {
        "name": "John Doe's mobile",
        "countryCode": "1",
        "phoneNumber": "1234567890"
      },
      {
        "name": "Jane Smith's mobile",
        "countryCode": "1",
        "phoneNumber": "0987654321"
      }
    ],
    "webhookReceivers": [
      {
        "name": "Sample webhook",
        "serviceUri": "http://www.example.com/webhook",
        "useCommonAlertSchema": true
      }
    ]
  },
  "location": "Global",
  "tags": {}
}
```





## <a name="next-steps"></a>Volgende stappen

- [Algemene waarschuwing schemadefinities voor Webhooks/Logic Apps/Azure Functions.](https://aka.ms/commonAlertSchemaDefinitions)



