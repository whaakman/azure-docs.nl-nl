---
title: Azure IoT Hub en Event raster | Microsoft Docs
description: Gebruik Azure gebeurtenis raster voor het activeren van processen die op basis van acties die in IoT-Hub plaatsvinden.
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 096fcce979bd488a0fe9dead2b1232a057d0ae02
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions---preview"></a>Reageren op gebeurtenissen van de IoT Hub met behulp van de gebeurtenis raster acties starten - Preview

Azure IoT Hub worden geïntegreerd met Azure Event raster zodat u kunt meldingen naar andere services verzenden en downstream processen te activeren. Configureer uw business-toepassingen om te luisteren naar IoT Hub gebeurtenissen, zodat u op kritieke gebeurtenissen op een betrouwbare, veilige en schaalbare manier reageren kunt. Bijvoorbeeld: maken van een toepassing meerdere acties zoals het bijwerken van een database, het maken van een ticket en het leveren van een e-mailmelding telkens wanneer een nieuwe IoT-apparaat wordt geregistreerd met uw iothub uit te voeren. 

[Azure Event raster] [ lnk-eg-overview] is een volledig beheerde gebeurtenis routing-service die gebruikmaakt van een publiceren-abonneren model. Gebeurtenis raster bevat ingebouwde ondersteuning voor Azure-services zoals [Azure Functions](../azure-functions/functions-overview.md) en [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md), en waarschuwingen op niet-Azure-services met behulp van webhooks kunt leveren. Zie voor een volledige lijst van de gebeurtenis-handlers die ondersteuning biedt voor gebeurtenis raster [een inleiding tot Azure gebeurtenis raster][lnk-eg-overview]. 

![Azure Event raster-architectuur](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Regionale beschikbaarheid

De gebeurtenis raster integratie is openbare Preview is dus beschikbaar in een beperkt aantal regio's. De integratie werkt voor IoT hubs zich bevinden in de volgende gebieden:

* VS - midden
* VS - oost
* VS - oost 2
* West-centraal VS
* VS - west
* VS - west 2

## <a name="event-types"></a>Typen gebeurtenissen

IoT Hub publiceert de volgende typen gebeurtenissen: 

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Gepubliceerd als een apparaat is geregistreerd bij een IoT-hub. |
| Microsoft.Devices.DeviceDeleted | Wanneer een apparaat wordt verwijderd uit een IoT-hub gepubliceerd. | 

Gebruik de Azure-portal of Azure CLI om te configureren welke gebeurtenissen voor het publiceren van elke IoT-hub. Voor een voorbeeld kunt u de zelfstudie [e-mailmeldingen verzenden over gebeurtenissen van Azure IoT Hub met Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md). 

## <a name="event-schema"></a>Gebeurtenisschema

IoT Hub-gebeurtenissen bevatten alle informatie die u nodig hebt om te reageren op wijzigingen in de levenscyclus van uw apparaat. U kunt een gebeurtenis met IoT Hub door te controleren dat de eigenschap eventType met begint identificeren **Microsoft.Devices**. Zie voor meer informatie over het gebruik van eigenschappen van gebeurtenis raster gebeurtenis de [gebeurtenis raster gebeurtenis schema](../event-grid/event-schema.md).

### <a name="device-created-schema"></a>Schema voor apparaten die zijn gemaakt

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
    "deviceId": "LogicAppTestDevice",
    "operationTimestamp": "2018-01-02T19:17:44.4383997Z",
    "opType": "DeviceCreated"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Zie voor een gedetailleerde beschrijving van elke eigenschap [Azure gebeurtenis raster gebeurtenis schema voor IoT Hub](../event-grid/event-schema-iot-hub.md)

## <a name="filter-events"></a>Filtergebeurtenissen

IoT Hub gebeurtenisabonnementen kunnen gebeurtenissen op basis van het type en apparaat gebeurtenisnaam filteren. Onderwerp-filters in gebeurtenis raster werk op basis van **voorvoegsel** en **achtervoegsel** overeenkomt, zodat de gebeurtenissen met een overeenkomende onderwerp worden geleverd op de abonnee. 

Het onderwerp van de IoT-gebeurtenissen heeft de indeling:

```json
devices/{deviceId}
```

### <a name="tips-for-consuming-events"></a>Tips voor het verbruik van gebeurtenissen

Toepassingen die IoT Hub gebeurtenissen verwerken, moeten deze aanbevolen procedures volgen:

* Meerdere abonnementen kunnen worden geconfigureerd voor de route-gebeurtenissen naar de dezelfde gebeurtenis-handler, dus is het belangrijk dat niet wordt ervan uitgegaan dat er gebeurtenissen van een bepaalde bron zijn. Controleer altijd het onderwerp van het bericht om ervoor te zorgen dat ze afkomstig zijn van de IoT-hub die u verwacht. 
* Niet, wordt ervan uitgegaan dat alle gebeurtenissen die u ontvangt, zijn de typen die u verwacht. Controleer altijd de eventType voordat het bericht wordt verwerkt.
* Berichten kunt volgorde of na een vertraging optreden. De etag-veld gebruiken om te begrijpen of uw gegevens over objecten up-to-date.



## <a name="next-steps"></a>Volgende stappen

* [Probeer de zelfstudie IoT Hub-gebeurtenissen](../event-grid/publish-iot-hub-events-to-logic-apps.md)
* [Meer informatie over de gebeurtenis raster][lnk-eg-overview]
* [Vergelijk de verschillen tussen routering IoT Hub gebeurtenissen en -berichten][lnk-eg-compare]

<!-- Links -->
[lnk-eg-overview]: ../event-grid/overview.md
[lnk-eg-compare]: iot-hub-event-grid-routing-comparison.md