---
title: Azure Event raster schema voor IoT Hub | Microsoft Docs
description: Pagina met naslaginformatie voor de eigenschappen van IoT Hub en event schema-indeling
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.service: event-grid
ms.topic: reference
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 812ca3ba546112f54a76319fda853d441ce34f1b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="azure-event-grid-event-schema-for-iot-hub"></a>Azure Event raster gebeurtenis schema voor IoT-Hub

Dit artikel bevat de eigenschappen en het schema voor gebeurtenissen van Azure IoT Hub. Zie voor een inleiding tot gebeurtenis schema's, [Azure gebeurtenis raster gebeurtenis schema](event-schema.md). 

## <a name="available-event-types"></a>Typen beschikbare gebeurtenissen

Azure IoT Hub verzendt de volgende typen gebeurtenissen:

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Gepubliceerd als een apparaat is geregistreerd bij een IoT-hub. |
| Microsoft.Devices.DeviceDeleted | Wanneer een apparaat wordt verwijderd uit een IoT-hub gepubliceerd. | 

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Het schema voor gebeurtenissen DeviceCreated en DeviceDeleted hebben dezelfde structuur. Deze voorbeeldgebeurtenis ziet u het schema van een gebeurtenis die optreedt wanneer een apparaat is geregistreerd bij een IoT-hub:

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

### <a name="event-properties"></a>Eigenschappen van gebeurtenis

Alle gebeurtenissen bevatten dezelfde op het hoogste niveau gegevens: 

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| id | tekenreeks | De unieke id voor de gebeurtenis. |
| Onderwerp | tekenreeks | Volledige resource-pad naar de gegevensbron. Dit veld is niet beschrijfbaar. Gebeurtenis raster bevat deze waarde. |
| Onderwerp | tekenreeks | Publisher gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| EventType | tekenreeks | Een van de typen van de geregistreerde gebeurtenis van de bron van deze gebeurtenis. |
| eventTime | tekenreeks | De tijd dat de gebeurtenis wordt gegenereerd, gebaseerd op de UTC-tijd van de provider. |
| gegevens | object | Gebeurtenisgegevens IoT Hub.  |
| dataVersion | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Gebeurtenis raster definieert het schema van de eigenschappen op het hoogste niveau. Gebeurtenis raster bevat deze waarde. |

De inhoud van het gegevensobject zijn verschillend voor elke gebeurtenisuitgever. Het gegevensobject bevat gebeurtenissen die IoT Hub, de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| hubName | tekenreeks | Naam van de IoT-Hub waar het apparaat is gemaakt of verwijderd. |
| deviceId | tekenreeks | De unieke id van het apparaat. Deze hoofdlettergevoelig tekenreeks mag maximaal 128 tekens lang en ondersteunt de ASCII-7-bits alfanumerieke tekens, plus de volgende speciale tekens bevatten: `- : . + % _ # * ? ! ( ) , = @ ; $ '`. |
| operationTimestamp | tekenreeks | De ISO8601 tijdstempel van de bewerking. |
| opType | tekenreeks | Het gebeurtenistype is opgegeven voor deze bewerking door de IoT-Hub: ofwel `DeviceCreated` of `DeviceDeleted`.
| Twin | object | Informatie over de apparaat-twin die het cloud-represenation van metagegevens van apparaten van toepassing is. | 
| apparaat-id | tekenreeks | De unieke id van het apparaat twin. | 
| ETag | tekenreeks | Een stukje informatie op die de inhoud van het apparaat twin beschrijft. Elke etag is uniek per apparaat twin gegarandeerd. | 
| status | tekenreeks | Hiermee wordt aangegeven of de apparaat-twin is ingeschakeld of uitgeschakeld. | 
| statusUpdateTime | tekenreeks | De ISO8601 tijdstempel van de status van het laatste apparaat twin bijwerken. |
| connectionState | tekenreeks | Hiermee wordt aangegeven of het apparaat is verbonden of verbroken. | 
| lastActivityTime | tekenreeks | De ISO8601 tijdstempel van de laatste activiteit. | 
| cloudToDeviceMessageCount | geheel getal | Telling van cloud naar verzonden apparaatberichten aan dit apparaat. | 
| authenticationType | tekenreeks | Type-verificatie gebruikt voor dit apparaat: beide `SAS`, `SelfSigned`, of `CertificateAuthority`. |
| x509Thumbprint | tekenreeks | Vingerafdruk van het is een unieke waarde voor de x509 certificaat, meestal gebruikt voor een bepaald certificaat niet vinden in een certificaatarchief. De vingerafdruk van het dynamisch wordt gegenereerd op basis van het SHA1-algoritme en niet fysiek in het certificaat bestaat. | 
| primaryThumbprint | tekenreeks | Primaire vingerafdruk voor de x509 certificaat. |
| secondaryThumbprint | tekenreeks | Secundaire vingerafdruk voor de x509 certificaat. | 
| versie | geheel getal | Een geheel getal dat wordt verhoogd bij elke keer dat het apparaat twin wordt bijgewerkt. |
| gewenste | object | Een gedeelte van de eigenschappen die kunnen worden alleen door de toepassing van de back-end geschreven en gelezen door het apparaat. | 
| gerapporteerd | object | Een gedeelte van de eigenschappen die kunnen worden alleen door het apparaat is geschreven en gelezen door de toepassing van de back-end. |
| lastUpdated | tekenreeks | De ISO8601 tijdstempel van de laatste apparaat twin eigenschap bijwerken. | 

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)
* Zie voor meer informatie over hoe IoT Hub en Event raster samenwerken, [reageren op gebeurtenissen van de IoT Hub met behulp van de gebeurtenis raster acties starten](../iot-hub/iot-hub-event-grid.md).