---
title: Azure IoT Edge-module technische activa maken | Microsoft Docs
description: Maak de technische assets voor een IoT Edge-module.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 877e42dc8d6e69834cf989122f855ada560b813c
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54201241"
---
# <a name="prepare-your-iot-edge-module-technical-assets"></a>Voorbereiden van uw IoT Edge-module technische activa

Dit artikel beschrijft de vereisten die uw IoT Edge module technische zaken voldoen moeten voordat het werd gepubliceerd op Azure Marketplace.

## <a name="understanding-iot-edge-modules-and-getting-started"></a>Inzicht krijgen in IoT Edge-modules en aan de slag

Een IoT Edge-module is een compatibel is met Docker-container die is gemaakt om uit te voeren op een IoT Edge-apparaat.

- Zie voor meer informatie over IoT Edge-modules, [inzicht in Azure IoT Edge-modules](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules).
- Als u wilt beginnen met de ontwikkeling van uw IoT Edge-module, Zie [vereisten en hulpprogramma's voor het ontwikkelen van IoT Edge-modules](https://docs.microsoft.com/azure/iot-edge/module-development).

## <a name="technical-requirements"></a>Technische vereisten

De volgende technische vereisten moeten worden voldaan opdat uw IoT Edge-module worden gecertificeerd en gepubliceerd op Azure Marketplace.

### <a name="platform-support"></a>Platformondersteuning

Uw IoT Edge-module moet een van de volgende opties voor platform ondersteunen.

#### <a name="tier-1-platforms-supported-by-iot-edge"></a>Laag 1-platformen worden ondersteund door de IoT Edge

Ondersteuning voor alle laag 1-platformen worden ondersteund door de IoT Edge (zoals vastgelegd in [ondersteuning voor Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support)). Deze optie wordt aangeraden omdat dit een betere klantervaring biedt. Kan de modules die voldoen aan deze criteria worden gepresenteerd. Er moet een module met behulp van deze optie platform:

- Geef een `latest` tag en een versietag (bijvoorbeeld `1.0.1`) die zijn gebouwd met de GitHub-manifest tags [manifest-hulpprogramma](https://github.com/estesp/manifest-tool).
- Gebruik de [het Marketplace-tabblad](./cpp-marketplace-tab.md) om toe te voegen een koppeling naar [compatibel IoT Edge gecertificeerde apparaten](https://aka.ms/iot-edge-certified). Deze koppeling wordt omgezet naar `http://aka.ms/iot-edge-certified`, een website waar klanten kunnen bladeren of zoeken naar apparaten gecertificeerd. Deze website is ook bekend als de [Azure IoT Edge Certified](https://catalog.azureiotsolutions.com/) apparaatcatalogus.

#### <a name="a-subset-of-tier-1-platforms-supported-by-iot-edge"></a>Een subset van laag 1-platformen worden ondersteund door de IoT Edge
  
Ondersteuning voor een subset (ten minste één) van laag 1-platformen worden ondersteund door de IoT Edge (zoals vastgelegd in [ondersteuning voor Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support)). Er moet een module met behulp van deze optie platform:

- Geef een `latest` tag en een versietag (bijvoorbeeld `1.0.1`) die zijn gebouwd met de GitHub-manifest tags [manifest-hulpprogramma](https://github.com/estesp/manifest-tool) als meer dan één platform wordt ondersteund. Manifest tags zijn optioneel als er op één platform wordt ondersteund.
- Gebruik de [Marketplace tabblad](./cpp-marketplace-tab.md) voor een koppeling naar ten minste één IoT Edge-apparaat in de [Azure IoT Edge Certified](https://catalog.azureiotsolutions.com/) apparaatcatalogus.

### <a name="device-dimensions"></a>Apparaat dimensies

IoT Edge module dimensies (CPU/RAM-geheugen/opslag/GPU/enzovoort) op de betreffende IoT Edge-apparaten moeten voldoen aan de volgende vereisten:

- De module moet **werk met ten minste één certified voor IoT Edge** apparaat in de [Azure IoT Edge Certified](https://catalog.azureiotsolutions.com/) apparaatcatalogus.
- De **minimale hardwarevereisten voor** moet worden gedocumenteerd als het laatste punt in de beschrijving van de aanbieding (onder de [Marketplace tabblad](./cpp-marketplace-tab.md)). Desgewenst kunt u ook de aanbevolen hardwarevereisten vermelden als deze aanzienlijk verschillen. Bijvoorbeeld, de volgende sectie toevoegen aan het einde van de beschrijving van uw aanbieding:

 ```html
    <p><u>Minimum hardware requirements:</u> Linux x64 and arm32  OS, 1GB of RAM, 500 Mb of storage</p>
 ```

### <a name="configuration"></a>Configuratie

Dit omvat ook standaardconfiguratie-instellingen voor het maken van de implementatie van een IoT Edge-apparaat zo eenvoudig mogelijk. De container, omvat mogelijk ook de IoT Edge Module SDK om te communiceren met de edgeHub en IoT Hub.

#### <a name="default-configuration"></a>Standaardconfiguratie

IoT Edge-modules moeten worden gestart met de standaardinstellingen die is opgegeven in [SKU-tabblad van de Cloud Partner-portal](./cpp-skus-tab.md). De volgende standaardinstellingen zijn beschikbaar:

- Standaard **routes**
- Standaard **apparaatdubbel-gewenste eigenschappen**
- Standaard **omgevingsvariabelen**
- Standaard **createOptions**

In een scenario waarbij een parameter die is vereist voor een standaardwaarde geen zin (bijvoorbeeld het IP-adres van de server van de klant), voegt u een parameter als de standaardwaarde. Deze waarde wordt tussen haakjes en in hoofdletters. In dit voorbeeld zou u de volgende standaard-omgevingsvariabele instellen:

```
    ServerIPAddress = <MY_SERVER_IP_ADDRESS>
```

#### <a name="configuration-documentation"></a>Configuratiedocumentatie

Alle configuratie-instellingen van een IoT Edge-module moeten duidelijk worden gedocumenteerd (hoe het gebruik van de routes twin gewenste eigenschappen, omgevingsvariabelen en createOptions.) Een koppeling naar de documentatie of de documentatie moet deel uitmaken van de beschrijving van uw aanbieding/sku.

### <a name="tags-and-versioning"></a>Tags en versiebeheer

Klanten moeten kunnen eenvoudig implementeren een module en updates automatisch gegevens ophalen uit de marketplace (in een scenario developer.) Ook moeten ze kunnen gebruiken en blokkeren van een exacte versie die ze hebben getest (in een productiescenario.)

Om te voldoen aan deze klant verwachtingen en worden gepubliceerd in de marketplace, IoT Edge-modules moeten voldoen aan de volgende vereisten:

- Opnemen van een manifest `latest` code, die de meest recente versie op alle ondersteunde platforms verwijst.
- Versie tags moet van het formulier X.Y.Z, waarbij X, Y en Z gehele getallen zijn.
- Een versielabel '', zoals opnemen `1.0.1`, die verwijst naar een specifieke versie op alle ondersteunde platforms.
- Niet bijwerken van 'versie' tags, zoals `1.0.1`, omdat ze onveranderbare moeten zijn.

>[!Note]
>(Optioneel) versiebeheer kan bevatten 'rolling versie' tags als `2.0` en `1.0`. Dit biedt ondersteuning voor meerdere primaire versies parallel onderhouden.

### <a name="telemetry"></a>Telemetrie

Modules met behulp van de IoT-SDK-Module moeten de unieke module-id ingesteld op `PublisherId.OfferId.SkuId` voor telemetrie. Een unieke id kunt de Azure Marketplace om te identificeren van het aantal uitgevoerde module-exemplaren die worden uitgevoerd.

 Gebruik de volgende methoden van de Module van IoT SDK's om in te stellen de `ProductInfo` aan deze id:

- [C\#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.productinfo?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_ProductInfo) 
- [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.productinfo?view=azure-java-stable)

Minder nauwkeurig inzichten zijn voor modules die niet van de IoT-SDK-Module gebruikmaken is beschikbaar via de Cloud Partner-Portal, zoals het aantal downloads.

### <a name="security"></a>Beveiliging

IoT Edge-modules moeten vragen naar de minste bevoegdheden toegang tot de host mogelijk. [Modules in beschermde modus](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) moeten worden vermeden.

### <a name="module-iot-sdk"></a>Module IoT-SDK

Met inbegrip van de IoT-SDK-Module is niet een vereiste voor certificering. Met inbegrip van de IoT-SDK-Module kan evenwel een betere gebruikerservaring. Als u bijvoorbeeld voor de ondersteuning van routering of verzenden van berichten naar de Cloud.

De IoT-SDK-Module is vereist voor het ophalen van telemetrische gegevens over het aantal module-exemplaren die worden uitgevoerd.


## <a name="recertification-process"></a>Proces voor certificering

<!-- Add legal time windows--> Partners gewaarschuwd wanneer er sprake is van een belangrijke wijziging die gevolgen heeft voor hun modules zo opstellen, zoals:

- Ondersteuningsmatrix voor laag 1-os/arch ondersteund door de IoT Edge
- SDK voor IoT-Module
- IoT Edge-Runtime
- De richtlijnen van IoT Edge module Certificeringsinstantie

Partners moeten hun-modules bijwerken en opnieuw certificeren met behulp van het hulpprogramma Cloud Partner-Portal.

## <a name="host-your-iot-edge-module-in-an-azure-container-registry"></a>Host uw IoT Edge-module in een Azure Container Registry

Als u wilt uploaden uw IoT Edge-module voor de Cloud Partner-Portal, moet u eerst om te hosten in een [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR). De module moet bevatten alle tags die u publiceren wilt, met inbegrip van de installatiekopielabels waarnaar wordt verwezen door een manifest tag.


## <a name="next-steps"></a>Volgende stappen

- [Uw IoT Edge-module-aanbieding maken](./cpp-create-offer.md)
