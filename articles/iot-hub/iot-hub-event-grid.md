---
title: Azure IoT Hub en Event Grid | Microsoft Docs
description: Gebruik Azure Event Grid voor het activeren van processen die op basis van acties die in IoT Hub plaatsvinden.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/14/2018
ms.author: kgremban
ms.openlocfilehash: 3c12e98137f44ac094adaae282b5d56d30061e60
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44719848"
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions"></a>Reageren op gebeurtenissen van IoT-Hub met behulp van Event Grid om acties starten

Azure IoT Hub kan worden geïntegreerd met Azure Event Grid, zodat u kunt meldingen van gebeurtenissen naar andere services verzenden en downstream processen te activeren. Uw zakelijke toepassingen om te luisteren naar IoT Hub-gebeurtenissen, zodat u op kritieke gebeurtenissen op een betrouwbare, schaalbare en veilige manier reageren kunt configureren. Bijvoorbeeld, bouw een toepassing meerdere acties uitvoeren zoals het bijwerken van een database, het maken van een ticket en het leveren van een e-mailmelding telkens wanneer een nieuwe IoT-apparaat is geregistreerd bij uw IoT-hub. 

[Azure Event Grid] [ lnk-eg-overview] is een volledig beheerde service voor gebeurtenisroutering die gebruikmaakt van een publish-subscribe-model. Event Grid heeft ingebouwde ondersteuning voor Azure-services zoals [Azure Functions](../azure-functions/functions-overview.md) en [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md), en waarschuwingen kunt leveren aan niet-Azure-services met behulp van webhooks. Zie voor een volledige lijst van de gebeurtenis-handlers die ondersteuning biedt voor Event Grid, [een inleiding tot Azure Event Grid][lnk-eg-overview]. 

