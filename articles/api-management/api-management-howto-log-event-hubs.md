---
title: Hoe u gebeurtenissen registreren bij Azure Event Hubs in Azure API Management | Microsoft Docs
description: Leer hoe u gebeurtenissen registreren bij Azure Event Hubs in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: apimpm
ms.openlocfilehash: a8cda04ccc39e53962ec8c4b57d24df539f38825
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51233895"
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Hoe u gebeurtenissen registreren bij Azure Event Hubs in Azure API Management
Azure Event Hubs is een zeer schaalbare service voor inkomende gegevens die miljoenen gebeurtenissen per seconde kan opnemen, voor verwerking en analyse van de enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en toepassingen. Eventhubs fungeert als de 'voordeur' van een gebeurtenispijplijn en zodra de gegevens zijn verzameld in een event hub, kunnen worden omgezet en opgeslagen met elke gewenste realtime analyseprovider of batching/opslagadapters. Event Hubs koppelt de productie van een gebeurtenissenstroom los van het gebruik van deze gebeurtenissen, zodat de consumenten ervan toegang hebben tot de gebeurtenissen op basis van hun eigen planning.

In dit artikel is een aanvulling op de [Azure API Management integreren met Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video en wordt beschreven hoe u API Management-gebeurtenissen met Azure Event Hubs.

## <a name="create-an-azure-event-hub"></a>Een Azure Event Hub maken

Zie voor gedetailleerde stappen voor het maken van een event hub en verbindingsreeksen die u nodig hebt voor het verzenden en ontvangen van gebeurtenissen naar en van de Event Hub ophalen [maken van een Event Hubs-naamruimte en een event hub met behulp van de Azure-portal](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="create-an-api-management-logger"></a>Een API Management-logger maken
Nu dat u een Event Hub hebt, de volgende stap is het configureren van een [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) in uw API Management-service die gebeurtenissen naar de Event Hub zich kan aanmelden.

U kunt API Management zijn geconfigureerd met behulp van de [API Management REST API](https://aka.ms/smapi). Controleer voordat u de REST-API voor het eerst gebruikt, de [vereisten](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) en zorg ervoor dat u hebt [toegang ingeschakeld tot de REST-API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

Als u wilt een logger maken, moet u een HTTP PUT-aanvraag van de volgende URL-sjabloon:

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2017-03-01`

* Vervang `{your service}` met de naam van uw API Management service-exemplaar.
* Vervang `{new logger name}` met de gewenste naam voor uw nieuwe logger. U wilt verwijzen naar deze naam wanneer u configureert de [logboek naar eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) beleid

Voeg de volgende headers toe aan de aanvraag:

* Content-Type: application/json
* Autorisatie: SharedAccessSignature 58...
  * Voor instructies over het genereren van de `SharedAccessSignature` Zie [Azure API Management REST API-verificatie](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Geef de hoofdtekst van de aanvraag van de volgende sjabloon:

```json
{
  "loggerType" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `loggerType` moet worden ingesteld op `AzureEventHub`.
* `description` biedt een optionele beschrijving van het logboek en kan een tekenreeks met lengte nul indien gewenst.
* `credentials` bevat de `name` en `connectionString` van uw Azure Event Hub.

Wanneer u de aanvraag als het logboek is gemaakt, statuscode `201 Created` wordt geretourneerd. Hieronder ziet u een voorbeeld op basis van het bovenstaande voorbeeld van een aanvraag.

```json
{
    "id": "/loggers/{new logger name}",
    "loggerType": "azureEventHub",
    "description": "Sample logger description",
    "credentials": {
        "name": "Name of the Event Hub from the Portal",
        "connectionString": "{{Logger-Credentials-xxxxxxxxxxxxxxx}}"
    },
    "isBuffered": true,
    "resourceId": null
}
```

> [!NOTE]
> Zie voor andere mogelijke retourcodes en hun redenen [een Logger maken](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). Zie voor meer informatie over andere bewerkingen, zoals de lijst, bijwerken en verwijderen uit te voeren, de [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entity-documentatie.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Logboek-eventhubs-beleid configureren

Zodra uw logger is geconfigureerd in API Management, u kunt uw log-eventhubs-beleid configureren voor de gewenste gebeurtenissen. Het logboek naar Event hubs-beleid kan worden gebruikt in de beleidssectie voor binnenkomende of in de beleidssectie voor uitgaande.

1. Blader naar de APIM-instantie.
2. Selecteer het tabblad API.
3. Selecteer de API die u wilt toevoegen van het beleid. In dit voorbeeld wordt er een beleid om te worden toegevoegd de **Echo-API** in de **onbeperkt** product.
4. Selecteer **Alle bewerkingen**.
5. Selecteer het tabblad ontwerpen boven aan het scherm.
6. Klik in het venster van de verwerking van binnenkomend of uitgaand, op het driehoekje (naast het potlood).
7. Selecteer de Code-editor. Zie voor meer informatie, [hoe u beleid instellen of bewerken](set-edit-policies.md).
8. Plaats de cursor in de `inbound` of `outbound` beleidssectie.
9. Selecteer in het venster aan de rechterkant, **Geavanceerde beleidsregels** > **Log to EventHub**. Deze voegt de `log-to-eventhub` beleidssjabloon-instructie.

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```
Vervang `logger-id` met de waarde die u hebt gebruikt voor `{new logger name}` in de URL van de logger maken in de vorige stap.

U kunt een expressie die een tekenreeks geretourneerd als de waarde voor de `log-to-eventhub` element. In dit voorbeeld wordt een tekenreeks met de datum en tijd, servicenaam, aanvraag-id, aanvraag IP-adres en de naam van bewerking geregistreerd.

Klik op **opslaan** om op te slaan van de configuratie van het bijgewerkte beleid. Het beleid actief is en gebeurtenissen worden geregistreerd in de aangewezen Event Hub zodra deze is opgeslagen.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over Azure Event Hubs
  * [Aan de slag met Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Berichten ontvangen met EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Programmeerhandleiding voor Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Meer informatie over de integratie van API Management en Event Hubs
  * [Verwijzing naar de Logger-entiteit](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity)
  * [documentatie voor log Event hub beleid](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Uw API's met Azure API Management, Eventhubs en Runscope controleren](api-management-log-to-eventhub-sample.md)  
* Meer informatie over [integratie met Azure Application Insights](api-management-howto-app-insights.md)

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
