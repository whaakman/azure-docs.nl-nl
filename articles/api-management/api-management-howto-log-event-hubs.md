---
title: Het registreren van gebeurtenissen voor Azure Event Hubs in Azure API Management | Microsoft Docs
description: Informatie over het vastleggen van gebeurtenissen in Azure Event Hubs in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: apimpm
ms.openlocfilehash: 77c3e41dd4b1fdf7e518de67b353f69fcb758c60
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Het registreren van gebeurtenissen voor Azure Event Hubs in Azure API Management
Azure Event Hubs is een zeer schaalbare service voor inkomende gegevens die miljoenen gebeurtenissen per seconde kan opnemen, voor verwerking en analyse van de enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en toepassingen. Event Hubs fungeert als de 'voordeur' van een gebeurtenispijplijn en zodra gegevens zijn verzameld in een event hub, kunnen worden omgezet en opgeslagen met een realtime-analyseprovider of batchverwerking/opslagadapters. Event Hubs koppelt de productie van een gebeurtenissenstroom los van het gebruik van deze gebeurtenissen, zodat de consumenten ervan toegang hebben tot de gebeurtenissen op basis van hun eigen planning.

Dit artikel is een aanvulling op de [Azure API Management integreren met Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video en wordt beschreven hoe u API Management-gebeurtenissen met Azure Event Hubs.

## <a name="create-an-azure-event-hub"></a>Een Azure Event Hub maken

Zie voor gedetailleerde stappen voor het maken van een event hub en ophalen van verbindingsreeksen die u nodig hebt voor het verzenden en ontvangen van gebeurtenissen naar en van de Event Hub [een Event Hubs-naamruimte en een event hub met de Azure portal maken](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="create-an-api-management-logger"></a>Maken van een API Management-logboek
Nu dat u een Event Hub hebt, de volgende stap is voor het configureren van een [berichtenlogboek](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) in uw API Management-service zodat deze gebeurtenissen in de Event Hub vastleggen kan.

API Management voorkomen zijn geconfigureerd met behulp van de [API Management REST API](http://aka.ms/smapi). Controleer voordat u de REST-API voor het eerst gebruikt, de [vereisten](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) en zorg ervoor dat er [toegang tot de REST-API ingeschakeld](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

Moet een HTTP PUT-aanvraag van de volgende URL-sjabloon voor het maken van een logboek:

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* Vervang `{your service}` met de naam van uw API Management-service-exemplaar.
* Vervang `{new logger name}` met de gewenste naam voor uw nieuwe berichtenlogboek. U verwijzen naar deze naam bij het configureren van de [logboek voor eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) beleid

De volgende headers toevoegen aan de aanvraag:

* Content-Type: application/json
* Autorisatie: SharedAccessSignature 58...
  * Voor instructies voor het genereren van de `SharedAccessSignature` Zie [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Geef de hoofdtekst van de aanvraag van de volgende sjabloon:

```json
{
  "loggertype" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `loggertype`moet worden ingesteld op `AzureEventHub`.
* `description`biedt een optionele beschrijving van het logboek en tekenreekslengte van nul kan zijn, indien gewenst.
* `credentials`bevat de `name` en `connectionString` van uw Azure Event Hub.

Wanneer u de aanvraag als een statuscode van is gemaakt door het logboek `201 Created` wordt geretourneerd.

> [!NOTE]
> Zie voor andere mogelijke retourcodes en hun redenen [maken van een logboek](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). Zie voor het uitvoeren van andere bewerkingen zoals lijst, bijwerken en verwijderen de [berichtenlogboek](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entiteit documentatie.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Logboek-eventhubs-beleid configureren

Zodra uw berichtenlogboek in API Management is geconfigureerd, kunt u uw logboek-eventhubs-beleid voor de gewenste gebeurtenissen logboekregistratie configureren. Het logboek voor eventhubs-beleid kan worden gebruikt in de sectie binnenkomende beleid of de beleidssectie voor uitgaande.

1. Blader naar de APIM-instantie.
2. Selecteer het tabblad API.
3. Selecteer de API die u wilt toevoegen van het beleid. In dit voorbeeld wordt een beleid voor toevoegt de **Echo-API** in de **onbeperkt** product.
4. Selecteer **alle bewerkingen**.
5. Selecteer het tabblad ontwerpen boven aan het scherm.
6. Klik in het venster inkomende of uitgaande verwerking op de driehoek (naast de pen).
7. Selecteer de Code-editor. Zie voor meer informatie [het instellen of beleidsregels bewerken](set-edit-policies.md).
8. Plaats de cursor in de `inbound` of `outbound` beleidssectie.
9. Selecteer in het venster aan de rechterkant **Geavanceerde beleidsregels** > **logboek EventHub**. Hierdoor wordt de `log-to-eventhub` beleidssjabloon-instructie.

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```
Vervang `logger-id` met de naam van het API Management-logboek die u in de vorige stap hebt geconfigureerd.

U kunt een expressie die een tekenreeks retourneert als de waarde voor de `log-to-eventhub` element. In dit voorbeeld wordt een tekenreeks met de datum en tijd, servicenaam, aanvraag-id, aanvraag IP-adres en de naam van bewerking geregistreerd.

Klik op **opslaan** de bijgewerkte beleidsconfiguratie opslaan. Het beleid actief is en worden gebeurtenissen vastgelegd op de aangewezen Event Hub zodra deze is opgeslagen.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over Azure Event Hubs
  * [Aan de slag met Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Berichten ontvangen met EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Programmeerhandleiding voor Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Meer informatie over de integratie van API Management en Event Hubs
  * [Entiteitsverwijzing berichtenlogboek](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity)
  * [documentatie voor logboek-eventhub-beleid](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Uw API's met Azure API Management, Event Hubs en Runscope bewaken](api-management-log-to-eventhub-sample.md)    

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