![Azure Event Grid-architectuur](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Regionale beschikbaarheid

De Event Grid-integratie is beschikbaar voor IoT-hubs in de regio's waar Event Grid wordt ondersteund. Zie voor de meest recente lijst met regio's, [een inleiding tot Azure Event Grid][lnk-eg-overview]. 

## <a name="event-types"></a>Gebeurtenistypen

IoT Hub publiceert de volgende typen gebeurtenissen: 

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Gepubliceerd wanneer een apparaat is geregistreerd bij een IoT-hub. |
| Microsoft.Devices.DeviceDeleted | Wanneer een apparaat wordt verwijderd uit een IoT-hub gepubliceerd. |
| Microsoft.Devices.DeviceConnected | Wanneer een apparaat is verbonden met een IoT-hub gepubliceerd. |
| Microsoft.Devices.DeviceDisconnected | Gepubliceerd wanneer een apparaat niet is verbonden met een IoT-hub. |

Gebruik de Azure portal of Azure CLI om te configureren welke gebeurtenissen om te publiceren vanaf elke IoT-hub. Raadpleeg de zelfstudie voor een voorbeeld [e-mailmeldingen verzenden over Azure IoT Hub-gebeurtenissen met Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md).

## <a name="event-schema"></a>Gebeurtenisschema

IoT Hub-gebeurtenissen bevatten alle informatie die u nodig hebt om te reageren op wijzigingen in de levenscyclus van uw apparaat. U kunt een gebeurtenis met IoT Hub door te controleren dat de eigenschap type gebeurtenis met begint identificeren **Microsoft.Devices**. Zie voor meer informatie over het gebruik van de eigenschappen van Event Grid-gebeurtenis, de [Event Grid-gebeurtenisschema](../event-grid/event-schema.md).

### <a name="device-connected-schema"></a>Verbonden apparaat-schema

Het volgende voorbeeld ziet u het schema van een verbonden apparaat-gebeurtenis: 

```json
[{  
  "id": "f6bbf8f4-d365-520d-a878-17bf7238abd8", 
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>", 
  "subject": "devices/LogicAppTestDevice", 
  "eventType": "Microsoft.Devices.DeviceConnected", 
  "eventTime": "2018-06-02T19:17:44.4383997Z", 
  "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice",
    "moduleId" : "DeviceModuleID",
  }, 
  "dataVersion": "1", 
  "metadataVersion": "1" 
}]
```

### <a name="device-created-schema"></a>Schema van het apparaat is gemaakt

Het volgende voorbeeld ziet u het schema van een apparaat dat wordt gemaakt van gebeurtenis: 

```json
[{
  "id": "56afc886-767b-d359-d59e-0da7877166b2",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceCreated",
  "eventTime": "2018-01-02T19:17:44.4383997Z",
  "data": {
    "twin": {
      "deviceId": "LogicAppTestDevice",
      "etag": "AAAAAAAAAAE=",
      "deviceEtag":"null",
      "status": "enabled",
      "statusUpdateTime": "0001-01-01T00:00:00",
      "connectionState": "Disconnected",
      "lastActivityTime": "0001-01-01T00:00:00",
      "cloudToDeviceMessageCount": 0,
      "authenticationType": "sas",
      "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
      },
      "version": 2,
      "properties": {
        "desired": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        },
        "reported": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        }
      }
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Zie voor een gedetailleerde beschrijving van elke eigenschap [Azure Event Grid-gebeurtenisschema voor IoT-Hub](../event-grid/event-schema-iot-hub.md)

## <a name="filter-events"></a>Filtergebeurtenissen

IoT Hub-gebeurtenisabonnementen kunnen filteren, gebeurtenissen op basis van de naam van de type en het apparaat van de gebeurtenis. Onderwerp filters in Event Grid werken op basis van **begint met** (voorvoegsel) en **eindigt met** (achtervoegsel) overeenkomt met. Het filter wordt gebruikt een `AND` -operator, zodat gebeurtenissen met een onderwerp die overeenkomen met het voorvoegsel en het achtervoegsel wordt geleverd aan de abonnee. 

De indeling maakt gebruik van het onderwerp van IoT-gebeurtenissen:

```json
devices/{deviceId}
```
## <a name="limitations-for-device-connected-and-device-disconnected-events"></a>Beperkingen voor het apparaat is aangesloten en apparaat verbroken gebeurtenissen

Voor het ontvangen van verbonden apparaten en gebeurtenissen van apparaat is verbroken, moet u de koppeling D2C of C2D-koppeling openen voor uw apparaat. Als uw apparaat MQTT-protocol gebruikt, blijft de IoT Hub de C2D koppeling openen. U kunt voor AMQP de C2D-koppeling openen door het aanroepen van de [Async-API ontvangen](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync?view=azure-dotnet). De koppeling D2C is geopend als u telemetrie verzendt. Als de apparaatverbinding is flikkerende, dat wil zeggen het apparaat verbinding maakt en vaak de verbinding verbreekt, ontvangt niet elk één verbinding staat, maar wordt de status van de verbinding die elke minuut van momentopname is gepubliceerd. In het geval van een storing van de IoT Hub publiceren we de status van het apparaat verbinding zodra de onderbreking is over. Als het apparaat wordt verbroken tijdens deze storing, wordt het apparaat niet-verbonden gebeurtenis binnen 10 minuten worden gepubliceerd.

## <a name="tips-for-consuming-events"></a>Tips voor het gebruik van gebeurtenissen

Toepassingen die werken met IoT Hub-gebeurtenissen moeten deze aanbevolen procedures te volgen:

* Meerdere abonnementen kunnen worden geconfigureerd om gebeurtenissen routeren naar de dezelfde gebeurtenis-handler, dus is het belangrijk dat u niet wordt ervan uitgegaan dat er gebeurtenissen van een bepaalde bron zijn. Controleer altijd het onderwerp van het bericht om ervoor te zorgen dat het afkomstig is van de IoT-hub die u verwacht. 
* Geen, wordt ervan uitgegaan dat alle gebeurtenissen die u ontvangt, zijn de typen die u verwacht. Controleer het type gebeurtenis altijd voordat het bericht wordt verwerkt.
* Berichten kunnen niet de juiste volgorde of na een vertraging. De etag-veld gebruiken om te begrijpen als uw gegevens over objecten bijgewerkt is.

## <a name="next-steps"></a>Volgende stappen

* [Raadpleeg de zelfstudie IoT Hub-gebeurtenissen](../event-grid/publish-iot-hub-events-to-logic-apps.md)
* [Informatie over het rangschikken van gebeurtenissen ‘apparaat verbonden’ en ‘verbinding met apparaat verbroken’.](../iot-hub/iot-hub-how-to-order-connection-state-events.md)
* [Meer informatie over Event Grid][lnk-eg-overview]
* [Vergelijk de verschillen tussen routering IoT Hub-gebeurtenissen en -berichten][lnk-eg-compare]

<!-- Links -->
[lnk-eg-overview]: ../event-grid/overview.md
[lnk-eg-compare]: iot-hub-event-grid-routing-comparison.md